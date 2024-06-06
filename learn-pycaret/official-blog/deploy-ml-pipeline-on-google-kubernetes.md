# 在Google Kubernetes上部署机器学习流水线

### 在Google Kubernetes Engine上部署机器学习流水线

#### 作者：Moez Ali

![一步一步的初学者指南：如何在Google Kubernetes Engine上容器化和部署机器学习流水线](https://cdn-images-1.medium.com/max/2000/1\*P-JjI7MXq6UJV9Xab-B9qg.png)

### 复习

在我们上一篇关于在云端部署机器学习流水线的文章中，我们演示了如何使用PyCaret开发机器学习流水线，使用Docker进行容器化，并使用Microsoft Azure Web App Services作为Web应用程序进行服务。如果您之前没有听说过PyCaret，请阅读这篇[公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)以了解更多信息。

在本教程中，我们将使用之前构建和部署的相同机器学习流水线和Flask应用程序。这次我们将演示如何在Google Kubernetes Engine上容器化和部署机器学习流水线。

### 👉 本教程的学习目标

* 了解什么是容器，什么是Docker，什么是Kubernetes，以及什么是Google Kubernetes Engine？
* 构建Docker镜像并将其上传到Google容器注册表（GCR）。
* 创建集群并部署一个带有Flask应用程序的机器学习流水线作为Web服务。
* 查看一个使用训练好的机器学习流水线实时预测新数据点的Web应用程序。

之前我们演示了如何在Heroku PaaS上部署机器学习流水线，以及如何使用Docker容器在Azure Web Services上部署机器学习流水线。

本教程将涵盖从构建Docker镜像、将其上传到Google容器注册表，然后部署预训练的机器学习流水线和Flask应用程序到Google Kubernetes Engine（GKE）的整个工作流程。

### 💻 本教程的工具箱

### PyCaret

[PyCaret](https://www.pycaret.org/)是一个开源的、低代码的Python机器学习库，用于训练和部署机器学习流水线和模型到生产环境中。可以使用pip轻松安装PyCaret。

```
pip install pycaret
```

### Flask

[Flask](https://flask.palletsprojects.com/en/1.1.x/)是一个允许您构建Web应用程序的框架。Web应用程序可以是商业网站、博客、电子商务系统，或者是使用训练好的模型实时提供数据预测的应用程序。如果您没有安装Flask，可以使用pip安装它。

### Google Cloud Platform

Google Cloud Platform（GCP）是由Google提供的一套云计算服务，运行在Google用于其终端用户产品（如Google搜索、Gmail和YouTube）的相同基础设施上。如果您还没有GCP帐户，可以在[这里](https://console.cloud.google.com/getting-started)注册。如果您是第一次注册，您将获得1年的免费信用额度。

### 让我们开始吧。

在我们开始学习Kubernetes之前，让我们先了解一下什么是容器以及为什么我们需要它？

![https://www.freepik.com/free-photos-vectors/cargo-ship](https://cdn-images-1.medium.com/max/2000/1\*SlzsvRhA71oFOhAjE1Hs0A.jpeg)

您是否曾经遇到过这样的问题：您的代码在您的计算机上运行正常，但是当朋友尝试运行完全相同的代码时，却无法正常工作？如果您的朋友重复了完全相同的步骤，他或她应该得到相同的结果，对吗？这个问题的答案是**环境**。您朋友的环境与您的环境不同。

环境包括什么？→ 编程语言（如Python）以及使用该语言构建和测试应用程序时使用的所有库和依赖项的确切版本。

如果我们可以创建一个可以传输到其他计算机上的环境（例如：您朋友的计算机或像Google Cloud Platform这样的云服务提供商），我们就可以在任何地方重现结果。因此，**容器**是一种软件类型，它将应用程序及其所有依赖项打包在一起，以便应用程序可以在不同的计算环境中可靠地运行。

> 那么Docker是什么呢？

![](https://cdn-images-1.medium.com/max/2000/1\*EJx9QN4ENSPKZuz51rC39w.png)

Docker是一家提供软件（也称为Docker）的公司，允许用户构建、运行和管理容器。虽然Docker的容器是最常见的，但还有其他不太知名的**替代品**，如[LXD](https://linuxcontainers.org/lxd/introduction/)和[LXC](https://linuxcontainers.org/)提供容器解决方案。

现在您了解了容器和特别是Docker，让我们来了解一下Kubernetes是什么。
Kubernetes 是由 Google 在 2014 年开发的一款强大的开源系统，用于管理容器化应用程序。简单来说，Kubernetes 是一个用于在机器集群上运行和协调容器化应用程序的系统。它是一个旨在完全管理容器化应用程序生命周期的平台。

![Photo by chuttersnap on Unsplash](https://cdn-images-1.medium.com/max/23216/0*2ZayMwt1Un8-9ZFA)

### 特点

✔️ **负载均衡：** 自动在容器之间分配负载。

✔️ **扩展：** 当需求发生变化，例如高峰时段、周末和假期，通过增加或减少容器来自动扩展。

✔️ **存储：** 保持多个应用程序实例的存储一致。

✔️ **自愈：** 自动重新启动失败的容器，并终止不响应用户定义健康检查的容器。

✔️ **自动化部署：** 您可以自动化 Kubernetes 来为您的部署创建新容器，删除现有容器，并将它们的所有资源转移到新容器。

### 如果已经使用 Docker，为什么还需要 Kubernetes？

想象一下这样一个场景：您需要在多台机器上运行多个 Docker 容器，以支持一个企业级的机器学习应用程序，在白天和黑夜有不同的工作负载。尽管听起来很简单，但手动完成这项工作是一项繁重的任务。

您需要在正确的时间启动正确的容器，找出它们如何相互通信，处理存储考虑事项，并处理失败的容器或硬件。Kubernetes 解决了这个问题，允许大量容器和谐地协同工作，减轻了运维负担。

> ## 将 **Docker 与 Kubernetes 进行比较是错误的。** 这是两种不同的技术。Docker 是一种软件，允许您将应用程序容器化，而 Kubernetes 是一个容器管理系统，允许创建、扩展和监控数百甚至数千个容器。

在任何应用程序的生命周期中，Docker 用于在部署时打包应用程序，而 Kubernetes 用于管理应用程序的其余生命周期。

![通过 Kubernetes / Docker 部署的应用程序生命周期](https://cdn-images-1.medium.com/max/3200/1*dBJjxZrfdMppXhdwjZLX6w.png)

### 什么是 Google Kubernetes Engine？

Google Kubernetes Engine 是在 Google Cloud Platform 上实现 _Google 的开源 Kubernetes_ 的系统。简单明了！

GKE 的其他热门替代方案包括 [Amazon ECS](https://aws.amazon.com/ecs/) 和 [Microsoft Azure Kubernetes Service](https://azure.microsoft.com/en-us/services/kubernetes-service/)。

### 最后一次，您是否理解了这些内容？

* **容器** 是一种软件类型，将应用程序及其所有依赖项打包，使应用程序能够在一个计算环境中可靠地运行到另一个计算环境。
* **Docker** 是用于构建和管理容器的软件。
* **Kubernetes** 是一个用于在集群环境中管理容器化应用程序的开源系统。
* **Google Kubernetes Engine** 是在 Google Cloud Platform 上实现开源 Kubernetes 框架的系统。

在本教程中，我们将使用 Google Kubernetes Engine。为了跟随操作，请确保您拥有 Google Cloud Platform 账户。[点击这里](https://console.cloud.google.com/getting-started) 免费注册账户。

### 设定业务背景

一家保险公司希望通过在患者入院时使用人口统计和基本患者健康风险指标更好地预测患者费用，从而改进现金流预测。

![](https://cdn-images-1.medium.com/max/2000/1*qM1HiWZ_uigwdcZ0_ZL6yA.png)

_(_[_数据来源_](https://www.kaggle.com/mirichoi0218/insurance#insurance.csv)_)_

### 目标

构建并部署一个 Web 应用程序，用户可以在基于 Web 的表单中输入患者的人口统计和健康信息，然后输出预测的费用金额。

### 任务

* 为部署训练和开发一个机器学习管道。
* 使用 Flask 框架构建 Web 应用程序。它将使用训练好的机器学习管道实时生成新数据点的预测。
* 构建一个 Docker 镜像并将容器上传到 Google Container Registry (GCR)。
* 创建集群并在 Google Kubernetes Engine 上部署应用程序。

由于我们已经在初始教程中涵盖了前两项任务，我们将快速回顾它们，然后专注于上述列表中的剩余项目。如果您有兴趣了解如何使用 PyCaret 在 Python 中开发机器学习管道以及使用 Flask 框架构建 Web 应用程序，请阅读[此教程](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)。

### 👉 开发一个机器学习管道
我们正在使用 Python 中的 PyCaret 进行训练和开发机器学习管道，该管道将作为我们 Web 应用的一部分。机器学习管道可以在集成开发环境（IDE）或笔记本中开发。我们已经使用笔记本来运行下面的代码：

当你在 PyCaret 中保存一个模型时，基于 \*\*setup() \*\*函数中定义的配置，将创建整个转换管道。所有相互依赖关系都会自动协调。查看存储在 ‘deployment\_28042020’ 变量中的管道和模型：

![使用 PyCaret 创建的机器学习管道](https://cdn-images-1.medium.com/max/2000/1\*P7EXfIxqZZGrpeLgDdk1vQ.png)

### 👉 构建 Web 应用

本教程不侧重于构建 Flask 应用程序。这里仅讨论完整性。现在我们的机器学习管道已经准备就绪，我们需要一个 Web 应用程序，可以连接到我们训练好的管道，以便实时生成新数据点的预测。我们使用 Python 中的 Flask 框架创建了 Web 应用程序。该应用程序有两个部分：

* 前端（使用 HTML 设计）
* 后端（使用 Flask 开发）

这是我们的 Web 应用程序外观：

![本地机器上的 Web 应用程序](https://cdn-images-1.medium.com/max/3780/1\*tc\_6S8NztYKB85rPUJd1uQ.png)

如果到目前为止你还没有跟随进行，没问题。你可以简单地从 GitHub 上 fork 这个 [仓库](https://github.com/pycaret/pycaret-deployment-google)。此时你的项目文件夹应该是这个样子的：

![https://www.github.com/pycaret/pycaret-deployment-google](https://cdn-images-1.medium.com/max/3796/1\*CcId22jB-BMCen8o1hWNdQ.png)

现在我们有一个完全功能的 Web 应用程序，我们可以开始将该应用程序容器化并部署到 Google Kubernetes Engine。

### 在 Google Kubernetes Engine 上部署机器学习管道的 10 个步骤：

#### 👉 步骤 1 — 在 GCP 控制台中创建新项目

登录到您的 GCP 控制台，然后转到“管理资源”

![Google 云平台控制台 → 管理资源](https://cdn-images-1.medium.com/max/3832/1\*OS16COUUns7uBnpyUxH9-w.png)

点击 **创建新项目**

![Google 云平台控制台 → 管理资源 → 创建新项目](https://cdn-images-1.medium.com/max/3834/1\*QJz8fITeJJWP44yPm2v4vQ.png)

#### 👉 步骤 2 — 导入项目代码

点击控制台窗口顶部的 \*\*激活 Cloud Shell \*\*按钮以打开 Cloud Shell。

![Google 云平台（项目信息页面）](https://cdn-images-1.medium.com/max/3834/1\*Mbcd4RlkCcz98Pbf4KSUAA.png)

在 Cloud Shell 中执行以下代码以克隆本教程中使用的 GitHub 仓库。

```
git clone https://github.com/pycaret/pycaret-deployment-google.git
```

![git clone https://github.com/pycaret/pycaret-deployment-google.git](https://cdn-images-1.medium.com/max/3838/1\*g\_RQ30jDG4UsyS84mh-qrw.png)

#### 👉 步骤 3 — 设置项目 ID 环境变量

执行以下代码设置 PROJECT\_ID 环境变量。

```
export PROJECT_ID=**pycaret-kubernetes-demo**
```

_pycaret-kubernetes-demo_ 是我们在上述第 1 步中选择的项目名称。

#### 👉 步骤 4 — 构建 Docker 镜像

通过执行以下代码构建应用程序的 Docker 镜像并为上传打标签：

```
docker build -t gcr.io/${PROJECT_ID}/insurance-app:v1 .
```

![成功构建 Docker 时返回的消息](https://cdn-images-1.medium.com/max/3834/1\*Zo7\_W7pG6JhFvHbzyQeEsA.png)

您可以通过运行以下代码来检查可用的镜像：

```
docker images
```

![在 Cloud Shell 上运行“docker images”命令的输出](https://cdn-images-1.medium.com/max/3834/1\*0paobe\_W8tmdCF1xhX4BgA.png)

#### 👉 步骤 5 — 上传容器镜像

1.  认证到 [Container Registry](https://cloud.google.com/container-registry)（您只需要运行一次）：

    gcloud auth configure-docker
2.  执行以下代码将 Docker 镜像上传到 Google Container Registry：

    docker push gcr.io/${PROJECT\_ID}/insurance-app:v1

#### 👉 步骤 6 — 创建集群

现在容器已上传，您需要一个集群来运行容器。集群包含一组运行 Kubernetes 的 Compute Engine VM 实例。

1.  为 gcloud 工具设置您的项目 ID 和 Compute Engine 区域选项：

    gcloud config set project $PROJECT\_ID gcloud config set compute/zone **us-central1**
2.  通过执行以下代码创建一个集群：

    gcloud container clusters create **insurance-cluster** --num-nodes=2

![Google 云平台 → Kubernetes Engine → 集群](https://cdn-images-1.medium.com/max/3832/1\*l2sHrv5nuFjDKiyAtjYapQ.png)

#### 👉 步骤 7 — 部署应用程序

要在 GKE 集群上部署和管理应用程序，您必须与 Kubernetes 集群管理系统通信。执行以下命令部署应用程序：

```
kubectl create deployment insurance-app --image=gcr.io/${PROJECT_ID}/insurance-app:v1
```
![通过 kubectl 创建部署返回的输出](https://cdn-images-1.medium.com/max/3836/1\*p0\_A6PZnfYJ4mnttM7lzzA.png)

#### 👉 第 8 步— 将您的应用程序暴露给互联网

默认情况下，在 GKE 上运行的容器无法从互联网访问，因为它们没有外部 IP 地址。执行以下代码将应用程序暴露给互联网：

```
kubectl expose deployment insurance-app --type=LoadBalancer --port 80 --target-port 8080
```

#### 👉 第 9 步— 检查服务

执行以下代码以获取服务的状态。**EXTERNAL-IP** 是您可以在浏览器中使用的网址，用于查看发布的应用程序。

```
kubectl get service
```

![Cloud Shell → kubectl get service](https://cdn-images-1.medium.com/max/3832/1\*aRWl7frtmvPYaYjAoloFgQ.png)

👉 第 10 步— 在 [http://34.71.77.61:8080](http://34.71.77.61:8080) 上查看应用程序运行情况

![最终应用程序上传至 http://34.71.77.61:8080](https://cdn-images-1.medium.com/max/3838/1\*bKuZiYSPdE8T5SLKXx5B\_Q.png)

**注意：** 在发布此故事时，该应用程序将从公共地址中移除，以限制资源消耗。

[此教程的 GitHub 仓库链接](https://www.github.com/pycaret/pycaret-deployment-google)

[Microsoft Azure 部署的 GitHub 仓库链接](https://www.github.com/pycaret/pycaret-azure-deployment)

[Heroku 部署的 GitHub 仓库链接](https://www.github.com/pycaret/deployment-heroku)

### PyCaret 1.0.1 即将发布！

我们收到了社区的大力支持和反馈。我们正在积极改进 PyCaret 并为下一个版本做准备。**PyCaret 1.0.1 将更加强大和优秀**。如果您想分享您的反馈并帮助我们进一步改进，您可以在网站上[填写此表格](https://www.pycaret.org/feedback)或在我们的[GitHub](https://www.github.com/pycaret/)或[LinkedIn](https://www.linkedin.com/company/pycaret/)页面上留言。

关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)并订阅我们的[YouTube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)频道，了解更多关于 PyCaret 的信息。

### 想了解特定模块吗？

在首次发布的 1.0.0 版本中，PyCaret 提供了以下可用于使用的模块。点击下面的链接查看 Python 中的文档和示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 还可以查看：

PyCaret 在 Notebook 中的入门教程：

[聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101) [回归](https://www.pycaret.org/reg101) [分类](https://www.pycaret.org/clf101)

### 想要贡献吗？

PyCaret 是一个开源项目。欢迎每个人贡献。如果您想贡献，请随时处理[开放问题](https://github.com/pycaret/pycaret/issues)。在 dev-1.0.1 分支上接受带有单元测试的拉取请求。

如果您喜欢 PyCaret，请在我们的[GitHub 仓库](https://www.github.com/pycaret/pycaret)上给我们 ⭐️。

Medium：[https://medium.com/@moez\_62905/](https://medium.com/@moez\_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

LinkedIn：[https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter：[https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)