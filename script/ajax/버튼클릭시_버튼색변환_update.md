
## 버튼 Html
```html
   <button class="btn btn-white" id="notificationBtn${row.loanSeqNo }" onclick='alarmTalk("${row.loanGubun}","${row.loanSeqNo }")'>표출</button>
   <button class="btn btn-white" id="deleteBtn${row.loanSeqNo }" onclick='deleteBtn("${row.loanSeqNo }")'>삭제</button>
```

## 버튼 script
```javascript
function deleteBtn(seqNo){
	$("#deleteBtn" + seqNo).css("background-color", "yellow");
	console.log(seqNo);
	
	$.ajax({
		  url : "/front/busan/multi/deleteBtn.do", // 보낼 controller 
		  type : "GET",
		  data: {"seqNo" : seqNo}, 
		  crossDomain: true,

		  success : function() {		  	
			 console.log("버튼 삭제 데이터 전송 성공!!"); 	
			/*  $("#notificationBtn").remove(); */
			  	  
		  },		  
		  error : function(data) {
			  console.log("전송 실패");
		  }
	  }); 
}

function alarmTalk(loanGubun, seqNo){
	$("#notificationBtn" + seqNo).css("background-color", "yellow");
	
	console.log(loanGubun);
	 $.ajax({
	  url : "/front/busan/multi/alarmTalk.do", // 보낼 controller 
	  type : "GET",
	  data: {"loanGubun": loanGubun, "seqNo" : seqNo},
	  crossDomain: true,

	  success : function() {		  	
		 console.log("알람톡 전송 성공!"); 	
		/*  $("#notificationBtn").remove(); */
		  	  
	  },		  
	  error : function(data) {
		  console.log("전송 실패");
	  }
  }); 

}

// 30초마다 새로고침
setTimeout((function() {
	  window.location.reload();
	}), 30000); 
```
## 알림톡 controller
```java
@GetMapping("/alarmTalk.do")
	@ResponseBody
	public void alarmTalk(HttpServletRequest req, HttpServletResponse res, String loanGubun, String seqNo) throws Exception {
		
		//알람 보낼 회원 리스트
		Map<String, String> sendParams = new HashMap<>();
//		sendParams.put("loanGubun", loanGubun); //층수
		sendParams.put("seqNo", seqNo); //키값 
		List<LoanSignBoardVo> sendMessageList = multiFrontService.selectGetAlarmPhoneList(sendParams);
			
		String result = "";
		if(sendMessageList != null) {
			//전화번호(receiver)를 받아서 톡 보내기
			String receiver = sendMessageList.get(0).getUserTel();	
			String subject="";
			String text="";
			String tempCode="";
			
			// L : 온라인 , R : 야간
			if(loanGubun.equals("R")) {
				subject = URLEncoder.encode("야간이용신청자료알림(부산)", "UTF-8");
				text = URLEncoder.encode("[국회부산도서관]\n\n야간이용 신청하신 자료가 1층 종합자료실 사서데스크에 도착하였으니 찾아가십시오.\n\n문의 : 051-608-8146", "UTF-8");
				tempCode = "DISP_BNK_0002";// R : 야간이용신청
			}else {
				subject = URLEncoder.encode("신청자료알림(부산)", "UTF-8");
				text = URLEncoder.encode("[국회부산도서관]\n\n신청하신 자료가 2층 주제자료실 사서데스크에 도착하였으니 찾아가십시오.\n\n문의 : 051-608-8153", "UTF-8");
				tempCode = "DISP_BNK_0001"; // L : 온라인 열람신청
			}

			String url = "http://kama.nanet.go.kr:8080/send/msg.do?SYSTEM_ID=nablcalling&DSTADDR=%s&CALLBACK=0516088114&SUBJECT=%s&TEXT=%s&TEXT_TYPE=0&MSG_TYPE=7&TEMPLATE_CODE=%s";		
			String request_url = String.format(url, receiver, subject, text, tempCode);
			result = HttpClient_URL.requestHttp(request_url);		
			JSONParser parser = new JSONParser();
			//JSONObject jsonObject = (JSONObject) parser.parse(result);
			
			//update ALARM_TALK = '1'
			Map<String, String> udateParams = new HashMap<>();
			udateParams.put("seqNo", seqNo);
			multiFrontService.updateIfYouGetAlarm(udateParams);
			
		}
	}

```

## 삭제 controller
```java
	@GetMapping("/deleteBtn.do")
	@ResponseBody
	public void deleteBtn(HttpServletRequest req, HttpServletResponse res, String seqNo) throws Exception {
		
		Map<String, String> params = new HashMap<>();
		params.put("seqNo", seqNo); //층수 구분
		multiFrontService.delete(params);	
	}
```
