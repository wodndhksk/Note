```html
<%@page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="UTF-8" />
		<meta http-equiv="X-UA-Compatible" content="IE=edge" />
		<meta name="viewport" content="width=device-width, initial-scale=1.0" />
		<link rel="stylesheet" href="/resources/assets/busan/front/fonts/NotoSansKr.css" />
		<link rel="stylesheet" href="/resources/assets/busan/front/fonts/TrendexSSi.css" />
		<link rel="stylesheet" href="/resources/assets/busan/front/css/common.min.css" />
		<link rel="stylesheet" href="/resources/assets/busan/front/css/front.min.css" />
		<script src="/resources/assets/busan/front/js/jquery-3.6.0.min.js"></script>
		<script src="/resources/assets/busan/front/js/schedule.js"></script>
		<title>스케쥴</title>
	</head>


<div class="multivision">
	<div class="multivision__block multivision__block--reference">
		<section class="multivision__attention">
			<!-- 온라인 열람신청 알림 -->
			<div class="notice notice--slider">
				<div class="notice__title">
					<small>Online Application</small>
					<h2>온라인 열람신청 알림</h2>
				</div>
				<div class="notice__list">
					<div class="list" id="loan_signboard">

						
					</div>
				</div>
			</div>
			<!-- // 온라인 열람신청 알림 -->
		</section>

		<section class="multivision__reference">
			<div class="reference">
				<div class="reference__block reference__block--rental">
					<div class="reference__title">
						<small>Rental date</small>
						<h1>대출일</h1>
					</div>
					<div class="reference__rentalTime">
						<div class="times">
							<div class="year"></div>
							<div class="month"></div>
							<div class="date"></div>
						</div>
						<div class="today">오늘은 <span>${dayOfWeek }요일</span> 입니다.</div>
					</div>
				</div>

				<div class="reference__block reference__block--return">
					<div class="reference__title">
						<small>Return date</small>
						<h1>반납일</h1>
					</div>
					<div class="reference__returnTime">
						<div class="times">
							<div class="year">
								<span>year</span>
								<strong>${returnYear }</strong>
							</div>
							<div class="month">
								<span>month</span>
								<strong>${returnMonth }</strong>
							</div>
							<div class="day">
								<span>day</span>
								<strong>${returnDays }</strong>
							</div>
							<div class="date">
								<span>Day of week</span>
								<strong>${returnDayOfWeek }</strong>
							</div>
						</div>
						<div class="clock">
							<div class="hour"></div>
							<div class="colon">:</div>
							<div class="minutes"></div>
						</div>
					</div>
				</div>
			</div>
		</section>
	</div>
</div>

<script>
	$.getScript("/resources/assets/busan/front/js/multivision_intervalReset.js")
		.done(function () {
			$.getScript("/resources/assets/busan/front/js/multivision_attention.js");
		})
		.done(function () {
			$.getScript("/resources/assets/busan/front/js/multivision_clock.js");
		});
</script>

<script>
var dataLength = 0;

function cleanBoard() {
	  for(var i=1;i<dataLength.length;i++){
		  $(".info").html(" ");
		  console.log("나 동작하니?");
	  }
	  console.log(dataLength);
}

function signBoard(){
	  $.ajax({
		  url : "/front/busan/multi/signboard.do", // 보낼 controller 
		  type : "GET",
		  crossDomain: true,
/* 		  dataType: "json", */
	  		
		  success : function(data) {		  	
			  console.log(data); //json object 확인용	
			  $("#loan_signboard").html(data);  //////////// Ajax로 Jsp view 
			  
		  },		  
		  error : function(data) {
			  console.log("xml 데이터 읽어오기 실패");
		  }
	  });
}	

signBoard();
setInterval(signBoard, 100000);
</script>

</html>


```
