# GitHub是你所需要的最好的AutoML工具

### GitHub是你所需要的最好的AutoML工具

#### 作者：Moez Ali

![PyCaret — 一个开源的、低代码的Python机器学习库！](https://cdn-images-1.medium.com/max/2000/1\*Qe1H5nFp506CKQJto0XU9A.png)

你可能会想，GitHub什么时候开始涉足自动化机器学习的领域了。实际上，并没有，但你可以使用它来测试你个性化的AutoML软件。在本教程中，我们将向你展示如何构建和容器化自己的自动化机器学习软件，并使用Docker容器在GitHub上进行测试。

我们将使用PyCaret 2.0，一个开源的、低代码的Python机器学习库，来开发一个简单的AutoML解决方案，并使用GitHub actions将其部署为Docker容器。如果你之前没有听说过PyCaret，你可以在[这里](https://towardsdatascience.com/announcing-pycaret-2-0-39c11014540e)阅读官方公告，或者在[这里](https://github.com/pycaret/pycaret/releases/tag/2.0)查看详细的发布说明。

### 👉 本教程的学习目标

* 理解什么是自动化机器学习，以及如何使用PyCaret 2.0构建一个简单的AutoML软件。
* 理解什么是容器，以及如何将你的AutoML解决方案部署为Docker容器。
* 了解GitHub actions是什么，以及如何使用它们来测试你的AutoML。

### 什么是自动化机器学习？

自动化机器学习（AutoML）是自动化机器学习中耗时、迭代的任务的过程。它允许数据科学家和分析师以高效的方式构建机器学习模型，同时保持模型的质量。任何AutoML软件的最终目标是根据一些性能指标确定最佳模型。

传统的机器学习模型开发过程需要大量的资源，需要丰富的领域知识和时间来生成和比较数十个模型。通过自动化机器学习，你可以加快开发生产就绪的机器学习模型所需的时间，而且非常容易和高效。

市面上有许多付费和开源的AutoML软件。几乎所有这些软件都使用相同的转换和基础算法集合。因此，在这些软件下训练的模型的质量和性能几乎是相同的。

付费的AutoML软件作为服务非常昂贵，如果你没有数十个用例，那么在经济上是不可行的。托管的机器学习作为服务平台相对便宜一些，但使用起来往往很困难，需要对特定平台有所了解。

在众多其他开源AutoML库中，PyCaret是一个相对较新的库，具有独特的低代码方法。PyCaret的设计和功能简单、人性化、直观。在短时间内，PyCaret已经被全球超过10万名数据科学家采用，我们是一个不断壮大的开发者社区。

### PyCaret是如何工作的？

PyCaret是一个用于监督和无监督机器学习的工作流自动化工具。它分为六个模块，每个模块都有一组可用于执行某些特定操作的函数。每个函数都接受一个输入并返回一个输出，在大多数情况下是一个经过训练的机器学习模型。截至第二个版本，可用的模块有：

* [分类](https://www.pycaret.org/classification)
* [回归](https://www.pycaret.org/regression)
* [聚类](https://www.pycaret.org/clustering)
* [异常检测](https://www.pycaret.org/anomaly-detection)
* [自然语言处理](https://www.pycaret.org/nlp)
* [关联规则挖掘](https://www.pycaret.org/association-rules)

PyCaret的所有模块都支持数据准备（超过25种基本预处理技术，提供了大量未训练模型和自定义模型的支持，自动超参数调整，模型分析和可解释性，自动模型选择，实验记录和简单的云部署选项。

![https://www.pycaret.org/guide](https://cdn-images-1.medium.com/max/2066/1\*wT0m1kx8WjY\_P7hrM6KDbA.png)

要了解更多关于PyCaret的信息，[点击这里](https://towardsdatascience.com/announcing-pycaret-2-0-39c11014540e)阅读我们的官方发布公告。

如果你想在Python中开始使用PyCaret，[点击这里](https://github.com/pycaret/pycaret/tree/master/examples)查看一系列示例笔记本。

### 👉 开始之前

在开始构建AutoML软件之前，让我们先了解一下以下术语。此时，你只需要对我们在本教程中使用的这些工具/术语有一些基本的理论知识即可。如果你想深入了解，本教程末尾有一些链接供你以后探索。

### **容器**
**容器**提供了一个便携且一致的环境，可以在不同环境中快速部署，以最大化**机器学习**应用的准确性、性能和效率。环境包含运行时语言（例如 Python）、所有库以及应用程序的依赖项。

### **Docker**

Docker 是一家提供软件（也称为 Docker）的公司，允许用户构建、运行和管理容器。虽然 Docker 的容器是最常见的，但还有其他不那么出名的_替代品_，比如[LXD](https://linuxcontainers.org/lxd/introduction/)和[LXC](https://linuxcontainers.org/)，它们也提供容器解决方案。

### GitHub

[GitHub](https://www.github.com/)是一个基于云的服务，用于托管、管理和控制代码。想象一下，你正在一个大团队中工作，有多人（有时数百人）在同一代码库上进行更改。PyCaret 本身就是一个开源项目的例子，数百名社区开发人员不断为源代码做出贡献。如果你以前没有使用过 GitHub，可以[注册](https://github.com/join)一个免费账户。

### **GitHub Actions**

GitHub Actions 帮助您在存储代码、协作进行拉取请求和处理问题的同一位置自动化软件开发工作流程。您可以编写称为操作的单个任务，并将它们组合以创建自定义工作流程。工作流程是您可以在存储库中设置的自定义自动化流程，用于构建、测试、打包、发布或部署 GitHub 上的任何代码项目。

### 👉 让我们开始吧

### 目标

训练并选择最佳的回归模型，根据数据集中的其他变量（如年龄、性别、BMI、子女、吸烟者和地区）预测患者费用。

### 👉 **步骤 1 — 开发 app.py**

这是 AutoML 的主文件，也是 Dockerfile 的入口点（请参见下面的步骤 2）。如果您以前使用过 PyCaret，那么这段代码对您来说应该是不言自明的。

前五行是关于从环境中导入库和变量。接下来的三行是用于将数据读取为 _pandas_ dataframe。第 12 行到第 15 行是根据环境变量导入相关模块，第 17 行及以后是关于 PyCaret 的函数，用于初始化环境、比较基本模型并将表现最佳的模型保存在您的设备上。最后一行将实验日志下载为 csv 文件。

### 👉 步骤 2 — 创建 Dockerfile

Dockerfile 只是一个包含几行指令的文件，保存在您的项目文件夹中，文件名为“Dockerfile”（区分大小写，无扩展名）。

另一种思考 Dockerfile 的方式是，它就像您在自己厨房里发明的食谱。当您与他人分享这样的食谱，如果他们按照食谱中的确切指令操作，他们将能够制作出相同质量的菜肴。类似地，您可以与他人分享您的 Dockerfile，然后他们可以基于该 Dockerfile 创建镜像并运行容器。

这个项目的 Dockerfile 很简单，只有 6 行。请参见下文：

Dockerfile 中的第一行导入了 python:3.7-slim 镜像。接下来的四行创建一个 app 文件夹，更新 **libgomp1** 库，并从 **requirements.txt** 文件中安装所有要求，这里只需要 pycaret。最后的两行定义了应用程序的入口点；这意味着当容器启动时，它将执行我们在步骤 1 中看到的 **app.py** 文件。

### 👉 步骤 3 — 创建 action.yml

Docker actions 需要一个元数据文件。元数据文件的文件名必须是 action.yml 或 action.yaml。元数据文件中的数据定义了您的操作的输入、输出和主入口点。操作文件使用 YAML 语法。

环境变量 dataset、target 和 usecase 在第 6 行、第 9 行和第 14 行分别声明。请参见 app.py 的第 4–6 行，了解我们如何在 app.py 文件中使用这些环境变量。

### 👉 步骤 4 — 在 GitHub 上发布 action

此时，您的项目文件夹应如下所示：

![https://github.com/pycaret/pycaret-git-actions](https://cdn-images-1.medium.com/max/2082/1\*qBWs9Yk2Kgycu1wUtZe2Ow.png)

点击 **‘Releases’**：

![GitHub Action — 点击 Releases](https://cdn-images-1.medium.com/max/2804/1\*rrr51HMFW0Sc6gD4A0Agtg.png)

起草一个新发布：

![GitHub Action — 起草一个新发布](https://cdn-images-1.medium.com/max/3698/1\*od3eRb8OaoeRhW4IT5ZduA.png)

填写详细信息（标签、发布标题和描述），然后点击 **‘发布发布’**：

![GitHub Action — 发布发布](https://cdn-images-1.medium.com/max/2292/1\*fW\_n0JkZQEoUk-OBIP-4Sw.png)

发布后，点击发布，然后点击 **‘市场’**：

![GitHub Action — 市场](https://cdn-images-1.medium.com/max/2814/1\*Dfa9llJIIUw501qaAUomRw.png)

点击 **‘使用最新版本’**：
![GitHub Action — 使用最新版本](https://cdn-images-1.medium.com/max/2364/1\*9F3GiDDYrIVcwvOmKIcMHA.png)

保存这些信息，这是您软件的安装详情。这是您需要在任何公共 GitHub 存储库上安装此软件所需的内容：

![GitHub Action — 安装](https://cdn-images-1.medium.com/max/2000/1\*UihPzGDhm2smpqOS2YW4Yg.png)

### 👉 第5步— 在 GitHub 存储库上安装软件

为了安装和测试我们刚刚创建的软件，我们创建了一个新的存储库 [\*\*pycaret-automl-test](https://github.com/pycaret/pycaret-automl-test) \*\*并上传了一些用于分类和回归的示例数据集。

要安装我们在上一步中发布的软件，请点击‘**Actions**’：

![https://github.com/pycaret/pycaret-automl-test/tree/master](https://cdn-images-1.medium.com/max/3776/1\*MQKaHVJwqTZQWzwjNn5rcQ.png)

![开始使用 GitHub Actions](https://cdn-images-1.medium.com/max/2000/1\*h37nTkjLQhrbWRSwIL-VEQ.png)

点击‘**set up a workflow yourself**’，将此脚本复制到编辑器中，然后点击**‘Start commit’**。

这是一个供 GitHub 执行的指令文件。第一个动作从第9行开始。第9到15行是一个动作，用于安装和执行我们之前开发的软件。第11行是我们引用软件名称的地方（参见上面第4步的最后部分）。第13到15行是用于定义环境变量的动作，例如数据集的名称（csv 文件必须上传到存储库）、目标变量的名称和用例类型。从第16行开始是另一个来自 [此存储库](https://github.com/actions/upload-artifact) 的动作，用于上传三个文件 model.pkl、实验日志作为 csv 文件和系统日志作为 .log 文件。

一旦开始提交，请点击**‘actions’**：

![GitHub Action — 工作流程](https://cdn-images-1.medium.com/max/2870/1\*rYW8L7Yvtj1BIsFL18jquw.png)

这里您可以监视构建日志，一旦工作流程完成，您也可以从这个位置收集文件。

![GitHub Action — 工作流程构建日志](https://cdn-images-1.medium.com/max/3062/1\*SD4IMJjgg\_PB-aAKxYDA0g.png)

![GitHub Action — 运行详情](https://cdn-images-1.medium.com/max/3034/1\*xmXuNcrm7pJ4F64R7mJXmQ.png)

您可以下载这些文件并解压到您的设备上。

### **文件: model**

这是一个 .pkl 文件，包含最终模型以及整个转换流水线。您可以使用此文件使用 predict\_model 函数在新数据集上生成预测。要了解更多，请[点击这里](https://www.pycaret.org/predict-model)。

### 文件: experiment-logs

这是一个 .csv 文件，包含您的模型所需的所有详细信息。它包含在 app.py 脚本中训练的所有模型、它们的性能指标、超参数和其他重要的元数据。

![实验日志文件](https://cdn-images-1.medium.com/max/3830/1\*i4fvedl-mtKMtOtWl2pfUQ.png)

### 文件: system-logs

这是 PyCaret 生成的系统日志文件。这可用于审计流程。它包含重要的元数据信息，对于排除软件中的错误非常有用。

![PyCaret 生成的系统日志文件](https://cdn-images-1.medium.com/max/3838/1\*QQ4Um9aRxLhyyLwW-oD4fg.png)

### **披露**

GitHub Actions 可让您直接在 GitHub 存储库中创建自定义软件开发生命周期工作流程。每个帐户都附带用于 Actions 的计算和存储数量，具体取决于您的帐户计划，详情请参阅[Actions 文档](https://docs.github.com/en/github/automating-your-workflow-with-github-actions/about-github-actions#about-github-actions)。

Actions 和 Action 服务的任何元素不得违反协议、[可接受使用政策](https://docs.github.com/en/github/site-policy/github-acceptable-use-policies)或 GitHub Actions [服务限制](https://docs.github.com/en/github/automating-your-workflow-with-github-actions/about-github-actions#usage-limits)。此外，不应将 Actions 用于以下用途：

* 加密货币挖掘；
* 无服务器计算；
* 使用我们的服务器来干扰，或试图干扰，或未经授权地访问任何服务、设备、数据、帐户或网络（除非经由[GitHub 漏洞赏金计划](https://bounty.github.com/)授权）；
* 为商业目的提供独立或集成的应用程序或服务，提供 Actions 或任何 Actions 元素；
* 与使用 GitHub Actions 的存储库相关的软件项目的生产、测试、部署或发布无关的任何其他活动。

为防止违反这些限制和滥用 GitHub Actions，GitHub 可能会监视您对 GitHub Actions 的使用。对 GitHub Actions 的滥用可能导致作业终止，或限制您使用 GitHub Actions 的能力。
### **本教程中使用的存储库：**

[**pycaret/pycaret-git-actions** *pycaret-git-actions. 通过在 GitHub 上创建帐户，为 pycaret/pycaret-git-actions 做出贡献。*github.com](https://github.com/pycaret/pycaret-git-actions) [**pycaret/pycaret-automl-test** *pycaret-automl-test. 通过在 GitHub 上创建帐户，为 pycaret/pycaret-automl-test 做出贡献。*github.com](https://github.com/pycaret/pycaret-automl-test)

使用这个轻量级的 Python 工作流自动化库，您可以实现无限可能。如果您觉得这个工具有用，请不要忘记在我们的 GitHub 存储库上给我们 ⭐️。

要了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

如果您想了解更多关于 PyCaret 2.0 的信息，请阅读这篇 [公告](https://towardsdatascience.com/announcing-pycaret-2-0-39c11014540e)。如果您之前使用过 PyCaret，您可能会对当前版本的 [发布说明](https://github.com/pycaret/pycaret/releases/tag/2.0) 感兴趣。

### 您可能也对以下内容感兴趣：

[在 Power BI 中使用 PyCaret 2.0 构建您自己的 AutoML](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d) [使用 Docker 在 Azure 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01) [在 Google Kubernetes Engine 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b) [在 AWS Fargate 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507) [构建并部署您的第一个机器学习 Web 应用程序](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99) [使用 AWS Fargate 无服务器部署 PyCaret 和 Streamlit 应用程序](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2) [使用 PyCaret 和 Streamlit 构建和部署机器学习 Web 应用程序](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104) [在 GKE 上部署使用 Streamlit 和 PyCaret 构建的机器学习应用程序](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

### 重要链接

[博客](https://medium.com/@moez_62905) [PyCaret 2.0 的发布说明](https://github.com/pycaret/pycaret/releases/tag/2.0) [用户指南 / 文档](https://www.pycaret.org/guide) [Github](http://www.github.com/pycaret/pycaret) [Stackoverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装 PyCaret](https://www.pycaret.org/install) [笔记本教程](https://www.pycaret.org/tutorial) [为 PyCaret 做出贡献](https://www.pycaret.org/contribute)

### 想了解特定模块的信息吗？

点击下面的链接查看文档和工作示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)