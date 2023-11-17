```xml
SELECT *
         FROM (SELECT ROWNUM AS rnum,
                        MAX(ROWNUM) OVER(ORDER BY ROWNUM DESC) AS TOTALROWS,
                         B.*
             FROM   (  
                 SELECT SO.ISSUE_DATE
                      ,SO.PLAN_DELIVERY_DATE
                      ,SO.STORE_CODE
                      ,SO.STORE_NAME
                      ,SO.REG_DATE
                FROM SD_SO_H SO
                WHERE 1=1
                  AND SO.ACTV_FLAG = 'Y'
                  AND SO.REG_DATE >= TRUNC(SYSDATE) -- 금일 조건
            )B
        )
        WHERE ROWNUM <![CDATA[<=]]> 5
```
