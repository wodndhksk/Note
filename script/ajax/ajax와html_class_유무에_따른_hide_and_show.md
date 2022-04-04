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
## 온라인 열람신청 하위 태그값이 없다면 서고알람신청을 보여준다
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
##  $("#loan_signboard").html(data); 의 data 값
```html
<%@page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions"%>
	
	
	
	<c:forEach var="row" items="${loanBoard}" varStatus="rowNum">
		<c:if test="${rowNum.first }">
			<div class="list__block swiper-slide">
			<ul>
		</c:if>
				<li>
					<div class="info">
						<div class="num">${rowNum.count }</div>
						
						<c:set var="nameValue" value="${row.userName }" />
													
						<c:if test="${fn:length(row.userName) < 4}"> 
							<div class="name length4"><div>${fn:substring(row.userName,0, 2) } *</div><sub>님</sub></div>
						</c:if>
						<c:if test="${fn:length(row.userName) > 3}"> 
							<div class="name length4"><div>${fn:substring(row.userName,0, 2) } **</div><sub>님</sub></div>
						</c:if>
						<div class="status">Online</div>
					</div>
				</li>
				
		<c:choose>
			<c:when test="${rowNum.last }">
				</ul>
				</div>
			</c:when>
			<c:when test="${rowNum.count % 10 == 0}">
				</ul>
				</div>
				<div class="list__block swiper-slide">
				<ul>
			</c:when>
		</c:choose>	
		
	</c:forEach>
```
## Controller
```java
	@GetMapping("/signboard.do")
	@ResponseBody
	public ModelAndView signboard(HttpServletRequest req, HttpServletResponse res, String loanGubun) throws Exception {
		
		ModelAndView mv = new ModelAndView();
		Map<String, String> params = new HashMap<>();
		params.put("loanGubun", loanGubun); //층수 구분
		List<LoanSignBoardVo> board = multiFrontService.selectList(params);	
		
		mv.addObject("loanBoard",board);
		return mv;

	}

```
