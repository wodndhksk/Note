## <foreach> 사용한다. (separator = 반복할때마다 ',' 붙임)
```xml
<!--카테고리 품목명 insert -->
	<insert id="insertCtgrSubNm">
		/*+ com.top.bo.system.category.mapper.CategoryMapper.insertCtgrSubNm [카테고리 품목명 등록] [] */
		insert into sit_comodity_nm(
								comodity_no
							   ,lang_cd
							   ,comodity_nm
		)
		values
			<foreach collection="categorySubLangs" item="item" separator=",">
			(
				  #{item.comodityNo, jdbcType=INTEGER}
				, #{item.langCd, jdbcType=VARCHAR}
			    , #{item.comodityNm, jdbcType=VARCHAR}
			)
			</foreach>
	</insert>

```
