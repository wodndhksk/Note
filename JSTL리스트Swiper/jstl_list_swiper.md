```html
<div class="swiper-container book__swiper">
	<div class="swiper-wrapper">

	<c:forEach var="row" items="${bookList }" varStatus="rowNum">
		<c:if test ="${rowNum.index % 9 == 0 }">
			<c:if test ="${rowNum.index > 0}">
					</ol>
				</div>
			</c:if>
			<div class="swiper-slide">
				<ol class="book__list">	
		</c:if>
		<li>
			<a href="#" class="book__button">
				<div class="image" id="${row.tbIdx}" onclick='onClickBookList("${row.tbIdx}")'>
					<img src="${row.image }" alt="" />
				</div>
				<div class="contents">
					<div class="title" id="${row.tbIdx}">${row.title }</div>
					<div class="publisher">${row.publisher }</div>
				</div>
			</a>
		</li>
		<c:if test="${rowNum.last }">
				</ol>
			</div>
		</c:if>
	</c:forEach>

</div>
```
