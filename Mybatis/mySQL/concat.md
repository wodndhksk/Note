### CONCAT(url)
```xml
<select id = "selectTwobyOne" parameterType="Map" resultType="Map">
	<![CDATA[
  	SELECT *, CONCAT('/admin/file-manager/view/', GET_COM_FILE_KEY(CONTENT_TYPE, IMG_SEQ), '.do') AS URL
	FROM tb_image_play
	WHERE SM_PARAM = #{sm_param}
	AND DLT_YN = 'N'
	and DISTR_DATE <= NOW()
    	and (
          CASE
            WHEN IMG_DSP_TYPE = 'A' THEN TRUE
            WHEN IMG_DSP_TYPE = 'B' THEN
              CASE 
                  WHEN IMG_DSP_START <= NOW() AND IMG_DSP_END >= NOW() THEN TRUE ELSE FALSE
              END
          END
        )
        ]]>
  </select>
```
