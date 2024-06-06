# 在 Power BI 中使用 PyCaret 进行主题建模

### 在 Power BI 中使用 PyCaret 进行主题建模

#### 作者：Moez Ali

![Power BI 中的 NLP 仪表板](https://cdn-images-1.medium.com/max/2624/1\*SyZczsDz5Pf-4Srfj\_p8vQ.png)

在我们的[上一篇文章](https://towardsdatascience.com/how-to-implement-clustering-in-power-bi-using-pycaret-4b5e34b1405b)中，我们演示了如何通过将 PyCaret 与 Power BI 集成，实现在报告和仪表板中添加机器学习层，从而使分析师和数据科学家能够在不增加额外许可费用的情况下进行聚类分析。

在本文中，我们将看到如何使用 PyCaret 在 Power BI 中实现主题建模。如果您之前没有听说过 PyCaret，请阅读这篇[公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)以了解更多信息。

### 本教程的学习目标

* 什么是自然语言处理？
* 什么是主题建模？
* 在 Power BI 中训练和实现潜在狄利克雷分配模型。
* 分析结果并在仪表板中可视化信息。

### 开始之前

如果您之前使用过 Python，那么您的计算机上可能已经安装了 Anaconda 发行版。如果没有，请[点击这里](https://www.anaconda.com/distribution/)下载带有 Python 3.7 或更高版本的 Anaconda 发行版。

![https://www.anaconda.com/products/individual](https://cdn-images-1.medium.com/max/2612/1\*sMceDxpwFVHDtdFi528jEg.png)

### 环境设置

在开始使用 PyCaret 的机器学习功能之前，我们需要创建一个虚拟环境并安装 pycaret。这是一个四步的过程：

[✅](https://fsymbols.com/signs/tick/) **第一步 - 创建一个 Anaconda 环境**

从开始菜单打开\*\*Anaconda Prompt\*\*并执行以下代码：

```
conda create --name **powerbi **python=3.7
```

_"powerbi" 是我们选择的环境名称。您可以使用任何您喜欢的名称。_

[✅](https://fsymbols.com/signs/tick/) **第二步 - 安装 PyCaret**

在 Anaconda Prompt 中执行以下代码：

```
pip install **pycaret**
```

安装可能需要 15-20 分钟。如果安装出现问题，请参阅我们的[GitHub](https://www.github.com/pycaret/pycaret)页面上的已知问题和解决方法。

[✅](https://fsymbols.com/signs/tick/)**第三步 - 在 Power BI 中设置 Python 目录**

创建的虚拟环境必须与 Power BI 关联。这可以通过 Power BI Desktop 中的全局设置完成（文件 → 选项 → 全局 → Python 脚本）。Anaconda 环境默认安装在以下位置：

C:\Users\***用户名**\*\Anaconda3\envs\\

![文件 → 选项 → 全局 → Python 脚本](https://cdn-images-1.medium.com/max/2000/1\*3qTuOM-N6ekhoiQmDpHgXg.png)

[✅](https://fsymbols.com/signs/tick/)**第四步 - 安装语言模型**

为了执行 NLP 任务，您必须在 Anaconda Prompt 中执行以下代码来下载语言模型。

首先在 Anaconda Prompt 中激活您的 conda 环境：

```
conda activate **powerbi**
```

下载英语语言模型：

```
python -m spacy download en_core_web_sm
python -m textblob.download_corpora
```

![python -m spacy download en\_core\_web\_sm](https://cdn-images-1.medium.com/max/3840/1\*savPqt23x7nBcK76-0MBxw.png)

![python -m textblob.download\_corpora](https://cdn-images-1.medium.com/max/3838/1\*NYaSehQvRp9ANsEpC\_GPQw.png)

### 什么是自然语言处理？

自然语言处理（NLP）是计算机科学和人工智能的一个子领域，涉及计算机与人类语言之间的交互。特别是，NLP 包括广泛的技术，用于编程计算机以处理和分析大量的自然语言数据。

NLP 软件在我们的日常生活中以各种方式帮助我们，很可能您甚至在不知情的情况下就在使用它。一些例子包括：

* **个人助手**：Siri、Cortana、Alexa。
* **自动补全**：在搜索引擎中（如 Google、Bing、百度、雅虎）。
* **拼写检查**：几乎在任何地方，例如浏览器、IDE（如 Visual Studio）、桌面应用程序（如 Microsoft Word）。
* **机器翻译**：Google 翻译。
* **文档摘要软件**：Text compactor、Autosummarizer。

![来源：https://clevertap.com/blog/natural-language-processing](https://cdn-images-1.medium.com/max/2800/1\*IEuGZY5vaWoVnTqQpoZUvQ.jpeg)

主题建模是一种在文本数据中发现抽象主题的统计模型。它是自然语言处理中的许多实际应用之一。

### 什么是主题建模？

主题建模是一种统计模型，属于无监督机器学习，用于在文本数据中发现抽象主题。主题建模的目标是自动找到一组文档中的主题/主题。

主题建模的一些常见用例包括：
* **主题建模** 是通过将文档分类为主题来总结大量文本数据的方法（这个想法与聚类非常相似）。
* **探索性数据分析** 用于了解数据，如客户反馈表、亚马逊评论、调查结果等。
* **特征工程** 是为监督式机器学习实验（如分类或回归）创建特征的过程。

用于主题建模的算法有几种。一些常见的算法包括潜在狄利克雷分配（LDA）、潜在语义分析（LSA）和非负矩阵分解（NMF）。每种算法都有其自己的数学细节，在本教程中不会涉及。我们将使用 PyCaret 的 NLP 模块在 Power BI 中实现一个潜在狄利克雷分配（LDA）模型。

如果您有兴趣了解 LDA 算法的技术细节，可以阅读[这篇论文](http://www.jmlr.org/papers/volume3/blei03a/blei03a.pdf)。

![来源：https://springerplus.springeropen.com/articles/10.1186/s40064-016-3252-8](https://cdn-images-1.medium.com/max/2000/1\*DYbV9YMI94QsUeRiiJyrSg.png)

### **用于主题建模的文本预处理**

为了从主题建模中获得有意义的结果，在将文本数据提供给算法之前，必须对其进行处理。这在几乎所有 NLP 任务中都很常见。文本的预处理与处理结构化数据（行和列中的数据）时通常使用的经典预处理技术不同。

PyCaret 通过应用超过 15 种技术（如**停用词去除**、**分词**、**词形还原**、**二元组/三元组提取**等）自动预处理文本数据。如果您想了解 PyCaret 中所有文本预处理功能的更多信息，[请点击这里](https://www.pycaret.org/nlp)。

### 设定业务背景

Kiva 是一个成立于 2005 年的国际非营利组织，总部位于旧金山。其使命是扩大对未开发社区的金融获取，以帮助他们蓬勃发展。

![来源：https://www.kiva.org/about](https://cdn-images-1.medium.com/max/2124/1\*U4zzTYo6MoCk6PxuZl3FBw.png)

在本教程中，我们将使用 Kiva 的开放数据集，其中包含 6,818 个已批准贷款申请人的贷款信息。数据集包括贷款金额、国家、性别以及一些文本数据，即借款人提交的申请。

![样本数据点](https://cdn-images-1.medium.com/max/3194/1\*jnQvTmQHhWpOSAgSMqaspg.png)

我们的目标是分析“_en_”列中的文本数据，找出抽象主题，然后利用这些主题来评估某些主题（或某些类型的贷款）对违约率的影响。

### 👉 让我们开始吧

现在您已经设置好 Anaconda 环境，了解了主题建模，并了解了本教程的业务背景，让我们开始吧。

### 1. 获取数据

第一步是将数据集导入到 Power BI Desktop 中。您可以使用 Web 连接器加载数据。（Power BI Desktop → 获取数据 → 从 Web）。

![Power BI Desktop → 获取数据 → 其他 → Web](https://cdn-images-1.medium.com/max/3828/1\*lGqJEUm2lVDcYNDdGNUfbw.png)

CSV 文件链接：[https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/kiva.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/kiva.csv)

### 2. 模型训练

要在 Power BI 中训练一个主题模型，我们需要在 Power Query Editor 中执行一个 Python 脚本（Power Query Editor → 转换 → 运行 Python 脚本）。运行以下代码作为 Python 脚本：

```
from **pycaret.nlp **import *
dataset = **get_topics**(dataset, text='en')
```

![Power Query Editor（转换 → 运行 Python 脚本）](https://cdn-images-1.medium.com/max/2000/1\*EwC-QI4m6DORCtdakOAPmQ.png)

PyCaret 中有 5 个可用的主题模型。

![](https://cdn-images-1.medium.com/max/2000/1\*LszI1w45K6i5pOBJ0ZmndA.png)

默认情况下，PyCaret 使用 4 个主题训练一个**潜在狄利克雷分配（LDA）**模型。可以轻松更改默认值：

* 要更改模型类型，请在 **get_topics()** 中使用 **model** 参数。
* 要更改主题数，请使用 **num_topics** 参数。

以下是使用 6 个主题的**非负矩阵分解**模型的示例代码。

```
from **pycaret.nlp **import *
dataset = **get_topics**(dataset, text='en', model='nmf', num_topics=6)
```

**输出：**

![主题建模结果（执行 Python 代码后）](https://cdn-images-1.medium.com/max/3834/1\*DY70gtEWPMy5BPiuKwWPPA.png)

![最终输出（点击表后）](https://cdn-images-1.medium.com/max/3840/1\*lbTGdPoqZQkYejsl01D4dQ.png)

新列包含主题权重，附加到原始数据集。这是在 Power BI 中应用查询后的最终输出的样子。

![Power BI Desktop 中的结果（应用查询后）](https://cdn-images-1.medium.com/max/3844/1\*btTSFxgmmEV8e7-Nw133mw.png)

### 3. 仪表板
在 Power BI 中使用主题权重后，以下是如何在仪表板中可视化并生成洞察力的示例：

![仪表板的摘要页面](https://cdn-images-1.medium.com/max/2624/1\*SyZczsDz5Pf-4Srfj\_p8vQ.png)

![仪表板的详细页面](https://cdn-images-1.medium.com/max/2660/1\*SVY-1iq0qXmh\_Dl8D3rl0w.png)

您可以从我们的 [GitHub](https://github.com/pycaret/powerbi-nlp) 下载 PBIX 文件和数据集。

如果您想了解如何在 Jupyter notebook 中使用 PyCaret 实现主题建模，请观看这个2分钟的视频教程：

如果您对主题建模感兴趣，您还可以查看我们的 NLP 101 [Notebook 教程](https://www.pycaret.org/nlp101)，适合初学者。

关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 并订阅我们的 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g) 频道，了解更多关于 PyCaret 的内容。

### 重要链接

[用户指南/文档](https://www.pycaret.org/guide) [GitHub 仓库](https://www.github.com/pycaret/pycaret) [安装 PyCaret](https://www.pycaret.org/install) [Notebook 教程](https://www.pycaret.org/tutorial) [为 PyCaret 做贡献](https://www.pycaret.org/contribute)

### 想了解特定模块吗？

截至第一个版本 1.0.0，PyCaret 提供以下模块供使用。点击下面的链接查看 Python 中的文档和工作示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 还可以参考：

PyCaret 在 Notebook 中的入门教程：

[聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101) [回归](https://www.pycaret.org/reg101) [分类](https://www.pycaret.org/clf101)

### 想要做贡献吗？

PyCaret 是一个开源项目，欢迎大家贡献。如果您想做贡献，请随时处理 [开放问题](https://github.com/pycaret/pycaret/issues)。我们接受在 dev-1.0.1 分支上带有单元测试的拉取请求。

如果您喜欢 PyCaret，请在我们的 [GitHub 仓库](https://www.github.com/pycaret/pycaret) 上给我们一个 ⭐️。

Medium：[https://medium.com/@moez\_62905/](https://medium.com/@moez\_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

LinkedIn：[https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter：[https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)