- if문 설명 :  CTNM>LNG_CD 값이 KO 이면 CTNM>CTGR_NM 값, KO가 아니면 NULL  AS 보여줄 컬럼값
- 두개의 row값의 모든 컬림값이 같고(중복) CTGR_NM_KO값만 다를때 하나는 NULL 값이고 하나는 값이 존재할것이다 따라서 GROUP BY를 통해 중복값을 묶고 
	 CTGR_NM_KO값만 다르니 CTGR_NM_KO값중 max 값을 (NULL 이 아닌값) GROUP BY에서 표출시킨다
```sql
SELECT
			 CT.CTGR_NO
			,CT.USE_SCOPE_CD
			,max(IF(CTNM.LNG_CD = 'KO', CTNM.CTGR_NM , NULL)) AS CTGR_NM_KO 
			,max(IF(CTNM.LNG_CD = 'EN', CTNM.CTGR_NM , NULL)) AS CTGR_NM_EN

			,CT.SORT_SEQ
			,CT.USE_YN
			,CT.DESCRIPTION
			,CT.SERV_TYPE

		FROM STD_CTGR CT

		INNER JOIN STD_CTGR_NM CTNM

		ON CT.CTGR_NO = CTNM.CTGR_NO
		
		GROUP BY CT.CTGR_NO
```
