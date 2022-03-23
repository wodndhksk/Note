## 1 요청만 할때 ( 따로 데이터를 받지 않는 경우 )
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

## 2 요청 후 데이터를 받을 때 (console.log(data) 로 확인가능)

```javascript
function signBoard(){


	  $.ajax({
		  url : "/front/busan/multi/signboard.do", // 보낼 controller 
		  type : "GET",
		  data: {"loanGubun": loanGubun},
		  crossDomain: true,
/* 		  dataType: "json", */
	  		
		  success : function(data) {		  	
			 // console.log(data); //json object 확인용	
			  $("#loan_signboard").html(data);
			  
			  $.getScript("/resources/assets/busan/front/js/multivision_intervalReset.js")
				.done(function () {
					$.getScript("/resources/assets/busan/front/js/multivision_attention.js");
				})
				.done(function () {
					$.getScript("/resources/assets/busan/front/js/multivision_clock.js");
				});
			  
		  },		  
		  error : function(data) {
			  console.log("xml 데이터 읽어오기 실패");
		  }
	  });
}	

```
