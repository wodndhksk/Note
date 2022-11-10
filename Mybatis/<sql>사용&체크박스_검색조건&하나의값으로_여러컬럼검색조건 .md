## HTML (jsp)
```html
<tbody>
<tr>
<th scope="row">등록일자</th>
<td class="input" colspan="5">
<!-- S : term-date-wrap -->
<span class="term-date-wrap">
	<span class="date-box">
		<input type="text" id="applyStartDtmSrc" name="applyStartDtmSrc" data-role="datepicker" class="ui-cal js-ui-cal" title="시작일 선택">
	</span>
	<span class="text">~</span>
	<span class="date-box">
		<input type="text" id="applyEndDtmSrc" name="applyEndDtmSrc" data-role="datepicker" class="ui-cal js-ui-cal" title="종료일 선택">
	</span>
	<span class="btn-group">
		<a href="javascript:void(0);" class="btn-sm btn-func" data-button-period="today">오늘</a>
		<a href="javascript:void(0);" class="btn-sm btn-func" data-button-period="week">일주일</a>
		<a href="javascript:void(0);" class="btn-sm btn-func" data-button-period="month" id="_periodMonth">한달</a>
		<a href="javascript:void(0);" class="btn-sm btn-func text-center" data-button-period="year">1년</a>
	</span>
</span>
<!-- E : term-date-wrap -->
</td>
<th scope="row">센터</th>
<td class="input" colspan="7">
	<div class="opt-keyword-box">
		<select id="centerCdSrc" name="centerCdSrc" class="ui-sel" title="검색어 선택">
			<option value="">전체</option>
			<c:forEach var="item" items="${centerCd}" varStatus="status">
				<option value="${item.codeDtlNo}">${item.codeDtlNameEn}</option>
			</c:forEach>
		</select>
	</div>
</td>
</tr>
<tr>
<th scope="row">검색값</th>
<td class="input" colspan="5">
	<!-- S : opt-keyword-box -->
	<div class="opt-keyword-box">
		<select class="ui-sel" id="unRegTypeSrc" name="unRegTypeSrc" title="검색조건 선택"
				style="width:150px;">
			<option value="allType" seleted>전체</option>
			<option value="trackingNum">트레킹번호</option>
			<option value="custIdNum">사서함번호</option>
		</select>
		<input type="text" class="ui-input" title="회원 입력" placeholder="검색어 입력" id="inputSrc" name="inputSrc">
	</div>
	<!-- E : opt-keyword-box -->
</td>
<th scope="row">상태</th>
<td class="input" colspan="7">
<ul class="ip-box-list">
	<li>
		<span class="ui-rdo">
			<span class="ui-chk">
				<input id="allStatus" name="allStatus" type="checkbox" value="" checked="checked"/>
				<label for="allStatus">전체&nbsp&nbsp</label>
			<c:forEach var="data" items="${unregisterCd}" varStatus="i+1">
				<input id="status${data.codeDtlNo}"  name="unregisterStusCdSrc" type="checkbox" value="${data.codeDtlNo}" checked="checked">
				<label for="status${data.codeDtlNo}" style="margin-right: 10px;">${data.codeDtlNameKo}</label>
			</c:forEach>
			</span>
		</span>
	</li>
</ul>
</td>
</tr>
</tbody>
```

## Search 객체
```java
package com.top.bo.biz.wms.unregister;

import lombok.Data;

import java.util.List;

@SuppressWarnings("serial")
@Data
public class UnregisterSearch {

    /**
     * 등록일자 start
     */
    private String applyStartDtmSrc;
    /**
     * 등록일자 end
     */
    private String applyEndDtmSrc;
    /**
     * 센터
     */
    private String centerCdSrc;
    /**
     * 검색값 검색조건 ( 전체 / 트레킹번호 / 사서함번호 )
     */
    private String unRegTypeSrc;
    /**
     * 검색값
     */
    private String inputSrc; //트레킹번호, 사서함번호
    /**
     * 상태
     */
    private List<String> unregisterStusCdSrc; // 상태


}
```
## 검색 조건 쿼리 (Mybatis)
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.top.bo.biz.wms.unregister.mapper.UnregisterSecondaryMapper">

	<!--[COUNT]미신청 화물 정보 조회-->
	<select id="selectUnregisterCount" resultType="int">
		select
		count(*)
		from ht_dlv_unregister_cgo h
		<include refid="join-where-unRegister"></include>
	</select>

	<!--미신청 화물 정보 조회-->
	<select id="selectUnregister" resultType="com.top.bo.biz.wms.unregister.Unregister">
		/*+ com.top.bo.biz.wms.unregister.mapper.UnregisterSecondaryMapper.Unregister [미신청 화물 정보 조회] [] */
		select
			 scdm.code_dtl_name as center_cd
			 ,h.real_dlv_track_no
			 ,h.unregister_stus_cd
			 ,h.cust_po_box_no as po_box_no
			 ,hmcm.nickname
			 ,hdri.rack_no
			 ,h.cust_nm
			 ,h.shopmall_info
			 ,h.prdt_info
			 ,sam.admin_name as wrk_user_id-- 작업자
			 ,h.reg_user_id
			 ,h.reg_dtm
			 ,h.modify_user_id
			 ,h.modify_dtm
		from ht_dlv_unregister_cgo h
		<include refid="join-where-unRegister"></include>
	</select>

	<sql id ="join-where-unRegister">
		inner join sy_code_dtl scd on scd.code_field = 'CENTER_CD' and h.center_auto_sn = scd.code_dtl_no
		inner join sy_code_dtl_ml scdm on scd.code_dtl_no = scdm.code_dtl_no and scdm.lang_cd ='EN'
		inner join ht_mbr_cust_m hmcm  on hmcm.po_box_no = h.cust_po_box_no
		inner join ht_dlv_rack_info hdri on hdri.center_auto_sn = h.center_auto_sn
		inner join ht_dlv_req_stus_h hdrsh on h.req_no = hdrsh.req_no
		left outer join sy_admin_mst sam on hdrsh.wrk_user_id = sam.admin_no
		inner join (
		select
		un.unregister_cgo_auto_sn
		,un.unregister_stus_cd
		from ht_dlv_unregister_cgo un
		<trim prefix="where" prefixOverrides="or">
		    <choose>
				<when test='src.unregisterStusCdSrc != null'>
					<foreach collection="src.unregisterStusCdSrc" item="item" index="index" separator="or" >
						un.unregister_stus_cd = #{item}
					</foreach>
				</when>
				<otherwise>
					un.unregister_stus_cd = ''
				</otherwise>
			</choose>
		</trim>
		)sub on h.unregister_cgo_auto_sn  = sub.unregister_cgo_auto_sn
		<trim prefix="where" prefixOverrides="and | or">
			<if test='src.applyStartDtmSrc != "" and src.applyStartDtmSrc != null'>
				and h.reg_dtm <![CDATA[>=]]> date_format(#{src.applyStartDtmSrc, jdbcType=VARCHAR}, '%Y-%m-%d')
			</if>
			<if test='src.applyEndDtmSrc != "" and src.applyEndDtmSrc != null'>
				and h.reg_dtm <![CDATA[<=]]> date_add(date_format(#{src.applyEndDtmSrc, jdbcType=VARCHAR}, '%Y-%m-%d'), interval 1 day)
			</if>
			<if test='src.centerCdSrc != "" and src.centerCdSrc != null '>
				and h.center_auto_sn = #{src.centerCdSrc, jdbcType=VARCHAR}
			</if>
			<if test='src.inputSrc != "" and src.inputSrc != null '>
				<choose>
					<when test="src.unRegTypeSrc == 'allType'">
						and  h.real_dlv_track_no = #{src.inputSrc, jdbcType=VARCHAR} <!-- 전체 -->
						or  h.cust_po_box_no = #{src.inputSrc, jdbcType=VARCHAR}
					</when>
					<when test="src.unRegTypeSrc == 'trackingNum'">
						and  h.real_dlv_track_no = #{src.inputSrc, jdbcType=VARCHAR} <!-- 트레킹번호 -->
					</when>
					<when test="src.unRegTypeSrc == 'custIdNum'">
						and  h.cust_po_box_no = #{src.inputSrc, jdbcType=VARCHAR} <!-- 사서함번호 -->
					</when>
				</choose>
			</if>
		</trim>
	</sql>

</mapper>
```
