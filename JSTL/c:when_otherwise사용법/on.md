```html
<nav class="navigation">
			<c:choose>
				<c:when test="${currentUrl eq '/front/use_kiosk/book/book_new.do'}">
					<a href="/front/use_kiosk/book/book_new.do" id="newBook" class="navigation__button on">
				</c:when>
				<c:otherwise>
					<a href="/front/use_kiosk/book/book_new.do" id="newBook" class="navigation__button">
				</c:otherwise>
			</c:choose>
					<div class="navigation__name">
						신간도서
						<small>NEW ARRIVAL BOOK</small>
					</div>
				</a>
			<c:choose>
				<c:when test="${currentUrl eq '/front/use_kiosk/book/book_best.do'}">
					<a href="/front/use_kiosk/book/book_best.do" id="bestBook" class="navigation__button on">
				</c:when>
				<c:otherwise>
					<a href="/front/use_kiosk/book/book_best.do" id="bestBook" class="navigation__button">
				</c:otherwise>
			</c:choose>
					<div class="navigation__name">
						대출도서베스트
						<small>LENDING BOOK BEST</small>
					</div>
				</a>
			<c:choose>
				<c:when test="${currentUrl eq '/front/use_kiosk/book/book_recom.do'}">
					<a href="/front/use_kiosk/book/book_recom.do" id="recomBook" class="navigation__button on">
				</c:when>
				<c:otherwise>
					<a href="/front/use_kiosk/book/book_recom.do" id="recomBook" class="navigation__button">
				</c:otherwise>
			</c:choose>
					<div class="navigation__name">
						추천도서
						<small>RECOMMENDED BOOK</small>
					</div>
				</a>
				<a href="#" class="navigation__button">
					<div class="navigation__name">
						나의서재
						<small>MY LIBRARY</small>
					</div>
				</a>
			</nav>

```
