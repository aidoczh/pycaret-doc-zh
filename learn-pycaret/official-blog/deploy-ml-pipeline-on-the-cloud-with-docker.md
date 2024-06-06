# 使用 Docker 在云端部署机器学习流水线

### 使用 Docker 容器在云端部署机器学习流水线

#### 作者：Moez Ali

![](https://cdn-images-1.medium.com/max/2000/1\*N3IRs4nRw4vcMt\_AHmbkPA.png)

### **回顾**

在我们的[上一篇文章](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)中，我们演示了如何使用 PyCaret 和 Flask 框架在 Python 中开发机器学习流水线，并将其部署为 Web 应用程序。如果您之前没有听说过 PyCaret，请阅读这篇[公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)以了解更多信息。

在本教程中，我们将使用之前构建和部署的相同的机器学习流水线和 Flask 应用程序。这次我们将演示如何使用[Microsoft Azure Web App Service](https://azure.microsoft.com/en-us/services/app-service/web/)将机器学习流水线部署为 Web 应用程序。

为了在 Microsoft Azure 上部署机器学习流水线，我们将需要将我们的流水线放入一个名为“Docker”的软件中进行容器化。如果您不知道什么是容器化，_没问题_——本教程就是关于这个的。

### 👉 本教程的学习目标

* 什么是容器？什么是 Docker？为什么我们需要它？
* 在本地计算机上构建 Docker 文件并将其发布到[Azure 容器注册表 (ACR)](https://azure.microsoft.com/en-us/services/container-registry/)。
* 使用我们上传到 ACR 的容器在 Azure 上部署 Web 服务。
* 查看一个使用训练好的机器学习流水线实时预测新数据点的 Web 应用程序。

在我们的上一篇文章中，我们介绍了模型部署的基础知识以及为什么需要它。如果您想了解更多关于模型部署的知识，请[点击这里](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)阅读我们的上一篇文章。

本教程将涵盖从本地构建容器到将其推送到 Azure 容器注册表，然后部署我们预训练的机器学习流水线和 Flask 应用程序到 Azure Web 服务的整个工作流程。

![工作流程：创建镜像 → 本地构建容器 → 推送到 ACR → 在云端部署应用](https://cdn-images-1.medium.com/max/2512/1\*4McqTG9jDQvl\_t-omPkEuA.png)

### 💻 本教程的工具箱

### PyCaret

[PyCaret](https://www.pycaret.org/)是一个开源的、低代码的 Python 机器学习库，用于训练和部署机器学习流水线和模型到生产环境中。可以使用 pip 轻松安装 PyCaret。

```
pip install **pycaret**
```

### Flask

[Flask](https://flask.palletsprojects.com/en/1.1.x/)是一个允许您构建 Web 应用程序的框架。Web 应用程序可以是商业网站、博客、电子商务系统，或者是使用训练好的模型实时生成预测的应用程序。如果您没有安装 Flask，可以使用 pip 安装它。

### **Docker**

[Docker](https://www.docker.com/)是一个旨在通过使用容器来更轻松地创建、部署和运行应用程序的工具。容器用于将应用程序及其所有必要组件（如库和其他依赖项）打包在一起，并将其作为一个整体进行传输。如果您之前没有使用过 Docker，本教程还涵盖了在 Windows 10 上安装 Docker 的过程。

### **Microsoft Azure**

[Microsoft Azure](https://azure.microsoft.com/en-ca/overview/what-is-azure/)是一组云服务，用于在一个庞大而全球化的网络上构建、管理和部署应用程序。常用于部署机器学习流水线的其他云服务有[Amazon Web Services (AWS)](https://aws.amazon.com/)、[Google Cloud](https://cloud.google.com)、[IBM Cloud](https://www.ibm.com/cloud)和[阿里云](https://www.alibabacloud.com/)。我们将在未来的教程中涵盖它们中的大部分。

如果您之前没有使用过 Microsoft Azure，您可以在这里[注册](https://azure.microsoft.com/en-ca/free/search/?\&ef\_id=EAIaIQobChMIm8Onqp6i6QIViY7ICh2QVA2jEAAYASAAEgK9FvD\_BwE:G:s\&OCID=AID2000061\_SEM\_EAIaIQobChMIm8Onqp6i6QIViY7ICh2QVA2jEAAYASAAEgK9FvD\_BwE:G:s\&dclid=CK6R8aueoukCFVbJyAoduGYLcQ)免费注册一个帐户。首次注册时，您将获得前30天的免费信用额度。您可以利用这个信用额度按照本教程构建自己的 Web 应用程序。

### 什么是容器，为什么我们需要它？

您是否曾经遇到过这样的问题：您的 Python 代码（或任何其他代码）在您的计算机上运行正常，但当您的朋友尝试运行完全相同的代码时，却无法正常工作？如果您的朋友重复了完全相同的步骤，他们应该得到相同的结果，对吗？这个问题的答案是——**环境**。您朋友的 Python 环境与您的不同。
一个环境包括什么？→ Python（或者你使用的其他语言）以及应用程序构建和测试时使用的所有库和依赖项的确切版本。

如果我们能够创建一个可以转移到其他机器上的环境（例如：你朋友的电脑或者像 Microsoft Azure 这样的云服务提供商），我们就可以在任何地方重现结果。因此，**容器是一种软件类型，它将应用程序及其所有依赖项打包起来，以便应用程序可以在不同的计算环境中可靠地运行**。

> ## “当你想到容器时，就想到集装箱。”

![https://www.freepik.com/free-photos-vectors/cargo-ship](https://cdn-images-1.medium.com/max/2000/1\*SlzsvRhA71oFOhAjE1Hs0A.jpeg)

这是最直观理解数据科学中容器的方式。**它们就像船上的集装箱一样**，目标是将一个集装箱的内容与其他集装箱隔离开来，以免混淆。这正是容器在数据科学中的用途。

现在我们理解了容器背后的隐喻，让我们来看看创建应用程序的隔离环境的其他选择。一个简单的替代方案是为每个应用程序使用单独的机器。

（1 台机器 = 1 个应用程序 = 没有冲突 = 一切都好）

使用单独的机器是直接的，但它无法与使用容器的好处相提并论，因为为每个应用程序维护多台机器是昂贵的、难以维护的，而且难以扩展。简而言之，在许多实际场景中，这是不切实际的。

创建隔离环境的另一种选择是**虚拟机**。在这里，容器再次更可取，因为它们需要更少的资源，非常便携，并且启动速度更快。

![虚拟机 vs. 容器](https://cdn-images-1.medium.com/max/3840/1\*snINBI0HUIYa0BWKyCO3Xg.jpeg)

你能看出虚拟机和容器之间的区别吗？当你使用容器时，你不需要虚拟机操作系统。想象一下在虚拟机上运行的 10 个应用程序，这将需要 10 个虚拟机操作系统，而使用容器时则不需要。

#### 我了解容器，但 Docker 是什么？

Docker 是一家提供软件（也称为 Docker）的公司，允许用户构建、运行和管理容器。虽然 Docker 的容器是最常见的，但还有其他不那么著名的替代品，如 [LXD](https://linuxcontainers.org/lxd/introduction/) 和 [LXC](https://linuxcontainers.org/) 提供容器解决方案。

在本教程中，我们将使用 **Docker Desktop for Windows** 创建一个容器，并将其发布到 Azure 容器注册表。然后，我们将使用该容器部署一个 Web 应用程序。

![](https://cdn-images-1.medium.com/max/2000/1\*EJx9QN4ENSPKZuz51rC39w.png)

#### Docker 镜像 vs. Docker 容器

Docker 镜像和 Docker 容器有什么区别？这是迄今为止最常见的问题，让我们马上澄清一下。有很多技术定义可用，但直观地认为 Docker 镜像是基于其创建容器的模具。镜像本质上是容器的快照。

如果你更喜欢稍微更技术性的定义，那么可以考虑这个：Docker 镜像在运行时在 Docker 引擎上运行时变成容器。

#### 打破炒作：

归根结底，Docker 只是一个带有几行指令的文件，保存在你的项目文件夹下，名为 **“Dockerfile”**。

另一种思考 Docker 文件的方式是，它们就像你在自己的厨房里发明的食谱。当你与其他人分享这些食谱，并且他们按照完全相同的指令操作时，他们能够制作出相同的菜肴。类似地，你可以与其他人分享你的 Docker 文件，然后他们可以基于该 Docker 文件创建镜像并运行容器。

现在你理解了容器、Docker 以及为什么我们应该使用它们，让我们快速设置业务背景。

### 设置业务背景

一家保险公司希望通过在住院时使用人口统计和基本患者健康风险指标更好地预测患者费用，从而改善现金流预测。

![](https://cdn-images-1.medium.com/max/2000/1\*qM1HiWZ\_uigwdcZ0\_ZL6yA.png)

_（[数据来源](https://www.kaggle.com/mirichoi0218/insurance#insurance.csv)）_

### 目标

构建和部署一个 Web 应用程序，其中输入患者的人口统计和健康信息，然后输出预测的费用金额。

### 任务

* 为部署训练和开发一个机器学习流水线。
* 使用 Flask 框架构建一个 Web 应用程序。它将使用训练好的机器学习流水线对实时的新数据点进行预测。
* 创建一个 Docker 镜像和容器。
* 将容器发布到 Azure 容器注册表（ACR）。
* 通过发布到 ACR 在容器中部署 Web 应用程序。一旦部署完成，它将变为公开可用，并可以通过 Web URL 访问。

由于我们在上一篇教程中已经涵盖了前两个任务，我们将快速回顾它们，并专注于上面列表中剩余的任务。如果您有兴趣了解如何使用 PyCaret 在 Python 中开发机器学习流水线，并使用 Flask 框架构建 Web 应用程序，可以阅读我们的[上一篇教程](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)。

### 👉 开发机器学习流水线

我们在 Python 中使用 PyCaret 进行训练和开发机器学习流水线，该流水线将作为我们的 Web 应用程序的一部分使用。机器学习流水线可以在集成开发环境（IDE）或笔记本中开发。我们使用笔记本运行以下代码：

当您在 PyCaret 中保存模型时，将根据 \*\*setup() \*\*函数中定义的配置创建整个转换流水线。所有相互依赖关系都会自动协调。请参见存储在 'deployment\_28042020' 变量中的流水线和模型：

![使用 PyCaret 创建的机器学习流水线](https://cdn-images-1.medium.com/max/2000/1\*NWoHVWJzO7i7gIvrlBnIiQ.png)

### 👉 构建 Web 应用程序

本教程不重点讨论构建 Flask 应用程序。这里只是为了完整性而讨论。现在我们的机器学习流水线已经准备好了，我们需要一个可以连接到我们训练好的流水线的 Web 应用程序，以实时生成新数据点的预测。我们使用 Python 中的 Flask 框架创建了 Web 应用程序。该应用程序由两部分组成：

* 前端（使用 HTML 设计）
* 后端（使用 Flask 开发）

这是我们的 Web 应用程序的外观：

![在本地机器上打开的 Web 应用程序](https://cdn-images-1.medium.com/max/2800/1\*EU4Cp9w1YHS2om8Mmfqh2g.png)

如果您想看到这个 Web 应用程序的实际效果，[点击这里](https://pycaret-insurance.herokuapp.com/)打开在 Heroku 上部署的 Web 应用程序（_可能需要几分钟才能打开_）。

如果您没有跟随操作，没问题。您可以从 GitHub 上简单地 fork 这个[存储库](https://github.com/pycaret/deployment-heroku)。如果您不知道如何 fork 一个存储库，请阅读这个[官方 GitHub 教程](https://help.github.com/en/github/getting-started-with-github/fork-a-repo)。此时，您的项目文件夹应该如下所示：

![https://github.com/pycaret/deployment-heroku](https://cdn-images-1.medium.com/max/2524/1\*D2LzCUWv5au7AI6dsgGjUA.png)

现在我们有了一个完全功能的 Web 应用程序，我们可以开始使用 Docker 将应用程序容器化的过程。

### 使用 Docker 部署 ML 流水线的 10 个步骤：

#### 👉 **第 1 步 — 安装 Windows 版 Docker Desktop**

您可以在 Mac 上使用 Docker Desktop，也可以在 Windows 上使用。根据您的操作系统，您可以从[此链接](https://docs.docker.com/docker-for-windows/install/)下载 Docker Desktop。本教程中我们将使用 Windows 版 Docker Desktop。

![https://hub.docker.com/editions/community/docker-ce-desktop-windows/](https://cdn-images-1.medium.com/max/2692/1\*jVBJIDIUyw9UJbzpv2zbpQ.png)

最简单的检查安装是否成功的方法是打开命令提示符并输入 'docker'。它应该打印帮助菜单。

![命令提示符](https://cdn-images-1.medium.com/max/2200/1\*5XYrNYDi6XlLrmIO4ZNHdQ.png)

#### 👉 **第 2 步 — 安装 Kitematic**

Kitematic 是一个直观的图形用户界面（GUI），用于在 Windows 或 Mac 上运行 Docker 容器。您可以从[Docker 的 GitHub 存储库](https://github.com/docker/kitematic/releases)下载 Kitematic。

![https://github.com/docker/kitematic/releases](https://cdn-images-1.medium.com/max/2508/1\*Tl5M7tNVH8smsnkaihxfpA.png)

下载后，只需将文件解压缩到所需位置即可。

#### 👉 第 3 步 — 创建 Dockerfile

创建 Docker 镜像的第一步是创建 Dockerfile。Dockerfile 只是一个带有一组指令的文件。本项目的 Dockerfile 如下所示：

Dockerfile 是区分大小写的，并且必须与其他项目文件放在同一个项目文件夹中。Dockerfile 没有扩展名，可以使用任何编辑器创建。我们使用[Visual Studio Code](https://code.visualstudio.com/)创建它。

#### 👉 第 4 步 — 创建 Azure 容器注册表
如果您没有 Microsoft Azure 账户或之前没有使用过，您可以免费[注册](https://azure.microsoft.com/zh-cn/free/search/?&ef_id=EAIaIQobChMIm8Onqp6i6QIViY7ICh2QVA2jEAAYASAAEgK9FvD_BwE:G:s&OCID=AID2000061_SEM_EAIaIQobChMIm8Onqp6i6QIViY7ICh2QVA2jEAAYASAAEgK9FvD_BwE:G:s&dclid=CK6R8aueoukCFVbJyAoduGYLcQ)。首次注册时，您将获得首个30天的免费信用额度。您可以使用这个信用额度在 Azure 上构建和部署 Web 应用程序。注册后，请按照以下步骤操作：

* 登录 [https://portal.azure.com](https://portal.azure.com)。
* 点击“创建资源”。
* 搜索“容器注册表”，然后点击“创建”。
* 选择订阅、资源组和注册表名称（在我们的例子中：**pycaret.azurecr.io** 是我们的注册表名称）。

![https://portal.azure.com → 登录 → 创建资源 → 容器注册表](https://cdn-images-1.medium.com/max/2560/1*InmsXcD7yfbeaMMzobwIJQ.png)

#### 👉 第5步— 构建 Docker 镜像

在 Azure 门户中创建注册表后，第一步是使用命令行构建一个 Docker 镜像。进入项目文件夹并执行以下代码。

```
docker build -t pycaret.azurecr.io/pycaret-insurance:latest . 
```

![使用 Anaconda 提示构建 Docker 镜像](https://cdn-images-1.medium.com/max/2566/1*6cPcluJCHV8cpgziPXcGzw.png)

* **pycaret.azurecr.io** 是在 Azure 门户上创建资源时获得的注册表名称。
* **pycaret-insurance** 是镜像的名称，\*\*latest\*\* 是标签。这可以是任何您想要的内容。

#### 👉 第6步— 从 Docker 镜像运行容器

现在镜像已经创建，我们将在本地运行一个容器，并在将其推送到 Azure 容器注册表之前测试应用程序。执行以下代码在本地运行容器：

```
docker run -d -p 5000:5000 pycaret.azurecr.io/pycaret-insurance
```

成功执行此命令后，它将返回创建的容器的 ID。

![在本地运行 Docker 容器](https://cdn-images-1.medium.com/max/2566/1*9g7OQNUA_8zLekDdWa3LHQ.png)

#### 👉 第7步— 在本地机器上测试容器

打开 Kitematic，您应该能够看到一个正在运行的应用程序。

![Kitematic — 用于管理 Mac 和 Windows 操作系统上容器的图形用户界面](https://cdn-images-1.medium.com/max/2690/1*CyJZ98AI5q7HbRa__-KWfg.png)

您可以通过在 Internet 浏览器中输入 localhost:5000 来查看应用程序的运行情况。它应该打开一个 Web 应用程序。

![在本地容器上运行的应用程序（localhost:5000）](https://cdn-images-1.medium.com/max/3824/1*wtDSmSt3Nsh1qQP7DC_kBg.png)

确保在完成后使用 Kitematic 停止应用程序，否则它将继续占用您计算机上的资源。

#### 👉 第8步— 验证 Azure 凭据

在将容器上传到 ACR 之前，还有一个最后的步骤是在本地机器上验证 Azure 凭据。在命令行中执行以下代码来完成验证：

```
docker login pycaret.azurecr.io
```

系统会提示您输入用户名和密码。用户名是您的注册表名称（在此示例中用户名为“pycaret”）。您可以在您创建的 Azure 容器注册表资源的访问密钥下找到密码。

![portal.azure.com → Azure 容器注册表 → 访问密钥](https://cdn-images-1.medium.com/max/3792/1*5pEA3466EIedSiPhe9CGcQ.png)

#### 👉 第9步— 将容器推送到 Azure 容器注册表

现在您已经验证了 ACR，可以通过执行以下代码将您创建的容器推送到 ACR：

```
docker push pycaret.azurecr.io/pycaret-insurance:latest
```

根据容器的大小，推送命令可能需要一些时间将容器传输到云端。

#### 👉 第10步— 创建 Azure Web 应用程序并查看模型运行情况

要在 Azure 上创建 Web 应用程序，请按照以下步骤操作：

* 登录 [https://portal.azure.com](https://portal.azure.com)。
* 点击“创建资源”。
* 搜索“Web 应用程序”，然后点击“创建”。
* 将您在（第9步）中推送的 ACR 镜像链接到您的应用程序。

![portal.azure.com → Web 应用程序 → 创建 → 基本信息](https://cdn-images-1.medium.com/max/2032/1*_4aEC8X867ybKhGrIl-L6A.png)

![portal.azure.com → Web 应用程序 → 创建 → Docker](https://cdn-images-1.medium.com/max/2170/1*kcZWeLbntnrUKRWTE7EzkQ.png)

**BOOM!! 应用程序现在在 Azure Web 服务上运行。**

![https://pycaret-insurance2.azurewebsites.net](https://cdn-images-1.medium.com/max/3812/1*zElHKEUtI_7NiEW6C5Z7dw.png)

**注意：**在发布此故事时，[https://pycaret-insurance2.azurewebsites.net](https://pycaret-insurance2.azurewebsites.net) 上的应用程序将被删除以限制资源消耗。

[\*\*本教程的 GitHub 存储库链接。](https://github.com/pycaret/pycaret-deployment-azure)\*\*

[\*\*Heroku 部署的 GitHub 存储库链接。](https://www.github.com/pycaret/deployment-heroku) _(不使用 Docker)_\*\*

### 下一个教程
在下一个部署机器学习流水线的教程中，我们将深入探讨如何使用 Google Cloud 和 Microsoft Azure 上的 Kubernetes 服务部署机器学习流水线。

关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 并订阅我们的 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g) 频道，以了解更多关于 PyCaret 的信息。

### 重要链接

[用户指南 / 文档](https://www.pycaret.org/guide) [GitHub 仓库](https://www.github.com/pycaret/pycaret) [安装 PyCaret](https://www.pycaret.org/install) [笔记本教程](https://www.pycaret.org/tutorial) [为 PyCaret 做贡献](https://www.pycaret.org/contribute)

### PyCaret 1.0.1 即将发布！

我们收到了社区的大力支持和反馈。我们正在积极努力改进 PyCaret 并为下一个版本做准备。**PyCaret 1.0.1 将更加强大和优秀**。如果您想分享您的反馈并帮助我们进一步改进，您可以在网站上 [填写此表格](https://www.pycaret.org/feedback) 或在我们的 [GitHub](https://www.github.com/pycaret/) 或 [LinkedIn](https://www.linkedin.com/company/pycaret/) 页面留言。

### 想了解特定模块吗？

截至首个版本 1.0.0，PyCaret 提供以下可用模块。点击下面的链接查看 Python 中的文档和示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 还可以查看：

PyCaret 笔记本中的入门教程：

[聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101) [回归](https://www.pycaret.org/reg101) [分类](https://www.pycaret.org/clf101)

### 想要贡献吗？

PyCaret 是一个开源项目。欢迎每个人贡献。如果您想贡献，请随时处理 [开放问题](https://github.com/pycaret/pycaret/issues)。我们接受带有单元测试的拉取请求，提交到 dev-1.0.1 分支。

如果您喜欢 PyCaret，请在我们的 [GitHub 仓库](https://www.github.com/pycaret/pycaret) 上给我们 ⭐️。

Medium：[https://medium.com/@moez_62905/](https://medium.com/@moez_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

LinkedIn：[https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter：[https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)