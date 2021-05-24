---
title: lightgbm
categories: 机器学习
tags: 回归评测指标
mathjax: true
abbrlink: 8df69e6b
date: 2021-04-28 16:10:07
top_img: https://i.loli.net/2021/05/24/oaG7K2NFcujmHnq.png
cover: https://i.loli.net/2021/05/24/oaG7K2NFcujmHnq.png
---


# Lightgbm 获取特征重要性

<!--more-->

lightgbm主要分为两种方式训练，一种是通过原生API另外一种是通过scikit-learn中python API 方式

> 该列是主要的特征

[点击跳转至百度](http://www.baidu.com)
![图片](https://upload-images.jianshu.io/upload_images/703764-605e3cc2ecb664f6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1.
2.
3.

```python 
feature_df = pd.DataFrame({
        'column': feature_names,
        'importance': lgb_model.feature_importances_,
    }).sort_values(by='importance',ascending = False)
## 得到前一百个特征
feature_df[:100]
```

# 回归评价指标和相关理解

## 回归预测评价指标对比

$ y_i 代表预测结果，y 代表真实值 $。

1. MAE 

$$ \frac{1}{m} \sum_{i=1}^n|y-y_i| $$
衡量真实值和预测值之间的差距，并且得到的是一个平均值，MAE二阶不可导，在xgboost、lightgbm中使用近似函数代替。

2. MSE

$$ \frac{1}{m} \sum_{i=1}^n(y-y_i)^2 $$
比MAE得到的值会更大，因为有一个平方，差值大于一会成倍放大，会更加倾向更新差值较大的数据。

3. RMSE

$$ \sqrt{\frac{1}{m} \sum_{i=1}^n(y-y_i)^2} $$
和MSE类似，只不过将结果缩放到和原数据同等数量级上。

4. MAPE

$$ \frac{1}{m} \sum_{i=1}^n|\frac{y-y_i}{y}| $$
用来衡量误差和占真实值比例，这也是在实际中常见的评估方式。

5. SMPE

$$ \frac{100\%}{m}\sum_{i=1}^n\frac{|y - y_i|}{(|y|+|y_i|)/2}$$
该损失函数是为了将MAPE中更加倾向优化小值的情况。
 
6. R2-score

$$ R^2 = 1 - \frac{\sum_i{(y-y_i)^2}}{\sum_i{(\bar{y} - y_i)^2}} $$
该系数的取值范围为$(-\infty) - 0$,最好结果是1，预测值和真实值完全拟合。如果预测结果还没有使用平均值结果好就会达到小于零的值。

7.  矫正决定系数

$$ R^2_{ad} = 1 - \frac{(1-R^2)(m-1)}{m-p-1} $$
其中m是样本数量，p是特征数量

    ```python 
    from sklearn.metrics import mean_squared_error #均方误差
    from sklearn.metrics import mean_absolute_error #平方绝对误差
    from sklearn.metrics import r2_score#R square
    #调用
    MSE：mean_squared_error(y_test,y_predict)
    RMSE:np.sqrt(mean_squared_error(y_test,y_predict))
    MAE：mean_absolute_error(y_test,y_predict)
    R2：r2_score(y_test,y_predict)
    Adjusted_R2：:1-((1-r2_score(y_test,y_predict))*(m-1))/(m-p-1)
    ```


## 思考如何评价一个模型是否拟合的好？
假设有五个样本标签为 {10,15,20,13,18} 预测结果为 {9,17,18,10,16}
不同指标结果如下： MAE：2  MSE:4.4  RMSE: $\sqrt{4.4}$ 
MAPE: (0.1 + 2/15+2/20+3/13+2/18)/5 = 0.135 
SMAPE: (2*1 / (10+9) + 2*2/(15+17) + 2*2/(20+18) + 2*3/(13+10) + 2*2/(16+18))/5 = 0.143
R2：0.65
R2-score: 这块没有特征的数量没有计算，在实际训练中该值是确定的，取决于特征数量

RMSE 值比MA值大的原因是因为大于一的值会放大，解释RMSE为什么会对异常值更加敏感, 也会有小于1的值会被缩小，但是小于1的值缩小值远不及放大的值。假设两个样本：真实值{10,15}，预测值{9.5,10}
MAE：2.75 RMSE: 5.05

## 为什么MAE倾向于拟合平均值？RMSE倾向于中位数？MAPE对小值更加敏感？
1. 为什么MAE倾向于拟合平均值？
2. RMSE倾向于拟合中位数？
3. MAPE对小值更加敏感？

## 怎么对比不同时间序列的预测准确性？
首先不同时间序列真实值并不是完全一致的。比如A的时间序列平均值为100，B的时间序列的平均值为10000，两个时间序列在量纲上存在差异，如果直接使用MAE，MSE, RMSE等指标会存在明显差异。
可以考虑将目标值最大最小变化到同一量纲上面

## 比较相同时间序列在不同模型中差异
可以比较在不同模型下预测结果值的大小