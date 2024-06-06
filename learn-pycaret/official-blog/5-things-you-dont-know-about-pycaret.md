# PyCaret 的 5 个你不知道的事情

### PyCaret 的 5 个你不知道的事情

#### 作者：Moez Ali

![来自 PyCaret 作者](https://cdn-images-1.medium.com/max/2000/1\*1HEakzOhZRd21FfAT3TyZw.png)

### PyCaret

PyCaret 是一个开源的 Python 机器学习库，用于在**低代码**环境中训练和部署监督学习和无监督学习模型。它以易用性和高效性而闻名。

与其他开源机器学习库相比，PyCaret 是一个备选的低代码库，可以用几个单词取代数百行代码。

如果你之前没有使用过 PyCaret 或想要了解更多，一个很好的起点是[这里](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)。

> ## “在与许多每天使用 PyCaret 的数据科学家交谈后，我列出了 PyCaret 的 5 个功能，它们鲜为人知，但非常强大。” — Moez Ali

### 👉 你可以调整无监督实验中的“n 参数”

在无监督机器学习中，“n 参数”即聚类实验中的聚类数、异常检测中异常值的比例以及主题建模中的主题数，至关重要。

当实验的最终目标是使用无监督实验的结果来预测结果（分类或回归）时，\*\*pycaret.clustering **模块中的 tune\_model() 函数、\*\*pycaret.anomaly **模块中的 tune\_model() 函数以及 **pycaret.nlp **模块中的 tune\_model() 函数非常有用。

为了理解这一点，让我们看一个使用 “[Kiva](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/kiva.csv)” 数据集的示例。

![](https://cdn-images-1.medium.com/max/2000/1\*-161ThHHhI7lMVHuY4jbsA.png)

这是一个微型银行贷款数据集，每一行代表一个借款人及其相关信息。列 ‘en’ 捕获每个借款人的贷款申请文本，列 ‘status’ 表示借款人是否违约（违约 = 1 或未违约 = 0）。

你可以使用 \*\*pycaret.nlp **中的 tune\_model() 函数，根据监督实验的目标变量（即预测最终目标变量所需的最佳主题数）来优化 **num\_topics **参数。你可以使用 **estimator** 参数（在本例中为 ‘xgboost’）定义训练模型。该函数返回一个经过训练的主题模型，并显示每次迭代的监督指标。

![](https://cdn-images-1.medium.com/max/2314/1\*RIOVzRCYsA-r-c1Iy7x\_5w.png)

### 👉 通过增加“n\_iter”来改善超参数调优的结果

\*\*pycaret.classification **模块和 **pycaret.regression **模块中的 tune\_model() 函数采用预定义的超参数调优的随机网格搜索。这里默认的迭代次数设置为 10。

\*\*tune\_model **的结果可能并不一定优于使用 **create\_model **创建的基本模型的结果。由于网格搜索是随机的，你可以增加 **n\_iter **参数来提高性能。请看下面的示例：

![](https://cdn-images-1.medium.com/max/2000/1\*LRu2R2f4rXYkOrWVC6ul5A.png)

### 👉 你可以在 setup 函数中以编程方式定义数据类型

当你初始化 **setup **函数时，将要求通过用户输入确认数据类型。在许多情况下，当你将脚本作为工作流的一部分运行或将其作为远程内核执行（例如 Kaggle 笔记本）时，需要以编程方式提供数据类型，而不是通过用户输入框。

请看下面使用 “[insurance](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/insurance.csv)” 数据集的示例。

![](https://cdn-images-1.medium.com/max/2000/1\*q2WFe3JgZ1SxSkiuuvonKQ.png)

**silent** 参数设置为 True 以避免输入，\*\*categorical\_features **参数以字符串形式接受分类列的名称，\*\*numeric\_features **参数以字符串形式接受数值列的名称。

### 👉 你可以忽略某些列进行模型构建

在许多情况下，数据集中存在一些特征，你不一定想要删除，但希望在训练机器学习模型时忽略这些特征。一个很好的例子是聚类问题，你希望在创建聚类时忽略某些特征，但后来需要这些列来分析聚类标签。在这种情况下，你可以使用 **setup **中的 **ignore\_features **参数来忽略这些特征。

在下面的示例中，我们将执行一个聚类实验，我们希望忽略 **‘Country Name’** 和 **‘Indicator Name’**。

![](https://cdn-images-1.medium.com/max/2000/1\*0xcKweKh77A-vgzb5u5\_mw.png)
### 👉在二元分类中，您可以优化概率阈值 %

在分类问题中，**假阳性**的成本几乎永远不同于**假阴性**的成本。因此，如果您正在为一个业务问题优化解决方案，其中**类型1**和**类型2**错误的影响不同，您可以通过分别定义真阳性、真阴性、假阳性和假阴性的成本，优化分类器的概率阈值值来优化自定义损失函数。默认情况下，所有分类器的阈值为0.5。

查看以下示例，使用“[credit](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/credit.csv)”数据集。

![](https://cdn-images-1.medium.com/max/2000/1\*oCsUyp91pSJSDdzi-ho6QA.png)

然后，您可以在`predict_model`函数中传递**0.2**作为`probability_threshold`参数，以将0.2用作分类正类的阈值。请参见以下示例：

### PyCaret 2.0.0 即将发布！

我们收到了来自数据科学界的压倒性支持和反馈。我们正在积极努力改进 PyCaret，并为下一个版本做准备。**PyCaret 2.0.0 将更加强大和优秀**。如果您想分享您的反馈并帮助我们进一步改进，您可以在网站上[填写此表格](https://www.pycaret.org/feedback)，或在我们的[GitHub](https://www.github.com/pycaret/)或[LinkedIn](https://www.linkedin.com/company/pycaret/)页面上留下评论。

关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)，订阅我们的[YouTube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)频道，了解更多关于 PyCaret 的信息。

### 想了解特定模块吗？

截至首个版本 1.0.0，PyCaret 提供以下可用模块。点击下面的链接查看 Python 中的文档和示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 还可以查看：

PyCaret 在 Notebook 中的入门教程：

[分类](https://www.pycaret.org/clf101) [回归](https://www.pycaret.org/reg101) [聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101)

### 想要贡献吗？

PyCaret 是一个开源项目。欢迎每个人贡献。如果您想贡献，请随时处理[开放问题](https://github.com/pycaret/pycaret/issues)。我们接受带有单元测试的拉取请求，该请求位于 dev-1.0.1 分支上。

如果您喜欢 PyCaret，请在我们的[GitHub 仓库](https://www.github.com/pycaret/pycaret)上给我们 ⭐️。

Medium: [https://medium.com/@moez\_62905/](https://medium.com/@moez\_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

LinkedIn: [https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter: [https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)