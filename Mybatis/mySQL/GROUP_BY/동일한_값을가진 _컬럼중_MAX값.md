### LOAN_SEQ_NO 은 다른 데이터와 값이 같을수도 있음 따라서  LOAN_SEQ_NO가 같은 여러데이터 중 max 값 select
```xml
SELECT  
			max(USER_TEL) as USER_TEL
		FROM 
			tb_loan_signboard_bnk
		WHERE
<!-- 			LOAN_GUBUN = #{loanGubun} AND -->
			LOAN_SEQ_NO=#{seqNo} 
<!-- 			LOAN_FINISH_YN = 'Y' AND
			CANCEL_YN is NULL AND
			ALARM_TALK is NULL -->
		<!-- ORDER BY
			SEQ_NO
		ASC -->
```
