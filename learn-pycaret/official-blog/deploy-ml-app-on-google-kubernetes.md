# 在 Google Kubernetes 上部署机器学习应用

### 在 Google Kubernetes Engine 上部署使用 Streamlit 和 PyCaret 构建的机器学习应用的逐步入门指南

#### 作者：Moez Ali

![在 Google Kubernetes Engine 上部署使用 Streamlit 和 PyCaret 构建的机器学习应用的逐步入门指南](https://cdn-images-1.medium.com/max/2000/1\*q-xQMoYByRdI7OOfM1qFXg.png)

### 回顾

在我们的[上一篇文章](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)中，我们演示了如何在云端开发一个使用 PyCaret 构建的机器学习流水线，并将训练好的模型部署为一个使用 Streamlit 开源框架构建的 Web 应用，并将其部署在 Heroku PaaS 上。如果你之前没有听说过 PyCaret，你可以阅读这篇[公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)了解更多信息。

在本教程中，我们将使用相同的机器学习流水线和 Streamlit 应用程序，并演示如何将它们容器化并部署到 Google Kubernetes Engine 上。

通过本教程，你将能够在 Google Kubernetes Engine 上构建和托管一个完全功能的容器化 Web 应用程序。这个 Web 应用程序可以用于使用训练好的机器学习模型生成在线预测（逐个预测）和批量预测（通过上传 CSV 文件）。最终的应用程序如下所示：

![最终应用程序（第 1 页）](https://cdn-images-1.medium.com/max/3832/1\*GxVKpxijk0tlqk-bO5Q3JQ.png)

### 👉 本教程中你将学到什么

* 什么是容器，什么是 Docker，什么是 Kubernetes，以及什么是 Google Kubernetes Engine？
* 构建一个 Docker 镜像并将其上传到 Google Container Registry（GCR）。
* 在 GCP 上创建一个集群，并将一个机器学习应用程序部署为 Web 服务。
* 查看一个使用训练好的机器学习流水线实时预测新数据点的 Web 应用程序。

在过去，我们已经介绍了使用 Docker 进行容器化和在 Azure、GCP 和 AWS 等云平台上部署的内容。如果你对这些内容感兴趣，可以阅读以下教程：

* [使用 PyCaret 和 Streamlit 构建和部署机器学习 Web 应用程序](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)
* [在 AWS Fargate 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507)
* [在 Google Kubernetes Engine 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b)
* [在 AWS Web 服务上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01)
* [在 Heroku PaaS 上构建和部署你的第一个机器学习 Web 应用程序](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)

### 💻 本教程的工具箱

### PyCaret

[PyCaret](https://www.pycaret.org/) 是一个开源的、低代码的 Python 机器学习库，用于训练和部署机器学习流水线和模型到生产环境中。可以使用 pip 轻松安装 PyCaret。

```
pip install **pycaret**
```

### Streamlit

[Streamlit](https://www.streamlit.io/) 是一个开源的 Python 库，可以轻松构建漂亮的定制化机器学习和数据科学 Web 应用程序。可以使用 pip 轻松安装 Streamlit。

```
pip install **streamlit**
```

### Google Cloud Platform

Google Cloud Platform（GCP）是由 Google 提供的一套云计算服务，运行在 Google 用于其终端用户产品（如 Google 搜索、Gmail 和 YouTube）的相同基础设施上。如果你还没有 GCP 的账号，你可以在[这里](https://console.cloud.google.com/getting-started)注册。如果你是第一次注册，你将获得 1 年的免费信用额度。

### 让我们开始吧。

在我们进入 Kubernetes 之前，让我们先了解一下什么是容器以及为什么我们需要它？

![https://www.freepik.com/free-photos-vectors/cargo-ship](https://cdn-images-1.medium.com/max/2000/1\*SlzsvRhA71oFOhAjE1Hs0A.jpeg)

你是否曾经遇到过这样的问题：你的代码在你的电脑上运行正常，但当你的朋友尝试运行完全相同的代码时，却无法正常工作？如果你的朋友重复了完全相同的步骤，他或她应该得到相同的结果，对吗？这个问题的答案是——环境。你朋友的环境与你的环境不同。

一个环境包括什么？→ 一个编程语言，比如 Python，以及在构建和测试应用程序时使用的所有库和依赖项的确切版本。
如果我们能够创建一个可以转移到其他机器的环境（例如：您朋友的计算机或像谷歌云平台这样的云服务提供商），我们就可以在任何地方重现结果。因此，\*\*\*容器 \*\*\*是一种软件类型，它将一个应用程序及其所有依赖项打包在一起，以便应用程序可以在一个计算环境中可靠地运行到另一个计算环境中。

> _**那么 Docker 是什么？**_

![](https://cdn-images-1.medium.com/max/2000/1\*EJx9QN4ENSPKZuz51rC39w.png)

\*\*Docker \*\*是一家提供软件（也称为 Docker）的公司，允许用户构建、运行和管理容器。虽然 Docker 的容器是最常见的，但还有其他不那么出名的 _替代方案_，比如[LXD](https://linuxcontainers.org/lxd/introduction/)和[LXC](https://linuxcontainers.org/)也提供容器解决方案。

现在您了解了容器和 Docker，让我们来了解一下 Kubernetes 是什么。

### 什么是 Kubernetes？

Kubernetes 是由谷歌于2014年开发的强大开源系统，用于管理容器化应用程序。简单来说，Kubernetes \*\*\*\*是一个用于在机器集群上运行和协调容器化应用程序的系统。它是一个旨在完全管理容器化应用程序生命周期的平台。

![照片由chuttersnap在Unsplash上提供](https://cdn-images-1.medium.com/max/14720/0\*49CVX837ZpkbRblC)

### 特点

✔️ \*\*负载均衡：\*\*自动在容器之间分配负载。

✔️ \*\*扩展：\*\*根据需求变化（如高峰时段、周末和假期）自动增加或减少容器。

✔️ \*\*存储：\*\*保持多个应用程序实例的存储一致。

✔️ **自愈** 自动重新启动失败的容器，并终止不响应您定义的健康检查的容器。

✔️ \*\*自动部署 \*\*您可以自动化 Kubernetes 来为您的部署创建新容器，删除现有容器，并将它们的所有资源转移到新容器。

### 如果已经有 Docker，为什么还需要 Kubernetes？

想象一种情景，您需要在多台机器上运行多个 Docker 容器，以支持一个企业级机器学习应用程序，在白天和黑夜有不同的工作负载。尽管听起来很简单，但手动完成这项工作是很费力的。

您需要在正确的时间启动正确的容器，找出它们如何相互通信，处理存储考虑事项，并处理失败的容器或硬件。Kubernetes 正在解决这个问题，通过允许大量容器和谐地协同工作，减少运营负担。

### 什么是 Google Kubernetes Engine？

Google Kubernetes Engine 是在谷歌云平台上实现的 _谷歌开源 Kubernetes_。简单！

GKE 的其他热门替代方案包括[Amazon ECS](https://aws.amazon.com/ecs/)和[Microsoft Azure Kubernetes Service](https://azure.microsoft.com/en-us/services/kubernetes-service/)。

### 最后一次，您理解了吗？

* \*\*容器 \*\*是一种软件类型，它将一个应用程序及其所有依赖项打包在一起，以便应用程序可以在一个计算环境中可靠地运行到另一个计算环境中。
* \*\*Docker \*\*是用于构建和管理容器的软件。
* \*\*Kubernetes \*\*是一个用于在集群环境中管理容器化应用程序的开源系统。
* **Google Kubernetes Engine** 是在谷歌云平台上实现的开源 Kubernetes 框架。

在本教程中，我们将使用 Google Kubernetes Engine。为了跟随操作，您必须拥有一个谷歌云平台账户。[点击这里](https://console.cloud.google.com/getting-started)免费注册。

### 设置业务背景

一家保险公司希望通过在患者入院时使用人口统计数据和基本患者健康风险指标更好地预测患者费用，从而改进现金流预测。

![](https://cdn-images-1.medium.com/max/2000/1\*qM1HiWZ\_uigwdcZ0\_ZL6yA.png)

_(_[_数据来源_](https://www.kaggle.com/mirichoi0218/insurance#insurance.csv)_)_

### 目标

构建一个支持在线（逐个）以及批量预测的网络应用程序，使用经过训练的机器学习模型和流水线。

### 任务

* 使用 PyCaret 训练、验证和开发机器学习流水线。
* 构建一个前端网络应用程序，具有两个功能：（i）在线预测和（ii）批量预测。
* 创建一个 Dockerfile。
* 在 Google Kubernetes Engine 上部署网络应用程序。一旦部署，它将变为公开可用，并可以通过 Web URL 访问。

### 👉 任务1 — 模型训练和验证
在集成开发环境（IDE）或笔记本中，可以在本地机器或云上进行训练和模型验证。如果您以前没有使用过 PyCaret，请[点击这里](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)了解更多关于 PyCaret 的信息，或在我们的[网站](https://www.pycaret.org/)上查看[入门教程](https://www.pycaret.org/tutorial)。

在本教程中，我们进行了两个实验。第一个实验使用 PyCaret 的默认预处理设置进行。第二个实验有一些额外的预处理任务，如**缩放和归一化、自动特征工程和将连续数据分成区间**。查看第二个实验的设置代码：

```
**# 实验2**

from **pycaret.regression** import *****

r2 = **setup**(data, target = 'charges', session_id = 123,
           normalize = True,
           polynomial_features = True, trigonometry_features = True,
           feature_interaction=True, 
           bin_numeric_features= ['age', 'bmi'])
```

![两个实验的信息网格对比](https://cdn-images-1.medium.com/max/2000/1\*TeqcOM-jBpkdeQu84c4Onw.png)

只需要几行代码就能完成这个神奇的过程。请注意，在**实验2**中，经过转换的数据集有62个特征用于训练，而原始数据集只有6个特征。所有新特征都是在 PyCaret 中进行转换和自动特征工程的结果。

![转换后数据集的列](https://cdn-images-1.medium.com/max/2000/1\*ju5RFYKGkAVEOvVjeoM5nQ.png)

PyCaret 中模型训练的示例代码：

```
# 模型训练和验证
lr = **create_model**('lr')
```

![线性回归模型的10折交叉验证](https://cdn-images-1.medium.com/max/2276/1\*TX-IzWHBekZBRSQi2T\_JTQ.png)

请注意转换和自动特征工程的影响。R2 增加了10%，付出的努力很少。我们可以比较两个实验中线性回归模型的**残差图**，观察转换和特征工程对模型的异方差性的影响。

```
# 绘制训练模型的残差图
plot_model**(lr, plot = 'residuals')
```

![线性回归模型的残差图](https://cdn-images-1.medium.com/max/2876/1\*LxVMcK4hNvBvEj1tyqxfWQ.png)

机器学习是一个迭代的过程。迭代次数和使用的技术取决于任务的重要性以及如果预测错误会产生的影响。在医院的重症监护室中，用于预测患者结果的机器学习模型的严重性和影响远远超过用于预测客户流失的模型。

在本教程中，我们只进行了两次迭代，并且将第二个实验中的线性回归模型用于部署。然而，在这个阶段，模型仍然只是笔记本/IDE 中的一个对象。要将其保存为可以传输和被其他应用程序使用的文件，请执行以下代码：

```
# 保存转换管道和模型
**save_model**(lr, model_name = 'deployment_28042020')
```

当您在 PyCaret 中保存模型时，将基于在\*\*setup() \*\*函数中定义的配置创建整个转换管道。所有的相互依赖关系都会自动协调。查看存储在 'deployment\_28042020' 变量中的管道和模型：

![使用 PyCaret 创建的管道](https://cdn-images-1.medium.com/max/2000/1\*NWoHVWJzO7i7gIvrlBnIiQ.png)

我们已经完成了训练和模型选择。最终的机器学习管道和线性回归模型现在保存为 pickle 文件（deployment\_28042020.pkl），将在 Web 应用程序中用于对新数据点生成预测。

### 👉 任务2 — 构建前端 Web 应用程序

现在我们的机器学习管道和模型已经准备好了，可以开始构建一个前端 Web 应用程序，用于对新数据点生成预测。该应用程序将通过 csv 文件上传支持“在线”和“批量”预测。让我们将应用程序代码分解为三个主要部分：

### 头部/布局

此部分导入库，加载训练好的模型，并创建一个基本布局，顶部有一个徽标，侧边栏有一个下拉菜单，用于在“在线”和“批量”预测之间切换。

![app.py — 代码片段第1部分](https://cdn-images-1.medium.com/max/2268/1\*xAnCZ1N\_BNoPW7FoA-NXrA.png)

### 在线预测

此部分处理初始应用程序函数，逐个进行在线预测。我们使用 streamlit 的小部件，如_数字输入、文本输入、下拉菜单和复选框_，收集用于训练模型的数据点，如年龄、性别、BMI、子女、吸烟者、地区。

![app.py — 代码片段第2部分](https://cdn-images-1.medium.com/max/2408/1\*eFeq1wINsUUnvLJfuL-GOA.png)
### 批量预测

批量预测是该应用程序功能的第二层。在 streamlit 中使用的 **file\_uploader** 小部件用于上传 csv 文件，然后调用 PyCaret 中的原生 \*\*predict\_model() \*\*函数来生成预测结果，这些结果使用 streamlit 的 write() 函数显示。

![app.py — 代码片段第 3 部分](https://cdn-images-1.medium.com/max/2410/1\*u-g2iLy\_gV7hom71PM3CEA.png)

如果您还记得上面任务 1 中，我们完成了一个线性回归模型，该模型是在从 6 个原始特征中提取的 62 个特征上训练的。Web 应用程序的前端有一个输入表单，只收集六个特征，即年龄、性别、BMI、子女、吸烟者、地区。

我们如何将这些新数据点的 6 个特征转换为用于训练模型的 62 个特征？我们不需要担心这一部分，因为 PyCaret 通过编排转换流水线来自动处理这一部分。当您对使用 PyCaret 训练的模型调用预测函数时，所有转换都会自动应用（按顺序），然后从训练模型生成预测结果。

\*\*测试应用程序 \*\*在将应用程序发布到 Heroku 之前的最后一步是在本地测试 Web 应用程序。打开 Anaconda Prompt 并导航到项目文件夹，执行以下代码：

```
**streamlit** run app.py
```

![Streamlit 应用程序测试 — 在线预测](https://cdn-images-1.medium.com/max/3832/1\*GxVKpxijk0tlqk-bO5Q3JQ.png)

![Streamlit 应用程序测试 — 批量预测](https://cdn-images-1.medium.com/max/3836/1\*P5tit2pMf5qiQqU\_wjQMVg.png)

现在我们有了一个完全功能的 Web 应用程序，我们可以开始将应用程序容器化并部署到 Google Kubernetes Engine。

### 👉 任务 3 — 创建 Dockerfile

为了部署我们的应用程序，我们需要一个在运行时成为容器的 Docker 镜像。使用 Dockerfile 创建 Docker 镜像。Dockerfile 只是一个带有一组指令的文件。该项目的 Dockerfile 如下所示：

Dockerfile 的最后部分（从第 23 行开始）是特定于 Streamlit 的，通常不需要。Dockerfile 区分大小写，必须与其他项目文件一起放在项目文件夹中。

### 👉 任务 4 — 在 GKE 上部署 ML 流水线：

如果您想跟着操作，您需要从 GitHub 上 fork 这个 [仓库](https://github.com/pycaret/pycaret-streamlit-google)。

![https://github.com/pycaret/pycaret-streamlit-google](https://cdn-images-1.medium.com/max/3816/1\*C5WvNEM3U59hHoAjFtE\_EQ.png)

按照以下简单的 10 个步骤在 GKE 集群上部署应用程序。

#### 步骤 1 — 在 GCP 控制台中创建新项目

登录到您的 GCP 控制台，然后转到管理资源

![Google Cloud Platform 控制台 → 管理资源](https://cdn-images-1.medium.com/max/3832/1\*OS16COUUns7uBnpyUxH9-w.png)

点击 **创建新项目**

![Google Cloud Platform 控制台 → 管理资源 → 创建新项目](https://cdn-images-1.medium.com/max/3814/1\*mI3sxfCPrUbt8OtLpa6ViQ.png)

#### 步骤 2 — 导入项目代码

单击控制台窗口右上角的 \*\*激活 Cloud Shell \*\*按钮以打开 Cloud Shell。

![Google Cloud Platform（项目信息页面）](https://cdn-images-1.medium.com/max/3828/1\*KSlqCD2VMDvQo4Oft7nqaA.png)

在 Cloud Shell 中执行以下代码以克隆本教程中使用的 GitHub 仓库。

```
git clone [https://github.com/pycaret/pycaret-streamlit-google.git](https://github.com/pycaret/pycaret-streamlit-google.git)
```

#### 步骤 3 — 设置项目 ID 环境变量

执行以下代码设置 PROJECT\_ID 环境变量。

```
export PROJECT_ID=**pycaret-streamlit-gcp**
```

_pycaret-streamlit-gcp_ 是我们在上面第 1 步中选择的项目名称。

#### 步骤 4 — 构建 Docker 镜像

通过执行以下代码构建应用程序的 Docker 镜像并为上传打标签：

```
docker build -t gcr.io/${PROJECT_ID}/insurance-streamlit:v1 .
```

![成功构建 Docker 镜像时返回的消息](https://cdn-images-1.medium.com/max/3830/1\*5HY6RQRrRzDjsmQEZK\_Qbg.png)

您可以通过运行以下代码来检查可用的镜像：

```
**docker **images
```

#### 步骤 5 — 上传容器镜像

1.  验证 [Container Registry](https://cloud.google.com/container-registry)（您只需要运行一次）：

    gcloud auth configure-docker
2.  执行以下代码将 Docker 镜像上传到 Google Container Registry：

    docker push gcr.io/${PROJECT\_ID}/insurance-streamlit:v1

#### 步骤 6 — 创建集群

现在容器已上传，您需要一个集群来运行容器。集群由一组 Compute Engine VM 实例组成，运行 Kubernetes。

1.  为 gcloud 工具设置项目 ID 和 Compute Engine 区域选项：

    gcloud config set project $PROJECT\_ID gcloud config set compute/zone **us-central1**
2.  通过执行以下代码创建一个集群：
```markdown
    gcloud container clusters create **streamlit-cluster** --num-nodes=2

![Google Cloud Platform → Kubernetes Engine → Clusters](https://cdn-images-1.medium.com/max/3832/1*hNX145tbVPjtTFOSLvjXnw.png)

#### 步骤 7 — 部署应用程序

要在 GKE 集群上部署和管理应用程序，您必须与 Kubernetes 集群管理系统通信。执行以下命令以部署应用程序：

```
kubectl create deployment insurance-streamlit --image=gcr.io/${PROJECT_ID}/insurance-streamlit:v1
```

#### 步骤 8 — 将应用程序暴露给互联网

默认情况下，在 GKE 上运行的容器无法从互联网访问，因为它们没有外部 IP 地址。执行以下代码将应用程序暴露给互联网：

```
kubectl expose deployment insurance-streamlit --type=LoadBalancer --port 80 --target-port **8501**
```

#### 步骤 9 — 检查服务

执行以下代码以获取服务的状态。**EXTERNAL-IP** 是您可以在浏览器中使用的网址，查看发布的应用程序。

```
kubectl get service
```

#### 步骤 10 — 在网址上查看应用程序运行情况

![App Published on https://34.70.49.248 — Page 1](https://cdn-images-1.medium.com/max/3834/1*zHRwykiazKdjL32SE_Uj8g.png)

![App Published on https://34.70.49.248 — Page 2](https://cdn-images-1.medium.com/max/3824/1*YhRqNABfQOIcMq2owc0pmw.png)

**注意：** 当本故事发布时，该应用程序将从公共地址中移除，以限制资源消耗。

[此教程的 GitHub 存储库链接](https://github.com/pycaret/pycaret-streamlit-google)

[Microsoft Azure 部署的 GitHub 存储库链接](https://www.github.com/pycaret/pycaret-azure-deployment)

[Heroku 部署的 GitHub 存储库链接](https://www.github.com/pycaret/deployment-heroku)

### PyCaret 2.0.0 即将发布！

我们收到了社区的大力支持和反馈。我们正在积极改进 PyCaret 并为下一个版本做准备。**PyCaret 2.0.0 将更加强大和优秀**。如果您想分享您的反馈并帮助我们进一步改进，可以在网站上[填写此表格](https://www.pycaret.org/feedback)，或在我们的[GitHub](https://www.github.com/pycaret/)或[LinkedIn](https://www.linkedin.com/company/pycaret/)页面上留言。

关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)，订阅我们的[YouTube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)频道，了解更多关于 PyCaret 的内容。

### 想了解特定模块吗？

截至第一个版本 1.0.0，PyCaret 有以下模块可供使用。点击下面的链接查看 Python 中的文档和示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 还可查看：

PyCaret 在 Notebook 中的入门教程：

[分类](https://www.pycaret.org/clf101) [回归](https://www.pycaret.org/reg101) [聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101)

### 想要贡献吗？

PyCaret 是一个开源项目。欢迎每个人贡献。如果您想贡献，请随时处理[开放问题](https://github.com/pycaret/pycaret/issues)。我们接受带有单元测试的拉取请求，分支为 dev-1.0.1。

如果您喜欢 PyCaret，请在我们的[GitHub 仓库](https://www.github.com/pycaret/pycaret)上给我们 ⭐️。

Medium: [https://medium.com/@moez_62905/](https://medium.com/@moez_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

LinkedIn: [https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter: [https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)
```