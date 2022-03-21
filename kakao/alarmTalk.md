```java
package egovframework.com.admin.scheduler.busan;

import java.net.URLEncoder;

import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;

import egovframework.com.core.util.HttpClient_URL;

public class KdsTest {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		String result = "";
		
		//String url = String.format("https://lib.sejong.go.kr/main/site/rental/roomInfo.do?room_id=R01&reserve_date=20220223");

		try {
			/*
			String url = String.format("http://busan.nanet.go.kr/api/nablNotice.do?newsTarget=NOTICE001&startRowNum=0&endRowNum=5");
			result = HttpClient_URL.requestHttp(url, "");
			
			System.out.println("################## : "+ result);

			JSONParser parser = new JSONParser();
			JSONObject jsonObject = (JSONObject) parser.parse(result);
			
			JSONObject dataObj = (JSONObject) jsonObject.get("data");
			System.out.println("################## : "+ dataObj);
		    
			JSONArray contObj = (JSONArray) dataObj.get("contentList");
			System.out.println("################## : "+ contObj);
			*/

			/*
			String url = String.format("https://dapi.kakao.com/v3/search/book?target=isbn&query=9788954653558");
			String authorization = "KakaoAK b44ad7e82a166d0e405517c4114e7035";
			result = HttpClient_URL.requestHttp(url, authorization);
			
			System.out.println("################## : "+ result);
			*/

			//String url = String.format("http://data4library.kr/api/loanItemSrch?authKey=089768e9f2f7ee6f8fbf5f2a19abccdd8fa985a6ef2d5d5434292161cadacd71&startDt=2022-02-01&endDt=2022-02-22");
			
			//String subject = URLEncoder.encode("신청자료알림(부산)", "UTF-8");
			//String text = URLEncoder.encode("[국회부산도서관]\n\n신청하신 자료가 2층 주제자료실 사서데스크에 도착하였으니 찾아가십시오.\n\n문의 : 051-608-8153", "UTF-8");
			//String tempCode = "DISP_BNK_0001"; // L : 온라인 열람신청

			String receiver = "01087197947";
			
			String subject = URLEncoder.encode("야간이용신청자료알림(부산)", "UTF-8");
			String text = URLEncoder.encode("[국회부산도서관]\n\n야간이용 신청하신 자료가 1층 종합자료실 사서데스크에 도착하였으니 찾아가십시오.\n\n문의 : 051-608-8146", "UTF-8");
			String tempCode = "DISP_BNK_0002";	// R : 야간이용신청
			String url = "http://kama.nanet.go.kr:8080/send/msg.do?SYSTEM_ID=nablcalling&DSTADDR=%s&CALLBACK=0516088114&SUBJECT=%s&TEXT=%s&TEXT_TYPE=0&MSG_TYPE=7&TEMPLATE_CODE=%s";
			
			String request_url = String.format(url, receiver, subject, text, tempCode);
			result = HttpClient_URL.requestHttp(request_url);
			
			System.out.println("################## : "+ request_url); 
			System.out.println("################## : "+ result);
			
			JSONParser parser = new JSONParser();
			JSONObject jsonObject = (JSONObject) parser.parse(result);

			System.out.println("################## : "+ jsonObject.get("RESULT_CODE"));
			System.out.println("################## : "+ jsonObject.get("RESULT_MESSAGE"));
			
			/*

################## : http://kama.nanet.go.kr:8080/send/msg.do?SYSTEM_ID=nablcalling&DSTADDR=01087197947&CALLBACK=0516088114&SUBJECT=%EC%95%BC%EA%B0%84%EC%9D%B4%EC%9A%A9%EC%8B%A0%EC%B2%AD%EC%9E%90%EB%A3%8C%EC%95%8C%EB%A6%BC%28%EB%B6%80%EC%82%B0%29&TEXT=%5B%EA%B5%AD%ED%9A%8C%EB%B6%80%EC%82%B0%EB%8F%84%EC%84%9C%EA%B4%80%5D%0A%0A%EC%95%BC%EA%B0%84%EC%9D%B4%EC%9A%A9+%EC%8B%A0%EC%B2%AD%ED%95%98%EC%8B%A0+%EC%9E%90%EB%A3%8C%EA%B0%80+1%EC%B8%B5+%EC%A2%85%ED%95%A9%EC%9E%90%EB%A3%8C%EC%8B%A4+%EC%82%AC%EC%84%9C%EB%8D%B0%EC%8A%A4%ED%81%AC%EC%97%90+%EB%8F%84%EC%B0%A9%ED%95%98%EC%98%80%EC%9C%BC%EB%8B%88+%EC%B0%BE%EC%95%84%EA%B0%80%EC%8B%AD%EC%8B%9C%EC%98%A4.%0A%0A%EB%AC%B8%EC%9D%98+%3A+051-608-8146&TEXT_TYPE=0&MSG_TYPE=7&TEMPLATE_CODE=DISP_BNK_0002
################## : {"RESULT_CODE":"200","RESULT_MESSAGE":"성공"}
################## : 200
################## : 성공
 
			 * */
	    	
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		

	}

}

```
