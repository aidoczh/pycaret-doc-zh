# 时间序列 101 - 初学者指南

### 时间序列 101 — 初学者指南

#### 一份面向初学者的时间序列预测简介

![Photo by Chris Liverani on Unsplash](https://cdn-images-1.medium.com/max/7262/0\*AfqHPFyS5tc-9Amn)

### 👉 什么是时间序列数据？

时间序列数据是在不同时间点收集的同一主题的数据，比如**一个国家每年的 GDP，某家公司一段时间内的股价，或者你自己每秒记录的心跳**，事实上，任何你可以在不同时间间隔连续捕获的数据都是时间序列数据。

下面是一个时间序列数据的示例，下图展示了特斯拉公司（股票代码：TSLA）去年的每日股价走势。右侧的 y 轴表示美元价值（图表上最后一个点即 $701.91 是本文撰写时（2021年4月12日）的最新股价）。

![时间序列数据示例 — 特斯拉公司（股票代码：TSLA）每日股价 1年间隔。](https://cdn-images-1.medium.com/max/2874/1\*5lgEl2\_2wx3b7YEiROYPtA.png)

另一方面，更传统的数据集，如客户信息、产品信息、公司信息等，这些数据在单个时间点存储信息，被称为横截面数据。

看下面这个数据集的例子，追踪了美国在2020年上半年最畅销的电动汽车。请注意，与追踪一段时间内售出的汽车不同，下图追踪了同一时间段内的不同汽车，如特斯拉、雪佛兰和日产。

![来源：Forbes](https://cdn-images-1.medium.com/max/2000/1\*pzKQuUCGJldCQcORZlLvXQ.jpeg)

很容易区分横截面数据和时间序列数据之间的区别，因为两种数据集的分析目标大不相同。在第一个分析中，我们关注的是特斯拉股价的时间走势，而在后者中，我们想要分析同一时间段内的不同公司，即2020年上半年。

然而，典型的现实世界数据集很可能是混合的。想象一下像沃尔玛这样每天销售成千上万种产品的零售商。如果你分析某一天的产品销售情况，例如，如果你想找出平安夜最畅销的商品是什么，这将是一个横截面分析。相反，如果你想了解一种特定商品的销售情况，比如 PS4 在一段时间内的销售情况（比如过去5年），那么这就变成了时间序列分析。

总之，时间序列数据和横截面数据的分析目标不同，现实世界的数据集很可能是时间序列数据和横截面数据的混合。

### 👉 什么是时间序列预测？

时间序列预测就是其字面意思，即预测未来的未知值。然而，与科幻电影不同，在现实世界中，这种预测略显乏味。它涉及收集历史数据，为算法准备数据（算法简单来说就是背后的数学），然后根据从历史数据中学到的模式预测未来值。

你能想到为什么公司或任何人会对任何时间序列（GDP、月销售额、库存、失业率、全球温度等）的未来值感兴趣吗？让我从商业角度给你一些解释：

* 零售商可能有兴趣预测每个 SKU 级别的未来销售，以进行规划和预算编制。
* 一家小商家可能有兴趣预测每家店铺的销售额，以便安排合适的资源（在繁忙时段增加人手，反之亦然）。
* 像谷歌这样的软件巨头可能有兴趣了解一天中最繁忙的时间或一周中最繁忙的日子，以便相应地安排服务器资源。
* 卫生部门可能有兴趣预测累积接种的 COVID 疫苗数量，以便了解预期形成群体免疫的关键时刻。

### 👉 时间序列预测方法

时间序列预测大致可分为以下几类：

* **经典 / 统计模型** — 移动平均、指数平滑、ARIMA、SARIMA、TBATS
* **机器学习** — 线性回归、XGBoost、随机森林，或任何带有降维方法的机器学习模型
* **深度学习** — RNN、LSTM

本教程侧重于使用 _**机器学习**_ 进行时间序列预测。在本教程中，我将使用 Python 中一个名为 [PyCaret](https://www.pycaret.org) 的开源、低代码机器学习库的回归模块。如果你之前没有使用过 PyCaret，你可以在[这里](https://www.pycaret.org/guide)快速入门。尽管如此，你无需具备任何关于 PyCaret 的先验知识，也可以跟着本教程进行学习。

### 👉 PyCaret 回归模块
PyCaret **回归模块**是一个监督学习模块，用于估计一个或多个**自变量**（通常称为“结果变量”或“目标”）与一个**因变量**（通常称为“特征”或“预测变量”）之间的关系。

回归的目标是预测连续值，如销售额、数量、温度、顾客数量等。PyCaret中的所有模块都提供许多[预处理](https://www.pycaret.org/preprocessing)功能，通过[setup](https://www.pycaret.org/setup)函数准备数据进行建模。它提供了超过25种现成的算法和[多个图表](https://www.pycaret.org/plot-model)来分析训练模型的性能。

### 👉 数据集

在本教程中，我使用了美国航空乘客数据集。您可以从[Kaggle](https://www.kaggle.com/chirag19/air-passengers)下载该数据集。该数据集提供了1949年至1960年期间美国航空乘客的月度总数。

```
**# 读取csv文件
**import pandas as pd
data = pd.read_csv('AirPassengers.csv')
data['Date'] = pd.to_datetime(data['Date'])
data.head()
```

![样本行](https://cdn-images-1.medium.com/max/2000/0\*1-RkItCLsIyv3HpN.png)

```
**# 创建12个月的移动平均线
**data['MA12'] = data['Passengers'].rolling(12).mean()

**# 绘制数据和移动平均线
**import plotly.express as px
fig = px.line(data, x="Date", y=["Passengers", "MA12"], template = 'plotly_dark')
fig.show()
```

![带有移动平均线为12的美国航空乘客数据集时间序列图](https://cdn-images-1.medium.com/max/2618/0\*iwkq5c6sZM63PU9u.png)

由于机器学习算法无法直接处理日期，让我们从日期中提取一些简单的特征，如月份和年份，并删除原始日期列。

```
**# 从日期中提取月份和年份**
data['Month'] = [i.month for i in data['Date']]
data['Year'] = [i.year for i in data['Date']]

**# 创建一个数字序列
**data['Series'] = np.arange(1,len(data)+1)

**# 删除不必要的列并重新排列
**data.drop(['Date', 'MA12'], axis=1, inplace=True)
data = data[['Series', 'Year', 'Month', 'Passengers']] 

**# 检查数据集的头部**
data.head()
```

![提取特征后的样本行](https://cdn-images-1.medium.com/max/2000/0\*h8TaSF1bi9-Y9Psa.png)

```
**# 将数据集拆分为训练集和测试集
**train = data[data['Year'] < 1960]
test = data[data['Year'] >= 1960]

**# 检查形状
**train.shape, test.shape
>>> ((132, 4), (12, 4))
```

在初始化设置之前，我手动拆分了数据集。另一种方法是将整个数据集传递给PyCaret，并让它处理拆分，在这种情况下，您将需要在设置函数中传递data\_split\_shuffle = False，以避免在拆分之前对数据集进行洗牌。

### 👉 初始化设置

现在是时候初始化设置函数了，在其中我们将显式地传递训练数据、测试数据和交叉验证策略，使用fold\_strategy参数。

```
**# 导入回归模块
**from pycaret.regression import *

**# 初始化设置
s = setup(data = train, test_data = test, target = 'Passengers', fold_strategy = 'timeseries', numeric_features = ['Year', 'Series'], fold = 3, transform_target = True, session_id = 123)
```

### 👉 训练和评估所有模型

```
best = compare_models(sort = 'MAE')
```

![compare\_models的结果](https://cdn-images-1.medium.com/max/2000/1\*Z7VBrEv1Sh5z532cNy6\_qQ.png)

基于交叉验证的MAE，最佳模型是\*\*最小角度回归\*\*（MAE: 22.3）。让我们在测试集上检查得分。

```
prediction_holdout = predict_model(best);
```

![predict\_model(best)函数的结果](https://cdn-images-1.medium.com/max/2000/0\*O0gKIfX126Z0Ni-B.png)

测试集上的MAE比交叉验证的MAE高出12%。不太好，但我们将继续使用它。让我们绘制实际和预测的曲线以可视化拟合情况。

```
**# 在原始数据集上生成预测
**predictions = predict_model(best, data=data)

**# 在数据集中添加一个日期列
**predictions['Date'] = pd.date_range(start='1949-01-01', end = '1960-12-01', freq = 'MS')

**# 折线图
fig = px.line(predictions, x='Date', y=["Passengers", "Label"], template = 'plotly_dark')

**# 添加一个垂直矩形来分隔测试集
fig.add_vrect(x0="1960-01-01", x1="1960-12-01", fillcolor="grey", opacity=0.25, line_width=0)fig.show()
```

![实际和预测的美国航空乘客（1949-1960）](https://cdn-images-1.medium.com/max/2624/0\*\_1esCD8Bx6MZ14Mj.png)

灰色背景是测试期间（即1960年）。现在让我们完成模型，即在整个数据集上训练最佳模型，即_最小角度回归_（这次包括测试集）。

```
final_best = finalize_model(best)
```

### 👉 创建未来评分数据集
现在我们已经在整个数据集（1949年至1960年）上训练了我们的模型，让我们通过1964年预测未来五年。为了使用我们的最终模型生成未来预测，我们首先需要创建一个包含未来日期的月份、年份和序列列的数据集。

```python
future_dates = pd.date_range(start='1961-01-01', end='1965-01-01', freq='MS')

future_df = pd.DataFrame()

future_df['Month'] = [i.month for i in future_dates]
future_df['Year'] = [i.year for i in future_dates]    
future_df['Series'] = np.arange(145, (145+len(future_dates)))
future_df.head()
```

![future_df的示例行](https://cdn-images-1.medium.com/max/2000/0*N26BSbKSR9u3k7hv.png)

现在，让我们使用future_df进行评分并生成预测。

```python
predictions_future = predict_model(final_best, data=future_df)
predictions_future.head()
```

![predictions_future的示例行](https://cdn-images-1.medium.com/max/2000/0*c97sliOBqExx6Hs_.png)

### **👉 绘制实际数据和预测**

```python
concat_df = pd.concat([data, predictions_future], axis=0)
concat_df_i = pd.date_range(start='1949-01-01', end='1965-01-01', freq='MS')
concat_df.set_index(concat_df_i, inplace=True)
fig = px.line(concat_df, x=concat_df.index, y=["Passengers", "Label"], template='plotly_dark')
fig.show()
```

![1949年至1960年的实际数据和1961年至1964年的预测美国航空乘客人数](https://cdn-images-1.medium.com/max/2614/0*x8TJJUO--Unxheeg.png)

希望您觉得这个教程很容易。如果您认为自己已经准备好迈入下一个级别，可以查看我的关于[使用PyCaret进行多时间序列预测的高级教程](https://towardsdatascience.com/multiple-time-series-forecasting-with-pycaret-bc0a779a22fe)。

### 即将推出！

我将很快撰写一篇关于使用[PyCaret异常检测模块](https://pycaret.readthedocs.io/en/latest/api/anomaly.html)对时间序列数据进行无监督异常检测的教程。如果您想获取更多更新，可以关注我的[Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/)和[Twitter](https://twitter.com/moezpycaretorg1)。

使用Python中这个轻量级工作流自动化库，您可以实现无限可能。如果您觉得这很有用，请不要忘记在我们的GitHub存储库上点赞⭐️。

要了解更多关于PyCaret的信息，请关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)和[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

加入我们的Slack频道。邀请链接[在这里](https://join.slack.com/t/pycaret/shared_invite/zt-p7aaexnl-EqdTfZ9U~mF0CwNcltffHg)。

### 您可能还感兴趣：

[在Power BI中使用PyCaret 2.0构建您自己的AutoML](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d) [使用Docker在Azure上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01) [在Google Kubernetes Engine上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b) [在AWS Fargate上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507) [构建和部署您的第一个机器学习Web应用程序](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99) [使用AWS Fargate无服务器部署PyCaret和Streamlit应用程序](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2) [使用PyCaret和Streamlit构建和部署机器学习Web应用程序](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104) [在GKE上部署使用Streamlit和PyCaret构建的机器学习应用程序](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

### 重要链接

[文档](https://pycaret.readthedocs.io/en/latest/installation.html) [博客](https://medium.com/@moez_62905) [GitHub](http://www.github.com/pycaret/pycaret) [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html) [笔记本教程](https://pycaret.readthedocs.io/en/latest/tutorials.html) [为PyCaret做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 想了解特定模块吗？

点击下面的链接查看文档和工作示例。
[分类](https://pycaret.readthedocs.io/en/latest/api/classification.html) [回归](https://pycaret.readthedocs.io/en/latest/api/regression.html) [聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html) [异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html) [自然语言处理](https://pycaret.readthedocs.io/en/latest/api/nlp.html) [关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)