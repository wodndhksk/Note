```mysql
select tcf.FL_KEY, tcf.FL_ORG_NM, tv.VIDEO_URL , tv.VIDEO_TITLE , tv.VIDEO_DESCRIPTION, date_format(tv.REG_DATE 
, '%Y.%m.%d') as VIDEO_DATE 
FROM 
tb_com_file tcf left join tb_video tv on tcf.FL_PRNT_KEY = tv.VIDEO_SEQ 

where tv.CONTENT_TYPE = 'CT03' 
and tv.USE_YN ='Y' 
and tv.DLT_YN <> 'Y' 
and tcf.THUMBNAILYN ='Y' 
and tv.SM_PARAM = 'dt04_video01' 

and tv.VIDEO_URL != '[object Object]';
```
