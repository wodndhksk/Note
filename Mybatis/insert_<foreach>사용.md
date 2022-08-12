- private List\<CategoryLang\> categoryLangs;
- 리스트 형태의 categoryLangs 객체를 foreach 로 한번에 여러개 insert


```sql
<insert id="inserCtgrNm">
		/*+ com.top.bo.system.category.mapper.CategoryMapper.inserCtgrNm [카테고리명 등록] [] */
		INSERT INTO SIT_CTGR_NM(
							CTGR_NO
							,LNG_CD
							,CTGR_NM
		)
		VALUES
			<foreach collection="categoryLangs" item="item" separator=",">
		    (
				#{item.ctgrNo, jdbcType=INTEGER}
				,#{item.lngCd, jdbcType=VARCHAR}
				,#{item.ctgrNm, jdbcType=VARCHAR}
			)
			</foreach>
	</insert>
```
