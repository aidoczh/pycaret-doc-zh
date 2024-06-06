# 使用 PyCaret 进行时间序列异常检测

#### [实战教程](https://towardsdatascience.com/tagged/hands-on-tutorials)

### 使用 PyCaret 进行时间序列异常检测

#### 一篇关于使用 PyCaret 的无监督异常检测模块对时间序列数据进行检测的逐步教程

![PyCaret — 一个Python中的开源、低代码机器学习库](https://cdn-images-1.medium.com/max/2604/1\*O-lbKPXdK7716BK8MLpTQA.png)

### 👉 简介

这是一篇逐步、适合初学者的教程，介绍了如何使用 PyCaret 的无监督异常检测模块在时间序列数据中检测异常。

#### 本教程的学习目标

* 什么是异常检测？异常检测的类型。
* 业务中的异常检测用例。
* 使用 PyCaret 训练和评估异常检测模型。
* 标记异常值并分析结果。

### 👉 PyCaret

PyCaret 是一个开源的、低代码机器学习库和端到端模型管理工具，用Python编写，用于自动化机器学习工作流程。它因易用性、简单性以及快速高效地构建和部署端到端机器学习原型而广受欢迎。

PyCaret 是一个替代低代码库，可以用少量代码取代数百行代码。这使得实验周期变得指数级快速和高效。

PyCaret **简单且易于使用**。PyCaret 中执行的所有操作都顺序存储在一个**Pipeline**中，该Pipeline完全自动化用于部署。无论是填补缺失值、独热编码、转换分类数据、特征工程，甚至超参数调整，PyCaret 都可以自动化完成。

要了解更多关于 PyCaret，请查看他们的[GitHub](https://www.github.com/pycaret/pycaret)。

### 👉 安装 PyCaret

安装 PyCaret 非常简单，只需几分钟即可完成。我们强烈建议使用虚拟环境，以避免与其他库可能发生的冲突。

PyCaret 的默认安装是 pycaret 的精简版本，只安装了[这里列出的硬依赖](https://github.com/pycaret/pycaret/blob/master/requirements.txt)。

```
# 安装精简版本（默认）
pip install pycaret

# 安装完整版本
pip install pycaret[full]
```

当您安装 pycaret 的完整版本时，还会安装[这里列出的所有可选依赖](https://github.com/pycaret/pycaret/blob/master/requirements-optional.txt)。

### 👉 什么是异常检测

异常检测是一种用于识别与大多数数据显著不同的**罕见项目、事件或观察**的技术。

通常，异常项目会转化为某种问题，例如：

* 银行欺诈，
* 结构缺陷，
* 医疗问题，
* 错误等。

异常检测算法大致可分为以下几类：

\*\*(a) 监督：\*\* 当数据集具有标识哪些交易是异常和哪些是正常的标签时使用。（这类似于监督分类问题）。

\*\*(b) 无监督：\*\* 无监督意味着没有标签，模型在完整数据上进行训练，并假设大多数实例是正常的。

**(c) 半监督：** 模型仅在正常数据上进行训练（没有任何异常）。当训练好的模型用于新数据点时，它可以预测新数据点是正常的还是异常的（基于训练模型中数据的分布）。

![异常检测的业务用例](https://cdn-images-1.medium.com/max/3200/0\*viL5WxtnFLMCyFXo)

### 👉 PyCaret 异常检测模块

PyCaret 的[**异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html)** 模块是一个用于识别**罕见项目**、**事件**或**观察**的无监督机器学习模块。它提供了超过15种算法和[多个图表](https://www.pycaret.org/plot-model)来分析训练模型的结果。

### 👉 数据集

我将使用包含从2014年7月到2015年1月每半小时的出租车乘客数量的 NYC 出租车乘客数据集。您可以从[这里](https://raw.githubusercontent.com/numenta/NAB/master/data/realKnownCause/nyc_taxi.csv)下载数据集。

```
import pandas as pd
data = pd.read_csv('https://raw.githubusercontent.com/numenta/NAB/master/data/realKnownCause/nyc_taxi.csv')

data['timestamp'] = pd.to_datetime(data['timestamp'])

data.head()
```

![数据样本](https://cdn-images-1.medium.com/max/2000/1\*PR6dPBsezOTlHg23y6HJLA.png)

```
# 创建移动平均线
data['MA48'] = data['value'].rolling(48).mean()
data['MA336'] = data['value'].rolling(336).mean()

# 绘图
import plotly.express as px
fig = px.line(data, x="timestamp", y=['value', 'MA48', 'MA336'], title='NYC 出租车行程', template='plotly_dark')
fig.show()
```
![value, moving_average(48), and moving_average(336)](https://cdn-images-1.medium.com/max/2626/1\*7\_u3piw7krj-g\_hw98pYxA.png)

### 👉 数据准备

由于算法无法直接处理日期或时间戳数据，我们将从时间戳中提取特征，并在训练模型之前删除实际的时间戳列。

```python
**# 删除移动平均列**
data.drop(['MA48', 'MA336'], axis=1, inplace=True)

**# 将时间戳设置为索引**
data.set_index('timestamp', drop=True, inplace=True)

**# 将时间序列重采样为小时级别**
data = data.resample('H').sum()

**# 从日期中创建特征**
data['day'] = [i.day for i in data.index]
data['day_name'] = [i.day_name() for i in data.index]
data['day_of_year'] = [i.dayofyear for i in data.index]
data['week_of_year'] = [i.weekofyear for i in data.index]
data['hour'] = [i.hour for i in data.index]
data['is_weekday'] = [i.isoweekday() for i in data.index]

data.head()
```

![转换后的数据示例行](https://cdn-images-1.medium.com/max/2000/1\*tEOAoRNWE6Djjqw4TAzDrg.png)

### 👉 实验设置

在 PyCaret 的所有模块中，setup 函数是开始任何机器学习实验的第一个且唯一必需的步骤。除了默认执行一些基本的处理任务外，PyCaret 还提供了各种预处理功能。要了解 PyCaret 中所有预处理功能的更多信息，可以参考[链接](https://pycaret.org/preprocessing/)。

```python
**# 初始化 setup**
from pycaret.anomaly import *
s = setup(data, session_id = 123)
```

![pycaret.anomaly 模块中的 setup 函数](https://cdn-images-1.medium.com/max/2740/1\*XtjlemIJlJzHWC\_jdEuUxA.png)

每当在 PyCaret 中初始化 setup 函数时，它会对数据集进行分析并推断出所有输入特征的数据类型。在这种情况下，可以看到 day\_name 和 is\_weekday 被推断为分类变量，其余的被推断为数值变量。按 Enter 键继续。

![setup 的输出 —— 为了显示而截断](https://cdn-images-1.medium.com/max/2000/1\*Za0hBYrKkUCWIZOBwO6cYg.png)

### 👉 模型训练

要查看所有可用算法的列表：

```python
**# 查看可用模型列表**
models()
```

![models() 函数的输出](https://cdn-images-1.medium.com/max/2000/1\*ukgNW9AQxGMQJp46ZVdQgQ.png)

在本教程中，我使用的是孤立森林（Isolation Forest），但你可以将下面代码中的 ID 'iforest' 替换为任何其他模型 ID 来更改算法。如果想要了解更多关于孤立森林算法的信息，可以参考[这里](https://en.wikipedia.org/wiki/Isolation\_forest)。

```python
**# 训练模型**
iforest = create_model('iforest', fraction = 0.1)
iforest_results = assign_model(iforest)
iforest_results.head()
```

![iforest\_results 的示例行](https://cdn-images-1.medium.com/max/2036/1\*ZqqojxS5Ef99RFxXwV7m\_w.png)

注意到新增了两列，即 Anomaly（异常）列，其中异常值为 1，正常值为 0，以及 Anomaly\_Score（异常分数）列，它是一个连续值，也就是决策函数（算法根据该分数确定异常）。

```python
**# 检查异常值**
iforest_results[iforest_results['Anomaly'] == 1].head()
```

![iforest\_results 的示例行（筛选 Anomaly == 1）](https://cdn-images-1.medium.com/max/2016/1\*HaOFOsWbixByVdRBK-FkfQ.png)

现在我们可以将异常值绘制在图上进行可视化。

```python
import plotly.graph_objects as go

**# 在 y 轴上绘制数值，在 x 轴上绘制日期**
fig = px.line(iforest_results, x=iforest_results.index, y="value", title='纽约出租车行程 - 无监督异常检测', template = 'plotly_dark')

**# 创建异常日期列表**
outlier_dates = iforest_results[iforest_results['Anomaly'] == 1].index

**# 获取异常值的 y 值以绘制**
y_values = [iforest_results.loc[i]['value'] for i in outlier_dates]

fig.add_trace(go.Scatter(x=outlier_dates, y=y_values, mode = 'markers', 
                name = '异常值', 
                marker=dict(color='red',size=10)))
        
fig.show()
```

![纽约出租车行程 —— 无监督异常检测](https://cdn-images-1.medium.com/max/2632/1\*Xg78KCHEgSRVbY4lKOX3Kw.png)

注意到模型在 1 月 1 日附近检测到了多个异常值，这是新年前夜。模型还在 1 月 18 日至 1 月 22 日之间检测到了几个异常值，这是 _北美暴风雪_（一场快速而破坏性的暴风雪）席卷东北部地区，纽约市周边地区降雪量达到 30 厘米的时候。

如果你在搜索引擎中搜索其他红色点附近的日期，可能能找到这些点被模型检测为异常的原因的线索（希望如此）。

希望你能欣赏 PyCaret 的易用性和简洁性。只需几行代码和几分钟的实验，我就训练了一个无监督异常检测模型，并对时间序列数据进行了异常检测。

### 即将推出！
下周我将撰写一篇关于如何使用[PyCaret回归模块](https://pycaret.readthedocs.io/en/latest/api/regression.html)在PyCaret中训练自定义模型的教程。您可以在[Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/)和[Twitter](https://twitter.com/moezpycaretorg1)上关注我，以便在发布新教程时立即收到通知。

使用这个轻量级的Python工作流自动化库，您可以实现无限可能。如果您觉得这个工具有用，请不要忘记在我们的GitHub存储库上给我们 ⭐️。

想了解更多关于PyCaret的信息，请关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)和[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

加入我们的Slack频道。邀请链接在[这里](https://join.slack.com/t/pycaret/shared_invite/zt-p7aaexnl-EqdTfZ9U~mF0CwNcltffHg)。

### 您可能还感兴趣：

[使用PyCaret 2.0在Power BI中构建您自己的AutoML](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d) [使用Docker在Azure上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01) [在Google Kubernetes Engine上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b) [在AWS Fargate上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507) [构建和部署您的第一个机器学习Web应用程序](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99) [使用AWS Fargate无服务器部署PyCaret和Streamlit应用程序](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2) [使用PyCaret和Streamlit构建和部署机器学习Web应用程序](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104) [在GKE上部署使用Streamlit和PyCaret构建的机器学习应用程序](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

### 重要链接

[文档](https://pycaret.readthedocs.io/en/latest/installation.html) [博客](https://medium.com/@moez_62905) [GitHub](http://www.github.com/pycaret/pycaret) [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html) [笔记本教程](https://pycaret.readthedocs.io/en/latest/tutorials.html) [为PyCaret做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 想了解特定模块的信息吗？

点击下面的链接查看文档和示例。

[分类](https://pycaret.readthedocs.io/en/latest/api/classification.html) [回归](https://pycaret.readthedocs.io/en/latest/api/regression.html) [聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html) [异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html) [自然语言处理](https://pycaret.readthedocs.io/en/latest/api/nlp.html) [关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)