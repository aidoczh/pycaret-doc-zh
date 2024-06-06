# 构建和部署你的第一个机器学习 Web 应用

### 构建和部署你的第一个机器学习 Web 应用

#### 一位初学者的指南：使用 PyCaret 在 Python 中训练和部署机器学习流水线

#### 作者：Moez Ali

![](https://cdn-images-1.medium.com/max/2000/1\*NWklye0cNThqH\_cTImozlA.png)

在我们的[上一篇文章](https://towardsdatascience.com/machine-learning-in-power-bi-using-pycaret-34307f09394a)中，我们演示了如何使用[PyCaret](https://www.pycaret.org/)在 Power BI 中训练和部署机器学习模型。如果你之前没有听说过 PyCaret，请阅读我们的[公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)以快速入门。

在本教程中，我们将使用 PyCaret 开发一个**机器学习流水线**，其中包括预处理转换和回归模型，用于根据人口统计学和基本患者健康风险指标（如年龄、BMI、吸烟状况等）预测患者住院费用。

### 👉 你将在本教程中学到什么

* 什么是部署，以及为什么要部署机器学习模型。
* 使用 PyCaret 开发机器学习流水线并训练模型。
* 使用名为“Flask”的 Python 框架构建一个简单的 Web 应用程序。
* 在“Heroku”上部署 Web 应用程序并查看你的模型运行情况。

### 💻 本教程中我们将使用哪些工具？

### PyCaret

[PyCaret](https://www.pycaret.org/) 是一个开源的、低代码的 Python 机器学习库，用于在生产环境中训练和部署机器学习流水线和模型。可以使用 pip 轻松安装 PyCaret。

```
# 适用于本地计算机上的 Jupyter 笔记本
pip install **pycaret**

# 适用于 Azure 笔记本和 Google Colab
!pip install **pycaret**
```

### Flask

[Flask](https://flask.palletsprojects.com/en/1.1.x/) 是一个框架，可以用于构建 Web 应用程序。Web 应用程序可以是商业网站、博客、电子商务系统，或者是使用训练好的模型实时生成预测结果的应用程序。如果你还没有安装 Flask，可以使用 pip 安装它。

```
# 安装 Flask
pip install **Flask**
```

### GitHub

[GitHub](https://www.github.com/) 是一个基于云的服务，用于托管、管理和控制代码。想象一下，你正在一个大型团队中工作，有多个人（有时是数百人）在进行更改。PyCaret 本身就是一个开源项目的例子，数百名社区开发人员不断为源代码做出贡献。如果你之前没有使用过 GitHub，可以[注册](https://github.com/join)一个免费账户。

### Heroku

[Heroku](https://www.heroku.com/) 是一个平台即服务（PaaS），它基于托管容器系统，提供集成的数据服务和强大的生态系统，可以部署 Web 应用程序。简单来说，这将允许你将应用程序从本地计算机部署到云端，以便任何人都可以使用 Web URL 访问它。在本教程中，我们选择使用 Heroku 进行部署，因为当你[注册](https://signup.heroku.com/)一个新账户时，它提供免费的资源时间。

![机器学习工作流程（从训练到在 PaaS 上部署）](https://cdn-images-1.medium.com/max/2000/1\*GCRVoOwIKL\_AhmrwOtQwaA.png)

### 为什么要部署机器学习模型？

部署机器学习模型是将模型在生产环境中提供给 Web 应用程序、企业软件和 API 使用的过程，这些应用程序和软件可以通过提供新数据点并生成预测来使用训练好的模型。

通常，机器学习模型的构建是为了用于预测结果（分类问题的二进制值，如 1 或 0，回归问题的连续值，聚类问题的标签等）。生成预测的方式有两种：（i）批量预测；（ii）实时预测。在我们的[上一个教程](https://towardsdatascience.com/machine-learning-in-power-bi-using-pycaret-34307f09394a)中，我们演示了如何在 Power BI 中部署机器学习模型并进行批量预测。在本教程中，我们将看到如何部署一个机器学习模型以进行实时预测。

### 业务问题

一家保险公司希望通过在患者住院时使用人口统计学和基本患者健康风险指标来更好地预测患者费用，从而改进其现金流预测。

![](https://cdn-images-1.medium.com/max/2000/0\*10e8RTwI5t0Wi8fg.png)

_(_[_数据来源_](https://www.kaggle.com/mirichoi0218/insurance#insurance.csv)_)_

### 目标

构建一个 Web 应用程序，在 Web 表单中输入患者的人口统计学和健康信息，以预测费用。

### 任务

* 训练和验证模型，并开发一个机器学习流水线以进行部署。
* 使用一个基本的 HTML 前端构建一个包含独立变量（年龄、性别、BMI、子女数、吸烟者、地区）输入表单。
* 使用 Flask 框架构建 Web 应用程序的后端。
* 在 Heroku 上部署 Web 应用。一旦部署完成，它将变为公开可访问，并可以通过 Web URL 访问。

### 👉 任务 1 — 模型训练和验证

在集成开发环境（IDE）或笔记本中进行模型训练和验证，可以在本地计算机上或云端进行。在本教程中，我们将使用 Jupyter Notebook 中的 PyCaret 来开发机器学习流水线并训练回归模型。如果您之前没有使用过 PyCaret，[点击这里](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46) 了解更多关于 PyCaret 的信息，或查看我们[网站](https://www.pycaret.org/)上的 [入门教程](https://www.pycaret.org/tutorial)。

在本教程中，我们进行了两个实验。第一个实验使用 PyCaret 中的默认预处理设置（缺失值填充、分类编码等）。第二个实验包含一些额外的预处理任务，如缩放和归一化、自动特征工程以及将连续数据分成区间。查看第二个实验的设置示例：

```
# 实验 2

from **pycaret.regression** import *****

r2 = **setup**(data, target = 'charges', session_id = 123,
           normalize = True,
           polynomial_features = True, trigonometry_features = True,
           feature_interaction=True, 
           bin_numeric_features= ['age', 'bmi'])
```

![两个实验的信息网格比较](https://cdn-images-1.medium.com/max/2000/0\*lA\_5MECr5Onj0nRS.png)

只需几行代码，就能实现神奇的效果。请注意，在**实验 2**中，经过转换的数据集有 62 个特征用于训练，而这些特征仅来自原始数据集中的 7 个特征。所有新特征都是在 PyCaret 中的转换和自动特征工程的结果。

![转换后数据集中的列](https://cdn-images-1.medium.com/max/2000/0\*c6jeng5IXupSzJtE.png)

PyCaret 中模型训练和验证的示例代码：

```
# 模型训练和验证
lr = **create_model**('lr')
```

![线性回归模型的 10 折交叉验证](https://cdn-images-1.medium.com/max/2276/0\*qGM8XwQdUpU5YK2z.png)

请注意转换和自动特征工程的影响。R2 值增加了 10%，而付出的努力却很少。我们可以比较两个实验中线性回归模型的**残差图**，观察转换和特征工程对模型**异方差性**的影响。

```
# 绘制训练模型的残差图
plot_model**(lr, plot = 'residuals')
```

![线性回归模型的残差图](https://cdn-images-1.medium.com/max/2876/0\*FJDkF4CzHtwfGlbg.png)

机器学习是一个**迭代**过程。迭代次数和使用的技术取决于任务的重要性，以及如果预测错误会产生什么影响。在医院 ICU 实时预测患者结果的机器学习模型的严重性和影响远远大于用于预测客户流失的模型。

在本教程中，我们仅进行了两次迭代，第二个实验中的线性回归模型将用于部署。然而，在这个阶段，模型仍然只是笔记本中的一个对象。要将其保存为可以传输并被其他应用程序使用的文件，请运行以下代码：

```
# 保存转换流水线和模型
**save_model**(lr, model_name = 'c:/*username*/ins/deployment_28042020')
```

在 PyCaret 中保存模型时，基于在**setup()**函数中定义的配置创建整个转换流水线。所有相互依赖关系都会自动协调。查看存储在 'deployment\_28042020' 变量中的流水线和模型：

![使用 PyCaret 创建的流水线](https://cdn-images-1.medium.com/max/2000/0\*ZY7Ep3vsER-6G7ug.png)

我们已经完成了训练和选择模型以进行部署的第一个任务。最终的机器学习流水线和线性回归模型现在以文件的形式保存在本地驱动器中，位置在**save\_model()**函数中定义的位置。（在此示例中：c:/_username_/ins/deployment\_28042020.pkl）。

### 👉 任务 2 — 构建 Web 应用程序

现在我们的机器学习流水线和模型已经准备就绪，我们将开始构建一个可以连接到它们并实时生成新数据预测的 Web 应用程序。此应用程序有两个部分：

* 前端（使用 HTML 设计）
* 后端（使用 Python 中的 Flask 开发）

### Web 应用程序的前端
通常，Web 应用程序的前端是使用 HTML 构建的，但这不是本文的重点。我们使用了一个简单的 HTML 模板和一个 CSS 样式表来设计一个输入表单。以下是我们 Web 应用程序前端页面的 HTML 片段。

![来自 home.html 文件的代码片段](https://cdn-images-1.medium.com/max/2388/1\*t0a5Ev7oFJf73oIY8lkHZQ.png)

构建简单应用程序并不需要成为 HTML 专家。有许多免费平台提供 HTML 和 CSS 模板，并通过拖放界面快速构建漂亮的 HTML 页面。

**CSS 样式表** CSS（层叠样式表）描述了 HTML 元素在屏幕上的显示方式。这是一种有效控制应用程序布局的方式。样式表包含诸如背景颜色、字体大小和颜色、边距等信息。它们以 .css 文件的形式保存在外部，并通过包含 1 行代码链接到 HTML。

![来自 home.html 文件的代码片段](https://cdn-images-1.medium.com/max/2392/1\*2CecMn4-O-slFc6Tf1BK1w.png)

### Web 应用程序的后端

Web 应用程序的后端是使用 Flask 框架开发的。对于初学者来说，直观地将 Flask 视为 Python 中可以像导入其他库一样导入的库。请查看我们使用 Python 中 Flask 框架编写的后端示例代码片段。

![来自 app.py 文件的代码片段](https://cdn-images-1.medium.com/max/2444/0\*2KpsfiiecB5mCtab.png)

如果您还记得上面的第 1 步，我们已经确定了一个线性回归模型，该模型是在 PyCaret 自动创建的 62 个特征上训练的。然而，我们的 Web 应用程序的前端只收集了六个特征，即年龄、性别、BMI、子女、吸烟者、地区。

我们如何将新数据点的 6 个特征实时转换为模型训练的 62 个特征？通过在模型训练期间应用一系列转换，编码变得越来越复杂和耗时。

在 PyCaret 中，所有转换，如分类编码、缩放、缺失值填充、特征工程甚至特征选择，在生成预测之前都会自动实时执行。

> ## _想象一下，在您甚至可以使用模型进行预测之前，您必须编写的所有转换的代码量。实际上，当您考虑机器学习时，您应该考虑整个 ML 流水线，而不仅仅是模型。_

**测试应用程序** 在将应用程序发布到 Heroku 之前的最后一步是在本地测试 Web 应用程序。打开 Anaconda Prompt 并导航到计算机上保存 **'app.py'** 的文件夹。使用以下代码运行 python 文件：

```
python **app.py**
```

![当执行 app.py 时在 Anaconda Prompt 中的输出](https://cdn-images-1.medium.com/max/2204/0\*NvcdEyGUoUWWoJKZ.png)

执行后，将 URL 复制到浏览器中，应该会打开托管在本地计算机（127.0.0.1）上的 Web 应用程序。尝试输入测试值，查看预测功能是否正常。在下面的示例中，一个 19 岁的女性吸烟者在西南地区没有子女的预期账单为 20,900 美元。

![在本地计算机上打开的 Web 应用程序](https://cdn-images-1.medium.com/max/3780/0\*GBP1kfSwpBzstWzI.png)

恭喜！您现在已经构建了您的第一个机器学习应用程序。现在是时候将此应用程序从本地计算机移到云端，以便其他人可以通过 Web URL 使用它。

### 👉 任务 3 — 在 Heroku 上部署 Web 应用程序

现在模型已经训练好，机器学习流水线已准备就绪，并且应用程序已在我们的本地计算机上进行了测试，我们准备在 Heroku 上开始部署。有几种方法可以将应用程序源代码上传到 Heroku。最简单的方法是将 GitHub 存储库链接到您的 Heroku 帐户。

如果您想跟着操作，可以从 GitHub 上 fork 这个 [存储库](https://github.com/pycaret/deployment-heroku)。如果您不知道如何 fork 存储库，请阅读这篇[官方 GitHub 教程](https://help.github.com/en/github/getting-started-with-github/fork-a-repo)。

![https://www.github.com/pycaret/deployment-heroku](https://cdn-images-1.medium.com/max/2524/0\*GPOio0x0TnQFr8r8.png)

到目前为止，您已经熟悉了上面显示的存储库中的所有文件，除了两个文件，即 '**requirements.txt**' 和 '**Procfile**'。

![requirements.txt](https://cdn-images-1.medium.com/max/2440/0\*XHMUGa90vc6csKfO.png)

**requirements.txt** 文件是一个文本文件，包含执行应用程序所需的 Python 包的名称。如果这些包没有安装在应用程序运行的环境中，应用程序将失败。

![Procfile](https://cdn-images-1.medium.com/max/2470/0\*NFbZtaStgESRUIol.png)
**Procfile** 只是一行代码，它向 Web 服务器提供启动指令，指示在有人登录应用程序时应首先执行哪个文件。在这个例子中，我们的应用程序文件名为 '**app.py**'，应用程序的名称也是 '**app**'（因此是 app:app）。

一旦所有文件都上传到 GitHub 存储库中，我们现在可以开始在 Heroku 上进行部署。按照以下步骤操作：

**第一步 - 在 heroku.com 上注册并点击 'Create new app'**

![Heroku 仪表盘](https://cdn-images-1.medium.com/max/3108/0\*MXhm58jzLVET5Xa4.png)

**第二步 - 输入应用程序名称和地区**

![Heroku - 创建新应用程序](https://cdn-images-1.medium.com/max/2000/0\*8Lu1Fc9A7iGnVJCm.png)

**第三步 - 连接到托管代码的 GitHub 存储库**

![Heroku - 连接到 GitHub](https://cdn-images-1.medium.com/max/3092/0\*VyAQvI2kDr2SsXYz.png)

**第四步 - 部署分支**

![Heroku - 部署分支](https://cdn-images-1.medium.com/max/3022/0\*A5Tg\_Qt5cZ6aLl92.png)

**第五步 - 等待 5-10 分钟，然后 BOOM**

![Heroku - 部署成功](https://cdn-images-1.medium.com/max/3078/0\*TFPIem6Q5k6DKszI.png)

应用程序已发布到 URL: [https://pycaret-insurance.herokuapp.com/](https://pycaret-insurance.herokuapp.com/)

![https://pycaret-insurance.herokuapp.com/](https://cdn-images-1.medium.com/max/3772/0\*Fr199orKNCkPMBSf.png)

在结束本教程之前，还有一件事要看。

到目前为止，我们已经构建并部署了一个与我们的机器学习流水线配合使用的 Web 应用程序。现在想象一下，您已经有一个企业应用程序，您希望将模型的预测集成到其中。您需要的是一个 Web 服务，您可以通过输入数据点进行 API 调用，并获得预测结果。为了实现这一点，我们在我们的 'app.py' 文件中创建了 _**predict\_api**_ 函数。请看代码片段：

![来自 app.py 文件（Web 应用程序的后端）的代码片段](https://cdn-images-1.medium.com/max/2000/0\*JvwXvC3bBpKaPTE\_.png)

以下是如何使用 Python 和 requests 库使用此 Web 服务的示例：

```
import requests
url = 'https://pycaret-insurance.herokuapp.com/predict_api'
pred = requests.post(url,json={'age':55, 'sex':'male', 'bmi':59, 'children':1, 'smoker':'male', 'region':'northwest'})
print(pred.json())
```

![在 Notebook 中向已发布的 Web 服务发出请求以生成预测](https://cdn-images-1.medium.com/max/2474/0\*a9T8yMRXwymlccdr.png)

### 下一个教程

在下一个部署机器学习流水线的教程中，我们将深入探讨使用 Docker 容器部署机器学习流水线。我们将演示如何在 Linux 上轻松部署和运行容器化的机器学习应用程序。

关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和订阅我们的 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g) 频道，了解更多关于 PyCaret 的信息。

### 重要链接

[用户指南 / 文档](https://www.pycaret.org/guide) [GitHub 存储库](https://www.github.com/pycaret/pycaret) [安装 PyCaret](https://www.pycaret.org/install) [Notebook 教程](https://www.pycaret.org/tutorial) [为 PyCaret 做贡献](https://www.pycaret.org/contribute)

### 想要了解特定模块吗？

截至第一个版本 1.0.0，PyCaret 提供以下模块供使用。点击下面的链接查看 Python 中的文档和工作示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 还可以参考：

PyCaret 在 Notebook 中的入门教程：

[聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101) [回归](https://www.pycaret.org/reg101) [分类](https://www.pycaret.org/clf101)

### 开发计划中有什么？

我们正在积极改进 PyCaret。我们未来的开发计划包括一个新的 **时间序列预测** 模块，与 **TensorFlow** 的集成，以及对 PyCaret 可扩展性的重大改进。如果您想分享您的反馈并帮助我们进一步改进，您可以在网站上填写[此表格](https://www.pycaret.org/feedback)或在我们的 [GitHub](https://www.github.com/pycaret/) 或 [LinkedIn](https://www.linkedin.com/company/pycaret/) 页面上留言。

### 想要做贡献吗？
PyCaret 是一个开源项目。欢迎所有人贡献。如果您想要贡献，请随时在 [开放问题](https://github.com/pycaret/pycaret/issues) 上进行工作。拉取请求将在 dev-1.0.1 分支上接受单元测试。

如果您喜欢 PyCaret，请在我们的 [GitHub 仓库](https://www.github.com/pycaret/pycaret) 上给我们 ⭐️。

Medium：[https://medium.com/@moez\_62905/](https://medium.com/@moez\_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

LinkedIn：[https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter：[https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)