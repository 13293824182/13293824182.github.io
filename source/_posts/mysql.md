---
title: mysql
tags: 经验总结
categories: mysql
keywords: mysql
top-img: 'https://i.loli.net/2021/05/28/d7p4BMJxrEnNach.jpg'
cover: 'https://i.loli.net/2021/05/28/d7p4BMJxrEnNach.jpg'
abbrlink: 9520183a
date: 2021-05-28 22:43:39
mathjax:
description:
---

<!---more--->


### 从a中筛选数据，然后从筛选数据中b中筛选相关字段
```sql
with b as (select * from a),
c as (select * from d) ##最后不加逗号
select * from b

with c (columns1，columns2) as (select columns1，columns2 from d)
```


### distinct 要放在最前面,结果只显示b_columns的结果
```sql
select distinct b_column from a
```

### 创建临时表并且使用完表会删除
```sql
-- 如果存在删除
DROP TABLE IF EXISTS final_list2;
DECLARE GLOBAL TEMPORARY TABLE final_list2(
    order_no_22 int null,
    batch_date_new timestamp null,
    pay_no int null,
    check_no int null,
    batch_no int null,
    batch_date timestamp null,
    order_type int null,
    order_no int null
    )
    ON
COMMIT PRESERVE ROWS
WITH NORECOVERY;
```

### 条件语句
```sql
不等于：<>
不为空：is not null 
if (sex = "男","女","男")  -- 如果第一个为真 为女  假为男  实现男女互换
ifnull(a,b)  -- 如果a不为空，结果为a；如果a为空，结果为b
```

### 更新数据 最好不要采用join操作会有重复数据导致结果增多
```sql
-- 如果更新多个字段在（）内添加就好
update itfendonl.cust_doc_list_test_check_batch_bank a
set (batch_date_new,b)=(select batch_date_new,b
from final_list2 b
where a.pay_no=b.pay_no)
-- where a.bank_date is null
```

### 增加新字段
```sql
ALTER TABLE table_name ADD column_name datatype
```

### 在表中插入数据, 更新表时候不能使用union，需要新建一个临时表
```sql
DROP TABLE IF EXISTS pay_list;
DECLARE GLOBAL TEMPORARY TABLE pay_list(
    batch_no int null,
    flag int null,
    pay_date timestamp null,
    bank_date timestamp null
        )
        ON
COMMIT PRESERVE ROWS
WITH NORECOVERY;
 
insert into  pay_list (batch_no,bank_date,flag,pay_date)  
select distinct b.batch_no,b.bank_date, b.flag,b.pay_date
from itwennyt.cust_payment_list_2 b
```
