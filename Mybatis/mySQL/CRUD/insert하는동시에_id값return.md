## Insert문이 실행되는 동시에 Auto increment 된 PK 값을 리턴 (리턴한다기 보다는 객체에 PK 값에 자동 set됨)
- keyPropery에 받을 값(변수)만 설정해주면 됨

```xml
<insert id="insertCategory" parameterType="java.util.HashMap">
		/*+ com.top.bo.system.category.mapper.CategoryMapper.insertCategory [카테고리 등록] [] */

		<selectKey resultType="int" keyProperty="ctgrNo" order="AFTER">
			SELECT LAST_INSERT_ID()
		</selectKey>

		INSERT INTO SIT_CTGR(
								   DLV_SVC_CD
								 , SORT_SEQ
								 , USE_YN
								 , CTGR_DESC
								 , RGTR_NO
								 , REG_DTM
								 , MDFR_NO
								 , MDFCN_DTM
		)
		VALUES (
			   	 #{dlvSvcCd, jdbcType=VARCHAR}
			   , #{sortSeq, jdbcType=INTEGER}
			   , #{useYn, jdbcType=VARCHAR}
			   , #{ctgrDesc, jdbcType=VARCHAR}
			   , #{rgtrNo, jdbcType=INTEGER}
			   , CURRENT_TIMESTAMP()
			   , #{mdfrNo, jdbcType=INTEGER}
			   , CURRENT_TIMESTAMP()
			   )

	</insert>
```
