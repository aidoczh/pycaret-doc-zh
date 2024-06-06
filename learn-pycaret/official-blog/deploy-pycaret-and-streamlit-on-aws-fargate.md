# 在 AWS Fargate 上部署 PyCaret 和 Streamlit

### 使用 AWS Fargate 部署 PyCaret 和 Streamlit 应用程序 —— 无服务器基础设施

#### 作者：Moez Ali

![一个逐步指南，教你如何在 AWS Fargate 上将机器学习流水线容器化并部署](https://cdn-images-1.medium.com/max/2000/1\*QznGlPsGrGQS4DadTunLXw.png)

### 回顾

在我们的[上一篇文章](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)中，我们演示了如何使用 PyCaret 构建机器学习流水线，并将其作为 Streamlit 网络应用程序部署到 Google Kubernetes Engine。如果你之前没有听说过 PyCaret，你可以阅读这个[公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)来开始了解。

在本教程中，我们将使用之前构建的相同的网络应用程序和机器学习流水线，并演示如何使用 AWS Fargate 部署它，AWS Fargate 是用于容器的无服务器计算。

通过本教程，你将能够在 AWS 上构建和托管一个完全功能的容器化网络应用程序，而无需提供任何服务器基础设施。

![网络应用程序](https://cdn-images-1.medium.com/max/2800/1\*TesAmfCyanOeMEPiYxInUg.png)

### 👉 本教程的学习目标

* 什么是容器？什么是 Docker？什么是 Kubernetes？
* 什么是 Amazon Elastic Container Service (ECS)、AWS Fargate 和无服务器部署？
* 构建并推送 Docker 镜像到 Amazon Elastic Container Registry。
* 使用无服务器基础设施（即 AWS Fargate）部署网络应用程序。

本教程将涵盖从本地构建 Docker 镜像、上传到 Amazon Elastic Container Registry、创建集群，然后使用 AWS 托管基础设施定义和执行任务的整个工作流程。

在过去，我们已经介绍了在其他云平台上的部署，比如 Azure 和 Google。如果你对这些感兴趣，可以阅读以下教程：

* [将 Streamlit 应用程序部署到 Google Kubernetes Engine](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)
* [使用 PyCaret 和 Streamlit 构建和部署机器学习网络应用程序](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)
* [在 AWS Fargate 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507)
* [在 Google Kubernetes Engine 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b)
* [在 AWS Web Service 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01)
* [在 Heroku PaaS 上构建和部署你的第一个机器学习网络应用程序](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)

### 💻 本教程的工具箱

### PyCaret

[PyCaret](https://www.pycaret.org/) 是一个开源的、低代码的 Python 机器学习库，用于训练和部署机器学习流水线和模型到生产环境中。可以使用 pip 轻松安装 PyCaret。

```
pip install pycaret
```

### Streamlit

[Streamlit](https://www.streamlit.io/) 是一个开源的 Python 库，可以轻松构建漂亮的定制化机器学习和数据科学 Web 应用程序。可以使用 pip 轻松安装 Streamlit。

```
pip install streamlit
```

### 适用于 Windows 10 Home 的 Docker Toolbox

[Docker](https://www.docker.com/) 是一个旨在通过使用容器来更轻松地创建、部署和运行应用程序的工具。容器用于将应用程序及其所需的所有组件（如库和其他依赖项）打包在一起，并将其作为一个整体进行传输。如果你之前没有使用过 Docker，本教程还涵盖了在 **Windows 10 Home** 上安装 Docker Toolbox（旧版）。在[之前的教程](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01)中，我们介绍了如何在 **Windows 10 Pro 版本**上安装 Docker Desktop。

### 亚马逊网络服务（AWS）

亚马逊网络服务（AWS）是由亚马逊提供的一个全面且广泛采用的云平台。它拥有全球数据中心的超过175个功能齐全的服务。如果你之前没有使用过 AWS，你可以[注册](https://aws.amazon.com/)一个免费账户。

### ✔️ 让我们开始吧……

### 什么是容器？

在我们开始使用 AWS Fargate 进行实现之前，让我们了解一下什么是容器，以及为什么我们需要它？

![https://www.freepik.com/free-photos-vectors/cargo-ship](https://cdn-images-1.medium.com/max/2000/1\*SlzsvRhA71oFOhAjE1Hs0A.jpeg)
你是否遇到过这样的问题：你的代码在你的电脑上运行得很好，但是当朋友尝试运行完全相同的代码时，却无法正常工作？如果你的朋友重复了完全相同的步骤，他们应该得到相同的结果，对吗？这个问题的答案是**环境**。你朋友的环境与你的不同。

环境包括什么？→ 编程语言，如Python，以及使用该语言构建和测试应用程序时所使用的所有库和依赖项的确切版本。

如果我们可以创建一个可以传输到其他机器上的环境（例如：你朋友的电脑或像Google Cloud Platform这样的云服务提供商），我们就可以在任何地方重现结果。因此，**容器**是一种软件类型，它将应用程序及其所有依赖项打包起来，以便应用程序可以在不同的计算环境中可靠地运行。

### 什么是Docker？

Docker是一家提供软件（也称为Docker）的公司，允许用户构建、运行和管理容器。虽然Docker的容器是最常见的，但还有其他不太出名的替代品，如[LXD](https://linuxcontainers.org/lxd/introduction/)和[LXC](https://linuxcontainers.org/)。

![](https://cdn-images-1.medium.com/max/2000/1\*EJx9QN4ENSPKZuz51rC39w.png)

现在你理论上理解了容器是什么，以及Docker如何用于容器化应用程序，让我们想象一个场景：你需要在一群机器上运行多个容器，以支持一个企业级机器学习应用程序，在白天和晚上有不同的工作负载。这在现实生活中非常常见，尽管听起来很简单，但手动完成这项工作是很费力的。

你需要在正确的时间启动正确的容器，弄清它们如何相互通信，处理存储考虑事项，处理失败的容器或硬件以及其他无数的事情！

管理数百甚至数千个容器以保持应用程序运行的整个过程被称为**容器编排**。现在不要陷入技术细节中。

此时，你必须认识到管理现实生活中的应用程序需要不止一个容器，并且管理所有基础设施以保持容器运行是繁琐、手动且具有行政负担的。

这就引出了**Kubernetes**。

### 什么是Kubernetes？

Kubernetes是由Google于2014年开发的用于管理容器化应用程序的开源系统。简单来说，Kubernetes是一种在一组机器上运行和协调容器化应用程序的系统。

![Photo by chuttersnap on Unsplash](https://cdn-images-1.medium.com/max/14720/0\*vscKcwTh1qNmKv3s)

虽然Kubernetes是由Google开发的开源系统，几乎所有主要的云服务提供商都提供Kubernetes作为托管服务。例如：由亚马逊提供的**Amazon Elastic Kubernetes Service (EKS)**，由谷歌提供的**Google Kubernetes Engine (GKE)**，以及由微软提供的**Azure Kubernetes Service (AKS)**。

到目前为止，我们已经讨论和了解了：

✔️ 一个**容器**

✔️ Docker

✔️ Kubernetes

在介绍AWS Fargate之前，还有一件事要讨论，那就是亚马逊自己的容器编排服务**Amazon Elastic Container Service (ECS)**。

### AWS Elastic Container Service (ECS)

Amazon Elastic Container Service (Amazon ECS)是亚马逊自家的容器编排平台。ECS的理念与Kubernetes类似（它们都是编排服务）。

ECS是一项AWS原生服务，这意味着它只能在AWS基础设施上使用。另一方面，**EKS**基于Kubernetes，这是一个开源项目，可供在多云（AWS、GCP、Azure）甚至本地运行的用户使用。

亚马逊还提供了基于Kubernetes的容器编排服务，称为**Amazon Elastic Kubernetes Service (Amazon EKS)**。尽管ECS和EKS的目的非常相似，即编排容器化应用程序，但在定价、兼容性和安全性方面存在一些差异。没有最佳答案，解决方案的选择取决于具体的用例。

无论你使用的是ECS还是EKS这样的容器编排服务，你都可以通过以下两种方式实现底层基础设施：

1. 手动管理集群和底层基础设施，如虚拟机/服务器/（也称为EC2实例）。
2. 无服务器——完全不需要管理任何东西。只需上传容器，就完成了。← **这就是AWS Fargate。**

![Amazon ECS underlying infrastructure](https://cdn-images-1.medium.com/max/2798/1\*k4famzZ1w2Ee5XMHRo1Ggw.png)

### AWS Fargate — 无服务器容器计算
AWS Fargate是一种用于容器的无服务器计算引擎，可以与Amazon Elastic Container Service (ECS)和Amazon Elastic Kubernetes Service (EKS)一起使用。Fargate使您可以专注于构建应用程序。Fargate消除了预配和管理服务器的需求，允许您按应用程序指定和支付资源，并通过设计实现应用程序隔离来提高安全性。

Fargate分配适量的计算资源，消除了选择实例和扩展集群容量的需求。您只需支付运行容器所需的资源，因此不会出现过度预配和支付额外服务器的情况。

![AWS Fargate的工作原理 - https://aws.amazon.com/fargate/](https://cdn-images-1.medium.com/max/4668/1\*WWQBLhVao-FN\_FCrnkPhQg.png)

没有哪种方法是最好的答案。选择无服务器还是手动管理EC2集群取决于具体情况。以下是一些可以帮助您做出选择的指针：

**ECS EC2（手动方法）**

* 您完全依赖于AWS。
* 您已经有一个专门的运维团队来管理AWS资源。
* 您已经在AWS上有一定规模的存在，即您已经在管理EC2实例。

**AWS Fargate**

* 您没有庞大的运维团队来管理AWS资源。
* 您不想要运维责任，或者想要减少运维责任。
* 您的应用程序是无状态的（无状态应用程序是指在一个会话中生成的客户端数据不会在下一个会话中与该客户端一起使用的应用程序）。

### 设置业务背景

一家保险公司希望通过在患者住院时使用人口统计学和基本患者健康风险指标更好地预测患者费用，从而改善现金流预测。

![](https://cdn-images-1.medium.com/max/2000/1\*qM1HiWZ\_uigwdcZ0\_ZL6yA.png)

_(_[_数据来源_](https://www.kaggle.com/mirichoi0218/insurance#insurance.csv)_)_

### 目标

构建并部署一个Web应用程序，用户可以在基于Web的表单中输入患者的人口统计和健康信息，然后输出预测的费用金额。

### 任务

* 使用PyCaret训练、验证和开发一个机器学习流水线。
* 构建一个具有两个功能的前端Web应用程序：（i）在线预测和（ii）批量预测。
* 创建一个Dockerfile。
* 创建并执行一个任务，使用AWS Fargate无服务器基础架构部署应用程序。

由于我们已经在我们的[上一篇教程](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)中涵盖了前两个任务，我们将快速回顾它们，然后专注于上面列表中剩下的项目。如果您有兴趣了解如何使用PyCaret在Python中开发机器学习流水线，并使用Streamlit框架构建Web应用程序，请阅读[这篇教程](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)。

### 👉 任务1 - 模型训练和验证

我们使用Python中的PyCaret来训练和开发一个机器学习流水线，该流水线将作为我们Web应用程序的一部分使用。机器学习流水线可以在集成开发环境（IDE）或笔记本中开发。我们使用笔记本来运行以下代码：

当您在PyCaret中保存一个模型时，根据\*\*setup() \*\*函数中定义的配置创建整个转换流水线。所有相互依赖关系都会自动协调。请参见存储在“deployment\_28042020”变量中的流水线和模型：

![使用PyCaret创建的机器学习流水线](https://cdn-images-1.medium.com/max/2000/1\*P7EXfIxqZZGrpeLgDdk1vQ.png)

### 👉 任务2 - 构建前端Web应用程序

现在我们的机器学习流水线和模型已经准备好了，我们可以开始构建一个前端Web应用程序，该应用程序可以对新数据点进行预测。该应用程序将通过csv文件上传支持“在线”和“批量”预测。让我们将应用程序代码分解为三个主要部分：

### 头部/布局

此部分导入库，加载训练好的模型，并创建一个基本布局，顶部有一个徽标，侧边栏有一个下拉菜单，用于在“在线”和“批量”预测之间切换。

![app.py - 代码片段第1部分](https://cdn-images-1.medium.com/max/2268/1\*xAnCZ1N\_BNoPW7FoA-NXrA.png)

#### 在线预测

此部分处理初始应用程序函数，即逐个进行在线预测。我们使用Streamlit的小部件，如数字输入、文本输入、下拉菜单和复选框，收集用于训练模型的数据点，如年龄、性别、BMI、子女、吸烟者、地区。

![app.py - 代码片段第2部分](https://cdn-images-1.medium.com/max/2408/1\*eFeq1wINsUUnvLJfuL-GOA.png)

#### 批量预测
批量预测是该应用程序功能的第二层。在streamlit中使用**file\_uploader**小部件上传csv文件，然后调用PyCaret的本地\*\*predict\_model() \*\*函数生成预测结果，并使用streamlit的write()函数显示。

![app.py — 代码片段第3部分](https://cdn-images-1.medium.com/max/2410/1\*u-g2iLy\_gV7hom71PM3CEA.png)

**测试应用程序**在将应用程序部署到AWS Fargate之前，我们需要在本地测试应用程序。打开Anaconda Prompt并导航到项目文件夹，执行以下代码：

```
streamlit run app.py
```

![Streamlit应用程序测试 — 在线预测](https://cdn-images-1.medium.com/max/2800/1\*TesAmfCyanOeMEPiYxInUg.png)

### 👉 任务3 — 创建Dockerfile

为了将应用程序容器化以进行部署，我们需要一个Docker镜像，该镜像在运行时成为一个容器。使用Dockerfile创建Docker镜像。Dockerfile只是一个带有一组指令的文件。该项目的Dockerfile如下所示：

Dockerfile的最后部分（从第23行开始）是特定于Streamlit的。Dockerfile区分大小写，并且必须与其他项目文件放在同一个文件夹中。

### 👉 任务4 — 在AWS Fargate上部署：

按照以下简单的9个步骤在AWS Fargate上部署应用程序：

#### 👉 步骤1 — 安装Docker Toolbox（适用于Windows 10 Home）

为了在本地构建docker镜像，您需要在计算机上安装Docker。如果您使用的是Windows 10 64位：Pro，Enterprise或Education（版本号15063或更高），您可以从[DockerHub](https://hub.docker.com/editions/community/docker-ce-desktop-windows/)下载Docker Desktop。

但是，如果您使用的是Windows 10 Home，则需要从[Dockers GitHub页面](https://github.com/docker/toolbox/releases)安装最新版本的旧版Docker Toolbox（v19.03.1）。

![https://github.com/docker/toolbox/releases](https://cdn-images-1.medium.com/max/2000/1\*wn3zVxR0d5rZFDkvhEHi1Q.png)

下载并运行**DockerToolbox-19.03.1.exe**文件。

最简单的检查安装是否成功的方法是打开命令提示符并键入'docker'。它应该打印帮助菜单。

![Anaconda Prompt检查docker](https://cdn-images-1.medium.com/max/2198/1\*f5l4Tds3EOTFSPx6CT5M7w.png)

#### 👉 步骤2 — 在Elastic Container Registry（ECR）中创建存储库

**(a) 登录到AWS控制台并搜索Elastic Container Registry：**

![AWS控制台](https://cdn-images-1.medium.com/max/2000/1\*XCvjm\_Ho1CiaNg59y3MPiw.png)

**(b) 创建新存储库：**

![在Amazon Elastic Container Registry上创建新存储库](https://cdn-images-1.medium.com/max/3822/1\*alFdHEfwYrdZ5J9d14gGgA.png)

![创建存储库](https://cdn-images-1.medium.com/max/3822/1\*BeVF99WdFAPApWLS83SJ3Q.png)

点击“Create Repository”。

**(c) 点击“View push commands”：**

![pycaret-streamlit-aws存储库的推送命令](https://cdn-images-1.medium.com/max/2000/1\*WC-0ShGuB0MB6LgTq07B0Q.png)

#### 👉 步骤3 — 执行推送命令

使用Anaconda Prompt导航到项目文件夹，并执行上一步中复制的命令。在执行这些命令之前，您必须在Dockerfile和其他代码所在的文件夹中。

这些命令用于构建docker镜像，然后将其上传到AWS ECR。

#### 👉 步骤4 — 检查已上传的镜像

点击您创建的存储库，您将在上一步中看到已上传镜像的图像URI。复制图像URI（在下面的第6步中会用到）。

![](https://cdn-images-1.medium.com/max/3828/1\*VuYsEXDoSmmHlEFfYgOAhg.png)

#### 👉 步骤5 — 创建和配置集群

**(a) 点击左侧菜单上的“Clusters”：**

![创建集群 — 步骤1](https://cdn-images-1.medium.com/max/3834/1\*eGOSlysIcdpDZi9GnPAhHw.png)

**(b) 选择“Networking only”，然后点击“Next step”：**

![选择Networking Only模板](https://cdn-images-1.medium.com/max/2000/1\*a0VectBKdBhmZC\_My5OylQ.png)

**(c) 配置集群（输入集群名称），然后点击“Create”：**

![配置集群](https://cdn-images-1.medium.com/max/3780/1\*6AMEaRIr4Rz1qt\_ZmhDy4Q.png)

点击“Create”。

**(d) 集群已创建：**

![集群已创建](https://cdn-images-1.medium.com/max/3824/1\*1UfMJt807V92-jc6Z9ZlfQ.png)

#### 👉 步骤6 — 创建新的任务定义

在Amazon ECS中运行Docker容器需要一个任务定义。任务定义中可以指定的一些参数包括：每个任务中每个容器使用的Docker镜像，每个任务或每个容器内存使用的CPU和内存。

**(a) 点击“Create new task definition”：**

![创建新的任务定义](https://cdn-images-1.medium.com/max/3820/1\*6ET40juZ2owkA1xdDOsDHg.png)

**(b) 选择“FARGATE”作为启动类型：**

![选择启动类型兼容性](https://cdn-images-1.medium.com/max/3822/1\*1Ebz8wmfSisxcrultB86nQ.png)

**(c) 填写详细信息：**
![配置任务和容器定义（第1部分）](https://cdn-images-1.medium.com/max/2000/1\*JqrJPuts4QpVBUK2pKFPpg.png)

![配置任务和容器定义（第2部分）](https://cdn-images-1.medium.com/max/2000/1\*SoM892EIZ2NpSzUCUg10rA.png)

**(d) 点击“添加容器”并填写详细信息：**

![在任务定义中添加容器](https://cdn-images-1.medium.com/max/2508/1\*Kt9zGo0kk4bAUyrWhedU4Q.png)

点击右下角的“创建任务”。

![](https://cdn-images-1.medium.com/max/3828/1\*DZpHXH5iy3daszNT4RYoEQ.png)

#### 👉 第7步— 执行任务定义

在上一步中，我们创建了一个将启动容器的任务。现在，通过点击“操作”下的**“运行任务”**来执行任务。

![](https://cdn-images-1.medium.com/max/3836/1\*nuUekT3eyCeDRoeZlTXk\_Q.png)

**(a) 点击“切换到启动类型”以将类型更改为 Fargate：**

![](https://cdn-images-1.medium.com/max/3850/1\*\_TMuygT58eKgMJQStWCwQw.png)

**(b) 从下拉菜单中选择 VPC 和子网：**

![](https://cdn-images-1.medium.com/max/3812/1\*w7uipHeBNz9RhaBFsN85Bw.png)

点击右下角的“运行任务”。

#### 👉 第8步— 允许来自网络设置的入站端口8501

在我们可以通过公共 IP 地址看到应用程序运行之前的最后一步是允许端口8501（由 streamlit 使用）通过创建新规则。为此，请按照以下步骤操作：

**(a) 点击任务**

![](https://cdn-images-1.medium.com/max/3834/1\*lZh9LgN8vgctY3Xa\_aeMrg.png)

**(b) 点击 ENI Id：**

![](https://cdn-images-1.medium.com/max/3832/1\*K1L\_vExR8-2q-6b020voPQ.png)

**(c) 点击安全组**

![](https://cdn-images-1.medium.com/max/3822/1\*vPhVnBMZTXqBQj0ntWx5WA.png)

**(d) 滚动到底部，点击“编辑入站规则”**

![](https://cdn-images-1.medium.com/max/3828/1\*nWb74Ex5UWs-yJOZs5Ecew.png)

**(e) 添加一个自定义 TCP 规则，端口为8501**

![](https://cdn-images-1.medium.com/max/3826/1\*uqgV\_Fr5NPGzWzwQ5LHAxw.png)

### 👉 恭喜！您已经在 AWS Fargate 上无服务器地发布了您的应用程序。使用带有端口8501的公共 IP 地址来访问该应用程序。

![应用程序发布在 99.79.189.46:8501 上](https://cdn-images-1.medium.com/max/3834/1\*q9GXNH-YCL2vT7Q-Uj9clQ.png)

**注意：** 当本故事发布时，该应用程序将从公共地址中移除，以限制资源消耗。

[此教程的 GitHub 存储库链接](https://www.github.com/pycaret/pycaret-streamlit-aws)

[Google Kubernetes 部署的 GitHub 存储库链接](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

[Heroku 部署的 GitHub 存储库链接](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)

### PyCaret 2.0.0 即将推出！

我们收到了社区的大力支持和反馈。我们正在积极改进 PyCaret 并为下一个版本做准备。**PyCaret 2.0.0 将更加强大和优秀**。如果您想分享您的反馈并帮助我们进一步改进，您可以在网站上[填写此表格](https://www.pycaret.org/feedback)，或在我们的[GitHub](https://www.github.com/pycaret/)或[LinkedIn](https://www.linkedin.com/company/pycaret/)页面上留下评论。

关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)并订阅我们的[YouTube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)频道，了解更多关于 PyCaret 的信息。

### 想了解特定模块吗？

截至首次发布的 1.0.0 版本，PyCaret 提供以下模块供使用。点击下面的链接查看 Python 中的文档和示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 还请参阅：

在 Notebook 中的 PyCaret 入门教程：

[分类](https://www.pycaret.org/clf101) [回归](https://www.pycaret.org/reg101) [聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101)

### 想要贡献吗？

PyCaret 是一个开源项目。欢迎每个人贡献。如果您想贡献，请随时处理[开放问题](https://github.com/pycaret/pycaret/issues)。我们接受带有单元测试的拉取请求，并将其合并到 dev-1.0.1 分支中。

如果您喜欢 PyCaret，请在我们的[GitHub 仓库](https://www.github.com/pycaret/pycaret)上给我们 ⭐️。

Medium：[https://medium.com/@moez\_62905/](https://medium.com/@moez\_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)
LinkedIn: [https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter: [https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)