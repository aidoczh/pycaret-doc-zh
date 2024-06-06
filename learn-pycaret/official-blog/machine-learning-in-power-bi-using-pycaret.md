# 在 Power BI 中使用 PyCaret 进行机器学习

### 在 Power BI 中使用 PyCaret 进行机器学习

#### 一份逐步教程，让您能在几分钟内在 Power BI 中实现机器学习

#### 作者：Moez Ali

![机器学习与商业智能的结合](https://cdn-images-1.medium.com/max/2000/1\*Q34J2tT\_yGrVV0NU38iMig.jpeg)

### **PyCaret 1.0.0**

上周，我们宣布了 [PyCaret](https://www.pycaret.org)，这是一个在 Python 中的开源机器学习库，可以在一个\*\*低代码\*\*环境中训练和部署机器学习模型。在我们的[上一篇文章](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46) 中，我们演示了如何在 Jupyter Notebook 中使用 PyCaret 来训练和部署 Python 中的机器学习模型。

在本文中，我们将介绍一个**逐步教程**，展示如何将 PyCaret 集成到 [Power BI](https://powerbi.microsoft.com/en-us/) 中，从而使分析师和数据科学家能够在他们的仪表板和报告中添加一层机器学习，而无需额外的许可证或软件成本。PyCaret 是一个开源且\*\*免费使用\*\*的 Python 库，具有广泛的功能，专门设计用于在 Power BI 中运行。

通过本文，您将学会如何在 Power BI 中实现以下功能：

- **聚类** — 将具有相似特征的数据点分组。
- \*\*异常检测\*\* — 识别数据中的罕见观察值 / 异常值。
- \*\*自然语言处理\*\* — 通过主题建模分析文本数据。
- \*\*关联规则挖掘\*\* — 在数据中找到有趣的关系。
- \*\*分类\*\* — 预测二元分类标签。
- \*\*回归\*\* — 预测销售额、价格等连续值

> ## “PyCaret 通过为业务分析师、领域专家、公民数据科学家和经验丰富的数据科学家提供**免费、开源和低代码**的机器学习解决方案，实现了机器学习和高级分析的民主化”。

### Microsoft Power BI

Power BI 是一款商业分析解决方案，让您可以可视化数据并在组织内共享见解，或将其嵌入到您的应用程序或网站中。在本教程中，我们将通过将 PyCaret 库导入 Power BI，使用 [Power BI Desktop](https://powerbi.microsoft.com/en-us/downloads/) 进行机器学习。

### 开始之前

如果您以前使用过 Python，很可能已经在计算机上安装了 Anaconda 发行版。如果没有，请[点击这里](https://www.anaconda.com/distribution/)下载带有 Python 3.7 或更高版本的 Anaconda 发行版。

![https://www.anaconda.com/distribution/](https://cdn-images-1.medium.com/max/2612/1\*sMceDxpwFVHDtdFi528jEg.png)

### 设置环境

在 Power BI 中使用 PyCaret 的机器学习功能之前，我们必须创建一个虚拟环境并安装 pycaret。这是一个三步过程：

[✅](https://fsymbols.com/signs/tick/) **第一步 — 创建一个 Anaconda 环境**

从开始菜单打开\*\* Anaconda Prompt \*\*，运行以下代码：

```
conda create --name **myenv** python=3.6
```

![Anaconda Prompt — 创建环境](https://cdn-images-1.medium.com/max/2198/1\*Yv-Ee99UJXCW2iTL1HUr5Q.png)

[✅](https://fsymbols.com/signs/tick/) **第二步 — 安装 PyCaret**

在 Anaconda Prompt 中运行以下代码：

```
conda activate **myenv**
pip install pycaret
```

安装可能需要 10 – 15 分钟。

[✅](https://fsymbols.com/signs/tick/)**第三步 — 在 Power BI 中设置 Python 目录**

创建的虚拟环境必须与 Power BI 关联。这可以通过 Power BI Desktop 中的全局设置完成（文件 → 选项 → 全局 → Python 脚本）。Anaconda 环境默认安装在：

C:\Users\***用户名**\*\AppData\Local\Continuum\anaconda3\envs\myenv

![文件 → 选项 → 全局 → Python 脚本](https://cdn-images-1.medium.com/max/2000/1\*zQMKuyEk8LGrOPE-NByjrg.png)

### 📘 示例 1 — 在 Power BI 中进行聚类

聚类是一种机器学习技术，用于将具有相似特征的数据点分组。这些分组对于探索数据、识别模式和分析数据子集非常有用。一些常见的聚类业务用例包括：

✔ 用于营销目的的客户细分。

✔ 用于促销和折扣的客户购买行为分析。

✔ 在流行病爆发中识别地理集群，如 COVID-19。

在本教程中，我们将使用 PyCaret 的 [github 仓库](https://github.com/pycaret/pycaret/blob/master/datasets/jewellery.csv) 上提供的 **‘jewellery.csv’** 文件。您可以使用 Web 连接器加载数据。（Power BI Desktop → 获取数据 → 从 Web）。

\*\*csv 文件链接: [\*\*https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/jewellery.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/jewellery.csv)
![Power BI Desktop → 获取数据 → 其他 → 网页](https://cdn-images-1.medium.com/max/2000/1\*MdUeug0LSZu451-fBI5J\_Q.png)

![来自 jewellery.csv 的样本数据点](https://cdn-images-1.medium.com/max/2000/1\*XhXJjUHpEqOc7-RQ1fWoYQ.png)

#### **K-Means 聚类**

为了训练一个聚类模型，我们将在 Power Query Editor 中执行 Python 脚本（Power Query Editor → 转换 → 运行 Python 脚本）。

![Power Query Editor 中的选项卡](https://cdn-images-1.medium.com/max/2000/1\*F18LNIkoWtAFr4P80J-U8Q.png)

运行以下代码作为 Python 脚本：

```
from **pycaret.clustering **import *****
dataset = **get_clusters**(data = dataset)
```

![Power Query Editor（转换 → 运行 Python 脚本）](https://cdn-images-1.medium.com/max/2000/1\*nYqJWQM6NI3q3tLJXIVxtg.png)

#### **输出:**

![聚类结果（代码执行后）](https://cdn-images-1.medium.com/max/2000/1\*RCYtFO6XDGI2-qbZdYeMfQ.png)

![最终输出（点击表格后）](https://cdn-images-1.medium.com/max/2000/1\*PXWUtrYrNikCRDqhn\_TgDw.png)

原始表格中附加了一个名为“Cluster”的新列，其中包含标签。

一旦应用查询（Power Query Editor → 主页 → 关闭并应用），您可以在 Power BI 中可视化聚类结果：

![](https://cdn-images-1.medium.com/max/2000/1\*8im-qPdXXBblPD7jiodQpg.png)

默认情况下，PyCaret 使用 4 个聚类（即表中的所有数据点被分为 4 组）训练 K-Means 聚类模型。可以轻松更改默认值：

* 要更改聚类数目，可以在 **get_clusters( )** 函数中使用 **num_clusters** 参数。
* 要更改模型类型，请在 **get_clusters( )** 中使用 **model** 参数。

以下是使用 6 个聚类训练 K-Modes 模型的示例代码：

```
from **pycaret.clustering **import *
dataset = **get_clusters**(dataset, model = 'kmodes', num_clusters = 6)
```

PyCaret 提供了 9 种现成的聚类算法：

![](https://cdn-images-1.medium.com/max/2000/1\*Wdy201wGxmV3NwS9lzHwsA.png)

在训练聚类模型之前，PyCaret 会自动执行所有必要的预处理任务，例如[缺失值插补](https://pycaret.org/missing-values/)（如果表中有任何缺失或空值）、[归一化](https://www.pycaret.org/normalization)或[独热编码](https://pycaret.org/one-hot-encoding/)。点击[此处](https://www.pycaret.org/preprocessing)了解有关 PyCaret 预处理功能的更多信息。

💡 在此示例中，我们使用 **get_clusters( )** 函数将聚类标签分配给原始表格。每次刷新查询时，都会重新计算聚类。实现这一目标的另一种方法是使用 **predict_model( )** 函数，使用 Python 或 Power BI 中的预训练模型来预测聚类标签（请参见下面的示例 5，了解如何在 Power BI 环境中训练机器学习模型）。

💡 如果您想学习如何在 Jupyter Notebook 中使用 Python 训练聚类模型，请参阅我们的[聚类 101 入门教程](https://www.pycaret.org/clu101)（无需编程背景）。

### 📘 示例 2 — 在 Power BI 中进行异常检测

异常检测是一种机器学习技术，用于通过检查与大多数行显著不同的表格行来识别**罕见的项目**、**事件**或**观察结果**。通常，异常项会导致某种问题，例如银行欺诈、结构缺陷、医疗问题或错误。异常检测的一些常见业务用例包括：

✔ 使用财务数据进行欺诈检测（信用卡、保险等）。

✔ 使用系统安全、恶意软件进行入侵检测或监测网络流量的波动。

✔ 在数据集中识别多变量离群值。

在本教程中，我们将使用 PyCaret 的 [github 仓库](https://github.com/pycaret/pycaret/blob/master/datasets/anomaly.csv) 上可用的 **'anomaly.csv'** 文件。您可以使用 Web 连接器加载数据（Power BI Desktop → 获取数据 → 从 Web）。

**csv 文件链接：[https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/anomaly.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/anomaly.csv)**

![来自 anomaly.csv 的样本数据点](https://cdn-images-1.medium.com/max/2476/1\*M0uBBbcEYizdZgpeKlftlQ.png)

#### K-最近邻异常检测器

与聚类类似，我们将在 Power Query Editor 中运行 Python 脚本（转换 → 运行 Python 脚本）来训练异常检测模型。运行以下代码作为 Python 脚本：

```
from **pycaret.anomaly **import *****
dataset = **get_outliers**(data = dataset)
```

![Power Query Editor（转换 → 运行 Python 脚本）](https://cdn-images-1.medium.com/max/2000/1\*re7Oj-bPUHok7pCbmeWFuw.png)

#### **输出:**

![异常检测结果（代码执行后）](https://cdn-images-1.medium.com/max/2000/1\*RCYtFO6XDGI2-qbZdYeMfQ.png)

![最终输出（点击表格后）](https://cdn-images-1.medium.com/max/2002/1\*J7\_5ZAM7dFNVnMcgxV\_N1A.png)

在原始表格中添加了两列。Label（1表示异常值，0表示正常值）和Score（具有高分数的数据点被归类为异常值）。

应用查询后，您可以在Power BI中可视化异常检测的结果：

![](https://cdn-images-1.medium.com/max/2000/1\*tfn6W5vV1pUE11hTPCzdpA.png)

默认情况下，PyCaret使用5%的比例训练**K最近邻异常检测器**（即表格中总行数的5%将被标记为异常值）。可以轻松更改默认值：

- 要更改比例值，可以在**get\_outliers()**函数中使用***fraction***参数。
- 要更改模型类型，可以在**get\_outliers()**函数中使用***model***参数。

以下是使用0.1比例训练**孤立森林**模型的代码示例：

```
from **pycaret.anomaly **import *
dataset = **get_outliers**(dataset, model = 'iforest', fraction = 0.1)
```

PyCaret中有10多种可供使用的异常检测算法：

![](https://cdn-images-1.medium.com/max/2000/1\*piuoq\_K4B2aiyzOCkDg8MA.png)

在训练异常检测模型之前，PyCaret会自动执行所有必要的预处理任务，例如[缺失值插补](https://pycaret.org/missing-values/)（如果表格中有任何缺失或null值）、[归一化](https://www.pycaret.org/normalization)或[独热编码](https://pycaret.org/one-hot-encoding/)。点击[此处](https://www.pycaret.org/preprocessing)了解有关PyCaret预处理功能的更多信息。

💡 在此示例中，我们使用**get\_outliers()**函数为分析分配了异常值标签和分数。每次刷新查询时，都会重新计算异常值。实现此功能的另一种方法是使用**predict\_model()**函数使用预训练模型在Python或Power BI中预测异常值（请参见下面的示例5，了解如何在Power BI环境中训练机器学习模型）。

💡 如果您想学习如何使用Jupyter Notebook在Python中训练异常检测器，请参阅我们的[异常检测101初学者教程](https://www.pycaret.org/ano101)（无需编程背景）。

### 📘 示例3 — 自然语言处理

在分析文本数据时，有多种技术可供选择，其中**主题建模**是一种常用的技术。主题建模是一种用于发现文档集合中抽象主题的统计模型。主题建模是文本挖掘中常用的工具，用于发现文本数据中的隐藏语义结构。

在本教程中，我们将使用PyCaret的[github存储库](https://github.com/pycaret/pycaret/blob/master/datasets/kiva.csv)上提供的**‘kiva.csv’**文件。您可以使用Web连接器加载数据（Power BI Desktop → 获取数据 → 从Web）。

**csv文件链接：[https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/kiva.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/kiva.csv)**

#### **潜在狄利克雷分配**

在Power Query Editor中将以下代码作为Python脚本运行：

```
from **pycaret.nlp **import *****
dataset = **get_topics**(data = dataset, text = 'en')
```

![Power Query Editor（转换→运行Python脚本）](https://cdn-images-1.medium.com/max/2000/1\*QNaOFbKVJtkG6TjH-z0nxw.png)

**‘en’**是包含表格**‘kiva’**中文本的列名。

#### 输出：

![主题建模结果（代码执行后）](https://cdn-images-1.medium.com/max/2000/1\*RCYtFO6XDGI2-qbZdYeMfQ.png)

![最终输出（点击表格后）](https://cdn-images-1.medium.com/max/2536/1\*kP9luTZMmeo7-uEI1lYKlQ.png)

代码执行后，原始表格将附加具有主题权重和主导主题的新列。有许多方法可以在Power BI中可视化主题模型的输出。以下是一个示例：

![](https://cdn-images-1.medium.com/max/2000/1\*yZHDO-9UXZ3L1lFBXMMCPg.png)

默认情况下，PyCaret使用4个主题训练潜在狄利克雷分配模型。可以轻松更改默认值：

- 要更改主题数量，可以在**get\_topics()**函数中使用***num\_topics***参数。
- 要更改模型类型，可以在**get\_topics()**函数中使用***model***参数。

以下是使用10个主题训练**非负矩阵分解模型**的示例代码：

```
from **pycaret.nlp **import *
dataset = **get_topics**(dataset, 'en', model = 'nmf', num_topics = 10)
```

PyCaret提供了以下可供使用的主题建模算法：

![](https://cdn-images-1.medium.com/max/2000/1\*YhRd9GgWw1kblnJezqZd5w.png)

### 📘 示例4 — Power BI中的关联规则挖掘
关联规则挖掘是一种基于规则的机器学习技术，用于发现数据库中变量之间的有趣关系。它旨在利用有趣度量来识别强规则。一些关联规则挖掘的常见业务用例包括：

✔ 市场篮子分析，以了解经常一起购买的物品。

✔ 医学诊断，帮助医生确定给定因素和症状的疾病发生概率。

在本教程中，我们将使用 PyCaret 的 [github 仓库](https://github.com/pycaret/pycaret/blob/master/datasets/france.csv) 上提供的 **‘france.csv’** 文件。您可以使用 Web 连接器加载数据。（Power BI Desktop → 获取数据 → 从 Web）。

**csv 文件链接: [https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/france.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/france.csv)**

![来自 france.csv 的样本数据点](https://cdn-images-1.medium.com/max/2484/1*2S-OwdafFh30hWTzFDC_WQ.png)

#### Apriori 算法

现在应该清楚了，所有 PyCaret 函数都是在 Power Query Editor 中作为 Python 脚本执行的（转换 → 运行 Python 脚本）。运行以下代码，使用 Apriori 算法训练一个关联规则模型：

```
from **pycaret.arules** import *
dataset = **get_rules**(dataset, transaction_id = 'InvoiceNo', item_id = 'Description')
```

![Power Query Editor (转换 → 运行 Python 脚本)](https://cdn-images-1.medium.com/max/2000/1*c2QmWam_1008OCEf0Ct46w.png)

**‘InvoiceNo’** 是包含交易 ID 的列，**‘Description’** 包含感兴趣的变量，即产品名称。

#### **输出:**

![关联规则挖掘结果（执行代码后）](https://cdn-images-1.medium.com/max/2000/1*RCYtFO6XDGI2-qbZdYeMfQ.png)

![最终输出（单击表格后）](https://cdn-images-1.medium.com/max/2518/1*H4rGqsxDtJyVu24yc_UWHw.png)

它返回一个包含前因和后果以及支持度、置信度、提升度等相关指标的表格。[点击这里](https://www.pycaret.org/association-rule) 了解更多关于 PyCaret 中关联规则挖掘的信息。

### 📘 示例 5 — Power BI 中的分类

分类是一种监督式机器学习技术，用于预测分类的类别标签（也称为二元变量）。分类的一些常见业务用例包括：

✔ 预测客户贷款/信用卡违约。

✔ 预测客户流失（客户是否会留下）。

✔ 预测患者结果（患者是否患病）。

在本教程中，我们将使用 PyCaret 的 [github 仓库](https://github.com/pycaret/pycaret/blob/master/datasets/employee.csv) 上提供的 **‘employee.csv’** 文件。您可以使用 Web 连接器加载数据。（Power BI Desktop → 获取数据 → 从 Web）。

**csv 文件链接: [https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/employee.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/employee.csv)**

**目标:** 表 **‘employee’** 包含公司中 15,000 名在职员工的信息，如在公司的时间、平均每月工作小时数、晋升历史、部门等。根据所有这些列（在机器学习术语中也称为 _特征_），目标是预测员工是否会离开公司，由列 **‘left’** 表示（1 表示是，0 表示否）。

与聚类、异常检测和 NLP 示例不同，这些属于无监督机器学习范畴，分类是一种 **监督技术**，因此它分为两部分实现：

#### **第 1 部分: 在 Power BI 中训练分类模型**

第一步是在 Power Query Editor 中创建表 **‘employee’** 的副本，用于训练模型。

![Power Query Editor → 右键单击 ‘employee’ → 复制](https://cdn-images-1.medium.com/max/2760/1*9t8FyRshmdBqzONMgMRQcQ.png)

在新创建的副本表 **‘employee (model training)’** 中运行以下代码以训练分类模型：

```
# 导入分类模块并设置环境

from **pycaret.classification** import *****
clf1 = **setup**(dataset, target = 'left', silent = True)

# 训练并保存 xgboost 模型

xgboost = **create_model**('xgboost', verbose = False)
final_xgboost = **finalize_model**(xgboost)
**save_model**(final_xgboost, 'C:/Users/*username*/xgboost_powerbi')
```

![Power Query Editor (转换 → 运行 Python 脚本)](https://cdn-images-1.medium.com/max/2000/1*0qLtTngg_uI31JTSPLNSiQ.png)

#### 输出:

此脚本的输出将是一个在指定位置保存的 **pickle 文件**。pickle 文件包含整个数据转换流水线以及训练好的模型对象。
💡 一个替代方案是在 Jupyter 笔记本中训练模型，而不是在 Power BI 中。在这种情况下，Power BI 仅用于在前端生成预测，使用在 Jupyter 笔记本中训练的预训练模型，该模型将作为 pickle 文件导入到 Power BI 中（请参阅下面的第 2 部分）。要了解有关在 Python 中使用 PyCaret 的更多信息，请[点击这里](https://www.pycaret.org/tutorial)。

💡 如果您想学习如何在 Python 中使用 Jupyter Notebook 训练分类模型，请查看我们的[二元分类 101 初学者教程](https://www.pycaret.org/clf101)（无需编码背景）。

PyCaret 中有 18 种现成的分类算法可供使用：

![](https://cdn-images-1.medium.com/max/2000/1\*hvcdSTqA6Qla7YlWMkBmhA.png)

#### 第 2 部分：使用训练好的模型生成预测

现在，我们可以在原始的 **‘employee’** 表上使用训练好的模型来预测员工是否会离开公司（1 或 0）以及概率 %。运行以下代码作为 Python 脚本以生成预测：

```
from **pycaret.classification** import *****
xgboost = **load_model**('c:/users/*username*/xgboost_powerbi')
dataset = **predict_model**(xgboost, data = dataset)
```

#### 输出：

![分类预测（执行代码后）](https://cdn-images-1.medium.com/max/2000/1\*RCYtFO6XDGI2-qbZdYeMfQ.png)

![最终输出（单击表后）](https://cdn-images-1.medium.com/max/2482/1\*9Ib1KC\_9MTYEV\_xd8fHExQ.png)

原始表格附加了两列。**‘Label’** 列表示预测结果，**‘Score’** 列是结果的概率。

在此示例中，我们对用于训练模型的相同数据进行了预测，仅用于演示目的。在实际设置中，**‘Left’** 列是实际结果，在预测时是未知的。

在本教程中，我们训练了一个 **极端梯度提升（‘xgboost’）** 模型，并用它生成了预测。我们之所以这样做，纯粹是为了简单起见。实际上，您可以使用 PyCaret 预测任何类型的模型或模型链。

PyCaret 的 **predict\_model( )** 函数可以与使用 PyCaret 创建的 pickle 文件无缝配合工作，因为它包含整个转换管道以及训练好的模型对象。[点击这里](https://www.pycaret.org/predict-model)了解有关 **predict\_model** 函数的更多信息。

💡 所有训练分类模型所需的预处理任务，如[缺失值插补](https://pycaret.org/missing-values/)（如果表中有任何缺失或空值）、[独热编码](https://pycaret.org/one-hot-encoding/)或[目标编码](https://www.pycaret.org/one-hot-encoding)，都会在训练模型之前自动执行。[点击这里](https://www.pycaret.org/preprocessing)了解有关 PyCaret 预处理功能的更多信息。

### 📘 示例 6— Power BI 中的回归

**回归** 是一种监督式机器学习技术，用于以最佳方式预测连续结果，给定过去数据及其相应的过去结果。与用于预测二元结果（如是或否（1 或 0））的分类不同，回归用于预测连续值，如销售额、价格、数量等。

在本教程中，我们将使用 pycaret 的 [github 仓库](https://github.com/pycaret/pycaret/blob/master/datasets/boston.csv) 上提供的 **‘boston.csv’** 文件。您可以使用 Web 连接器加载数据。（Power BI Desktop → 获取数据 → 从 Web 获取）。

**csv 文件链接: [https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/boston.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/boston.csv)**

**目标：** 表 **‘boston’** 包含波士顿的 506 所房屋的信息，如平均房间数、物业税率、人口等。根据这些列（在机器学习术语中也称为 _特征_），目标是预测房屋的中位价值，由列 **‘medv’** 表示。

#### 第 1 部分：在 Power BI 中训练回归模型

第一步是在 Power Query Editor 中创建 **‘boston’** 表的副本，用于训练模型。

在新的副本表中运行以下代码作为 Python 脚本：

```
# 导入回归模块并设置环境

from **pycaret.regression **import *****
clf1 = **setup**(dataset, target = 'medv', silent = True)

# 训练并保存 catboost 模型

catboost = **create_model**('catboost', verbose = False)
final_catboost = **finalize_model**(catboost)
**save_model**(final_catboost, 'C:/Users/*username*/catboost_powerbi')
```

#### 输出：

此脚本的输出将是一个在指定位置保存的 **pickle 文件**。pickle 文件包含整个数据转换管道以及训练好的模型对象。

PyCaret 中有超过 20 种现成的回归算法可供使用：
#### 第二部分：使用训练好的模型生成预测结果

我们现在可以使用训练好的模型来预测房屋的中位数价值。在原始表格\*_‘boston’_ \*\*\*中运行以下代码作为Python脚本：

```python
from **pycaret.classification** import *****
xgboost = **load_model**('c:/users/*username*/xgboost_powerbi')
dataset = **predict_model**(xgboost, data = dataset)
```

#### 输出结果：

![回归预测结果（代码执行后）](https://cdn-images-1.medium.com/max/2000/1\*RCYtFO6XDGI2-qbZdYeMfQ.png)

![最终输出结果（点击表格后）](https://cdn-images-1.medium.com/max/2408/1\*0A1cf\_nsj2SULtNEjEu4tA.png)

原始表格中附加了一个名为**‘Label’**的新列，其中包含了预测结果。

在这个例子中，我们仅为演示目的在相同的数据上进行了预测，而这些数据也是用于训练模型的。在实际情况下，**‘medv’**列是实际的结果，在预测时是未知的。

💡 在训练模型之前，所有预处理任务，如[缺失值插补](https://pycaret.org/missing-values/)（如果表格中有任何缺失或\*null\*值）、[独热编码](https://pycaret.org/one-hot-encoding/)或[目标变量转换](https://pycaret.org/transform-target/)，都会在训练模型之前自动执行。[点击这里](https://www.pycaret.org/preprocessing)了解更多关于PyCaret预处理功能的信息。

### 下一个教程

在\*\*使用PyCaret在Power BI中进行机器学习\*\*系列的下一个教程中，我们将更深入地探讨PyCaret中的高级预处理功能。我们还将看到如何在Power BI中将机器学习解决方案投入生产，并利用[PyCaret](https://www.pycaret.org)在Power BI前端的强大功能。

如果您想了解更多，请保持联系。

关注我们的[Linkedin](https://www.linkedin.com/company/pycaret/)页面并订阅我们的[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)频道。

### 还可以参考：

初学者级别的Python笔记本：

[聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101) [回归](https://www.pycaret.org/reg101) [分类](https://www.pycaret.org/clf101)

### 开发计划中有哪些内容？

我们正在积极改进PyCaret。我们未来的开发计划包括一个新的\*\*时间序列预测\*\*模块，与\*\*TensorFlow\*\*的集成以及对PyCaret的可扩展性的重大改进。如果您想分享您的反馈并帮助我们进一步改进，您可以在网站上[填写此表单](https://www.pycaret.org/feedback)或在我们的[Github](https://www.github.com/pycaret/)或[LinkedIn](https://www.linkedin.com/company/pycaret/)页面上留言。

### 想了解特定模块的内容吗？

截至第一个版本1.0.0，PyCaret有以下可用的模块。点击下面的链接查看Python中的文档和工作示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)

### 重要链接

[用户指南/文档](https://www.pycaret.org/guide) [Github仓库](https://www.github.com/pycaret/pycaret) [安装PyCaret](https://www.pycaret.org/install) [笔记本教程](https://www.pycaret.org/tutorial) [为PyCaret做贡献](https://www.pycaret.org/contribute)

如果您喜欢PyCaret，请在我们的[github仓库](https://www.github.com/pycaret/pycaret)上给我们一个⭐️。

在Medium上关注我：[https://medium.com/@moez\_62905/](https://medium.com/@moez\_62905/machine-learning-in-power-bi-using-pycaret-34307f09394a)