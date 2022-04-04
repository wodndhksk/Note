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
## 온라인 
```html
		<!-- 온라인 열람신청 알림 -->
			<div class="notice notice--slider">
				<div class="notice__title">
					<small>Online Application</small>
					<h2>온라인 열람신청 알림</h2>
				</div>
				<div class="notice__list">
					<div class="list swiper" >
						<div class="swiper-wrapper" id="loan_signboard"></div>					
					</div>
				</div>		
			</div>
			<!-- // 온라인 열람신청 알림 -->
			
			<!-- 서고자료 열람신청 안내 -->		
			<div class="notice notice--ma" style="display: none">
				<div class="notice__title">
					<small>Library Materials Application</small>
					<h2>온라인 열람신청 안내</h2>
				</div>
				<div class="ma">
					<div class="ma__block">
						<div class="ma__contents">
							<small>Howtoapply</small>
							<h2>신청방법</h2>
							<div>
								당일 17시 이전<br />
								온라인 신청
							</div>
						</div>
						<div class="ma__contents">
							<small>Placetoreceiveit</small>
							<h2>수령장소</h2>
							<div>
								2층 주제자료실<br />
								사서데스크
								<p>18시 이후는 <br />1층 종합자료실 사서데스크</p>
							</div>
						</div>
					</div>
				</div>
			</div> 
			
			<!-- //서고자료 열람신청 안내 -->
```
