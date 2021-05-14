---
title: pandas总结
tags: pandas
categories: 日常总结
abbrlink: b8b772a5
date: 2021-05-13 16:59:18
---

## pandas日常使用总结

### pandas 中没有获取众数

思路：通过统计次数，并且获取到最大次数即为众数


```
data[['diff_days','cust_no']].groupby('cust_no').agg(lambda x: x.value_counts().index[0]).reset_index()
```