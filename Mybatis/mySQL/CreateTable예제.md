## 테이블 생성 샘플
- 기본키 설정과 외래키 설정도 포함 

```sql
create table ht_std_fix_dlv_cost(

       fix_dlv_cost_sn bigint not null auto_increment comment '고정배송비용일련번호' , 
    comodity_sno bigint not null  comment '품목일련번호' , 
    apply_start_dtm timestamp not null  comment '적용시작일시' , 
    apply_end_dtm timestamp not null  comment '적용종료일시' , 
    apply_yn varchar(1) not null  comment '적용여부' , 
    fix_dlv_cost decimal(15.2) not null  comment '고정배송비용' , 
    prdt_add_yn varchar(1)  comment '상품추가여부' , 
    prdt_add_cnt decimal(6)  comment '상품추가건수' , 
    prdt_max_cnt decimal(6)  comment '상품최대건수' , 
    prdt_nm varchar(200)  comment '상품명' , 
    brand_nm varchar(200)  comment '브랜드명' , 
    real_measure_max_wt decimal(15.2)  comment '실측정최대중량' , 
    size_measure_max_wt decimal(15.2)  comment '부피측정최대중량' , 
    shopmall_url varchar(255)  comment '쇼핑몰url' , 
    img_url varchar(255)  comment '이미지url' , 
    reg_id varchar(12) not null  comment '등록id' , 
    reg_dtm timestamp not null  comment '등록일시' , 
    modify_id varchar(12) not null  comment '수정id' , 
    modify_dtm timestamp not null  comment '수정일시' , 

    primary key (fix_dlv_cost_sn),
    
    key ht_std_fix_dlv_cost_fk (comodity_sno)
    CONSTRAINT ht_std_fix_dlv_cost_fk FOREIGN KEY (comodity_sno) REFERENCES ht_sit_comodity (comodity_sno) 

  ) 
  comment= '고정배송비' 
;


```
