### ex) 2021-09-07 부터 2021-10-06일 까지 로 날짜 기간 설정
- 뒤에 +1을 해줘야 endDate 날짜까지 포함이 된다.

### mySql 쿼리문
```mysql
SELECT * FROM tb_log WHERE reg_date BETWEEN DATE('2021-09-07') AND DATE('2021-10-06')+1
```

### myBatis 표기법
```xml
A.REG_DATE BETWEEN STR_TO_DATE(#{START_DATE},'%Y-%m-%d-%s %H:%i:%S') AND STR_TO_DATE(#{END_DATE}+1,'%Y-%m-%d-%s %H:%i:%S')

```
