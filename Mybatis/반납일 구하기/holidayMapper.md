```xml
	<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="egovframework.com.admin.scheduler.busan.service.impl.HolidayMapper" >

<!-- 리스트 갯수 -->

	<select id="selectListCount" parameterType="java.util.HashMap" resultType="Integer">
		SELECT
			COUNT(TB_IDX) cnt
		FROM
			${TB_NAME}
		WHERE 1=1
			<if test="hdYear != null">
			HD_YEAR = #{hd_year} AND
			</if>
			<if test="hdMonth != null">
			HD_MONTH = #{hd_month}
			</if>
		
	</select>
	
	<select id="selectHolydayList" parameterType="java.util.HashMap" resultType="HolidayVo">
		SELECT  
			* 
		FROM 
			tb_holiday
		WHERE
			HD_YEAR = #{hd_year}
			AND
			HD_MONTH = #{hd_month}
		ORDER BY
			HD_SEQ
		ASC

	</select>
  
    <!-- 상세 -->
	<select id="selectHoliday" parameterType="Params" resultType="HolidayVo">
		SELECT  
			* 
		FROM 
			tb_holiday
		WHERE
			<if test="hdYear != null">
			HD_YEAR = #{hd_year} AND
			</if>
			<if test="hdMonth != null">
			HD_MONTH = #{hd_month}
			</if>
			
		ORDER BY
			HD_SEQ
		ASC
   </select>
   
   
	
	<!-- 등록 -->
	<insert id="insertHoliday" parameterType="Params">
		INSERT INTO 
			${TB_NAME}
		(
			<if test="hdSeq != null">HD_SEQ,</if>
			<if test="siteCd != null">SITE_CD,</if>
			<if test="hdYear != null">HD_YEAR,</if>
			<if test="hdMonth != null">HD_MONTH,</if>
			<if test="hdDay != null">HD_DAY,</if>
			<if test="regId != null">REG_ID,</if>
			<if test="modId != null">MOD_ID,</if>
			<if test="modDate != null">MOD_DATE,</if>
			<if test="delId != null">DEL_ID,</if>
			<if test="delDate != null">DEL_DATE,</if>
			<if test="hdDate != null">HD_DATE,</if>
			REG_DATE
		) values (
			<if test="hdSeq != null">#{hdSeq},</if>
			<if test="siteCd != null">#{siteCd},</if>
			<if test="hdYear != null">#{hdYear},</if>
			<if test="hdMonth != null">#{hdMonth},</if>
			<if test="hdDay != null">#{hdDay},</if>
			<if test="regId != null">#{regId},</if>
			<if test="modId != null">#{modId},</if>
			<if test="modDate != null">#{modDate},</if>
			<if test="delId != null">#{delId},</if>
			<if test="delDate != null">#{delDate},</if>
			<if test="hdDate != null">#{hdDate},</if>
			SYSDATE()
		)
	</insert>
	
	

   <delete id="deleteHoliday" parameterType="Params">
		DELETE FROM ${TB_NAME}
		WHERE 1=1
		<!-- <if test="content_type != null">
		AND CONTENT_TYPE = #{content_type} 
	    </if> -->
<!-- 		<iID"id > 0">AND BOOKKEY = #{id}</if> -->
	<if test="hdYear != null">
		HD_YEAR = #{hd_year}
		</if>
	</delete>
	
	<select id="selectHolydayCount" parameterType="java.util.HashMap" resultType="Integer">
	SELECT  
		COUNT(HD_SEQ) 
	FROM 
		tb_holiday
	WHERE
		hd_date
		
		BETWEEN DATE(#{startDate}) 
		AND 
		DATE(#{endDate})
		
	ORDER BY
		HD_SEQ
	ASC

</select>
	

	
	
</mapper>

```
