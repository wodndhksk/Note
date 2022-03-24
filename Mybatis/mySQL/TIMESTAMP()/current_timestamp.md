```xml
<!-- 수정 -->
	<update id="updateIfYouGetAlarm" parameterType="java.util.HashMap">
		UPDATE
			tb_loan_signboard_bnk
		SET
			ALARM_TALK = '1', ALARM_DT= current_timestamp()
		WHERE	
			LOAN_SEQ_NO = #{seqNo}
				
	</update>

```
