## 버튼 누르면 팝업에 text 내용 표출하기 예시 (목차버튼에 onclick 존재, pupup란 id="bookInfoText" 존재)
```html

<div class="button">
  <a href="#" class="button__block button__modal" data-id="modal1" onclick="bookInfo('${book.controlNo}')">목차보기</a>
  <a href="/front/busan/book/book_new.do" class="button__block">이전화면</a>
</div>

<div class="modal" id="modal1">
	<div class="modal__body">
		<div class="scrollBlock" id="bookInfoText">
			<!-- 목차내용란 -->
		</div>
		<div class="button">
			<a href="#" class="button__block button__modalClose on">닫기</a>
		</div>
	</div>
</div>

```
## 목차버튼 클릭시 ajax를 통해 청구기호값을 던지고 text data값을 받는다. 그후 목차 내용란 안에 InnerHTML 
```javascript
function bookInfo(controlNo){
			console.log("클릭!:" + controlNo);
			$.ajax({
				  url : "/front/busan/book/bookInfoList.do", // 보낼 controller 
				  type : "GET",
				  crossDomain: true,
				  data : {"controlNo": controlNo},
				  dataType: "text", 
			  		
				  success : function(data) {		
					  console.log("성공");
					  console.log(data); //json object 확인용	
					  $("#bookInfoText").html(data);  // 
					  
				  },		  
				  error : function(data) {
					  console.log("xml 데이터 읽어오기 실패");
				  }
			  });
		}
```

## 청구기호를 받는 처리를 한다(xml형태의 api 데이터 받기) 그후 text를 리턴한다
```java
@GetMapping("/bookInfoList.do")
	@ResponseBody
	public String getCallNo(String controlNo) throws Exception {
		//	Params p = new Params();
		//	p.put("site_cd", SITE_CD);
		//	p.put("code_id", "AT22");
		//	List<Record> getBookInfo =bookInfoService.getApiBookInfo(p);
		//		
		//	CONTROL_NO_URL = getBookInfo.get(0).get("API_CALL_INFO").toString();
			
			String url = "http://apis.data.go.kr/9720000/detailinfoservice/toc?controlno="+controlNo+"&serviceKey=Nrv0VJBL5OL5OdPhB23wqcul2GqcPwDDRhtl2xlc6vvNr%2FFMRIs7BMOV%2Fa7JTwcBzn3%2BouC%2FCghNHcAlgbQhfQ%3D%3D";
			DocumentBuilderFactory dbFactoty = DocumentBuilderFactory.newInstance();
			DocumentBuilder dBuilder = dbFactoty.newDocumentBuilder();
			Document doc = dBuilder.parse(url);
			
			doc.getDocumentElement().normalize();
			
			NodeList nList = doc.getElementsByTagName("toc");
			Params bookParams = new Params();
			
			String text = "";
			
			for(int temp = 0; temp < nList.getLength(); temp++){
				Node nNode = nList.item(temp);
				if(nNode.getNodeType() == Node.ELEMENT_NODE){					
					Element eElement = (Element) nNode;
					text = eElement.getChildNodes().item(0).toString();
					}			
				}		
			return text;
		}
```
