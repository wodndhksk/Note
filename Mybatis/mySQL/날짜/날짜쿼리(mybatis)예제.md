## 부등호는 <![CDATA[<=]]> 로 표현, END DATE 값은 +1을 해줘야 마지막 날까지 포함 따라서 DATE_ADD로 감싸고 INTERVAL 1 DAY (+1) 를 해준다
```xml
f.apply_end_dtm <![CDATA[<=]]> date_add(date_format(#{src.applyEndDtmSrc, jdbcType=VARCHAR}, '%Y-%m-%d'), interval 1 day)
```

## 검색조건 : 전체 예제 쿼리 

```xml
select
			f.fix_dlv_cost_sn
			 ,cnm.comodity_nm
			 ,ctnm.comodity_cat_nm
			 ,f.apply_yn
		     ,date_format(f.apply_start_dtm, '%Y-%m-%d') as 'start_dt'
			 ,date_format(f.apply_end_dtm, '%Y-%m-%d') as 'end_dt'
			 ,f.fix_dlv_cost

		from ht_std_fix_dlv_cost f
				 inner join ht_sit_comodity_nm cnm on f.comodity_sno = cnm.comodity_sno and cnm.lang_cd ='KO'
				 inner join ht_sit_comodity cm on f.comodity_sno = cm.comodity_sno
				 inner join ht_sit_comodity_cat_nm ctnm on ctnm.comodity_cat_mng_no  = cm.comodity_cat_sno  and ctnm.lang_cd ='KO'
		where 1=1
		<if test='src.comodityCatSnoSrc != "0" '>
			and ctnm.comodity_cat_mng_no = #{src.comodityCatSnoSrc, jdbcType=INTEGER}
		</if>
		<if test='src.comoditySnoSrc != "0" '>
			and cnm.comodity_sno = #{src.comoditySnoSrc, jdbcType=INTEGER}
		</if>
		<if test='src.applyYnSrc != "" '>
			and f.apply_yn = #{src.applyYnSrc, jdbcType=VARCHAR}
		</if>
		<if test='src.applyStartDtmSrc != "" '>
			and f.apply_start_dtm <![CDATA[>=]]> date_format(#{src.applyStartDtmSrc, jdbcType=VARCHAR}, '%Y-%m-%d')
		</if>
		<if test='src.applyEndDtmSrc != "" '>
			and f.apply_end_dtm <![CDATA[<=]]> date_add(date_format(#{src.applyEndDtmSrc, jdbcType=VARCHAR}, '%Y-%m-%d'), interval 1 day)
		</if>
```
