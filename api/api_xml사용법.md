```java
package egovframework.com.admin.scheduler.busan;

import java.net.URLEncoder;
import java.util.List;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.parsers.DocumentBuilderFactory;

import org.json.simple.JSONObject;
import org.json.simple.parser.JSONParser;
import org.w3c.dom.Document;
import org.w3c.dom.Element;
import org.w3c.dom.Node;
import org.w3c.dom.NodeList;

import egovframework.com.core.model.Params;
import egovframework.com.core.model.Record;
import egovframework.com.core.util.HttpClient_URL;

public class test22 {

	private String CONTROL_NO_URL;
	
	private String CONTROL_NO_KEY;
	
	
	private static String getTagValue(String tag, Element eElement) {
		NodeList nlList = eElement.getElementsByTagName(tag).item(0).getChildNodes();
		Node nValue = (Node) nlList.item(0);
		if(nValue == null) return null;
		return nValue.getNodeValue();
	}

	public static String getCallNo(String controlno) throws Exception {
			
//////////////////////////////////Spring에서 사용할 경우 로직/////////////////////////////////////			
//		DB에서 API url과 key값을 관리하고 있을 경우 (URL에 key, controlno값 붙여서 호출)
		//where절 조건 값
//		Params p = new Params();
//		p.put("site_cd", SITE_CD);
//		p.put("code_id", "AT22");
		
		//조건에 맞는 값 selectList
//		List<Record> getBookInfo =bookInfoService.getApiBookInfo(p);
		
		//API_CALL_INFO 라는 컬럼값 (API URL)
//		CONTROL_NO_URL = getBookInfo.get(0).get("API_CALL_INFO").toString();
		//API_KEY 라는 컬럼값 (API KEY)
//		CONTROL_NO_KEY = getBookInfo.get(0).get("API_KEY").toString();
		//DB에 저장한 값중 필요한 인자값을 %s 로 비워두고 String.format을 사용하여 붙이기
//		String url = String.format(CONTROL_NO_URL + CONTROL_NO_KEY, controlNo);
		
////////////////////////////////////////////////////////////////////////////////////////////////		
		
		String url = "http://apis.data.go.kr/9720000/detailinfoservice/detail?controlno=MONO1201515702&serviceKey=Nrv0VJBL5OL5OdPhB23wqcul2GqcPwDDRhtl2xlc6vvNr%2FFMRIs7BMOV%2Fa7JTwcBzn3%2BouC%2FCghNHcAlgbQhfQ%3D%3D";
		DocumentBuilderFactory dbFactoty = DocumentBuilderFactory.newInstance();
		DocumentBuilder dBuilder = dbFactoty.newDocumentBuilder();
		Document doc = dBuilder.parse(url);
		
		doc.getDocumentElement().normalize();
		System.out.println("Root element:" + doc.getDocumentElement().getNodeName());
		
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
	public static void main(String[] args) {
		
		
		try {
			getCallNo("MONO1201515702");
		} catch (Exception e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		
		

	}

}


```
