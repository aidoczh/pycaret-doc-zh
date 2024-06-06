# 使用 PyCaret 回归模块进行时间序列预测

### 使用 PyCaret 回归模块进行时间序列预测

![Photo by Lukas Blazek on Unsplash](https://cdn-images-1.medium.com/max/12288/0\*6t7FzC-AdfDlA9LI)

### PyCaret

PyCaret 是一个开源的低代码机器学习库和端到端模型管理工具，使用 Python 构建，用于自动化机器学习工作流程。它因其易用性、简洁性和能够快速高效地构建和部署端到端机器学习原型而广受欢迎。

PyCaret 是一个替代性的低代码库，可以用几行代码替代数百行代码。这使得实验周期指数级加快和高效。

PyCaret **简单且易于使用**。在 PyCaret 中执行的所有操作都按顺序存储在一个完全自动化的**流水线**中，用于部署。无论是填充缺失值、独热编码、转换分类数据、特征工程还是超参数调整，PyCaret 都可以自动化完成。要了解更多关于 PyCaret 的信息，请观看这个 1 分钟的视频。

本教程假设您具有一些关于 PyCaret 的基础知识和经验。如果您以前没有使用过，没问题 - 您可以通过以下教程快速入门：

* [PyCaret 2.2 来了 - 有什么新功能](https://towardsdatascience.com/pycaret-2-2-is-here-whats-new-ad7612ca63b)
* [宣布 PyCaret 2.0](https://towardsdatascience.com/announcing-pycaret-2-0-39c11014540e)
* [关于 PyCaret 你不知道的五件事](https://towardsdatascience.com/5-things-you-dont-know-about-pycaret-528db0436eec)

### 安装 PyCaret

安装 PyCaret 非常简单，只需几分钟即可完成。我们强烈建议使用虚拟环境，以避免与其他库可能发生的冲突。

PyCaret 的默认安装是一个精简版本的 pycaret，只安装了硬依赖项，这些依赖项在[这里](https://github.com/pycaret/pycaret/blob/master/requirements.txt)列出。

```
**# 安装精简版本（默认）
**pip install pycaret

**# 安装完整版本**
pip install pycaret[full]
```

当您安装 pycaret 的完整版本时，还会安装所有可选依赖项，这些依赖项在[这里](https://github.com/pycaret/pycaret/blob/master/requirements-optional.txt)列出。

### 👉 PyCaret 回归模块

PyCaret **回归模块**是一个监督学习模块，用于估计一个**因变量**（通常称为“结果变量”或“目标”）与一个或多个**自变量**（通常称为“特征”或“预测变量”）之间的关系。

回归的目标是预测连续值，如销售额、数量、温度、客户数量等。PyCaret 中的所有模块都提供许多[预处理](https://www.pycaret.org/preprocessing)功能，通过[setup](https://www.pycaret.org/setup)函数为建模准备数据。它提供了超过 25 种现成的算法和[多个图表](https://www.pycaret.org/plot-model)来分析训练模型的性能。

### 👉 使用 PyCaret 回归模块进行时间序列预测

时间序列预测可以大致分为以下几类：

* **经典/统计模型** - 移动平均、指数平滑、ARIMA、SARIMA、TBATS
* **机器学习** - 线性回归、XGBoost、随机森林或任何具有降维方法的机器学习模型
* **深度学习** - RNN、LSTM

本教程重点介绍第二类即_机器学习_。

PyCaret 的回归模块默认设置对于时间序列数据并不理想，因为它涉及一些对于有序数据（如时间序列数据）无效的数据准备步骤。

例如，将数据集拆分为训练集和测试集是随机进行的。对于时间序列数据，这是没有意义的，因为您不希望将最近的日期包含在训练集中，而历史日期是测试集的一部分。

时间序列数据还需要一种不同类型的交叉验证，因为它需要遵守日期的顺序。PyCaret 回归模块在评估模型时默认使用 k 折随机交叉验证。默认的交叉验证设置不适用于时间序列数据。

本教程的下一部分将演示如何轻松更改 PyCaret 回归模块的默认设置，使其适用于时间序列数据。

### 👉 数据集

为了本教程的目的，我使用了美国航空乘客数据集。您可以从[Kaggle](https://www.kaggle.com/chirag19/air-passengers)下载数据集。

```
**# 读取 csv 文件
**import pandas as pd
data = pd.read_csv('AirPassengers.csv')
data['Date'] = pd.to_datetime(data['Date'])
data.head()
```

![示例行](https://cdn-images-1.medium.com/max/2000/1\*9Gn9v-CWD3Eca3V92edtDw.png)
```
**# 创建12个月的移动平均线
**data['MA12'] = data['Passengers'].rolling(12).mean()

**# 绘制数据和移动平均线
**import plotly.express as px
fig = px.line(data, x="Date", y=["Passengers", "MA12"], template = 'plotly_dark')
fig.show()
```

![US Airline Passenger Dataset Time Series Plot with Moving Average = 12](https://cdn-images-1.medium.com/max/2618/1\*vXL7m7exxTid0kGcU9jGUA.png)

由于算法无法直接处理日期，让我们从日期中提取一些简单的特征，如月份和年份，并丢弃原始日期列。

```
**# 从日期中提取月份和年份**
data['Month'] = [i.month for i in data['Date']]
data['Year'] = [i.year for i in data['Date']]

**# 创建一个数字序列
**data['Series'] = np.arange(1,len(data)+1)

**# 删除不必要的列并重新排列
**data.drop(['Date', 'MA12'], axis=1, inplace=True)
data = data[['Series', 'Year', 'Month', 'Passengers']] 

**# 检查数据集的前几行**
data.head()
```

![提取特征后的样本行](https://cdn-images-1.medium.com/max/2000/1\*T4xXVYhrfIyB35EDjl645A.png)

```
**# 将数据拆分为训练集和测试集
**train = data[data['Year'] < 1960]
test = data[data['Year'] >= 1960]

**# 检查形状
**train.shape, test.shape
>>> ((132, 4), (12, 4))
```

在初始化设置之前，我已经手动拆分了数据集。另一种方法是将整个数据集传递给 PyCaret，并让其处理拆分，这种情况下，您将需要在设置函数中传递 data\_split\_shuffle = False 以避免在拆分之前对数据集进行洗牌。

### 👉 **初始化设置**

现在是时候初始化设置函数了，在这里我们将明确传递训练数据、测试数据和交叉验证策略，使用 fold\_strategy 参数。

```
**# 导入回归模块**
from pycaret.regression import *

**# 初始化设置**
s = setup(data = train, test_data = test, target = 'Passengers', fold_strategy = 'timeseries', numeric_features = ['Year', 'Series'], fold = 3, transform_target = True, session_id = 123)
```

### 👉 **训练和评估所有模型**

```
best = compare_models(sort = 'MAE')
```

![compare\_models 的结果](https://cdn-images-1.medium.com/max/2000/1\*Mnqplw1KbJYm9fxyH-46Tg.png)

基于交叉验证的 MAE，最佳模型是\*\*最小角回归\*\*（MAE: 22.3）。让我们在测试集上检查得分。

```
prediction_holdout = predict_model(best);
```

![predict\_model(best) 函数的结果](https://cdn-images-1.medium.com/max/2000/1\*Us888u-jaVQzasN8Kn3z6A.png)

测试集上的 MAE 比交叉验证的 MAE 高出 12%。虽然不太理想，但我们会继续努力。让我们绘制实际线和预测线以可视化拟合情况。

```
**# 在原始数据集上生成预测**
predictions = predict_model(best, data=data)

**# 在数据集中添加日期列**
predictions['Date'] = pd.date_range(start='1949-01-01', end = '1960-12-01', freq = 'MS')

**# 线图**
fig = px.line(predictions, x='Date', y=["Passengers", "Label"], template = 'plotly_dark')

**# 添加垂直矩形以分隔测试集**
fig.add_vrect(x0="1960-01-01", x1="1960-12-01", fillcolor="grey", opacity=0.25, line_width=0)

fig.show()
```

![实际和预测的美国航空乘客数（1949–1960）](https://cdn-images-1.medium.com/max/2624/1\*BlfRbXuxwcgvs-zrpK8C0Q.png)

灰色背景是测试期间（即 1960 年）。现在让我们完成模型，即在整个数据集上训练最佳模型，即_最小角回归_（这次包括测试集）。

```
final_best = finalize_model(best)
```

### 👉 创建未来评分数据集

现在我们已经在整个数据集（1949年至1960年）上训练了我们的模型，让我们预测未来五年，直到1964年。要使用最终模型生成未来预测，我们首先需要创建一个包含未来日期的 Month、Year、Series 列的数据集。

```
future_dates = pd.date_range(start = '1961-01-01', end = '1965-01-01', freq = 'MS')

future_df = pd.DataFrame()

future_df['Month'] = [i.month for i in future_dates]
future_df['Year'] = [i.year for i in future_dates]    
future_df['Series'] = np.arange(145,(145+len(future_dates)))

future_df.head()
```

![future\_df 的样本行](https://cdn-images-1.medium.com/max/2000/1\*KD4G6VVmbuq-w\_6088Vl8Q.png)

现在，让我们使用 future\_df 进行评分和生成预测。

```
predictions_future = predict_model(final_best, data=future_df)
predictions_future.head()
```

![predictions\_future 的样本行](https://cdn-images-1.medium.com/max/2000/1\*5mqx2Qi2En2VPq9zNZG87g.png)

让我们绘制它。

```
concat_df = pd.concat([data,predictions_future], axis=0)
concat_df_i = pd.date_range(start='1949-01-01', end = '1965-01-01', freq = 'MS')
concat_df.set_index(concat_df_i, inplace=True)

fig = px.line(concat_df, x=concat_df.index, y=["Passengers", "Label"], template = 'plotly_dark')
fig.show()
```

![实际（1949–1960）和预测（1961–1964）的美国航空乘客数](https://cdn-images-1.medium.com/max/2614/1\*IjglwJEeU2hZMsjxM8yPbg.png)

是不是很简单？
使用这个轻量级的 Python 工作流自动化库，您可以实现无限可能。如果您觉得这个工具很有用，请不要忘记在我们的 GitHub 仓库上给我们 ⭐️。

想了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

加入我们的 Slack 频道。邀请链接在 [这里](https://join.slack.com/t/pycaret/shared_invite/zt-p7aaexnl-EqdTfZ9U~mF0CwNcltffHg)。

### 您可能还感兴趣：

[使用 PyCaret 2.0 在 Power BI 中构建您自己的 AutoML](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d) [使用 Docker 在 Azure 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01) [在 Google Kubernetes Engine 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b) [在 AWS Fargate 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507) [构建和部署您的第一个机器学习 Web 应用程序](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99) [使用 AWS Fargate 无服务器部署 PyCaret 和 Streamlit 应用程序](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2) [使用 PyCaret 和 Streamlit 构建和部署机器学习 Web 应用程序](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104) [在 GKE 上部署使用 Streamlit 和 PyCaret 构建的机器学习应用程序](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

### 重要链接

[文档](https://pycaret.readthedocs.io/en/latest/installation.html) [博客](https://medium.com/@moez_62905) [GitHub](http://www.github.com/pycaret/pycaret) [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装 PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html) [笔记本教程](https://pycaret.readthedocs.io/en/latest/tutorials.html) [为 PyCaret 做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 想了解特定模块吗？

点击下面的链接查看文档和示例。

[分类](https://pycaret.readthedocs.io/en/latest/api/classification.html) [回归](https://pycaret.readthedocs.io/en/latest/api/regression.html) [聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html) [异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html) [自然语言处理](https://pycaret.readthedocs.io/en/latest/api/nlp.html) [关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)