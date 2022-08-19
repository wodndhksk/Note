### 3개의 테이블을 조인해서 group by로 합쳐서 보여주는 selelct 쿼리

```xml
<select id="selectAllCategorySub" resultType="com.top.bo.biz.system.category.CategorySub">
		SELECT
			  cnm.CTGR_NM AS "CTGR_LNG_CD_KO"
			 ,cp.PDLT_NO
		     ,cp.CTGR_NO
			 ,MAX(IF(nm.LNG_CD = 'KO', nm.PDLT_NM , NULL)) AS "LNG_CD_KO"
			 ,MAX(IF(nm.LNG_CD = 'EN', nm.PDLT_NM , NULL)) AS "LNG_CD_EN"
			 ,cp.SORT_SEQ
			 ,cp.USE_YN
			 ,cp.HSCODE

			 ,cp.TRF_RAT
			 ,cp.SPCL_RAT
			 ,cp.BAS_EXCS_AMT
			 ,cp.EDU_RAT
			 ,cp.VILLAGE_RAT
			 ,cp.LIQU_RAT
			 ,cp.CLEA_CAT_CD
			 ,cp.RGTR_NO

		FROM SIT_CTGR_PDLT cp

		INNER JOIN SIT_CTGR_PDLT_NM nm
			ON cp.PDLT_NO = nm.PDLT_NO

		INNER JOIN SIT_CTGR_NM cnm
		    ON cnm.CTGR_NO = cp.CTGR_NO

		INNER JOIN SIT_CTGR c
		    ON c.CTGR_NO = cp.CTGR_NO

		WHERE cnm.LNG_CD ="KO"
			AND c.USE_YN ="Y"
		<if test='ctgrNo != null and ctgrNo != 0'>
			AND cp.CTGR_NO = #{ctgrNo, jdbcType=INTEGER}
		</if>
		GROUP BY cp.PDLT_NO
	</select>
```
