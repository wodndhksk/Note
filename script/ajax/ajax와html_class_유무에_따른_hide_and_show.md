## Jquery를 사용하여 Html 태그의 하위 값의 유무를 판단

```javascript

 $.ajax({
		  url : "/front/busan/multi/signboard.do", // 보낼 controller 
		  type : "GET",
		  data: {"loanGubun": loanGubun},
		  crossDomain: true,

	  		
		  success : function(data) {		  	

			  $("#loan_signboard").html(data);
				
				let noticeCheck = $(".notice__list .swiper-slide").length; // 해당 class이름 태그의  길이 값
					console.log("사용자 : " + noticeCheck)
					if (noticeCheck === 0) {  // 해당class 태그의 값이 0 일시 .notice 는 hide, notice--ma 는 show 한다
						$(".notice").hide();  
						$(".notice--ma").show();

					} else {
						const swiper = new Swiper(".swiper", {
							// Optional parameters
							direction: "vertical",
							autoplay: {
								delay: 20000,
								disableOnInteraction: false,
							},

							observeParents: true,
							observe: true,
							loop: true,
						});
					}
			  
		  },		  
		  error : function(data) {
			  console.log("xml 데이터 읽어오기 실패");
		  }
	  });
```
