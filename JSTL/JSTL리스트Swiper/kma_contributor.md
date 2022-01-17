```html
<div class="donation__contents">
				<div class="donation__swiper">
					<div class="swiper-wrapper">
						<!-- 각 배경별로 bg1 ~ 15까지 클래스명으로 처리 -->	
						<c:set var="i" value="1" />
						<c:set var="j" value="1" />
					<c:forEach var="row" items="${contributorList }" varStatus="rowNum">
						<c:if test="${i % 16 == 0}">
							<c:set var="i" value="1" />
						</c:if>
						
						<c:if test="${rowNum.index == 0 }" >
						<div class="swiper-slide bg${i }">
							<div class="donation__contents">
						</c:if>
						
								<div class="donation__block">
									<div class="name">${row.name }</div>
									<div class="units">${row.contribute }</div>
									<div class="amount">${row.donation }</div>
								</div>
								
								<c:if test="${rowNum.last }">
									</div>	
									</div>
								</c:if>
								<c:set var="j" value="${j+1 }" />
								
								<c:if test="${j % 15 == 0 }">
								</div>	
								</div>	
									<c:set var="i" value="${i+1 }" />
									<c:set var="j" value="1" />	
									<div class="swiper-slide bg${i }">
										<div class="donation__contents">											
								</c:if>
								
						</c:forEach>
						
						
					</div>
				</div>
			</div>

```
