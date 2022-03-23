##1 요청만 할때 ( 따로 데이터를 받지 않는 경우 )
```javascript

function alarmTalk(loanGubun, seqNo){
	console.log(loanGubun);
	 $.ajax({
	  url : "/front/busan/multi/alarmTalk.do", // 보낼 controller 
	  type : "GET",
	  data: {"loanGubun": loanGubun, "seqNo" : seqNo},
	  crossDomain: true,

	  success : function() {		  	
		 console.log("알람톡 전송 성공!"); 	
		 $("#notificationBtn").remove();
		  	  
	  },		  
	  error : function(data) {
		  console.log("전송 실패");
	  }
  }); 

}
```
