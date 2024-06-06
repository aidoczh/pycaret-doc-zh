# 使用 PyCaret 正确预测潜在客户得分

### 使用 PyCaret 正确预测潜在客户得分

#### 一步一步指南，教你如何利用 PyCaret 构建潜在客户得分模型，并提高营销活动的投资回报率。

![使用 PyCaret 正确预测潜在客户转化 — 图片由作者提供](https://cdn-images-1.medium.com/max/2674/1\*UKajo1\_fRw6h5UpW7lQhWQ.png)

### **介绍**

如今，潜在客户是许多企业的推动力。随着订阅制业务模式的发展，尤其是在初创企业领域，将潜在客户转化为付费客户对企业的生存至关重要。简单来说，“潜在客户”代表对购买你的产品/服务感兴趣的潜在客户。

通常，当你通过第三方服务获取潜在客户，或者通过自己运营营销活动获得潜在客户时，通常会包括以下信息：

* 潜在客户的姓名和联系方式
* 潜在客户属性（人口统计、社交、客户偏好）
* 来源（Facebook 广告、网站落地页、第三方等）
* 在网站上花费的时间、点击次数等
* 推荐详情等。

### 一览潜在客户管理流程

![潜在客户管理流程一览 — 图片由作者提供](https://cdn-images-1.medium.com/max/2236/1\*IvCN08YMCv2gh6Apt80SiA.png)

市场营销和销售部门在潜在客户管理上投入了大量时间、金钱和精力，这个概念包括潜在客户生成、资格认定和货币化三个关键阶段。

### 👉潜在客户生成

潜在客户生成是引导客户对你的业务产品或服务产生兴趣或询问的开始。潜在客户的创建是为了将这种兴趣或询问转化为销售。互联网上有无数第三方公司承诺生成最佳潜在客户。然而，你也可以通过自己运营营销活动来实现。生成潜在客户的方法通常属于广告范畴，但也可能包括非付费来源，如有机搜索引擎结果或现有客户的推荐。

### 👉 **潜在客户资格认定**

潜在客户资格认定是指确定哪些潜在客户最有可能进行实际购买的过程。这是销售漏斗的一个组成部分，通常吸引了许多潜在客户，但只转化了其中的一小部分。简单来说，潜在客户资格认定意味着**评估和优先考虑潜在客户，以确定转化的可能性**，这样你的市场营销和销售部门就可以追踪优先考虑的潜在客户，而不是所有可能有数千个的潜在客户。

### 👉**潜在客户转化**

潜在客户转化是最终将合格的潜在客户转化为付费客户的阶段。它涵盖了所有刺激购买产品或服务的营销实践，并推动潜在客户做出购买决定的过程。这是一个货币化或结束阶段，其结果通常定义了整个营销活动的成功。

### 👉 **潜在客户得分真正意味着什么？**

想象一下，你的团队有许多潜在客户（潜在客户），但没有足够的资源来追踪所有这些潜在客户。无论你是一个以产品为主导的企业，拥有大量免费用户，拥有众多潜在客户的出色入口漏斗，或者只是一个出色的门到门销售团队，最终，**你需要优先考虑销售团队的时间，并为他们提供“最佳”潜在客户。**

> ## 问题是，你如何做到这一点，以便**最大化你的获胜率**？

做到这一点的一种简单方法是分析历史数据，并查看导致潜在客户转化为销售的属性。例如，历史上可能有一个特定的国家、城市或邮政编码，在那里潜在客户转化为销售的概率达到90%。同样，你的数据也可以告诉你，在你的网站上花费了超过20分钟的客户大多数时间会转化为销售。使用这些业务规则，你可以创建一个**潜在客户得分系统**，根据这些业务规则为每个潜在客户附加分数（分数越高越好）。

这种方法的问题在于，你只能用业务规则覆盖到一定程度。随着业务的扩张，你可以收集的数据类型和种类将呈指数增长。在某个时候，手动基于规则的系统将不足以继续增加价值。

> ## **机器学习登场**

你可以从机器学习的角度来处理**潜在客户得分系统**，在这里，你可以根据客户属性、潜在客户来源、推荐和其他可用细节训练机器学习模型，目标将是**潜在客户转化（是或否）**。

如何获得目标变量？嗯，大多数客户关系管理系统，如 Salesforce、Zoho 或 Microsoft Dynamics，都可以跟踪个体潜在客户及其状态。潜在客户的状态将帮助你创建目标变量。
> 一个需要注意的问题是，您必须确保在训练数据集中不泄露任何信息。例如，您的CRM系统可能存储有关转介费用的信息，想象一下如果您在训练数据中使用了这些信息，那么从技术上讲这是一种泄露，因为您只会在转化后支付转介费，而这是您事后才知道的。

![预测性线索评分工作流程-作者许可的图像](https://cdn-images-1.medium.com/max/2000/1\*roT\_nhFL9cdR5Dg0QfLR5A.png)

### 让我们从实际示例开始 👇

### 什么是 PyCaret？

[PyCaret](https://www.pycaret.org/) 是一个开源的、低代码的 Python 机器学习库和端到端模型管理工具，用于自动化机器学习工作流程。使用 PyCaret，您可以高效地构建和部署端到端的机器学习流水线。要了解更多关于 PyCaret 的信息，请查看他们的 [GitHub](https://www.github.com/pycaret/pycaret)。

![PyCaret 的特点-作者的图像](https://cdn-images-1.medium.com/max/2084/0\*FdaGo2BLH96-e-4\_.png)

### 安装 PyCaret

```
# 安装 PyCaret
pip install pycaret
```

### 👉 数据集

在本教程中，我使用了来自 Kaggle 的一个 [Lead Conversion](https://www.kaggle.com/ashydv/leads-dataset) 数据集。该数据集包含超过 9000 个潜在客户的特征，例如潜在客户来源、潜在客户的来源、在网站上花费的总时间、在网站上的总访问次数、人口统计信息以及目标列 Converted（表示转化为 1，未转化为 0）。

```
# 导入库
import pandas as pd
import numpy as np

# 读取 csv 数据
data = pd.read_csv('Leads.csv')
data.head()
```

![示例数据集-作者的图像](https://cdn-images-1.medium.com/max/2076/1\*SuUA\_\_cJ\_KdbQJzDrT9U0A.png)

### 👉 探索性数据分析

```
# 检查数据信息
data.info()
```

![data.info() - 作者的图像](https://cdn-images-1.medium.com/max/2000/1\*7s\_MXGiVe7\_4Bxf7fQ0SjA.png)

请注意，有几列存在许多缺失值。处理缺失值有几种方法。我将让 PyCaret 自动处理缺失值。如果您想了解有关在 PyCaret 中填充缺失值的不同方法的更多信息，请查看此 [文档链接](https://pycaret.org/missing-values/)。

直观地说，在网站上花费的时间和活动得分以及潜在客户来源是关于潜在客户转化的非常重要的信息。让我们通过可视化来探索它们之间的关系：

![按照在网站上花费的总时间、活动得分和来源进行的潜在客户转化-作者的图像](https://cdn-images-1.medium.com/max/2000/1\*8TqGGUwZbaNpu4mDXVming.png)

请注意，来自“添加表单”的潜在客户很可能无论在网站上花费的时间还是得分如何都会转化为销售。通过 API 或网站的落地页产生的潜在客户则有不同的情况。得分较高且在网站上花费的时间较长的潜在客户更有可能转化为最终销售。

### 👉 数据准备

对于 PyCaret 中的所有模块，setup 是在 PyCaret 中执行任何机器学习实验的第一个且唯一必需的步骤。此函数负责在训练模型之前进行所有必要的数据准备工作。除了执行一些基本的默认处理任务外，PyCaret 还提供了广泛的预处理功能。要了解 PyCaret 中所有预处理功能的更多信息，您可以查看此 [链接](https://pycaret.org/preprocessing/)。

```
# 初始化 setup
from pycaret.classification import *
s = setup(data, target='Converted', ignore_features=['Prospect ID', 'Lead Number'])
```

![pycaret.classification 中的 setup 函数-作者的图像（图像被截断）](https://cdn-images-1.medium.com/max/2008/1\*oVPHiRph-nzLSB04GIcryA.png)

在 PyCaret 中初始化 setup 函数后，它会自动对数据集进行分析，并推断出所有输入变量的数据类型。如果一切都推断正确，您可以按 Enter 键继续。您还可以在 setup 中使用 numeric\_features 和 categorical\_features 参数来强制/覆盖数据类型。

还要注意，我在 setup 函数中传递了 ignore\_features = \['Prospect ID', 'Lead Number']，以便在训练模型时不考虑它们。这样做的好处是 PyCaret 不会从数据集中删除该列，它只会在模型训练时在后台忽略它。因此，在生成最终预测时，您无需担心自己手动连接 ID。

![setup 的输出-被截断以显示-作者的图像（图像被截断）](https://cdn-images-1.medium.com/max/2000/1\*U-BErPLaoUkIePiO\_6pQLQ.png)

### 👉 模型训练与选择
现在数据准备工作已经完成，让我们通过使用compare_models功能开始训练过程。该函数训练模型库中的所有算法，并使用交叉验证评估多个性能指标。

```
**# 比较所有模型**
best_model = compare_models(sort='AUC')
```

![compare_models的输出 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*2nhlUOcws5Fp3ahjZXXuGA.png)

基于**AUC**，最佳模型是**Catboost分类器**，平均10折交叉验证AUC为**0.9864**。

```
**# 打印best_model参数**
print(best_model.get_all_params())

**# 除了catboost，你可以这样做：**
print(best_model)
```

![Catboost超参数 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*bJhAnG55xtkNzIgGZNlciQ.png)

### 👉 模型分析

### **AUC-ROC曲线**

AUC-ROC曲线是分类问题的性能测量，用于不同阈值设置。ROC是概率曲线，AUC代表可分离性的程度或度量。它表明模型能够区分类别的能力。AUC值越高，模型在预测正类和负类方面的能力就越好。虽然评估和比较不同模型的性能非常有帮助，但将这个指标转化为业务价值并不容易。

```
**# AUC曲线**
plot_model(best_model, plot = 'auc')
```

![最佳模型的AUC曲线 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*r1FSRw2KrRS5U8k-xlKtRw.png)

### **SHAP值**

与AUC-ROC不同，shap值不会告诉您任何关于模型性能的信息，而是解释具有某个特定值的特征相对于如果该特征采用某个基线值时的预测的影响。在下面的图表中，y轴（左侧）显示了模型的所有重要特征，x轴是相关特征的Shapley值，颜色刻度（右侧）是特征的实际值。图中每个特征上的每个点代表一个客户线索（来自测试集） — 重叠在一起。

Shap值越高（x轴），正类的可能性就越高（在这种情况下是转化）。因此，从顶部开始阅读，我将解释这些线索被标记为“阅读邮件后会回复”的shap值很高，与基线相比有更高的转化可能性。相反，如果您看到“响铃”标签，情况正好相反，shap值位于基线值的左侧，即负的shap值，这意味着该特征对转化起着反作用。要更详细地了解shap值，请参阅此[链接](https://github.com/slundberg/shap)。

```
**# Shapley值**
interpret_model(best_model)
```

![最佳模型的Shapley特征重要性图 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*ww3sMXldXtxko2wKxQyBqg.png)

### 特征重要性图

特征重要性图是解释模型结果的另一种方式。虽然Shap值仅适用于复杂的基于树的模型，但特征重要性图更常见，可用于不同类型的模型族。与Shap值不同，特征重要性并不告诉我们特征对特定类别的影响，它只告诉我们特征是否重要。

```
**# 特征重要性**
plot_model(best_model, plot = 'feature')
```

![最佳模型的特征重要性图 — 图片由作者提供](https://cdn-images-1.medium.com/max/2140/1\*2VrWvA8YG5OBcSqCpUt0eQ.png)

### 混淆矩阵

混淆矩阵是查看模型性能的另一种方式。在所有可能的工具中，这可能是最简单的之一。它基本上将预测与实际标签进行比较，并将它们分为四个象限：

* 真正例（**预测：**转化，**实际：**转化）
* 真负例（**预测：**未转化，**实际：**未转化）
* 假正例（**预测：**转化，**实际：**未转化）
* 假负例（**预测：**未转化，**实际：**转化）

如果将这四个象限相加，将等于测试集中的客户线索数（1667 + 70 + 84 + 952 = 2,773）。

* 952位客户（右下象限）是真正例，这些是模型预测会转化并且确实转化的线索；
* 70个线索是假正例（_这是您可能会浪费精力的地方_）；
* 84个线索是假负例（_错失的机会_）；以及
* 1,667个线索是真负例（_没有影响_）。

    **# 混淆矩阵**plot_model(best_model, plot = 'confusion_matrix')

![最佳模型的混淆矩阵 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*LYCxb-mEI2PB\_OAitEURZQ.png)
到目前为止，我们已经为建模准备好了数据（当您运行设置函数时，PyCaret会自动执行此操作），训练了多个模型以选择基于AUC的最佳模型，通过不同的图表分析了性能，如AUC-ROC曲线、特征重要性、混淆矩阵和Shapley值。然而，我们还没有回答最重要的问题：

> ## **这个模型的商业价值是什么，为什么我们应该使用这个模型？**

为了将商业价值与该模型联系起来，让我们做一些假设：

- 将潜在客户转化为销售将为第一年带来120美元的收入
- 追踪优先级潜在客户（由模型预测）所花费的时间和精力为15美元
- 模型错过的机会（假阴性）会产生负面的120美元机会成本（您可以选择是否添加此成本，因为这不是实际成本，而是机会成本，完全取决于用例）

如果您在这里稍微做一些数学运算，您将得出**88,830美元的利润**。具体如下：

![$ Impact of Model over 2,773 Customers — Image by Author](https://cdn-images-1.medium.com/max/2000/1\*2IzdyZeAL1HybxYwLYKEXw.png)

这可能是一个不错的模型，但它并不是一个商业智能型的模型，因为我们还没有考虑成本/利润的假设。默认情况下，任何机器学习算法都会优化传统指标，如AUC。为了实现商业目标，我们必须使用业务指标来训练、选择和优化模型。

### 👉 在PyCaret中添加自定义指标

感谢PyCaret，使用add\_metric函数实现这一点非常容易。

```
**# 创建自定义函数
**def calculate_profit(y, y_pred):
    tp = np.where((y_pred==1) & (y==1), (120-15), 0)
    fp = np.where((y_pred==1) & (y==0), -15, 0)
    fn = np.where((y_pred==0) & (y==1), -120, 0)
    return np.sum([tp,fp,fn])

**# 将指标添加到PyCaret
**add_metric('profit', 'Profit', calculate_profit)
```

现在让我们再次运行compare\_models：

```
**# 比较所有模型**
best_model = compare_models(sort='Profit')
```

![Output from compare\_models — Image by Author](https://cdn-images-1.medium.com/max/2000/1\*nps6sq9lYaM4QkZkvXWRfg.png)

请注意，这次添加了一个新列**Profit**，而\*\*Catboost分类器\*\*不再是基于Profit的最佳模型。现在是\*\*Light Gradient Boosting Machine。\*\*尽管在这个示例中差异不大，但根据您的数据和假设，有时这可能会相当于数百万美元。

```
**# 混淆矩阵**
plot_model(best_model, plot = 'confusion_matrix')
```

![Confusion Matrix for LightGBM — Image by Author](https://cdn-images-1.medium.com/max/2000/1\*Lcipv42jm4ahagD5PX96mg.png)

客户总数仍然是相同的（测试集中的2,773名客户），变化的是模型如何在假阳性和假阴性上出现错误。让我们根据相同的假设（如上所述）为其赋予一些美元价值：

![$ Impact of Model over 2,773 Customers — Image by Author](https://cdn-images-1.medium.com/max/2000/1\*sp-3GdlsAyQZBCFxxj3ThA.png)

利润现在是89,925美元，而在使用Catboost分类器时为88,830美元。这是一个1.2%的提升，根据假阳性和假阴性的数量和成本，这可能会转化为数百万美元。除此之外，您还可以做一些其他事情，例如通过显式优化**Profit**而不是AUC、准确率、召回率、精确率或任何其他传统指标来调整最佳模型的超参数。

### 如何使用模型生成潜在客户评分？

好了，您现在可能会问，我们已经选择了最佳模型，如何将该模型应用于新的潜在客户以生成评分？这并不难。

```
**# 创建数据副本
**data_new = data.copy()
data_new.drop('Converted', axis=1, inplace=True)

**# 使用predict_model生成标签
**predict_model(best_model, data=data_new, raw_score=True)
```

![Predictions generated using the best \_model — Image by Author](https://cdn-images-1.medium.com/max/2258/1\*HZO6S5ObCgEDhTcMIOcmuQ.png)

请注意，数据集中添加了最后三列 — 标签（1 = 转化，0 = 未转化），Score\_0和Score\_1是介于0到1之间的每个类别的概率。例如，第一个观察的Score\_0为0.9973，表示未转化的概率为99.7%。

我是一名常规博主，我主要写有关PyCaret及其在现实世界中用例的文章，如果您想自动收到通知，可以关注我的[Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/)和[Twitter](https://twitter.com/moezpycaretorg1)。

![PyCaret — Image by Author](https://cdn-images-1.medium.com/max/2412/0\*PLdJpNCTXdttEn8W.png)

![PyCaret — Image by Author](https://cdn-images-1.medium.com/max/2402/0\*IvqhUYDstXqz55eF.png)
使用这个轻量级的 Python 工作流自动化库，您可以实现无限可能。如果您觉得这个工具有用，请不要忘记在我们的 GitHub 仓库上给我们 ⭐️。

想了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

加入我们的 Slack 频道。邀请链接在 [这里](https://join.slack.com/t/pycaret/shared_invite/zt-p7aaexnl-EqdTfZ9U~mF0CwNcltffHg)。

### 重要链接

[文档](https://pycaret.readthedocs.io/en/latest/installation.html) [博客](https://medium.com/@moez_62905) [GitHub](http://www.github.com/pycaret/pycaret) [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装 PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html) [笔记本教程](https://pycaret.readthedocs.io/en/latest/tutorials.html) [为 PyCaret 做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 更多与 PyCaret 相关的教程:

[**使用 PyCaret 正确预测客户流失** towardsdatascience.com](https://towardsdatascience.com/predict-customer-churn-the-right-way-using-pycaret-8ba6541608ac) [**使用 PyCaret 构建，使用 FastAPI 部署** 一个逐步，适合初学者的教程，介绍如何使用 PyCaret 构建端到端的机器学习流水线... towardsdatascience.com](https://towardsdatascience.com/build-with-pycaret-deploy-with-fastapi-333c710dc786) [**使用 PyCaret 进行时间序列异常检测** 一个关于使用 PyCaret 对时间序列数据进行无监督异常检测的逐步教程 towardsdatascience.com](https://towardsdatascience.com/time-series-anomaly-detection-with-pycaret-706a6e2b2427) [**通过 PyCaret 和 Gradio 加速您的机器学习实验** 一个逐步教程，快速开发和交互式地与机器学习流水线进行交互 towardsdatascience.com](https://towardsdatascience.com/supercharge-your-machine-learning-experiments-with-pycaret-and-gradio-5932c61f80d9) [**使用 PyCaret 进行多时间序列预测** 一个关于使用 PyCaret 预测多时间序列的逐步教程 towardsdatascience.com](https://towardsdatascience.com/multiple-time-series-forecasting-with-pycaret-bc0a779a22fe)