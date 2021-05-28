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
with b as (select * from a)
select * from b
```

### distinct 要放在最前面,结果只显示b_columns的结果
```sql
select distinct b_column from a
```
