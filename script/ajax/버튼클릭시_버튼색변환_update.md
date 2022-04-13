
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
