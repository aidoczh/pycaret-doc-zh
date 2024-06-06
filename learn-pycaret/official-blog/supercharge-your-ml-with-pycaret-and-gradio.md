# 用 PyCaret 和 Gradio 加速你的机器学习

### 用 PyCaret 和 Gradio 加速你的机器学习实验

#### 一步一步教程，快速开发和交互式机器学习流程

![Photo by Hunter Harritt on Unsplash](https://cdn-images-1.medium.com/max/10944/0\*izwo6BPsV7Ru4b6r)

### 👉 介绍

本教程是一个逐步进行的、面向初学者的解释，展示如何在几分钟内集成 Python 中两个强大的开源库 [PyCaret](https://www.pycaret.org) 和 [Gradio](https://www.gradio.app/)，并加速你的机器学习实验。

本教程是一个“hello world”示例，我使用了 UCI 的 [鸢尾花数据集](https://archive.ics.uci.edu/ml/datasets/iris)，这是一个多分类问题，目标是预测鸢尾花的类别。本示例中给出的代码可以在任何其他数据集上复现，无需进行任何重大修改。

### 👉 PyCaret

PyCaret 是一个开源的、低代码的机器学习库和端到端模型管理工具，用 Python 构建，可自动化机器学习工作流程。它因易用性、简单性以及快速高效地构建和部署端到端机器学习原型而广受欢迎。

PyCaret 是一个替代低代码库，可以用少量代码替换数百行代码。这使得实验周期指数级加快，效率大大提高。

PyCaret **简单且** **易于使用**。PyCaret 中执行的所有操作都按顺序存储在一个**Pipeline**中，该 Pipeline 完全自动化用于\*\*部署。\*\*无论是填补缺失值、独热编码、转换分类数据、特征工程，甚至是超参数调整，PyCaret 都可以自动化完成。

要了解更多关于 PyCaret 的信息，请访问他们的 [GitHub](https://www.github.com/pycaret/pycaret)。

### 👉 Gradio

Gradio 是一个开源的 Python 库，用于为你的机器学习模型创建可定制的 UI 组件。Gradio 可以让你通过在浏览器中拖放自己的图像、粘贴自己的文本、录制自己的声音等方式，轻松“玩弄”你的模型，并查看模型的输出。

Gradio 适用于：

* 围绕训练好的 ML 流程创建快速演示
* 获取模型性能的实时反馈
* 在开发过程中交互式调试你的模型

要了解更多关于 Gradio 的信息，请访问他们的 [GitHub](https://github.com/gradio-app/gradio)。

![PyCaret 和 Gradio 的工作流程](https://cdn-images-1.medium.com/max/2000/1\*CLPbvtAvxkI5MbnPFE59sQ.png)

### 👉 安装 PyCaret

安装 PyCaret 非常简单，只需几分钟即可完成。我们强烈建议使用虚拟环境，以避免与其他库可能发生的冲突。

PyCaret 的默认安装是一个精简版本的 pycaret，只安装了列在[这里](https://github.com/pycaret/pycaret/blob/master/requirements.txt)的硬依赖项。

```
**# 安装精简版本（默认）
**pip install pycaret

**# 安装完整版本**
pip install pycaret[full]
```

当你安装 pycaret 的完整版本时，还会安装所有列在[这里](https://github.com/pycaret/pycaret/blob/master/requirements-optional.txt)的可选依赖项。

### 👉 安装 Gradio

你可以通过 pip 安装 gradio。

```
pip install gradio
```

### 👉 让我们开始吧

```
**# 从 pycaret 仓库加载鸢尾花数据集**
from pycaret.datasets import get_data
data = get_data('iris')
```

![鸢尾花数据集的样本行](https://cdn-images-1.medium.com/max/2000/1\*qttXFQnZ3atRv\_qb9FtVTw.png)

### 👉 初始化设置

```
**# 初始化设置**
from pycaret.classification import *
s = setup(data, target = 'species', session_id = 123)
```

![](https://cdn-images-1.medium.com/max/2444/1\*m5Sgz4IGqGEKNjbar6hGfg.png)

每当你在 PyCaret 中初始化设置函数时，它会对数据集进行分析，并推断所有输入特征的数据类型。在这种情况下，你可以看到所有四个特征（_sepal\_length, sepal\_width, petal\_length 和 petal\_width_）都被正确识别为数值数据类型。你可以按 Enter 键继续。

![来自设置的输出 — 为显示而截断](https://cdn-images-1.medium.com/max/2000/1\*MNchQT8Y7E\_Lsg-66CVSFg.png)
在 PyCaret 的所有模块中，`setup` 函数是开始任何机器学习实验的第一个且唯一必需步骤。除了默认执行一些基本处理任务外，PyCaret 还提供了广泛的预处理功能，例如[缩放和转换](https://pycaret.org/normalization/)、[特征工程](https://pycaret.org/feature-interaction/)、[特征选择](https://pycaret.org/feature-importance/)，以及一些关键的数据准备步骤，如[独热编码](https://pycaret.org/one-hot-encoding/)、[缺失值填补](https://pycaret.org/missing-values/)、[过采样/欠采样](https://pycaret.org/fix-imbalance/)等。要了解 PyCaret 中所有预处理功能的更多信息，您可以查看这个[链接](https://pycaret.org/preprocessing/)。

![https://pycaret.org/preprocessing/](https://cdn-images-1.medium.com/max/2242/1\*7AOrLPzJWLFH90asByQqsg.png)

### 👉 比较模型

这是在 PyCaret 中进行 _任何_ 监督实验工作流中我们推荐的第一步。此函数使用默认超参数训练模型库中的所有可用模型，并使用交叉验证评估性能指标。

该函数的输出是一个表格，显示所有模型的平均交叉验证分数。可以使用 `fold` 参数（默认值为 10 折）定义折数。表格按选择的指标排序（从高到低），可以使用 `sort` 参数定义选择的指标（默认值为 'Accuracy'）。

```
best = compare_models(n_select = 15)
compare_model_results = pull()
```

`setup` 函数中的 `n_select` 参数控制返回的训练模型。在这种情况下，我将其设置为 15，表示返回前 15 个模型作为列表。第二行中的 `pull` 函数将 `compare_models` 的输出存储为 `pd.DataFrame`。

![从 compare_models 输出](https://cdn-images-1.medium.com/max/2060/1\*Qu62jca8TpZLkhZgUq1uFA.png)

```
len(best)
>>> 15

print(best[:5])
```

![从 print(best\[:5\]) 输出](https://cdn-images-1.medium.com/max/2000/1\*\_H72UEY5AQlYnQswyZ0xhQ.png)

### 👉 Gradio

现在我们已经完成建模过程，让我们使用 Gradio 创建一个简单的用户界面来与我们的模型进行交互。我将分两部分进行，首先创建一个函数，该函数将使用 PyCaret 的 `predict_model` 功能生成并返回预测结果，第二部分将该函数输入到 Gradio 中，并为交互设计一个简单的输入表单。

### **第一部分 — 创建内部函数**

代码的前两行将输入特征转换为 pandas DataFrame。第 7 行创建了一个在 `compare_models` 输出中显示的模型名称的唯一列表（这将用作 UI 中的下拉菜单）。第 8 行根据列表的索引值选择最佳模型（将通过 UI 传递），第 9 行使用 PyCaret 的 `predict_model` 功能对数据集进行评分。

### 第二部分 — 使用 Gradio 创建用户界面

下面代码中的第 3 行创建了一个模型名称的下拉菜单，第 4–7 行为每个输入特征创建了一个滑块，并将默认值设置为每个特征的平均值。第 9 行启动了一个 UI（在笔记本中以及在本地主机上，因此您可以在浏览器中查看）。

![运行 Gradio 界面的输出](https://cdn-images-1.medium.com/max/2910/1\*zVe2L4L8fqDL4zIN75rFwQ.png)

您可以在这里观看这个快速视频，看看如何轻松地与您的管道进行交互，并查询您的模型，而无需编写数百行代码或开发一个完整的前端。

希望您能欣赏 PyCaret 和 Gradio 中的易用性和简单性。在不到 25 行代码和几分钟的实验时间内，我使用 PyCaret 训练和评估了多个模型，并开发了一个轻量级的 UI 与 Notebook 中的模型进行交互。

### 即将推出！

下周我将撰写一篇关于使用 [PyCaret 异常检测模块](https://pycaret.readthedocs.io/en/latest/api/anomaly.html) 对时间序列数据进行无监督异常检测的教程。请关注我的 [Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/) 和 [Twitter](https://twitter.com/moezpycaretorg1) 获取更多更新。

使用 Python 中这个轻量级的工作流自动化库，您可以实现无限可能。如果您觉得这个工具有用，请不要忘记在我们的 GitHub 仓库上给我们 ⭐️。

要了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)。

加入我们的 Slack 频道。邀请链接在[这里](https://join.slack.com/t/pycaret/shared\_invite/zt-p7aaexnl-EqdTfZ9U\~mF0CwNcltffHg)。

### 您可能也对以下内容感兴趣：
[使用 PyCaret 2.0 在 Power BI 中构建自己的 AutoML](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d) [使用 Docker 在 Azure 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01) [在 Google Kubernetes Engine 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b) [在 AWS Fargate 上部署机器学习流水线](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507) [构建和部署你的第一个机器学习 Web 应用](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99) [使用 AWS Fargate 无服务器部署 PyCaret 和 Streamlit 应用](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2) [使用 PyCaret 和 Streamlit 构建和部署机器学习 Web 应用](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104) [在 GKE 上部署使用 Streamlit 和 PyCaret 构建的机器学习应用](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

### 重要链接

[文档](https://pycaret.readthedocs.io/en/latest/installation.html) [博客](https://medium.com/@moez\_62905) [GitHub](http://www.github.com/pycaret/pycaret) [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装 PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html) [Notebook 教程](https://pycaret.readthedocs.io/en/latest/tutorials.html) [为 PyCaret 做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 想了解特定模块吗？

点击下面的链接查看文档和工作示例。

[分类](https://pycaret.readthedocs.io/en/latest/api/classification.html) [回归](https://pycaret.readthedocs.io/en/latest/api/regression.html) [聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html) [异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html) [自然语言处理](https://pycaret.readthedocs.io/en/latest/api/nlp.html) [关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)