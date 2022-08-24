## PK 값이 같지않을 경우는 평소처러 INSERT문을, PK 값이 같을경우는 UPDATE문을 실행한다. ( ON DUPLICATE KEY 로 구분 ) 

```sql
<insert id="insertTaxation">
		/*+ com.top.bo.system.category.mapper.TaxationMapper.insertTaxation [과세운임 등록/수정] [] */

		INSERT INTO STD_TAXATION_TRANS_FEE(
											COUNTRY_CD
										  , BASIS_WT
										  , BASIS_AMT_EQLT_FEE
										  , BASIS_AMT_ABOVE_FEE
										  , RGST_ID
										  , RGST_DTM
										  , MOD_ID
										  , MOD_DTM
		)
		VALUES (
				#{countryCd, jdbcType=VARCHAR}
			   , #{basisWt, jdbcType=INTEGER}
			   , #{basisAmtEqltFee, jdbcType=VARCHAR}
			   , #{basisAmtAboveFee, jdbcType=INTEGER}
			   , #{rgstId, jdbcType=INTEGER}
			   , CURRENT_TIMESTAMP()
			   , #{modId, jdbcType=INTEGER}
			   , CURRENT_TIMESTAMP()
			   )
		ON DUPLICATE KEY
		UPDATE
			BASIS_WT = #{basisWt, jdbcType=INTEGER}
			, BASIS_AMT_EQLT_FEE = #{basisAmtEqltFee, jdbcType=VARCHAR}
			, BASIS_AMT_ABOVE_FEE = #{basisAmtAboveFee, jdbcType=INTEGER}
			, MOD_ID = #{modId, jdbcType=VARCHAR}
			, MOD_DTM = CURRENT_TIMESTAMP()

	</insert>
```
