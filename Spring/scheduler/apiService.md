## 스케쥴러에서 사용하는 

```java
package egovframework.com.admin.scheduler.busan.service.impl;

import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.URL;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

import javax.annotation.PostConstruct;
import javax.net.ssl.HttpsURLConnection;
import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

import egovframework.com.admin.book.service.BookInfoService;
import egovframework.com.core.model.Params;
import egovframework.com.core.model.Record;
import egovframework.com.core.util.HttpClient_URL;

@Component
public class ApiDataService {
	
	@Value("#{application['BUSAN_SITE_CD']}")
	private String SITE_CD;
	
	@Autowired
	private BookInfoService bookInfoService;
	
	private String CONTROL_NO_URL;
	
	private static String getTagValue(String tag, Element eElement) {
		NodeList nlList = eElement.getElementsByTagName(tag).item(0).getChildNodes();
		Node nValue = (Node) nlList.item(0);
		if(nValue == null) return null;
		return nValue.getNodeValue();
	}
	public String getCallNo(String controlNo) throws Exception {
	//	Params p = new Params();
	//	p.put("site_cd", SITE_CD);
	//	p.put("code_id", "AT22");
	//	List<Record> getBookInfo =bookInfoService.getApiBookInfo(p);
	//		
	//	CONTROL_NO_URL = getBookInfo.get(0).get("API_CALL_INFO").toString();
		
		String url = "http://apis.data.go.kr/9720000/detailinfoservice/detail?controlno="+controlNo+"&serviceKey=Nrv0VJBL5OL5OdPhB23wqcul2GqcPwDDRhtl2xlc6vvNr%2FFMRIs7BMOV%2Fa7JTwcBzn3%2BouC%2FCghNHcAlgbQhfQ%3D%3D";
		DocumentBuilderFactory dbFactoty = DocumentBuilderFactory.newInstance();
		DocumentBuilder dBuilder = dbFactoty.newDocumentBuilder();
		Document doc = dBuilder.parse(url);
		
		doc.getDocumentElement().normalize();
		
		NodeList nList = doc.getElementsByTagName("item");
		Params bookParams = new Params();
		
		String callNo = "";
		
		for(int temp = 0; temp < nList.getLength(); temp++){
			Node nNode = nList.item(temp);
			if(nNode.getNodeType() == Node.ELEMENT_NODE){			
				Element eElement = (Element) nNode;
			
				if("청구기호".equals(getTagValue("name", eElement))){
					callNo = getTagValue("value", eElement);
					break;
				}			
			}
		}
		
		return callNo;
	}

	
	public String jsonDateToString(List<Map> param, int num, String contents) {
		String result = param.get(num).get(contents).toString();
		
		return result;
	}
	
	public String replaceKakaoDate(String data) {
		String result = data;
		if(result.contains("[")) {
			result = result.replace("[", "");
			result = result.replace("]", "");
			result = result.replace("\"", "");
			result = result.replace("\"", "");
		}
		
		return result;
	}

	public  Map<String, String> kakaoApiObjects(String isbn){
		String thumbnail = "";
//		String contents = "";
		String authors = "";
		String translators = "";
		String publisher = "";
		String pubYear = "";
		String description = "";
		String datetime="";
		
		Map<String, String> returnMap = new HashMap<>();
		
		try {
									
			URL kakao_url = new URL("https://dapi.kakao.com/v3/search/book?sort=accuracy&page=1&size=10&query="+isbn+"&target=isbn");	
			
			String authorization = "KakaoAK f0388f68972195482ea4f5ad18f2407b";
			HttpsURLConnection con = (HttpsURLConnection)kakao_url.openConnection();
			
			con.setRequestMethod("GET");
	        con.setRequestProperty("Authorization", authorization);
	        int responseCode = con.getResponseCode();
	        BufferedReader br = new BufferedReader(new InputStreamReader(con.getInputStream(), "UTF-8"));
	        
			if(responseCode==200) { // 정상 호출
	            br = new BufferedReader(new InputStreamReader(con.getInputStream(), "UTF-8"));
	        } else {  // 에러 발생
	            br = new BufferedReader(new InputStreamReader(con.getErrorStream(), "UTF-8"));
	        }
			
			StringBuilder sb = new StringBuilder();
			String line;
			
			while((line = br.readLine()) != null){
				sb.append(line);
			}
			
			String xml = sb.toString();
			
			br.close();
			con.disconnect();
			
			JSONParser parser = new JSONParser();
			JSONObject obj = (JSONObject) parser.parse(xml);
			
			List<Map> jsonArray = (List<Map>) obj.get("documents");
			
			if(jsonArray.size() >= 1){
				
				JSONObject bookObject = (JSONObject) jsonArray.get(0);
				thumbnail = jsonArray.get(0).get("thumbnail").toString().replaceAll("R120x174.q85", "R678x0");
//				contents = bookObject.get("contents").toString();
				authors = jsonArray.get(0).get("authors").toString();
				translators = jsonArray.get(0).get("translators").toString();
				publisher = jsonArray.get(0).get("publisher").toString();
				pubYear = jsonArray.get(0).get("datetime").toString();
				description = jsonArray.get(0).get("contents").toString();
				datetime = jsonArray.get(0).get("datetime").toString();
			}
			 
			 returnMap.put("thumbnail", thumbnail);
			 returnMap.put("authors", authors);
			 returnMap.put("translators", translators);
			 returnMap.put("publisher", publisher);
			 returnMap.put("pubYear", pubYear);
			 returnMap.put("description", description);
			 returnMap.put("datetime", datetime);
			 
//			 returnMap.put("contents", contents);
			 
		} catch (Exception e) {
			
			System.out.println(e);
		}
		
		return returnMap;
	}

}


```
