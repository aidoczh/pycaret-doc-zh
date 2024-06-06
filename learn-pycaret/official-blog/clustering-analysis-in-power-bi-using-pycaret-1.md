# 在 Power BI 中使用 PyCaret 进行聚类分析

### 如何在 Power BI 中使用 PyCaret 实现聚类分析

#### 作者：Moez Ali

![在 Power BI 中的聚类仪表板](https://cdn-images-1.medium.com/max/2632/1\*sUeqYcENVII1RlyYA\_-Uxg.png)

在我们的[上一篇文章](https://towardsdatascience.com/build-your-first-anomaly-detector-in-power-bi-using-pycaret-2b41b363244e)中，我们演示了如何通过将 PyCaret 集成到 Power BI 中来构建异常检测器，从而使分析师和数据科学家能够在其报告和仪表板中添加一层机器学习，而无需额外的许可成本。

在本文中，我们将看到如何使用 PyCaret 在 Power BI 中实现聚类分析。如果您之前没有听说过 PyCaret，请阅读这篇[公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)以了解更多信息。

### 本教程的学习目标

* 什么是聚类？聚类的类型。
* 在 Power BI 中训练和实现无监督的聚类模型。
* 分析结果并在仪表板中可视化信息。
* 如何在 Power BI 生产环境中部署聚类模型？

### 开始之前

如果您之前使用过 Python，那么您可能已经在计算机上安装了 Anaconda 发行版。如果没有，请[点击这里](https://www.anaconda.com/distribution/)下载带有 Python 3.7 或更高版本的 Anaconda 发行版。

![https://www.anaconda.com/products/individual](https://cdn-images-1.medium.com/max/2612/1\*sMceDxpwFVHDtdFi528jEg.png)

### 环境设置

在我们开始在 Power BI 中使用 PyCaret 的机器学习功能之前，我们必须创建一个虚拟环境并安装 pycaret。这是一个三步过程：

[✅](https://fsymbols.com/signs/tick/) **步骤 1 — 创建一个 Anaconda 环境**

从开始菜单中打开 **Anaconda Prompt** 并执行以下代码：

```
conda create --name **myenv** python=3.7
```

[✅](https://fsymbols.com/signs/tick/) **步骤 2 — 安装 PyCaret**

在 Anaconda Prompt 中执行以下代码：

```
pip install pycaret
```

安装可能需要 15-20 分钟。如果安装过程中遇到问题，请查看我们的[GitHub](https://www.github.com/pycaret/pycaret)页面以获取已知问题和解决方案。

[✅](https://fsymbols.com/signs/tick/)**步骤 3 — 在 Power BI 中设置 Python 目录**

创建的虚拟环境必须与 Power BI 相关联。这可以通过 Power BI Desktop 中的全局设置完成（文件 → 选项 → 全局 → Python 脚本）。Anaconda 环境默认安装在：

C:\Users\***username**\*\AppData\Local\Continuum\anaconda3\envs\myenv

![文件 → 选项 → 全局 → Python 脚本](https://cdn-images-1.medium.com/max/2000/1\*zQMKuyEk8LGrOPE-NByjrg.png)

### 什么是聚类？

聚类是一种将具有相似特征的数据点分组的技术。这些分组对于探索数据、识别模式和分析数据子集非常有用。将数据组织成簇有助于识别数据中的潜在结构，并在许多行业中找到应用。一些常见的聚类业务用例包括：

✔ 用于营销目的的客户细分。

✔ 用于促销和折扣的客户购买行为分析。

✔ 在流行病爆发中识别地理簇，如 COVID-19。

### 聚类的类型

鉴于聚类任务的主观性质，有各种算法适用于不同类型的问题。每个算法都有自己的规则以及计算簇的数学原理。

本教程是关于在 Power BI 中使用名为 PyCaret 的 Python 库实现聚类分析。本教程不涉及特定算法细节和这些算法背后的数学原理。

![Ghosal A., Nandy A., Das A.K., Goswami S., Panday M. (2020) A Short Review on Different Clustering Techniques and Their Applications.](https://cdn-images-1.medium.com/max/2726/1\*2eQuIebjtTMJot27bWXgCQ.png)

在本教程中，我们将使用 K-Means 算法，这是最简单和最流行的无监督机器学习算法之一。如果您想了解更多关于 K-Means 的信息，可以阅读[这篇论文](https://stanford.edu/\~cpiech/cs221/handouts/kmeans.html)。

### 设定业务背景

在本教程中，我们将使用世界卫生组织的全球卫生支出数据库中的当前卫生支出数据集。该数据集包含从 2000 年到 2017 年，200 多个国家的卫生支出占国民 GDP 的百分比。

我们的目标是通过使用 K-Means 聚类算法在这些数据中找到模式和群组。

[数据来源](https://data.worldbank.org/indicator/SH.XPD.CHEX.GD.ZS)

![样本数据点](https://cdn-images-1.medium.com/max/2366/1\*E1z19x\_qa7rko1FZpAw61Q.png)

### 👉 让我们开始吧
现在您已经设置好了Anaconda环境，安装了PyCaret，了解了聚类分析的基础知识，并且对本教程的业务背景有了了解，让我们开始吧。

### 1. 获取数据

第一步是将数据集导入到Power BI Desktop中。您可以使用Web连接器加载数据（Power BI Desktop → 获取数据 → 从Web）。

![Power BI Desktop → 获取数据 → 其他 → Web](https://cdn-images-1.medium.com/max/3842/1\*JZ3MwRe8rJXp5e0ac7lamw.png)

csv文件链接：[https://github.com/pycaret/powerbi-clustering/blob/master/clustering.csv](https://github.com/pycaret/powerbi-clustering/blob/master/clustering.csv)

### 2. 模型训练

要在Power BI中训练一个聚类模型，我们需要在Power Query Editor中执行一个Python脚本（Power Query Editor → 转换 → 运行Python脚本）。运行以下代码作为Python脚本：

```
from **pycaret.clustering** import *
dataset = **get_clusters**(dataset, num_clusters=5, ignore_features=['Country'])
```

![Power Query Editor (转换 → 运行Python脚本)](https://cdn-images-1.medium.com/max/2000/1\*SK0XxzF9XZlwtGH1786OUQ.png)

我们使用**ignore\_features**参数在数据集中忽略了‘_Country_’列。有很多原因可能导致您不想使用某些列来训练机器学习算法。

PyCaret允许您隐藏而不是删除数据集中不需要的列，因为您可能会在以后的分析中需要这些列。例如，在这种情况下，我们不想使用‘Country’来训练算法，因此我们将其传递给**ignore\_features**。

PyCaret提供了超过8种现成的聚类算法。

![](https://cdn-images-1.medium.com/max/2632/1\*ihezKFr61Vrgu7E-0-JA5g.png)

默认情况下，PyCaret使用4个簇训练**K-Means聚类模型**。默认值可以轻松更改：

* 要更改模型类型，请在**get\_clusters()**中使用**model**参数。
* 要更改簇数，请使用**num\_clusters**参数。

查看使用6个簇的**K-Modes聚类**的示例代码。

```
from **pycaret.clustering **import *
dataset = **get_clusters**(dataset, model='kmodes', num_clusters=6, ignore_features=['Country'])
```

**输出：**

![聚类结果（执行Python代码后）](https://cdn-images-1.medium.com/max/2000/1\*RCYtFO6XDGI2-qbZdYeMfQ.png)

![最终输出（单击表后）](https://cdn-images-1.medium.com/max/3848/1\*a6mAzuXC8Ta6gRyolaF5uA.png)

附加了一个包含聚类标签的新列到原始数据集。然后将所有年份列**解枢**以规范化数据，以便在Power BI中进行可视化。

这是Power BI中最终输出的样子。

![在Power BI Desktop中的结果（应用查询后）](https://cdn-images-1.medium.com/max/2564/1\*oy\_X3VIdVPS32qQxkOeehw.png)

### 3. 仪表板

一旦您在Power BI中有了聚类标签，这里是一个示例，展示如何在仪表板中可视化以生成洞察：

![仪表板的摘要页面](https://cdn-images-1.medium.com/max/2632/1\*sUeqYcENVII1RlyYA\_-Uxg.png)

![仪表板的详细页面](https://cdn-images-1.medium.com/max/2632/1\*1ck--1zR\_hRPqREKDC7ztg.png)

您可以从我们的[GitHub](https://github.com/pycaret/powerbi-clustering)下载PBIX文件和数据集。

### 👉 在生产中实施聚类

上面演示的是在Power BI中实施聚类的一种简单方式。然而，重要的是要注意，上面展示的方法每次刷新Power BI数据集时都会训练聚类模型。这可能存在两个问题：

* 当使用新数据重新训练模型时，聚类标签可能会发生变化（例如：一些之前标记为簇1的数据点在重新训练时可能被标记为簇2）
* 您不希望每天花费数小时重新训练模型。

在Power BI中更有效的实施聚类的一种方法是使用预先训练的模型生成聚类标签，而不是每次重新训练模型。

### 预先训练模型

您可以使用任何集成开发环境（IDE）或笔记本来训练机器学习模型。在这个示例中，我们使用Visual Studio Code来训练一个聚类模型。

![在Visual Studio Code中训练模型](https://cdn-images-1.medium.com/max/2000/1\*5roevyCmjxWthy0bYyf4ow.png)

然后将训练好的模型保存为一个pickle文件，并导入到Power Query中生成聚类标签。

![将聚类管道保存为pickle文件](https://cdn-images-1.medium.com/max/2000/1\*XxknQxv\_O\_Cx1WJ4kzwPkQ.png)

如果您想了解如何在Jupyter笔记本中使用PyCaret实现聚类分析，可以观看这个2分钟的视频教程：

### 使用预训练模型

执行以下代码作为Python脚本，从预训练模型生成标签。
```python
from pycaret.clustering import *
dataset = predict_model('c:/.../clustering_deployment_20052020', data=dataset)
```
在这段代码中，我们使用了 `pycaret.clustering` 库中的函数。首先，我们导入了该库。然后，我们使用 `predict_model` 函数对数据集进行聚类预测。`predict_model` 函数的第一个参数是保存聚类模型的文件路径，第二个参数是要进行预测的数据集。在这个例子中，我们将数据集保存在 `dataset` 变量中，并将其作为参数传递给 `predict_model` 函数。
这个输出结果与我们之前看到的是一样的。不同之处在于，当你使用一个预训练模型时，标签是使用相同模型在新数据集上生成的，而不是重新训练模型。

### 在 Power BI 服务中使其正常运行

一旦你将 .pbix 文件上传到 Power BI 服务，还需要进行一些步骤，以实现将机器学习流水线无缝集成到数据流水线中。这些步骤包括：

* **为数据集启用定期刷新** — 要为包含带有 Python 脚本的数据集的工作簿启用定期刷新，请参阅[配置定期刷新](https://docs.microsoft.com/en-us/power-bi/connect-data/refresh-scheduled-refresh)，其中还包括有关**个人网关**的信息。
* **安装个人网关** — 需要在存放文件的计算机上安装一个**个人网关**，并且已安装 Python；Power BI 服务必须能够访问该 Python 环境。你可以获取有关如何[安装和配置个人网关](https://docs.microsoft.com/en-us/power-bi/connect-data/service-gateway-personal-mode)的更多信息。

如果你有兴趣了解更多关于聚类分析的内容，请查看我们的[笔记本教程](https://www.pycaret.org/clu101)。

### PyCaret 1.0.1 即将推出！

我们收到了社区的大力支持和反馈。我们正在积极努力改进 PyCaret 并为下一个版本做准备。**PyCaret 1.0.1 将更加强大和优秀**。如果你想分享你的反馈并帮助我们进一步改进，你可以在网站上[填写此表格](https://www.pycaret.org/feedback)，或在我们的[GitHub](https://www.github.com/pycaret/)或[LinkedIn](https://www.linkedin.com/company/pycaret/)页面上留下评论。

关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)并订阅我们的[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)频道，以了解更多关于 PyCaret 的内容。

### 重要链接

[用户指南 / 文档](https://www.pycaret.org/guide) [GitHub 代码库](https://www.github.com/pycaret/pycaret) [安装 PyCaret](https://www.pycaret.org/install) [笔记本教程](https://www.pycaret.org/tutorial) [为 PyCaret 做贡献](https://www.pycaret.org/contribute)

### 想了解特定模块吗？

截至首次发布的 1.0.0 版本，PyCaret 提供以下可供使用的模块。点击下面的链接查看 Python 中的文档和实际示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 还可参阅：

在笔记本中的 PyCaret 入门教程：

[聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101) [回归](https://www.pycaret.org/reg101) [分类](https://www.pycaret.org/clf101)

### 想要做出贡献吗？

PyCaret 是一个开源项目。欢迎每个人做出贡献。如果你想做出贡献，请随时处理[开放问题](https://github.com/pycaret/pycaret/issues)。我们接受带有单元测试的拉取请求，该请求应基于 dev-1.0.1 分支。

如果你喜欢 PyCaret，请在我们的[GitHub 代码库](https://www.github.com/pycaret/pycaret)上给我们 ⭐️。

Medium：[https://medium.com/@moez_62905/](https://medium.com/@moez_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

LinkedIn：[https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter：[https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)