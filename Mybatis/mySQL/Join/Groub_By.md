```xml
<!--LOAN_SEQ_NO, USER_NAME 컬럼이 중복인 경우 하나로 본다  ( 두 조건 컬럼이 같은 데이터가 10개면 그것을 동일한 하나의 데이터로 함) -->
<select id="selectList" parameterType="java.util.HashMap" resultType="LoanSignBoardVo">
		
	SELECT  
		LOAN_SEQ_NO, USER_NAME   //GROUP BY 컬럼이랑 일치 시켜야 함 
		FROM 
			tb_loan_signboard_bnk 
		WHERE
			LOAN_FINISH_YN is NULL AND
			CANCEL_YN is NULL AND
			ALARM_TALK = '1'  AND
			LOAN_GUBUN = #{loanGubun}
	GROUP BY
		LOAN_SEQ_NO, USER_NAME // GROUP BY 조건에 있는 컬럼끼리는 값이 같아야함
		
		ORDER BY
			LOAN_SEQ_NO
		DESC

	</select>

<!--LOAN_GUBUN, WEB_ID, USER_NAME, USER_TEL, LOAN_SEQ_NO 가 중복일시(GROUP BY) SELECT 는 GROUP BY 조건과 동일, 하지만 다른 조건을 추가하고 싶다면 하나만 가능*(  max(CONTROL_NO) AS CONTROL_NO, max(REG_DT) AS REG_DT  ) 와 같이 max만 가능  --> 
 <select id="selectList" parameterType="java.util.HashMap" resultType="LoanSignBoardVo">
		SELECT  
		LOAN_GUBUN, WEB_ID, USER_NAME, USER_TEL, LOAN_SEQ_NO
		, max(CONTROL_NO) AS CONTROL_NO
		, max(REG_DT) AS REG_DT
		FROM 
			tb_loan_signboard_bnk 
		WHERE
			LOAN_FINISH_YN is NULL
			AND
			CANCEL_YN is NULL
			AND
			ALARM_TALK is NULL
			
		GROUP BY
			LOAN_GUBUN, WEB_ID, USER_NAME, USER_TEL, LOAN_SEQ_NO
			
		ORDER BY
			LOAN_SEQ_NO
		DESC

	</select>

<!-- 위의 select count 조건-->
<select id="selectListCount" parameterType="java.util.HashMap" resultType="Integer">
		SELECT
			COUNT(*)
		FROM
		(
		
			SELECT LOAN_GUBUN, WEB_ID, USER_NAME, USER_TEL, LOAN_SEQ_NO
			, max(CONTROL_NO) AS CONTROL_NO
			, max(REG_DT) AS REG_DT
			FROM 
				tb_loan_signboard_bnk 
			WHERE
				LOAN_FINISH_YN is NULL
				AND
				CANCEL_YN is NULL
				AND
				ALARM_TALK is NULL
				
			GROUP BY
				LOAN_GUBUN, WEB_ID, USER_NAME, USER_TEL, LOAN_SEQ_NO
				
			ORDER BY
				LOAN_SEQ_NO
			DESC
		
		) T
		
	</select>

```
