## WHERE 1=1 대신 <trim>을 사용하여 해결
  - prefixOverrides="and" => and로 구분
  
  
## 예시코드
```xml
select
			c.country_cd
			 ,c.country_eng_nm
			 ,c.country_kor_nm
			 ,c.dlv_rgn_cd
			 ,cd.code_dtl_name
			 ,c.sort_seq_no
			 ,c.use_yn
			 ,c.modify_user_id
			 ,a.admin_name
			 ,date_format(c.modify_dtm, '%Y-%m-%d %h:%i:%s') as mod_dt

		from ht_std_dlv_country c
				 inner join sy_admin_mst a on a.admin_no = c.modify_user_id
				 inner join sy_code_dtl_ml cd on cd.code_field = 'DLV_AREA_CD' and cd.lang_cd ='KO' and cd.code_dtl_no = c.dlv_rgn_cd

		<trim prefix="where" prefixOverrides="and">
			<if test='useYnSrc != "" '>
				and c.use_yn = #{useYnSrc}
			</if>
			<if test='countryCdSrc != "" '>
				and c.country_cd like '%${countryCdSrc}%'
			</if>
			<if test='countryKorNmSrc != null '>
				and c.country_kor_nm like '%${countryKorNmSrc}%'
			</if>
			<if test='countryEngNmSrc != null '>
				and c.country_eng_nm like '%${countryEngNmSrc}%'
			</if>
			<if test='dlvZoneCdSrc != "" '>
				and c.dlv_rgn_cd = ${dlvZoneCdSrc}
			</if>
		</trim>

```
