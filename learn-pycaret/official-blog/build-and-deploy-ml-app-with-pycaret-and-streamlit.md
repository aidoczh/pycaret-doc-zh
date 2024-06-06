# 使用 PyCaret 和 Streamlit 构建和部署机器学习应用

### 使用 PyCaret 和 Streamlit 构建和部署机器学习 Web 应用

#### 一个初学者在 Heroku PaaS 上部署机器学习应用的指南

#### 作者：Moez Ali

![](https://cdn-images-1.medium.com/max/2000/1\*HuGxT33q9tj7FQikC3EB\_Q.png)

### 回顾

在我们上一篇关于在云端部署机器学习流水线的文章中，我们演示了如何使用 PyCaret 开发机器学习流水线，使用 Docker 封装 Flask 应用，并使用 AWS Fargate 无服务器部署。如果你之前没有听说过 PyCaret，你可以阅读这篇[公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)来了解更多。

在本教程中，我们将使用 PyCaret 训练一个机器学习流水线，并使用 [Streamlit](https://www.streamlit.io/) 开源框架创建一个 Web 应用。这个 Web 应用将是一个简单的界面，供业务用户使用训练好的机器学习流水线在新数据集上生成预测。

通过本教程，你将能够构建一个完全功能的 Web 应用，使用训练好的机器学习模型生成在线预测（逐个预测）和批量预测（通过上传 CSV 文件）。最终的应用如下图所示：

![https://pycaret-streamlit.herokuapp.com](https://cdn-images-1.medium.com/max/3826/1\*-scVDUhBbOIWievCj0DYjw.png)

### 👉 本教程中你将学到什么

* 什么是部署，为什么要部署机器学习模型？
* 使用 PyCaret 开发机器学习流水线并训练模型。
* 使用 Streamlit 开源框架构建一个简单的 Web 应用。
* 在 'Heroku' 上部署 Web 应用并观察模型的运行情况。

本教程将涵盖从在 Python 中训练机器学习模型和开发流水线，使用 Streamlit 开发一个简单的 Web 应用，以及将应用部署在 Heroku 云平台上的整个工作流程。

在过去，我们已经涵盖了使用 Docker 进行容器化以及在 Azure、GCP 和 AWS 等云平台上部署的内容。如果你对这些内容感兴趣，可以阅读以下文章：

* [在 AWS Fargate 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507)
* [在 Google Kubernetes Engine 上部署机器学习模型](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b)
* [在 AWS Web Service 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01)
* [在 Heroku PaaS 上构建和部署你的第一个机器学习 Web 应用](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)

### 💻 本教程所需工具

### PyCaret

[PyCaret](https://www.pycaret.org/) 是一个开源的、低代码的 Python 机器学习库，用于训练和部署机器学习流水线和模型到生产环境中。可以使用 pip 轻松安装 PyCaret。

```
pip install **pycaret**
```

### Streamlit

[Streamlit](https://www.streamlit.io/) 是一个开源的 Python 库，可以轻松构建漂亮的自定义机器学习和数据科学 Web 应用。可以使用 pip 轻松安装 Streamlit。

```
pip install **streamlit**
```

### GitHub

[GitHub](https://www.github.com/) 是一个基于云的服务，用于托管、管理和控制代码。想象一下你在一个大型团队中工作，有多个人（有时可能是数百人）在进行更改。PyCaret 本身就是一个开源项目的例子，数百名社区开发人员不断为源代码做出贡献。如果你之前没有使用过 GitHub，可以[注册](https://github.com/join)一个免费账号。

### Heroku

[Heroku](https://www.heroku.com/) 是一个平台即服务（PaaS），它使用托管容器系统、集成数据服务和强大的生态系统，实现了 Web 应用的部署。简单来说，这将允许你将应用从本地机器部署到云端，以便任何人都可以使用 Web URL 访问它。在本教程中，我们选择使用 Heroku 进行部署，因为它在[注册](https://signup.heroku.com/)新账号时提供免费资源小时数。

![机器学习工作流程（从训练到在 PaaS 上部署）](https://cdn-images-1.medium.com/max/2000/1\*XTizEjPOR4UKJphNsjbhBw.png)

### ✔️ 让我们开始吧……

### 为什么要部署机器学习模型？

部署机器学习模型是将模型投入生产环境的过程，以便 Web 应用、企业软件和 API 可以使用训练好的模型并对新数据点生成预测。
通常，机器学习模型被构建用于预测结果（例如[分类](https://www.pycaret.org/classification)中的二进制值即1或0，[回归](https://www.pycaret.org/regression)中的连续值，[聚类](https://www.pycaret.org/clustering)中的标签等）。有两种广泛的方式来预测新数据点：

### 👉 **在线预测**

在线预测场景适用于希望逐个数据点生成预测的情况。例如，您可以使用预测立即决定特定交易是否可能是欺诈性的。

### 👉 **批量预测**

批量预测在您希望一次为一组观察生成预测时很有用。例如，如果您想决定针对产品广告活动中的哪些客户进行定位，您将为所有客户获取预测分数，对其进行排序以确定哪些客户最有可能购买，然后可能针对最有可能购买的前5%客户进行定位。

> ## 在本教程中，我们将构建一个应用程序，可以通过上传包含新数据点的 csv 文件来进行在线预测和批量预测。

### 设定业务背景

一家保险公司希望通过在患者入院时使用人口统计数据和基本患者健康风险指标更好地预测患者费用，从而改善现金流预测。

![](https://cdn-images-1.medium.com/max/2000/1\*qM1HiWZ\_uigwdcZ0\_ZL6yA.png)

_(_[_数据来源_](https://www.kaggle.com/mirichoi0218/insurance#insurance.csv)_)_

### 目标

构建一个支持使用训练好的机器学习模型和流水线进行在线（逐个）和批量预测的 Web 应用程序。

### 任务

* 使用 PyCaret 训练、验证和开发一个机器学习流水线。
* 构建一个前端 Web 应用程序，具有两个功能：（i）在线预测和（ii）批量预测。
* 在 Heroku 上部署 Web 应用程序。一旦部署完成，它将变为公开可用，并可通过 Web URL 访问。

### 👉 任务1 — 模型训练和验证

训练和模型验证在集成开发环境（IDE）或笔记本中进行，可以在本地计算机上或云端进行。如果您以前没有使用过 PyCaret，请[点击这里](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)了解有关 PyCaret 的更多信息，或在我们的[网站](https://www.pycaret.org/)上查看[入门教程](https://www.pycaret.org/tutorial)。

在本教程中，我们进行了两个实验。第一个实验是在 PyCaret 中使用默认预处理设置进行的。第二个实验包括一些额外的预处理任务，如**缩放和归一化、自动特征工程和将连续数据分成区间**。查看第二个实验的设置代码：

```
**# 实验编号 2**

from **pycaret.regression** import *****

r2 = **setup**(data, target = 'charges', session_id = 123,
           normalize = True,
           polynomial_features = True, trigonometry_features = True,
           feature_interaction=True, 
           bin_numeric_features= ['age', 'bmi'])
```

![两个实验的信息网格比较](https://cdn-images-1.medium.com/max/2000/1\*TeqcOM-jBpkdeQu84c4Onw.png)

只需几行代码就可以完成这些神奇的操作。请注意，在**实验 2**中，经过转换的数据集有 62 个特征用于训练，而这些特征仅来自原始数据集中的 6 个特征。所有新特征都是在 PyCaret 中的转换和自动特征工程的结果。

![转换后数据集中的列](https://cdn-images-1.medium.com/max/2000/1\*ju5RFYKGkAVEOvVjeoM5nQ.png)

PyCaret 模型训练的示例代码：

```
# 模型训练和验证 
lr = **create_model**('lr')
```

![线性回归模型的 10 折交叉验证](https://cdn-images-1.medium.com/max/2276/1\*TX-IzWHBekZBRSQi2T\_JTQ.png)

请注意转换和自动特征工程的影响。R2 值增加了 10%，而付出的努力很少。我们可以比较两个实验的线性回归模型的**残差图**，观察转换和特征工程对模型**异方差性**的影响。

```
# 绘制训练模型的残差**
plot_model**(lr, plot = 'residuals')
```

![线性回归模型的残差图](https://cdn-images-1.medium.com/max/2876/1\*LxVMcK4hNvBvEj1tyqxfWQ.png)

机器学习是一个迭代过程。迭代次数和使用的技术取决于任务的重要性以及如果预测错误会产生什么影响。在医院急诊室中实时预测患者结果的机器学习模型的严重性和影响远远大于用于预测客户流失的模型。
在本教程中，我们只进行了两次迭代，第二次实验中的线性回归模型将用于部署。然而，在这个阶段，模型仍然只是笔记本/集成开发环境中的一个对象。要将其保存为可传输并被其他应用程序使用的文件，请执行以下代码：

```
# 保存转换管道和模型
**save_model**(lr, model_name = 'deployment_28042020')
```

在 PyCaret 中保存模型时，将基于在 \*\*setup() \*\*函数中定义的配置创建整个转换管道。所有相互依赖关系都将自动协调。查看存储在 ‘deployment\_28042020’ 变量中的管道和模型：

![使用 PyCaret 创建的管道](https://cdn-images-1.medium.com/max/2000/1\*NWoHVWJzO7i7gIvrlBnIiQ.png)

我们已经完成了训练和模型选择。最终的机器学习管道和线性回归模型现在被保存为一个 pickle 文件（deployment\_28042020.pkl），将在 Web 应用程序中用于生成新数据点的预测。

### 👉 任务 2 — 构建 Web 应用程序

现在我们的机器学习管道和模型已经准备就绪，我们将开始构建一个前端 Web 应用程序，该应用程序可以对新数据点生成预测。该应用程序将通过上传 csv 文件来支持‘在线’和‘批量’预测。让我们将应用程序代码分解为三个主要部分：

### **标题 / 布局**

此部分导入库，加载训练好的模型，并创建一个基本布局，顶部是一个标志，侧边栏上有一个 jpg 图像和一个下拉菜单，用于在‘在线’和‘批量’预测之间切换。

![app.py — 代码片段部分 1](https://cdn-images-1.medium.com/max/2268/1\*xAnCZ1N\_BNoPW7FoA-NXrA.png)

### **在线预测**

这部分处理应用程序的第一个功能，即在线（逐个）预测。我们使用 streamlit 的小部件，如 _number input, text input, drop down menu 和 checkbox_ 来收集用于训练模型的数据点，如年龄、性别、BMI、子女、吸烟者、地区。

![app.py — 代码片段部分 2](https://cdn-images-1.medium.com/max/2408/1\*eFeq1wINsUUnvLJfuL-GOA.png)

### **批量预测**

这部分处理第二个功能，即批量预测。我们使用 streamlit 的 **file\_uploader** 小部件上传一个 csv 文件，然后调用 PyCaret 的原生 \*\*predict\_model() \*\*函数生成预测结果，并使用 streamlit 的 write() 函数显示。

![app.py — 代码片段部分 3](https://cdn-images-1.medium.com/max/2410/1\*u-g2iLy\_gV7hom71PM3CEA.png)

如果您还记得上面的任务 1，我们最终确定了一个线性回归模型，该模型是在提取了 6 个原始特征的基础上训练的，这些特征共有 62 个。然而，我们的 Web 应用程序的前端只有一个输入表单，只收集了这六个特征，即年龄、性别、BMI、子女、吸烟者、地区。

如何将新数据点的 6 个特征转换为用于训练模型的 62 个特征？我们不需要担心这一部分，因为 PyCaret 通过协调转换管道来自动处理这一过程。当您在使用 PyCaret 训练的模型上调用预测函数时，所有转换都会自动应用（按顺序），然后从训练好的模型生成预测。

\*\*测试应用程序\*\* 在将应用程序发布到 Heroku 之前的最后一步是在本地测试 Web 应用程序。打开 Anaconda Prompt 并导航到您的项目文件夹，执行以下代码：

```
**streamlit **run app.py
```

![Streamlit 应用程序测试 — 在线预测](https://cdn-images-1.medium.com/max/3832/1\*GxVKpxijk0tlqk-bO5Q3JQ.png)

![Streamlit 应用程序测试 — 批量预测](https://cdn-images-1.medium.com/max/3836/1\*P5tit2pMf5qiQqU\_wjQMVg.png)

### 👉 任务 3 — 在 Heroku 上部署 Web 应用

现在模型已经训练好，机器学习管道已准备就绪，并且应用程序已在我们的本地机器上测试通过，我们准备开始在 Heroku 上部署。有几种方法可以将应用程序源代码上传到 Heroku。最简单的方法是将 GitHub 存储库链接到您的 Heroku 帐户。

如果您想跟着做，可以从 GitHub 上 fork 这个 [存储库](https://www.github.com/pycaret/pycaret-deployment-streamlit)。如果您不知道如何 fork 一个存储库，请阅读这个官方 GitHub 教程 [read this](https://help.github.com/en/github/getting-started-with-github/fork-a-repo)。

![https://www.github.com/pycaret/pycaret-deployment-streamlit](https://cdn-images-1.medium.com/max/2260/1\*IxxUdaHpWu8qqRxakPzG3g.png)

到目前为止，您对存储库中的所有文件都很熟悉，除了三个文件：‘requirements.txt’、‘setup.sh’ 和 ‘Procfile’。让我们看看它们是什么：

### requirements.txt
**requirements.txt** 文件是一个文本文件，包含了执行该应用程序所需的 Python 软件包的名称。如果这些软件包没有安装在应用程序运行的环境中，应用程序将无法正常运行。

![requirements.txt](https://cdn-images-1.medium.com/max/2222/1\*BB7NOG\_3GI4ue1J\_TdtgYQ.png)

### setup.sh

setup.sh 是一个为 bash 编程的脚本。它包含用 Bash 语言编写的指令，与 requirements.txt 类似，**它用于在云端创建必要的环境，以便我们的 streamlit 应用程序运行。**

![setup.sh](https://cdn-images-1.medium.com/max/2226/1\*Con6kr4kh0Ss\_puX7l32\_w.png)

### **Procfile**

Procfile 只是一行代码，为 Web 服务器提供启动指令，指示触发应用程序时应执行哪个文件。在这个例子中，“Procfile” 用于执行 **setup.sh**，这将为 streamlit 应用程序创建必要的环境，第二部分“streamlit run app.py” 是用于执行应用程序（这类似于在本地计算机上执行 streamlit 应用程序的方式）。

![Procfile](https://cdn-images-1.medium.com/max/2226/1\*b11lGrtlyNHpRcBmY1z4Bg.png)

一旦所有文件都上传到 GitHub 存储库，我们现在可以开始在 Heroku 上部署。按照以下步骤进行：

**第 1 步 — 在 heroku.com 上注册并点击“创建新应用”**

![Heroku 仪表板](https://cdn-images-1.medium.com/max/3108/1\*5tVQzeF-9HZgajee\_-2PZg.png)

**第 2 步 — 输入应用程序名称和地区**

![Heroku — 创建新应用](https://cdn-images-1.medium.com/max/2000/1\*yFTEWk8izcuZQFQer96zOQ.png)

**第 3 步 — 连接到您的 GitHub 存储库**

![Heroku — 连接到 GitHub](https://cdn-images-1.medium.com/max/3054/1\*wh45-7ZwbcM04OeV6nFgpw.png)

**第 4 步 — 部署分支**

![Heroku — 部署分支](https://cdn-images-1.medium.com/max/2990/1\*jMrL-8R0-ZWm4WrDObERcw.png)

**第 5 步 — 等待 10 分钟，成功！**

应用程序发布到 URL：[https://pycaret-streamlit.herokuapp.com/](https://pycaret-streamlit.herokuapp.com/)

![https://pycaret-streamlit.herokuapp.com/](https://cdn-images-1.medium.com/max/3826/1\*-scVDUhBbOIWievCj0DYjw.png)

### PyCaret 2.0.0 即将发布！

我们收到了社区的大力支持和反馈。我们正在积极努力改进 PyCaret，并为下一个版本做准备。**PyCaret 2.0.0 将更加强大和优秀**。如果您想分享您的反馈并帮助我们进一步改进，您可以在网站上[填写此表格](https://www.pycaret.org/feedback)，或在我们的 [GitHub](https://www.github.com/pycaret/) 或 [LinkedIn](https://www.linkedin.com/company/pycaret/) 页面上留言。

关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 并订阅我们的 [YouTube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g) 频道，了解更多关于 PyCaret 的信息。

### 想了解特定模块吗？

截至第一个版本 1.0.0，PyCaret 提供以下模块供使用。点击下面的链接查看 Python 中的文档和示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 还可查看：

PyCaret 在 Notebook 中的入门教程：

[聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101) [回归](https://www.pycaret.org/reg101) [分类](https://www.pycaret.org/clf101)

### 想要贡献吗？

PyCaret 是一个开源项目。欢迎每个人贡献。如果您想贡献，请随时处理 [开放问题](https://github.com/pycaret/pycaret/issues)。接受在 dev-1.0.1 分支上带有单元测试的拉取请求。

如果您喜欢 PyCaret，请在我们的 [GitHub 仓库](https://www.github.com/pycaret/pycaret)上给我们 ⭐️。

Medium: [https://medium.com/@moez\_62905/](https://medium.com/@moez\_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

LinkedIn: [https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter: [https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)