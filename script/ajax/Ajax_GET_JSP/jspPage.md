```html
<%@page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

	<c:forEach var="row" items="${loanBoard}" varStatus="rowNum">
		<c:if test="${rowNum.index == 0 }">
			<div class="list__block on">
			<ul>
		</c:if>
				<li>
					<div class="info">
						<div class="num">${rowNum.count }</div>
						<div class="name length4">${row.userName }<sub>님</sub></div>
						<div class="status">Online</div>
					</div>
				</li>
		<c:if test="${rowNum.last }">
			</ul>
			</div>
		</c:if>
				
		<c:if test="${rowNum.count % 10 == 0}">
			</ul>
			</div>
			<div class="list__block">
			<ul>
		</c:if>					
	</c:forEach>

```
