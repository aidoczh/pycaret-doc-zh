# 在 Power BI 中使用 PyCaret 进行异常检测

### 使用 PyCaret 在 Power BI 中构建您的第一个异常检测器

#### 逐步教程：在 Power BI 中实现异常检测

#### 作者：Moez Ali

![Power BI 中的异常检测仪表板](https://cdn-images-1.medium.com/max/2000/1\*sh9LrK5WiF1pBDDR1PCK0g.png)

在我们上一篇文章《[在 Power BI 中使用 PyCaret 进行机器学习](https://towardsdatascience.com/machine-learning-in-power-bi-using-pycaret-34307f09394a)》中，我们介绍了如何在 Power BI 中集成 PyCaret 的逐步教程，使分析师和数据科学家能够在他们的仪表板和报告中添加机器学习的层次，而无需额外的许可费用。

在本文中，我们将深入探讨如何使用 PyCaret 在 Power BI 中实现异常检测。如果您之前没有听说过 PyCaret，请阅读这篇[公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)以了解更多信息。

### 本教程的学习目标

* 什么是异常检测？异常检测的类型有哪些？
* 在 Power BI 中训练和实现无监督的异常检测器。
* 分析结果并在仪表板中可视化信息。
* 如何在 Power BI 生产环境中部署异常检测器？

### 开始之前

如果您之前使用过 Python，那么您的计算机上可能已经安装了 Anaconda 发行版。如果没有，请[点击这里](https://www.anaconda.com/distribution/)下载带有 Python 3.7 或更高版本的 Anaconda 发行版。

![https://www.anaconda.com/products/individual](https://cdn-images-1.medium.com/max/2612/1\*sMceDxpwFVHDtdFi528jEg.png)

### 设置环境

在我们开始在 Power BI 中使用 PyCaret 的机器学习功能之前，我们需要创建一个虚拟环境并安装 pycaret。这是一个三步骤的过程：

[✅](https://fsymbols.com/signs/tick/) **第一步 - 创建一个 Anaconda 环境**

从开始菜单打开 **Anaconda Prompt** 并执行以下代码：

```
conda create --name **myenv** python=3.7
```

[✅](https://fsymbols.com/signs/tick/) **第二步 - 安装 PyCaret**

在 Anaconda Prompt 中执行以下代码：

```
pip install pycaret
```

安装可能需要 15-20 分钟。如果安装遇到问题，请参阅我们的[GitHub](https://www.github.com/pycaret/pycaret)页面上的已知问题和解决方法。

[✅](https://fsymbols.com/signs/tick/)**第三步 - 在 Power BI 中设置 Python 目录**

创建的虚拟环境必须与 Power BI 关联。可以使用 Power BI Desktop 中的全局设置来完成此操作（文件 → 选项 → 全局 → Python 脚本）。Anaconda 环境默认安装在以下位置：

C:\Users\***username**\*\AppData\Local\Continuum\anaconda3\envs\myenv

![文件 → 选项 → 全局 → Python 脚本](https://cdn-images-1.medium.com/max/2000/1\*zQMKuyEk8LGrOPE-NByjrg.png)

### 什么是异常检测？

异常检测是一种机器学习技术，用于识别与大多数数据明显不同的**罕见项**、**事件**或**观察结果**。

通常，异常项会导致一些问题，例如银行欺诈、结构缺陷、医疗问题或错误。实现异常检测有三种方法：

\*\*(a) 监督式：\*\*当数据集具有标签，用于标识哪些交易是异常的，哪些是正常的时使用（类似于监督分类问题）。

\*\*(b) 半监督式：\*\*半监督式异常检测的思想是仅在正常数据上训练模型（不包含任何异常）。然后，当使用训练好的模型对未见过的数据点进行预测时，它可以根据训练模型中数据的分布来判断新数据点是否正常。

\*\*(c) 无监督式：\*\*顾名思义，无监督式意味着没有标签，因此没有训练和测试数据集。在无监督学习中，模型在完整数据集上进行训练，并假设大多数实例是正常的，同时寻找似乎与其余实例最不匹配的实例。有几种无监督式异常检测算法，例如孤立森林或单类支持向量机。每种算法都有自己的方法来识别数据集中的异常。

本教程是关于在 Power BI 中使用名为 PyCaret 的 Python 库实现无监督式异常检测。本教程不涉及这些算法背后的具体细节和数学原理。

![Goldstein M, Uchida S (2016) A Comparative Evaluation of Unsupervised Anomaly Detection Algorithms for Multivariate Data. PLo](https://cdn-images-1.medium.com/max/2800/1\*-Cnyg6-F-Qd4r1Ptcf6nNw.png)

### 设置业务背景
许多公司向员工发放企业信用卡（也称为采购卡或 P 卡），以有效管理运营采购。通常，员工需要通过电子方式提交这些报销请求。收集到的数据通常是交易性的，可能包括交易日期、供应商名称、费用类型、商家和金额。

在本教程中，我们将使用美国特拉华州教育部门 2014 年至 2019 年的州雇员信用卡交易数据。这些数据可以在他们的[开放数据](https://data.delaware.gov/Government-and-Finance/Credit-Card-Spend-by-Merchant/8pzf-ge27)平台上在线获取。

![https://data.delaware.gov/Government-and-Finance/Credit-Card-Spend-by-Merchant/8pzf-ge27](https://cdn-images-1.medium.com/max/3058/1\*c8KS7taBuTRlxJ7tTL964g.png)

**免责声明：**_本教程演示了在 Power BI 中使用 PyCaret 构建异常检测器。本教程中构建的示例仪表板绝不反映实际异常，也不旨在识别异常。_

### 👉 让我们开始吧

现在您已经设置了 Anaconda 环境，安装了 PyCaret，了解了异常检测的基础知识，并具备了本教程的业务背景，让我们开始吧。

### 1. 获取数据

第一步是将数据集导入到 Power BI Desktop 中。您可以使用 Web 连接器加载数据（Power BI Desktop → 获取数据 → 从 Web）。

![Power BI Desktop → 获取数据 → 其他 → Web](https://cdn-images-1.medium.com/max/3840/1\*WMQRdUPcw8VaG0HIOiGyQQ.png)

**CSV 文件链接：**[https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/delaware_anomaly.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/delaware_anomaly.csv)

### 2. 模型训练

要在 Power BI 中训练异常检测器，我们需要在 Power Query Editor 中执行 Python 脚本（Power Query Editor → 转换 → 运行 Python 脚本）。运行以下代码作为 Python 脚本：

```
from **pycaret.anomaly** import *
dataset = **get_outliers**(dataset, ignore_features=['DEPT_NAME', 'MERCHANT', 'TRANS_DT'])
```

![Power Query Editor (转换 → 运行 Python 脚本)](https://cdn-images-1.medium.com/max/2000/1\*jLYtThjhL2rAlfOtZnG0gQ.png)

我们通过将它们传递到 **ignore_features** 参数下来忽略数据集中的一些列。有许多原因可能导致您不希望使用某些列来训练机器学习算法。

PyCaret 允许您隐藏而不是删除数据集中不需要的列，因为您可能需要这些列进行后续分析。例如，在这种情况下，我们不想使用交易日期来训练算法，因此我们将其传递给 **ignore_features**。

PyCaret 中有超过 10 种现成的异常检测算法。

![](https://cdn-images-1.medium.com/max/2000/1\*piuoq_K4B2aiyzOCkDg8MA.png)

默认情况下，PyCaret 使用 5% 的比例训练 **K-最近邻异常检测器**（即表中总行数的 5% 将被标记为异常值）。默认值可以轻松更改：

* 要更改比例值，可以在 **get_outliers( )** 函数内使用 **fraction** 参数。
* 要更改模型类型，请在 **get_outliers( )** 内使用 **model** 参数。

以下是使用 0.1 比例训练 **孤立森林** 检测器的示例代码：

```
from **pycaret.anomaly** import *
dataset = **get_outliers**(dataset, model = 'iforest', fraction = 0.1, ignore_features=['DEPT_NAME', 'MERCHANT', 'TRANS_DT'])
```

**输出：**

![异常检测结果（执行 Python 代码后）](https://cdn-images-1.medium.com/max/2000/1*RCYtFO6XDGI2-qbZdYeMfQ.png)

![最终输出（单击表后）](https://cdn-images-1.medium.com/max/2280/1*dZbf7VmCxkPUcX_p7kKJ4w.png)

原始表格附加了两列。标签（1 = 异常值，0 = 正常值）和分数（具有较高分数的数据点被归类为异常值）。应用查询以在 Power BI 数据集中查看结果。

![Power BI Desktop 中的结果（应用查询后）](https://cdn-images-1.medium.com/max/2894/1*QFJ2DJX_bGSxutOdxNmwEg.png)

### 3. 仪表板

一旦在 Power BI 中有异常值标签，这里是如何在仪表板中可视化的示例：

![仪表板的摘要页面](https://cdn-images-1.medium.com/max/2624/1*7qWjee_M6PTrAd0PJdU1yg.png)

![仪表板的详细页面](https://cdn-images-1.medium.com/max/2634/1*4ISkFG8r3LtVJq0P3793Wg.png)

您可以从我们的[GitHub](https://github.com/pycaret/powerbi-anomaly-detection)下载 PBIX 文件和数据集。

### 👉 在生产环境中实施异常检测

上述演示的是在 Power BI 中实施异常检测的一种简单方法。然而，重要的是要注意，上述方法每次刷新 Power BI 数据集时都会训练异常检测器。这可能存在两个问题：
* 当模型使用新数据重新训练时，异常标签可能会发生变化（之前标记为异常的一些交易可能不再被视为异常）。
* 您不希望每天花费数小时的时间重新训练模型。

在 Power BI 中实现异常检测的另一种方法是在生产中使用预先训练好的模型进行标记，而不是在 Power BI 中训练模型。

### **事先训练模型**

您可以使用任何集成开发环境（IDE）或笔记本来训练机器学习模型。在这个例子中，我们使用 Visual Studio Code 来训练异常检测模型。

![Visual Studio Code 中的模型训练](https://cdn-images-1.medium.com/max/2014/1\*zzymbb9ySyl3jeaFQoHxDg.png)

训练好的模型会保存为 pickle 文件，并导入到 Power Query 中用于生成异常标签（1 或 0）。

![保存为文件的异常检测流程](https://cdn-images-1.medium.com/max/2000/1\*fLnTzbd-dTRtqwxmPqI4kw.png)

如果您想了解如何在 Jupyter 笔记本中使用 PyCaret 实现异常检测，请观看这个 2 分钟的视频教程：

### 使用预训练模型

执行以下代码作为 Python 脚本，从预训练模型生成标签。

```
from **pycaret.anomaly** import *
dataset = **predict_model**('c:/.../anomaly_deployment_13052020, data = dataset)
```

![Power Query 编辑器（Transform → Run python script）](https://cdn-images-1.medium.com/max/2000/1\*VMSuDzp7FpJgddT-NjTtUQ.png)

这将产生与上面看到的相同的输出。然而，当您使用预训练模型时，标签是使用相同模型在新数据集上生成的，而不是每次刷新 Power BI 数据集时重新训练模型。

![最终输出（点击表后）](https://cdn-images-1.medium.com/max/2280/1\*dZbf7VmCxkPUcX\_p7kKJ4w.png)

### **在 Power BI 服务中使其运行**

一旦您将 .pbix 文件上传到 Power BI 服务，还需要进行一些步骤，以实现将机器学习流水线无缝集成到数据流水线中。这些步骤包括：

* **为数据集启用定期刷新** — 要为包含带有 Python 脚本的数据集的工作簿启用定期刷新，请参阅[配置定期刷新](https://docs.microsoft.com/en-us/power-bi/connect-data/refresh-scheduled-refresh)，其中还包括有关**个人网关**的信息。
* **安装个人网关** — 您需要在存放文件的计算机上安装一个**个人网关**，并且已安装 Python；Power BI 服务必须能够访问该 Python 环境。您可以获取有关如何[安装和配置个人网关](https://docs.microsoft.com/en-us/power-bi/connect-data/service-gateway-personal-mode)的更多信息。

如果您有兴趣了解更多关于异常检测的信息，请查看我们的[笔记本教程](https://pycaret.org/ano101/)。

### PyCaret 1.0.1 即将发布！

我们收到了社区的大力支持和反馈。我们正在积极努力改进 PyCaret，并为下一个版本做准备。**PyCaret 1.0.1 将更加强大和优秀**。如果您想分享您的反馈并帮助我们进一步改进，您可以在网站上[填写此表格](https://www.pycaret.org/feedback)，或在我们的[GitHub](https://www.github.com/pycaret/)或[LinkedIn](https://www.linkedin.com/company/pycaret/)页面上留下评论。

关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)，订阅我们的[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)频道，了解更多关于 PyCaret 的信息。

### 重要链接

[用户指南 / 文档](https://www.pycaret.org/guide) [GitHub 仓库](https://www.github.com/pycaret/pycaret) [安装 PyCaret](https://www.pycaret.org/install) [笔记本教程](https://www.pycaret.org/tutorial) [为 PyCaret 做贡献](https://www.pycaret.org/contribute)

### 想了解特定模块吗？

截至第一个版本 1.0.0，PyCaret 提供以下模块供使用。点击下面的链接查看 Python 中的文档和示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 还可以查看：

在笔记本中的 PyCaret 入门教程：

[聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101) [回归](https://www.pycaret.org/reg101) [分类](https://www.pycaret.org/clf101)

### 想要做出贡献吗？
PyCaret 是一个开源项目。欢迎所有人贡献力量。如果您想要贡献，请随时在 [开放问题](https://github.com/pycaret/pycaret/issues) 上进行工作。拉取请求将在 dev-1.0.1 分支上接受单元测试。

如果您喜欢 PyCaret，请在我们的 [GitHub 仓库](https://www.github.com/pycaret/pycaret) 上给我们 ⭐️。

Medium：[https://medium.com/@moez\_62905/](https://medium.com/@moez\_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)

LinkedIn：[https://www.linkedin.com/in/profile-moez/](https://www.linkedin.com/in/profile-moez/)

Twitter：[https://twitter.com/moezpycaretorg1](https://twitter.com/moezpycaretorg1)