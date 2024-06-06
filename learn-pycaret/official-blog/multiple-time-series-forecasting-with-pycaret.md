# 使用 PyCaret 进行多时间序列预测

### 使用 PyCaret 进行多时间序列预测的逐步教程

#### 一步一步教你如何使用 PyCaret 进行多时间序列预测

![PyCaret — 一个在 Python 中构建的开源、低代码机器学习库](https://cdn-images-1.medium.com/max/2000/1\*c8mBuCW7nP0KGhwXQC98Eg.png)

### PyCaret

PyCaret 是一个开源的、低代码的机器学习库和端到端模型管理工具，用于自动化机器学习工作流程。它因其易用性、简洁性以及能够快速高效地构建和部署端到端机器学习原型而广受欢迎。

PyCaret 是一个替代低代码库，可以用几行代码代替数百行代码。这使得实验周期指数级加快和高效。

PyCaret **简单且易于使用**。在 PyCaret 中执行的所有操作都按顺序存储在一个**流水线**中，该流水线完全自动化用于部署。无论是填充缺失值、独热编码、转换分类数据、特征工程还是超参数调整，PyCaret 都可以自动化完成。

本教程假设您具有一些关于 PyCaret 的先前知识和经验。如果您以前没有使用过它，没问题 - 您可以通过以下教程快速入门：

* [PyCaret 2.2 来了 - 有什么新功能](https://towardsdatascience.com/pycaret-2-2-is-here-whats-new-ad7612ca63b)
* [宣布 PyCaret 2.0](https://towardsdatascience.com/announcing-pycaret-2-0-39c11014540e)
* [关于 PyCaret 你不知道的五件事](https://towardsdatascience.com/5-things-you-dont-know-about-pycaret-528db0436eec)

### **回顾**

在我的[上一个教程](https://towardsdatascience.com/time-series-forecasting-with-pycaret-regression-module-237b703a0c63)中，我演示了如何使用 PyCaret 通过[PyCaret 回归模块](https://pycaret.readthedocs.io/en/latest/api/regression.html)来预测时间序列数据。如果您还没有阅读过该教程，可以在继续本教程之前阅读[使用 PyCaret 回归模块进行时间序列预测](https://towardsdatascience.com/time-series-forecasting-with-pycaret-regression-module-237b703a0c63)教程，因为本教程构建在上一个教程中涵盖的一些重要概念之上。

### 安装 PyCaret

安装 PyCaret 非常简单，只需几分钟即可完成。我们强烈建议使用虚拟环境，以避免与其他库可能发生的冲突。

PyCaret 的默认安装是一个精简版本的 pycaret，只安装了[这里列出的](https://github.com/pycaret/pycaret/blob/master/requirements.txt)硬依赖项。

```
**# 安装精简版本（默认）
**pip install pycaret

**# 安装完整版本**
pip install pycaret[full]
```

当您安装 pycaret 的完整版本时，还会安装[这里列出的](https://github.com/pycaret/pycaret/blob/master/requirements-optional.txt)所有可选依赖项。

### 👉 PyCaret 回归模块

PyCaret **回归模块**是一个监督机器学习模块，用于估计一个**因变量**（通常称为“结果变量”或“目标”）与一个或多个**自变量**（通常称为“特征”或“预测变量”）之间的关系。

回归的目标是预测连续值，如销售额、数量、温度、客户数量等。PyCaret 中的所有模块都提供许多[预处理](https://www.pycaret.org/preprocessing)功能，通过[setup](https://www.pycaret.org/setup)函数为建模准备数据。它提供了超过 25 种现成的算法和[多个图表](https://www.pycaret.org/plot-model)来分析训练模型的性能。

### 👉 数据集

对于本教程，我将展示多时间序列数据预测的端到端实现，包括训练和预测未来值。

我使用了来自 Kaggle 的[Store Item Demand Forecasting Challenge](https://www.kaggle.com/c/demand-forecasting-kernels-only)数据集。该数据集有 10 个不同的商店，每个商店有 50 个商品，即总共有 500 个每日级别的时间序列数据，为期五年（2013年至2017年）。

![样本数据集](https://cdn-images-1.medium.com/max/2000/1\*VY7MljIxivAiYWSMmAjN6g.png)

### 👉 加载和准备数据
```python
# 读取 csv 文件
import pandas as pd
data = pd.read_csv('train.csv')
data['date'] = pd.to_datetime(data['date'])

# 将 store 和 item 列合并为 time_series
data['store'] = ['store_' + str(i) for i in data['store']]
data['item'] = ['item_' + str(i) for i in data['item']]
data['time_series'] = data[['store', 'item']].apply(lambda x: '_'.join(x), axis=1)
data.drop(['store', 'item'], axis=1, inplace=True)

# 从日期中提取特征
data['month'] = [i.month for i in data['date']]
data['year'] = [i.year for i in data['date']]
data['day_of_week'] = [i.dayofweek for i in data['date']]
data['day_of_year'] = [i.dayofyear for i in data['date']]

data.head()
```

以上代码是读取一个 csv 文件，并对数据进行处理的示例。首先，使用 pandas 库的 `read_csv` 函数读取了名为 `train.csv` 的文件。然后，将 `date` 列转换为日期格式。接下来，将 `store` 列和 `item` 列合并为 `time_series` 列，并删除了原始的 `store` 列和 `item` 列。然后，从日期中提取了一些特征，包括月份、年份、星期几和一年中的第几天。最后，展示了处理后的数据的前几行。

![数据示例行](https://cdn-images-1.medium.com/max/2000/1\*D3PBqLf-PsnGdTn7AjGmWw.png)

```
**# 检查唯一的时间序列**
data['time_series'].nunique()
>>> 500
```

### 👉 可视化时间序列

```
**# 在循环中绘制多个时间序列和移动平均线**

import plotly.express as px

for i in data['time_series'].unique():
    subset = data[data['time_series'] == i]
    subset['moving_average'] = subset['sales'].rolling(30).mean()
    fig = px.line(subset, x="date", y=["sales","moving_average"], title = i, template = 'plotly_dark')
    fig.show()
```

![store\_1\_item\_1 时间序列和30天移动平均线](https://cdn-images-1.medium.com/max/2608/1\*BlE7UEMFRCV6kbEK1oWTEA.png)

![store\_2\_item\_1 时间序列和30天移动平均线](https://cdn-images-1.medium.com/max/2616/1\*Vhc8EP7IbA-\_qdwyuENHaQ.png)

### 👉 开始训练过程

现在我们已经准备好数据，让我们开始训练循环。请注意，在所有函数中 verbose = False，以避免在训练过程中在控制台上打印结果。

下面的代码是围绕我们在数据准备步骤中创建的 time\_series 列的循环。总共有150个时间序列（10个商店 x 50个商品）。

第10行以下代码是为 time\_series 变量过滤数据集。循环内的第一部分是初始化 setup 函数，然后是 compare\_models 来找到最佳模型。第24-26行捕获结果，并将最佳模型的性能指标附加到名为 all\_results 的列表中。代码的最后部分使用 finalize\_model 函数在包括测试集中剩余的5%的整个数据集上重新训练最佳模型，并将整个流程（包括模型）保存为 pickle 文件。

现在我们可以从 all\_results 列表创建一个数据框。它将显示为每个时间序列选择的最佳模型。

```
concat_results = pd.concat(all_results,axis=0)
concat_results.head()
```

![concat\_results 数据示例行](https://cdn-images-1.medium.com/max/2000/1\*qgu9jP86L2gaZHi-TvM0SA.png)

### 训练过程 👇

![训练过程](https://cdn-images-1.medium.com/max/2560/1\*SwI6InjXRuB-TlQwsKZZRQ.gif)

### 👉 使用训练好的模型生成预测

现在我们已经训练好模型，让我们使用它们生成预测，但首先，我们需要为评分（X 变量）创建数据集。

```
**# 创建从2013年到2019年的日期范围**
all_dates = pd.date_range(start='2013-01-01', end = '2019-12-31', freq = 'D')

**# 创建空数据框**
score_df = pd.DataFrame()

**# 向数据集添加列**
score_df['date'] = all_dates
score_df['month'] = [i.month for i in score_df['date']]
score_df['year'] = [i.year for i in score_df['date']]
score_df['day_of_week'] = [i.dayofweek for i in score_df['date']]
score_df['day_of_year'] = [i.dayofyear for i in score_df['date']

score_df.head()
```

![score\_df 数据集示例行](https://cdn-images-1.medium.com/max/2000/1\*Dycm3JuIt0gsGPwPc4\_8fw.png)

现在让我们创建一个循环来加载训练好的流程，并使用 predict\_model 函数生成预测标签。

```
from pycaret.regression import load_model, predict_model

all_score_df = []

for i in tqdm(data['time_series'].unique()):
    l = load_model('trained_models/' + str(i), verbose=False)
    p = predict_model(l, data=score_df)
    p['time_series'] = i
    all_score_df.append(p)

concat_df = pd.concat(all_score_df, axis=0)
concat_df.head()
```

![concat\_df 数据示例行](https://cdn-images-1.medium.com/max/2000/1\*2l9A8jStBQEAULk9fqMObA.png)

现在我们将数据和 concat\_df 连接。

```
final_df = pd.merge(concat_df, data, how = 'left', left_on=['date', 'time_series'], right_on = ['date', 'time_series'])
final_df.head()
```

![final\_df 数据示例行](https://cdn-images-1.medium.com/max/2490/1\*TN9d9WssEBkn-GFSYVgNWg.png)

我们现在可以创建一个循环来查看所有图表。

```
for i in final_df['time_series'].unique()[:5]:
    sub_df = final_df[final_df['time_series'] == i]
    
    import plotly.express as px
    fig = px.line(sub_df, x="date", y=['sales', 'Label'], title=i, template = 'plotly_dark')
    fig.show()
```

![store\_1\_item\_1 实际销售额和预测标签](https://cdn-images-1.medium.com/max/2604/1\*tvjEor2VvHVFgumGgEtfKQ.png)

![store\_2\_item\_1 实际销售额和预测标签](https://cdn-images-1.medium.com/max/2598/1\*99cyEOPNr97uDMLKrnNqig.png)

希望您能欣赏 PyCaret 的易用性和简单性。在不到50行代码和一个小时的实验中，我已经训练了超过10,000个模型（25个估算器 x 500个时间序列），并将500个最佳模型投入生产以生成预测。

### 敬请期待！
下周我将撰写一篇关于使用 [PyCaret 异常检测模块](https://pycaret.readthedocs.io/en/latest/api/anomaly.html) 对时间序列数据进行无监督异常检测的教程。请关注我的 [Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/) 和 [Twitter](https://twitter.com/moezpycaretorg1) 获取更多更新。

使用这个轻量级的 Python 工作流自动化库，您可以实现无限可能。如果您觉得这个工具有用，请不要忘记在我们的 GitHub 仓库上给我们 ⭐️。

想了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

加入我们的 Slack 频道。邀请链接在[这里](https://join.slack.com/t/pycaret/shared_invite/zt-p7aaexnl-EqdTfZ9U~mF0CwNcltffHg)。

### 您可能也对以下内容感兴趣：

- [使用 PyCaret 2.0 在 Power BI 中构建您自己的 AutoML](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d)
- [使用 Docker 在 Azure 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01)
- [在 Google Kubernetes Engine 上部署机器学习模型](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b)
- [在 AWS Fargate 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507)
- [构建并部署您的第一个机器学习 Web 应用](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)
- [使用 AWS Fargate 无服务器部署 PyCaret 和 Streamlit 应用](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2)
- [使用 PyCaret 和 Streamlit 构建并部署机器学习 Web 应用](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)
- [在 GKE 上部署使用 Streamlit 和 PyCaret 构建的机器学习应用](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

### 重要链接

- [文档](https://pycaret.readthedocs.io/en/latest/installation.html)
- [博客](https://medium.com/@moez_62905)
- [GitHub](http://www.github.com/pycaret/pycaret)
- [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret)
- [安装 PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html)
- [笔记本教程](https://pycaret.readthedocs.io/en/latest/tutorials.html)
- [为 PyCaret 做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 想了解特定模块的信息吗？

点击下面的链接查看文档和示例。

- [分类](https://pycaret.readthedocs.io/en/latest/api/classification.html)
- [回归](https://pycaret.readthedocs.io/en/latest/api/regression.html)
- [聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html)
- [异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html)
- [自然语言处理](https://pycaret.readthedocs.io/en/latest/api/nlp.html)
- [关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)