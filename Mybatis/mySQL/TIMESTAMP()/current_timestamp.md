## <UPDATE문> ALARM_TALK ='1' ALARM_DT = 현재시간 DB에 반영

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
