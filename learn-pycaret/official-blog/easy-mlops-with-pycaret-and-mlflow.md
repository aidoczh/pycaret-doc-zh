# 使用 PyCaret 和 MLflow 实现简单的 MLOps

### 使用 PyCaret + MLflow 实现简单的 MLOps

#### 一份适合初学者的、分步教程，教你如何使用 PyCaret 在机器学习实验中集成 MLOps

![Photo by Adi Goldstein on Unsplash](https://cdn-images-1.medium.com/max/7832/0\*WSGn8A3YB42ALgSg)

### PyCaret

PyCaret 是一个开源的、低代码的机器学习库和端到端模型管理工具，使用 Python 编写，用于自动化机器学习工作流。它以易用性、简洁性和快速高效地构建和部署端到端机器学习原型的能力而闻名。

PyCaret 是一个替代低代码库，可以用少量代码替代数百行代码。这使得实验周期指数级加快和高效。

![PyCaret - 一个开源的、低代码的 Python 机器学习库](https://cdn-images-1.medium.com/max/2066/0\*IOqb01w3mfxSYsRi.png)

要了解更多关于 PyCaret 的信息，你可以查看他们的 [GitHub](https://www.github.com/pycaret/pycaret)。

### MLflow

MLflow 是一个开源平台，用于管理机器学习的生命周期，包括实验、可重现性、部署和中央模型注册。MLflow 目前提供四个组件：

![MLflow 是一个开源平台，用于管理机器学习的生命周期](https://cdn-images-1.medium.com/max/2852/1\*EQ48xHBYlnqBKoas54URpQ.png)

要了解更多关于 MLflow 的信息，你可以查看 [GitHub](https://github.com/mlflow/mlflow)。

### 安装 PyCaret

安装 PyCaret 非常简单，只需几分钟即可完成。我们强烈建议使用虚拟环境，以避免与其他库可能发生的冲突。

PyCaret 的默认安装是一个精简版本，只安装硬依赖项 [在这里列出](https://github.com/pycaret/pycaret/blob/master/requirements.txt)。

```
**# 安装精简版本（默认）
**pip install pycaret

**# 安装完整版本**
pip install pycaret[full]
```

当你安装完整版本的 pycaret 时，所有可选依赖项 [在这里列出](https://github.com/pycaret/pycaret/blob/master/requirements-optional.txt) 也会被安装。MLflow 是 PyCaret 的依赖项之一，因此不需要单独安装。

### 👉 让我们开始吧

在我讲解 MLOps 之前，让我们简要介绍一下机器学习生命周期的高级流程：

![机器学习生命周期 - 图片由作者提供（从左到右阅读）](https://cdn-images-1.medium.com/max/2580/1\*qYCj8HXZ0CUD4DROjU9MAg.png)

* \*\*业务问题 - \*\*这是机器学习工作流的第一步。根据用例和问题的复杂性，这可能需要几天到几周的时间才能完成。在这个阶段，数据科学家与主题专家（SME）会面，了解问题，与关键利益相关者进行访谈，收集信息，并设定项目的整体期望。
* \*\*数据获取和ETL - \*\*一旦完成问题理解，就可以使用在访谈中获得的信息从企业数据库中获取数据。
* \*\*探索性数据分析（EDA） - \*\*建模尚未开始。EDA 是你分析原始数据的地方。你的目标是探索数据并评估数据的质量、缺失值、特征分布、相关性等。
* \*\*数据准备 - \*\*现在是准备数据模型训练的时候了。这包括将数据分为训练集和测试集，填充缺失值，独热编码，目标编码，特征工程，特征选择等。
* \*\*模型训练和选择 - \*\*这是每个人都兴奋的一步。这涉及训练一系列模型，调整超参数，模型集成，评估性能指标，模型分析（如AUC、混淆矩阵、残差等），最后选择一个最佳模型，用于在业务中部署使用。
* \*\*部署和监控 - \*\*这是最后一步，主要涉及 MLOps。这包括将最终模型打包、创建 Docker 镜像、编写评分脚本，然后使其协同工作，并最终将其发布为可用于获取通过管道传入的新数据的预测的 API。

以前的做法非常繁琐、冗长，并且需要很多技术知识，我可能无法在一个教程中涵盖所有内容。然而，在本教程中，我将使用 PyCaret 来演示数据科学家如何以非常高效的方式完成所有这些工作。在我们进入实际部分之前，让我们再多谈一些关于 MLOps 的内容。

### 👉 **什么是 MLOps？**
MLOps是一门工程学科，旨在将由数据科学家通常执行的机器学习开发（实验（模型训练、超参数调整、模型集成、模型选择等））与ML工程和运营相结合，以标准化和简化机器学习模型在生产中的持续交付。

如果你是一个绝对的初学者，很可能对我所说的一无所知。别担心。让我给你一个简单的、非技术性的定义：

> *MLOps是一系列技术工程和运营任务，使你的机器学习模型能够被组织中的其他用户和应用程序使用。基本上，这是一种通过这种方式，你的工作，即*机器学习模型_被发布在线，以便其他人可以使用它们并满足一些业务目标。_

这是MLOps的一个非常简化的定义。实际上，它涉及的工作和好处比这更多，但如果你对这一切都是新手，这是一个很好的开始。

现在让我们按照上面图表所示的相同工作流程来进行一个实际演示，确保你已经安装了pycaret。

### 👉 业务问题

在本教程中，我将使用达顿商学院发表在[哈佛商业评论](https://hbsp.harvard.edu/product/UV0869-PDF-ENG)上的一个非常流行的案例研究。该案例涉及到两个将来要结婚的人的故事。一个名叫*格雷格*的男士想要买一枚戒指向一个名叫_莎拉_的女士求婚。问题是要找到莎拉会喜欢的戒指，但在他的好友建议后，格雷格决定购买一颗钻石，让莎拉自己选择。然后，格雷格收集了6000颗钻石的数据，包括它们的价格和属性，如切割、颜色、形状等。

### 👉 数据

在本教程中，我将使用达顿商学院发表在[哈佛商业评论](https://hbsp.harvard.edu/product/UV0869-PDF-ENG)上的一个非常流行的案例研究中的数据集。本教程的目标是根据其属性（如克拉重量、切割、颜色等）来预测钻石的价格。你可以从[PyCaret的数据集](https://github.com/pycaret/pycaret/tree/master/datasets)中下载数据集。

```
**# 从pycaret加载数据集
**from pycaret.datasets import get_data
data = get_data('diamond')
```

![数据样本行](https://cdn-images-1.medium.com/max/2000/0\*rDRvbnmVe7vDGPVM.png)

### 👉 探索性数据分析

让我们进行一些快速的可视化，评估独立特征（重量、切割、颜色、清晰度等）与目标变量即价格之间的关系。

```
**# 绘制克拉重量和价格的散点图**
import plotly.express as px
fig = px.scatter(x=data['Carat Weight'], y=data['Price'], 
                 facet_col = data['Cut'], opacity = 0.25, template = 'plotly_dark', trendline='ols',
                 trendline_color_override = 'red', title = '莎拉得到一颗钻石 - 一个案例研究')
fig.show()
```

![莎拉得到一颗钻石案例研究](https://cdn-images-1.medium.com/max/2328/0\*Lo6mo1OUPjb-Cenm.png)

让我们检查目标变量的分布。

```
**# 绘制直方图**
fig = px.histogram(data, x=["Price"], template = 'plotly_dark', title = '价格直方图')
fig.show()
```

![](https://cdn-images-1.medium.com/max/2316/0\*VjW1-hOpd7hVBtIj.png)

注意到价格的分布是右偏的，我们可以快速检查对数转换是否可以使价格近似正态，从而为假设正态性的算法提供一些机会。

```
import numpy as np

**# 创建数据的副本**
data_copy = data.copy()

**# 创建一个新特征Log_Price**
data_copy['Log_Price'] = np.log(data['Price'])

**# 绘制直方图**
fig = px.histogram(data_copy, x=["Log_Price"], title = '对数价格直方图', template = 'plotly_dark')
fig.show()
```

![](https://cdn-images-1.medium.com/max/2322/0\*KfvEU2c6f8LdjUPS.png)

这证实了我们的假设。这种转换将帮助我们摆脱偏斜，并使目标变量近似正态。基于此，我们将在训练模型之前对价格变量进行转换。

### 👉 数据准备

在PyCaret的所有模块中，设置是使用PyCaret进行任何机器学习实验的第一个且唯一强制性步骤。此函数负责所有训练模型之前所需的数据准备工作。除了执行一些基本的默认处理任务外，PyCaret还提供了各种预处理功能。要了解有关PyCaret中所有预处理功能的更多信息，你可以查看这个[链接](https://pycaret.org/preprocessing/)。

```
**# 初始化设置**
from pycaret.regression import *
s = setup(data, target = 'Price', transform_target = True, log_experiment = True, experiment_name = 'diamond')
```

![pycaret.regression模块中的设置函数](https://cdn-images-1.medium.com/max/2736/0\*dCiKXVXfpXyYQiYd.png)
当您在 PyCaret 中初始化设置函数时，它会对数据集进行分析，并推断所有输入特征的数据类型。如果所有数据类型都被正确推断，您可以按 Enter 键继续。

请注意：

* 我已经传递了 log\_experiment = True 和 experiment\_name = 'diamond' ，这将告诉 PyCaret 在您通过建模阶段时自动记录所有指标、超参数和模型工件的背后信息。这得益于与 [MLflow](https://www.mlflow.org) 的集成。
* 另外，我在设置中使用了 transform\_target = True。PyCaret 将在幕后使用 Box-Cox 转换来转换价格变量。它会以与对数转换相似的方式影响数据的分布（_在技术上有所不同_）。如果您想了解更多关于 Box-Cox 转换的信息，可以参考这个 [链接](https://onlinestatbook.com/2/transformations/box-cox.html)。

![设置输出 — 为显示而截断](https://cdn-images-1.medium.com/max/2000/0\*b5w1YKkwK2G9n\_YA.png)

### 👉 模型训练与选择

现在数据已经准备好进行建模，让我们通过使用 compare\_models 函数开始训练过程。它将训练模型库中的所有可用算法，并使用 k 折交叉验证评估多个性能指标。

```
**# 比较所有模型**
best = compare_models()
```

![compare\_models 的输出](https://cdn-images-1.medium.com/max/2000/0\*FZAGMj-lU-C\_kxRl.png)

```
**# 检查已训练模型的残差**
plot_model(best, plot = 'residuals_interactive')
```

![最佳模型的残差和 QQ 图](https://cdn-images-1.medium.com/max/2590/0\*yOzbuZjSXY4s2v4Z.png)

```
**# 检查特征重要性**
plot_model(best, plot = 'feature')
```

![](https://cdn-images-1.medium.com/max/2068/0\*m8k8VaglnYOkNx5x.png)

#### 完成并保存管道

现在让我们最终确定最佳模型，即在整个数据集（包括测试集）上训练最佳模型，然后将管道保存为 pickle 文件。

```
**# 完成模型**
final_best = finalize_model(best)

**# 将模型保存到磁盘
**save_model(final_best, 'diamond-pipeline')
```

save\_model 函数将整个管道（包括模型）保存为 pickle 文件在您的本地磁盘上。默认情况下，它将在与您的 Notebook 或脚本相同的文件夹中保存文件，但如果您愿意，也可以传递完整路径：

```
save_model(final_best, 'c:/users/moez/models/diamond-pipeline'
```

### 👉 部署

请记住，在设置函数中我们传递了 log\_experiment = True 以及 experiment\_name = 'diamond'。让我们看看 PyCaret 在幕后借助 MLflow 做了什么神奇的事情。要查看神奇之处，让我们启动 MLflow 服务器：

```
**# 在笔记本内（注意！符号在前面）
**!mlflow ui

**# 在相同文件夹的命令行中
**mlflow ui
```

现在打开浏览器，输入“localhost:5000”。它将打开一个类似这样的用户界面：

![https://localhost:5000](https://cdn-images-1.medium.com/max/3836/1\*yZ4zThh0tnY0uW8SsLCpdw.png)

上表中的每个条目代表一个训练运行，产生了一个经过训练的管道和一堆元数据，如运行的日期时间、性能指标、模型超参数、标签等。让我们点击其中一个模型：

![第一部分 — CatBoost 回归器](https://cdn-images-1.medium.com/max/3776/1\*TQEApDxCxDIoN6GWwvbBZw.png)

![第二部分 — CatBoost 回归器（续）](https://cdn-images-1.medium.com/max/2438/1\*RVC18B9Zk8rJkLp28jHkjA.png)

![第二部分 — CatBoost 回归器（续）](https://cdn-images-1.medium.com/max/3392/1\*1rsOUsPzyY3O0Djao2KlzQ.png)

请注意，您有一个 logged\_model 的地址路径。这是经过 Catboost 回归器训练的管道。您可以使用 load\_model 函数读取此管道。

```
**# 加载模型**
from pycaret.regression import load_model
pipeline = load_model('C:/Users/moezs/mlruns/1/b8c10d259b294b28a3e233a9d2c209c0/artifacts/model/model')

**# 打印管道
**print(pipeline)
```

![print(pipeline) 的输出](https://cdn-images-1.medium.com/max/2916/1\*E0dUApG1JQnddUfVHmCYvg.png)

现在让我们使用此管道在新数据上生成预测

```
**# 创建数据副本并删除价格
**data2 = data.copy()
data2.drop('Price', axis=1, inplace=True)

**# 生成预测
**from pycaret.regression import predict_model
predictions = predict_model(pipeline, data=data2)
predictions.head()
```

![从管道生成的预测](https://cdn-images-1.medium.com/max/2000/1\*IVtFV6oRqcsgyNQTHbb3QA.png)

哇！我们现在从训练过的管道中得到了推断。恭喜，如果这是您的第一个。请注意，所有转换，如目标转换、独热编码、缺失值填充等都在幕后自动发生。您得到了一个带有实际规模预测的数据框，这才是您关心的。

### 敬请期待！
今天我展示的是使用 MLflow 在生产环境中使用 PyCaret 训练的 Pipeline 的一种方法。在下一个教程中，我计划展示如何使用 MLflow 的原生 serving 功能来注册模型、对其进行版本管理并作为 API 提供服务。

使用这个轻量级的 Python 工作流自动化库，您可以实现无限的可能。如果您觉得这个工具有用，请不要忘记在我们的 GitHub 仓库上给我们一个 ⭐️。

要了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

加入我们的 Slack 频道。邀请链接在[这里](https://join.slack.com/t/pycaret/shared_invite/zt-p7aaexnl-EqdTfZ9U~mF0CwNcltffHg)。

### 您可能还对以下内容感兴趣：

- [使用 PyCaret 2.0 在 Power BI 中构建自己的 AutoML](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d)
- [使用 Docker 在 Azure 上部署机器学习 Pipeline](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01)
- [在 Google Kubernetes Engine 上部署机器学习 Pipeline](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b)
- [在 AWS Fargate 上部署机器学习 Pipeline](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507)
- [构建和部署您的第一个机器学习 Web 应用](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)
- [使用 AWS Fargate 无服务器部署 PyCaret 和 Streamlit 应用](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2)
- [使用 PyCaret 和 Streamlit 构建和部署机器学习 Web 应用](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)
- [在 GKE 上部署使用 Streamlit 和 PyCaret 构建的机器学习应用](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

### 重要链接

- [文档](https://pycaret.readthedocs.io/en/latest/installation.html)
- [博客](https://medium.com/@moez_62905)
- [GitHub](http://www.github.com/pycaret/pycaret)
- [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret)
- [安装 PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html)
- [Notebook 教程](https://pycaret.readthedocs.io/en/latest/tutorials.html)
- [为 PyCaret 做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 想了解特定模块的信息吗？

点击下面的链接查看文档和工作示例。

- [分类](https://pycaret.readthedocs.io/en/latest/api/classification.html)
- [回归](https://pycaret.readthedocs.io/en/latest/api/regression.html)
- [聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html)
- [异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html)
- [自然语言处理](https://pycaret.readthedocs.io/en/latest/api/nlp.html)
- [关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)