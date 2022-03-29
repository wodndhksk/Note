```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="egovframework.com.admin.book.service.impl.BookinfoMapper" >

<!-- 리스트 갯수 -->
	<select id="selectListCount" parameterType="Params" resultType="Integer">
		SELECT
			COUNT(TB_IDX) cnt
		FROM
			${TB_NAME}
		WHERE 1=1
		AND DEL_YN = 'N'
		<if test="site_cd != null">
				AND 
					SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					CONTENT_TYPE = #{content_type}
				</if>
		       
		
		<if test="useYn != null">
		AND 
			USE_YN = #{useYn}
		</if>
		<choose>
			<when test="searchType == 'title' and searchText != null">
				AND TITLE LIKE concat('%' , #{searchText} , '%')
			</when>
			<when test="searchType == 'author' and searchText != null">
				AND AUTHOR LIKE  concat('%' , #{searchText} , '%')
			</when>
			<when test="searchType == 'publisher' and searchText != null">
				AND PUBLISHER LIKE  concat('%' , #{searchText} , '%')
			</when>
			<when test="searchType == 'isbn' and searchText != null">
				AND ISBN LIKE '%'  concat('%' , #{searchText} , '%')
			</when>
		</choose>	
		
	</select>
	
	
	<!-- 리스트 -->
	<select id="selectList" parameterType="Params" resultType="Record">
	   select RES1.* FROM(
		SELECT
               Z.*
          FROM (
            SELECT
               X.*
               , @rownum:=@rownum+1 as NUM
            FROM (
			   SELECT	*
						<!-- TB_IDX,
						CALLNO,
						BOOKKEY,
						TITLE,
						AUTHOR,
						PUBLISHER,
						PUBYEAR,
						REGNO,
						ISBN,
						CASE
						WHEN IMAGE = '' THEN '/resources/assets/front/images/noimage.jpg'
						ELSE IMAGE
						END AS IMAGE,
						DESCRIPTION,
						SORT,
						USE_YN,
						DEL_YN,
						REG_ID,
						REG_DATE,
						MOD_ID,
						MOD_DATE,
						DEL_ID,
						DEL_DATE,
						CATEGORY,
						SITE_CD,
						CONTENT_TYPE,
						RESERVE_STATUS -->
			   FROM
				  ${TB_NAME} A
				  WHERE 1=1
				  <if test="useYn != null">
				AND 
					A.USE_YN = #{useYn}
				</if>
				<if test="site_cd != null">
				AND 
					A.SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					A.CONTENT_TYPE = #{content_type}
				</if>
				<if test="category != null">
				AND 
					A.CATEGORY = #{category}
				</if>
				
				
				<choose>
			         <when test="searchType == 'title' and searchText != null">
				       AND A.TITLE LIKE concat('%' , #{searchText} , '%')
			        </when>
			         <when test="searchType == 'author' and searchText != null">
			          	AND A.AUTHOR LIKE  concat('%' , #{searchText} , '%')
			         </when>
			         <when test="searchType == 'publisher' and searchText != null">
			          	AND A.PUBLISHER LIKE concat('%' , #{searchText} , '%')
			         </when>
			           <when test="searchType == 'isbn' and searchText != null">
			         	AND A.ISBN LIKE  concat('%' , #{searchText} , '%')
			        </when>
		     </choose>	
				  
			   )X, (select @rownum:=0) C
            )Z
      <if test="startX != null">
      WHERE
       	 Z.NUM BETWEEN #{startX} AND #{endX}
      </if>
        ) RES1
        order by RES1.sort desc
        LIMIT #{sql_paging_limit, jdbcType=INTEGER} OFFSET #{sql_paging_offset, jdbcType=INTEGER}
	</select>
	
	
	
	<!-- 상세 -->
	<select id="selectBest" parameterType="java.util.HashMap" resultType="OutBestBookVo">
		SELECT
			Z.*
		FROM (
			SELECT
				X.*,
				@rownum:=@rownum+1 as NUM
			FROM (
				SELECT
					TB_IDX,
					CALLNO,
					BOOKKEY,
					TITLE,
					AUTHOR,
					PUBLISHER,
					PUBYEAR,
					REGNO,
					ISBN,
					CASE
					WHEN IMAGE = '' THEN '/resources/assets/front/images/noimage.jpg'
					ELSE IMAGE
					END AS IMAGE,
					DESCRIPTION,
					SORT,
					USE_YN,
					DEL_YN,
					REG_ID,
					REG_DATE,
					MOD_ID,
					MOD_DATE,
					DEL_ID,
					DEL_DATE,
					CATEGORY,
					SITE_CD,
					CONTENT_TYPE,
					RESERVE_STATUS
				FROM
					tb_out_bestbook A
				WHERE 1=1
				<!-- <if test="content_type != null">
				   AND A.CONTENT_TYPE = #{content_type} 
				</if> -->
				<if test="useYn != null">
				AND 
					A.USE_YN = #{useYn}
				</if>
				<if test="bookkey != null">
				AND 
					A.BOOKKEY = #{bookkey}
				</if>
				<if test="site_cd != null">
				AND 
					A.SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					A.CONTENT_TYPE = #{content_type}
				</if>
				 
				
			) X, (select @rownum:=0) C
			ORDER BY X.SORT ASC
		) Z
		WHERE 
			Z.NUM  = 1
   </select>
   
   
   <!-- 상세 -->
	<select id="selectNew" parameterType="Params" resultType="OutBookNewVo">
	
		SELECT
			Z.*
		FROM (
			SELECT
				X.*,
				@rownum:=@rownum+1 as NUM
			FROM (
				SELECT
					TB_IDX,
					CALLNO,
					BOOKKEY,
					TITLE,
					AUTHOR,
					PUBLISHER,
					PUBYEAR,
					REGNO,
					ISBN,
					CASE
					    WHEN IMAGE = '' THEN '/resources/assets/front/images/noimage.jpg'
					    ELSE IMAGE
				  END AS IMAGE,
					DESCRIPTION,
					SORT,
					USE_YN,
					DEL_YN,
					REG_ID,
					REG_DATE,
					MOD_ID,
					MOD_DATE,
					DEL_ID,
					DEL_DATE,
					CATEGORY,
					SITE_CD,
					CONTENT_TYPE,
					RESERVE_STATUS
				FROM
					tb_out_newbook A
				WHERE 1=1
				<!-- <if test="content_type != null">
				   AND A.CONTENT_TYPE = #{content_type} 
				</if> -->
				<if test="useYn != null">
				AND 
					A.USE_YN = #{useYn}
				</if>
				<if test="bookkey != null">
				AND 
					A.BOOKKEY = #{bookkey}
				</if>
				<if test="site_cd != null">
				AND 
					A.SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					A.CONTENT_TYPE = #{content_type}
				</if>
				
			) X, (select @rownum:=0) C
			ORDER BY X.SORT ASC
		) Z
		WHERE 
			Z.NUM  = 1
   </select>
   
    <!-- 상세 -->
	<select id="selectRecom" parameterType="Params" resultType="OutRecomBookVo">
	
		SELECT
			Z.*
		FROM (
			SELECT
				X.*,
				@rownum:=@rownum+1 as NUM
			FROM (
				SELECT
					TB_IDX,
					CALLNO,
					BOOKKEY,
					TITLE,
					AUTHOR,
					PUBLISHER,
					PUBYEAR,
					REGNO,
					ISBN,
					CASE
					WHEN IMAGE = '' THEN '/resources/assets/front/images/noimage.jpg'
					ELSE IMAGE
					END AS IMAGE,
					DESCRIPTION,
					SORT,
					USE_YN,
					DEL_YN,
					REG_ID,
					REG_DATE,
					MOD_ID,
					MOD_DATE,
					DEL_ID,
					DEL_DATE,
					CATEGORY,
					SITE_CD,
					CONTENT_TYPE
				FROM
					tb_out_recombook A
				WHERE 1=1
				<!-- <if test="content_type != null">
				   AND A.CONTENT_TYPE = #{content_type} 
				</if> -->
				<if test="useYn != null">
				AND 
					A.USE_YN = #{useYn}
				</if>
				<if test="bookkey != null">
				AND 
					A.BOOKKEY = #{bookkey}
				</if>
				<if test="site_cd != null">
				AND 
					A.SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					A.CONTENT_TYPE = #{content_type}
				</if>
				
			) X, (select @rownum:=0) C
			ORDER BY X.SORT ASC
		) Z
		WHERE 
			Z.NUM  = 1
   </select>
   
   
	
	<!-- 등록 -->
	<insert id="insertBook" parameterType="Params">
		INSERT INTO 
			${TB_NAME}
		(
			<if test="tbIdx != null">TB_IDX,</if>
			<if test="callNo != null">CALLNO,</if>
			<if test="bookKey != null">BOOKKEY,</if>
			<if test="title != null">TITLE,</if>
			<if test="author != null">AUTHOR,</if>
			<if test="publisher != null">PUBLISHER,</if>
			<if test="pubYear != null">PUBYEAR,</if>
			<if test="regNo != null">REGNO,</if>
			<if test="isbn != null">ISBN,</if>
			<if test="image != null">IMAGE,</if>
			<if test="description != null">DESCRIPTION,</if>
			<if test="useYn != null">USE_YN,</if>
			<if test="sort != null">SORT,</if>
			<if test="regId != null">REG_ID,</if>
			<if test="reservestatus != null">RESERVE_STATUS,</if>
			<if test="content_type != null">CONTENT_TYPE,</if> 
			<if test="site_cd != null">SITE_CD,</if> 
			<if test="category != null">CATEGORY,</if>
			
			<if test="translators != null">TRANSLATORS,</if>
			<if test="nanetImg != null">NANET_IMG,</if> 
			<if test="kakaoImg != null">KAKAO_IMG,</if> 
			<if test="bkAge != null">BK_AGE,</if> 
			<if test="bkAgeNm != null">BK_AGE_NM,</if> 
			<if test="bkYear != null">BK_YEAR,</if> 
			<if test="bkMonth != null">BK_MONTH,</if> 
			<if test="bkWeek != null">BK_WEEK,</if> 		
			<if test="bkKind != null">BK_KIND,</if> 
			<if test="bkKindNm != null">BK_KIND_NM,</if> 
			<if test="bkSeq != null">BK_SEQ,</if> 
	
			<if test="shelfLocName != null">SHELF_LOC_NAME,</if> 
			<if test="shelfLocCode != null">SHELF_LOC_CODE,</if> 
			REG_DATE
		) values (
			<if test="tbIdx != null">#{tbIdx},</if>
			<if test="callNo != null">#{callNo},</if>
			<if test="bookKey != null">#{bookKey},</if>
			<if test="title != null">#{title},</if>
			<if test="author != null">#{author},</if>
			<if test="publisher != null">#{publisher},</if>
			<if test="pubYear != null">#{pubYear},</if>
			<if test="regNo != null">#{regNo},</if>
			<if test="isbn != null">#{isbn},</if>
			<if test="image != null">#{image},</if>
			<if test="description != null">#{description},</if>
			<if test="useYn != null">#{useYn},</if>
			<if test="sort != null">#{sort},</if>
			<if test="regId != null">#{regId},</if>
			<if test="reservestatus != null">#{reservestatus},</if>
			<if test="content_type != null">#{content_type},</if>  
			<if test="site_cd != null">#{site_cd},</if>  
			<if test="category != null">#{category},</if>
			
			<if test="translators != null">#{TRANSLATORS},</if>
			<if test="nanetImg != null">#{NANET_IMG},</if> 
			<if test="kakaoImg != null">#{KAKAO_IMG},</if> 
			<if test="bkAge != null">#{BK_AGE},</if> 
			<if test="bkAgeNm != null">#{BK_AGE_NM},</if> 
			<if test="bkYear != null">#{BK_YEAR},</if> 
			<if test="bkMonth != null">#{BK_MONTH},</if> 
			<if test="bkWeek != null">#{BK_WEEK},</if> 
			<if test="bkKind != null">#{BK_KIND},</if> 
			<if test="bkKindNm != null">#{BK_KIND_NM},</if> 
			<if test="bkSeq != null">#{BK_SEQ},</if> 
		 
			<if test="shelfLocName != null">#{shelfLocName},</if>  
			<if test="shelfLocCode != null">#{shelfLocCode},</if>  
			SYSDATE()
		)
	</insert>
	
	
	<!-- 수정 -->
	<update id="updateBook" parameterType="Params">
		UPDATE
			${TB_NAME}
		SET
			<if test="tbIdx != null">TB_IDX = #{tbIdx},</if>
			<if test="callNo != null">CALLNO = #{callNo},</if>
			<if test="bookKey != null">BOOKKEY = #{bookKey},</if>
			<if test="title != null">TITLE = #{title},</if>
			<if test="author != null">AUTHOR = #{author},</if>
			<if test="publisher != null">PUBLISHER = #{publisher},</if>
			<if test="pubYear != null">PUBYEAR = #{pubYear},</if>
			<if test="regNo != null">REGNO = #{regNo},</if>
			<if test="isbn != null">ISBN = #{isbn},</if>
			<if test="image != null">IMAGE = #{image},</if>
			<if test="description != null">DESCRIPTION = #{description},</if>
			<if test="useYn != null">USE_YN = #{useYn},</if>
			
			<if test="translators != null">TRANSLATORS = #{translators},</if>
			<if test="nanetImg != null">NANET_IMG = #{nanetImg},</if> 
			<if test="kakaoImg != null">KAKAO_IMG = #{kakaoImg},</if> 
			<if test="bkAge != null">BK_AGE = #{bkAge},</if> 
			<if test="bkAgeNm != null">BK_AGE_NM = #{bkAgeNm},</if> 
			<if test="bkYear != null">BK_YEAR = #{bkYear},</if> 
			<if test="bkMonth != null">BK_MONTH = #{bkMonth},</if> 
			<if test="bkWeek != null">BK_WEEK = #{bkWeek},</if> 
			
			<if test="bkKind != null">BK_KIND = #{bkKind},</if> 
			<if test="bkKindNm != null">BK_KIND_NM = #{bkKindNm},</if> 
			<if test="bkSeq != null">BK_SEQ = #{bkSeq},</if> 
			
			<if test="sortFlag != null">
				<choose>
					<when test="sortFlag == 'up'">
						SORT = SORT + 1,
					</when>
					<when test="sortFlag == 'down'">
						SORT = SORT - 1,
					</when>
				</choose>
			</if>
			
			<if test="SORT != null">SORT = #{SORT},</if>
			MOD_DATE = SYSDATE()
		    WHERE 1=1
		   <!--  <if test="content_type != null">
				   AND CONTENT_TYPE = #{content_type} 
		    </if> -->
			<if test="sortFlag != null">
				<choose>
					<when test="sortFlag == 'up'">
						AND SORT <![CDATA[>=]]> #{checkSortNum}
					</when>
					<when test="sortFlag == 'down'">
						AND SORT <![CDATA[>=]]> #{checkSortNum}
					</when>
				</choose>
			</if>
			<if test="sortFlag == null">
				AND BOOKKEY = #{SEQ}
			</if>
			<if test="site_cd != null">
				AND 
					SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					CONTENT_TYPE = #{content_type}
				</if>
				
	</update>
	
	
	
   <delete id="deleteBook" parameterType="Params">
		DELETE FROM ${TB_NAME}
		WHERE 1=1
		<!-- <if test="content_type != null">
		AND CONTENT_TYPE = #{content_type} 
	    </if> -->
		<if test="id > 0">AND BOOKKEY = #{id}</if>
		<if test="site_cd != null">
			AND 
				SITE_CD = #{site_cd}
		</if>
		<if test="content_type != null">
			AND 
				CONTENT_TYPE = #{content_type}
		</if>
		<if test="category != null">
			AND 
				CATEGORY = #{category}
		</if>
				
	</delete>
	
	
	<select id="selectAddSubList" parameterType="java.util.HashMap" resultType="OutBestBookVo">
	  (select tr.BOOKKEY, '-' AS ADDSUB
from TB_OUT_BESTBOOK tr left join TB_OUT_BESTBOOK_TMP tt on tr.BOOKKEY = tt.BOOKKEY
where tt.BOOKKEY is NULL 
<if test="site_cd != null">
				AND 
					tr.SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					tr.CONTENT_TYPE = #{content_type}
				</if>
			
)
  UNION ALL
 (select tt.BOOKKEY, '+' AS ADDSUB
from TB_OUT_BESTBOOK_TMP tt left join TB_OUT_BESTBOOK tr on tt.BOOKKEY = tr.BOOKKEY
where tr.BOOKKEY is NULL 
<if test="site_cd != null">
				AND 
					tt.SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					tt.CONTENT_TYPE = #{content_type}
</if>


)
	</select>
	
	
	<select id="selectAddSubList2" parameterType="java.util.HashMap" resultType="OutBookNewVo">
	  (select tr.BOOKKEY, '-' AS ADDSUB
from TB_OUT_NEWBOOK tr left join TB_OUT_NEWBOOK_TMP tt on tr.BOOKKEY = tt.BOOKKEY
where tt.BOOKKEY is NULL 
<if test="site_cd != null">
				AND 
					tr.SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					tr.CONTENT_TYPE = #{content_type}
				</if>
				
)
  UNION ALL
 (select tt.BOOKKEY, '+' AS ADDSUB
from TB_OUT_NEWBOOK_TMP tt left join TB_OUT_NEWBOOK tr on tt.BOOKKEY = tr.BOOKKEY
where tr.BOOKKEY is NULL 
<if test="site_cd != null">
				AND 
					tt.SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					tt.CONTENT_TYPE = #{content_type}
				</if>
				 
)
	</select>
	
	<select id="selectAddSubList3" parameterType="java.util.HashMap" resultType="OutRecomBookVo">
	  (select tr.BOOKKEY, '-' AS ADDSUB
from TB_OUT_RECOMBOOK tr left join TB_OUT_RECOMBOOK_TMP tt on tr.BOOKKEY = tt.BOOKKEY
where tt.BOOKKEY is NULL 
<if test="site_cd != null">
				AND 
					tr.SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					tr.CONTENT_TYPE = #{content_type}
				</if>
                
)
  UNION ALL
 (select tt.BOOKKEY, '+' AS ADDSUB
from TB_OUT_RECOMBOOK_TMP tt left join TB_OUT_RECOMBOOK tr on tt.BOOKKEY = tr.BOOKKEY
where tr.BOOKKEY is NULL 
<if test="site_cd != null">
				AND 
					tt.SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					tt.CONTENT_TYPE = #{content_type}
				</if>
               
)
	</select>
	
	
	
	 <select id="changeUseYn" parameterType="Params" resultType="Integer">
        update ${TB_NAME}
        set USE_YN = case when #{use_yn} = 'Y' then 'Y' else 'N' end
            ,MOD_ID = #{mode_id}, MOD_DATE = now()
        where TB_IDX = #{id} 
        and DEL_YN != 'Y'
        <if test="site_cd != null">
				AND 
					SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					CONTENT_TYPE = #{content_type}
				</if>
				
        ;
        select row_count();
    </select>
    
    
    <update id="bookSortUp" parameterType="Params">
		update
		${TB_NAME}
	set
		SORT  =  SORT - 1
		,MOD_ID = #{mode_id}, MOD_DATE = now()
	where
		SORT  = #{SORT} + 1
		<if test="site_cd != null">
				AND 
					SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					CONTENT_TYPE = #{content_type}
				</if>
				
		and DEL_YN = 'N';
	
	
	</update>
	
	<update id="bookSortUp2" parameterType="Params">
	update
		${TB_NAME}
	set
		SORT =  SORT + 1
		,MOD_ID = #{mode_id}, MOD_DATE = now()
	where
		TB_IDX = #{TB_IDX}		
		<if test="site_cd != null">
				AND 
					SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					CONTENT_TYPE = #{content_type}
				</if>
				 
		and DEL_YN = 'N';
	</update>
	



	<update id="bookSortDown" parameterType="Params">
		update
		${TB_NAME}
	set
		SORT = SORT + 1
		,MOD_ID = #{mode_id}, MOD_DATE = now()
	where
		SORT = #{SORT} - 1
		<if test="site_cd != null">
				AND 
					SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					CONTENT_TYPE = #{content_type}
				</if>
				
		and DEL_YN = 'N';
	
	
	</update>
	
	
	<update id="bookSortDown2" parameterType="Params">
	update
		${TB_NAME}
	set
		SORT = SORT - 1
		,MOD_ID = #{mode_id}, MOD_DATE = now()
	where
		TB_IDX	 = #{TB_IDX}	
		<if test="site_cd != null">
				AND 
					SITE_CD = #{site_cd}
				</if>
				<if test="content_type != null">
				AND 
					CONTENT_TYPE = #{content_type}
				</if>
				
		and DEL_YN = 'N';
	</update>
	
	
	
	 <select id="getApiBookInfo" parameterType="Params" resultType="Record">
        
        select 
        API_CALL_INFO 
        ,  API_KEY 
        , CODE_ID 
         from tb_api 
        where  DLT_YN = 'N'
        
        <if test="site_cd != null">
				AND 
					SITE_CD = #{site_cd}
		</if>
		<if test="code_id != null">
				AND 
					CODE_ID = #{code_id}
		</if>
				
    </select>
    
     <select id="getApiBookInfo2" parameterType="Params" resultType="Record">
        
        select 
        API_CALL_INFO 
        ,  API_KEY 
        , CODE_ID 
         from tb_api 
        where  DLT_YN = 'N'
        
        <if test="site_cd != null">
				AND 
					SITE_CD = #{site_cd}
		</if>
		<if test="code_id != null">
				AND 
					CODE_ID = #{code_id}
		</if>
				
    </select>
    
    
    
    <select id="getChannelInfoList" parameterType="Params" resultType="Record">
        
        select 
        code_id 
        ,  code_group_id 
         from common_code 
        where  delete_yn = 'N'
       and code_group_id = #{code_group_id}
       
				
    </select>
    
    
    
    
    
	
	
	
</mapper>
```
