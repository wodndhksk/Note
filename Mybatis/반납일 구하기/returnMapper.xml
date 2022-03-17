## 현재날짜와 현재날짜로부터 2주 뒤값을 받아 사이에 존재하는 휴일 개수 찾기
```xml
<select id="selectHolydayCount" parameterType="java.util.HashMap" resultType="Integer">
	SELECT  
		COUNT(HD_SEQ) 
	FROM 
		tb_holiday
	WHERE
		hd_date
		
		BETWEEN DATE(#{startDate}) 
		AND 
		DATE(#{endDate})
		
	ORDER BY
		HD_SEQ
	ASC

</select>
```
