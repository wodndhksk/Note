```java
package egovframework.com.admin.scheduler.busan;

import java.text.SimpleDateFormat;
import java.util.ArrayList;
import java.util.Calendar;
import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

import com.google.gson.Gson;

import egovframework.com.admin.book.service.BookInfoService;
import egovframework.com.admin.book.service.OutBestBookVo;
import egovframework.com.admin.book.service.OutBookNewBase;
import egovframework.com.admin.book.service.OutBookNewListDataVo;
import egovframework.com.admin.book.service.OutBookNewVo;
import egovframework.com.admin.common.service.CommonApiService;
import egovframework.com.admin.scheduler.busan.service.impl.ApiDataService;
import egovframework.com.core.model.Params;
import egovframework.com.core.util.ConvertUtil;
import egovframework.com.core.util.HttpClient_URL;
import egovframework.com.core.util.SecurityUtil;
import egovframework.com.core.util.ThumbnailUtil;
import egovframework.com.security.userdetail.UserInfo;
import lombok.extern.slf4j.Slf4j;

/**
 * 외부 (도서정보 스케쥴)
 *
 * @author uuser
 */
@Slf4j
@Component
public class BusanBookNewScheduler {
	@Autowired
	private BookInfoService bookInfoService;

	private boolean isDataDetail = false;

	@Autowired
	private CommonApiService commonApiService;
	
	@Autowired
	private ApiDataService apiDataService;

	@Value("#{application['BUSAN_OUT_BOOK_NEW_URL']}")
	private String NEW_URL;

	private String SITE_CD ="";

	private String SM_PARAM = "";

	@Scheduled(cron = "0 15 * * * * ") // 58 58 8-21 * * *
//	@Scheduled(fixedDelay=1000000) // Test (10초마다)
	public void NewBookInfoSchedule() {
		log.info(">>>>>>>>>>>>>>>>>>>>> BusanNewBookInfoSchedule start");
		Params params = new Params();

		UserInfo user = SecurityUtil.getUserInfo();

		List<OutBookNewVo> list = new ArrayList<>();
		OutBookNewVo vo = null;
		String result = "";
		Map<String, Object> inputMap = new HashMap<>();
		int index = 0;

		OutBookNewBase outBookNewBase = null;

		try {

			Date time = new Date();
			SimpleDateFormat format = new SimpleDateFormat("yyyyMMdd");

			Calendar cal = Calendar.getInstance();
			cal.setTime(time);
			cal.add(Calendar.MONTH, -1);

			String end_date = format.format(time);
			String start_date = format.format(cal.getTime());

			String request_url = NEW_URL;		
			result = HttpClient_URL.requestHttp(request_url);

			JSONParser parser = new JSONParser();
			JSONObject jsonObject = (JSONObject) parser.parse(result);
			JSONObject data = (JSONObject) jsonObject.get("data");
			List<Map> contentList = (List<Map>) data.get("contentList");
			
			Map<String, String> returnMap;
			SimpleDateFormat formatDate = new SimpleDateFormat("yyyy");
			String currentYear = formatDate.format(time);
			//////////////////API TEST//////////////////
			Map<String, String> returnMapTest = new HashMap<>();
			//국회api에서 isbn 값 가져오기
			String isbnTest = apiDataService.jsonDateToString(contentList, 0, "isbn");
			//isbn값으로 카카오 책 api 정보 가져오기
			returnMapTest = apiDataService.kakaoApiObjects(isbnTest);
			String a = returnMapTest.get("authors").toString();
			////////////////////////////////////////////
			
			for(int i=0; i<contentList.size(); i++) {
				vo = new OutBookNewVo();
				returnMap = new HashMap<>();
				
				String isbn = apiDataService.jsonDateToString(contentList, i, "isbn");
				if(isbn.length() > 13)
					isbn = isbn.substring(0, 13);
				returnMap = apiDataService.kakaoApiObjects(isbn);
				
				String pubYear ="";
				//출판날짜 날짜만 추출
				if(!"".equals(isbn)) {
					pubYear = returnMap.get("datetime").toString();
					pubYear = pubYear.substring(0, 10);
				}
				///////test//////
				String controlNo = apiDataService.jsonDateToString(contentList, i ,"controlNo");
				String title = apiDataService.jsonDateToString(contentList, i ,"title");
				/////////////////
				
				vo.setCallNo(apiDataService.jsonDateToString(contentList, i, "controlNo"));
				vo.setTitle(apiDataService.jsonDateToString(contentList, i, "title"));
				vo.setIsbn(isbn);
				//Bookkey에 API 'id'값 넣음
				vo.setBookKey(apiDataService.jsonDateToString(contentList, i, "id"));
				
				vo.setNanetImg(apiDataService.jsonDateToString(contentList, i, "imgUrl"));
				vo.setBkAge(apiDataService.jsonDateToString(contentList, i, "bkAge"));
				vo.setBkAgeNm(apiDataService.jsonDateToString(contentList, i, "bkAgeNm"));
				//현재날짜(년도)
				vo.setBkYear(currentYear);
				vo.setBkWeek(apiDataService.jsonDateToString(contentList, i, "bkWeek"));
				vo.setImage(apiDataService.jsonDateToString(contentList, i, "imgUrl"));
				
				//isbn값이 있으면
				if(!"".equals(isbn)) {
					vo.setAuthor(returnMap.get("authors").toString());
					vo.setPublisher(returnMap.get("publisher").toString());
					vo.setPubYear(pubYear);
					vo.setTranslators(returnMap.get("translators").toString());
					vo.setDescription(returnMap.get("description").toString());
					vo.setKakaoImg(returnMap.get("thumbnail").toString());			
				}
				
				list.add(vo);			
			}

			vo = null;

			if (list.size() > 0) {

				inputMap = new HashMap<>();
				try {
					// 2. 임시 테이블 비워주기

					inputMap.put("TB_NAME", "TB_OUT_NEWBOOK_TMP");
					inputMap.put("site_cd", SITE_CD);
					inputMap.put("content_type", "CT07");
					inputMap.put("sm_param", SM_PARAM);
					bookInfoService.deleteBook(inputMap);
				} catch (Exception e1) {
					e1.printStackTrace();
					// _log.error("OutEBookNewScheduler 2 : " + e1.toString());
				}

				try {
					for (OutBookNewVo v : list) {

						inputMap = new HashMap<>();
						inputMap = ConvertUtil.convertToMap(v);
						inputMap.remove("sort");
						inputMap.put("TB_NAME", "TB_OUT_NEWBOOK_TMP");
						inputMap.put("site_cd", SITE_CD);
						inputMap.put("content_type", "CT07");
						inputMap.put("sm_param", SM_PARAM);
						bookInfoService.insertBook(inputMap);

					}
				} catch (Exception e1) {
					e1.printStackTrace();
					// _log.error("OutEBookScheduler 3 : " + e1.toString());
				}

				inputMap = new HashMap<>();
				Map<String, Object> tmpMap = null;

				try {

					// 4. 임시 테이블과 실제 테이블 데이터 비교

					inputMap.put("site_cd", SITE_CD);
					inputMap.put("content_type", "CT07");
					inputMap.put("sm_param", SM_PARAM);
					List<OutBookNewVo> diffList = bookInfoService.selectAddSubList2(inputMap);
					List<String> addList = new ArrayList<>();

					// 4-1. 빠져야 하는데이터 제거
					for (OutBookNewVo v : diffList) {
						if ("-".equals(v.getAddsub())) {
							vo = new OutBookNewVo();
							inputMap.put("SEQ", v.getBookKey());
							inputMap.put("site_cd", SITE_CD);
							inputMap.put("content_type", "CT07");
							inputMap.put("sm_param", SM_PARAM);
							vo = bookInfoService.selectNew(inputMap);

							// inputMap = new HashMap<>();
							// inputMap.put("id", v.getBookKey());
							// inputMap.put("TB_NAME", "TB_OUT_BESTBOOK");
							// _OutBestBookDao.delete(inputMap);
							
							inputMap.put("id", v.getBookKey());
							inputMap.put("TB_NAME", "TB_OUT_NEWBOOK");
							bookInfoService.deleteBook(inputMap);

							/*
							 * tmpMap = new HashMap<>(); tmpMap.put("sortFlag", "down");
							 * tmpMap.put("checkSortNum", vo.getSort()); _OutNewBookDao.update(tmpMap);
							 */
						} else {
							// 추가 데이터
							addList.add(v.getBookKey());
						}
					}

					// 4-2. 추가 되어야 하는 데이터 입력
					for (OutBookNewVo v : list) {
						if (addList.contains(v.getBookKey())) {
							v.setSort(1);
							v.setRegId("");

							inputMap = new HashMap<>();
							inputMap = ConvertUtil.convertToMap(v);

							// 4-4. 최근 들어오는 도서가 맨위로
							tmpMap = new HashMap<>();
							tmpMap.put("sortFlag", "up");
							tmpMap.put("checkSortNum", 1);
							tmpMap.put("TB_NAME", "TB_OUT_NEWBOOK");
							tmpMap.put("site_cd", SITE_CD);
							tmpMap.put("content_type", "CT07");
							tmpMap.put("sm_param", SM_PARAM);
							bookInfoService.updateBook(tmpMap);

							inputMap.put("SEQ", v.getBookKey());
							inputMap.put("TB_NAME", "TB_OUT_NEWBOOK");
							inputMap.put("site_cd", SITE_CD);
							inputMap.put("content_type", "CT07");
							inputMap.put("sm_param", SM_PARAM);

							bookInfoService.insertBook(inputMap);
						} else {
							// 4-5. 증분 데이터가 없는경우 나머지는 기존 데이터 update
							v.setModId("");
							inputMap = new HashMap<>();
							inputMap = ConvertUtil.convertToMap(v);

							inputMap.put("SEQ", v.getBookKey());
							inputMap.remove("useYn");
							inputMap.remove("sort");
							inputMap.put("TB_NAME", "TB_OUT_NEWBOOK");
							inputMap.put("site_cd", SITE_CD);
							inputMap.put("content_type", "CT07");
							inputMap.put("sm_param", SM_PARAM);
							bookInfoService.updateBook(inputMap);
						}
					}

					// 4-6. 임시 테이블 비워주기
					inputMap = new HashMap<>();
					inputMap.put("TB_NAME", "TB_OUT_NEWBOOK_TMP");
					inputMap.put("site_cd", SITE_CD);
					inputMap.put("content_type", "CT07");
					inputMap.put("sm_param", SM_PARAM);
					bookInfoService.deleteBook(inputMap);

					// 메모리 초기화
					diffList = null;
					addList = null;

				} catch (Exception e1) {
					e1.printStackTrace();
					// _log.error("OutEBookScheduler 3 : " + e1.toString());
				}
			}

		} catch (Exception e) {
			e.printStackTrace();
			// _log.error("OutAudioScheduler 1 : " + e.toString());

		} finally {
			inputMap = null;
			list = null;
		}
		// vo = null;


	}

}


```
