# 在Tableau中使用PyCaret进行机器学习

### 在几分钟内设置机器学习流程的逐步集成指南

#### 作者：Andrew Cowan-Nagora

[PyCaret](https://www.pycaret.org/) 是一个最近发布的开源机器学习库，使用Python在一个低代码环境中训练和部署机器学习模型。要了解更多关于PyCaret的信息，请阅读这篇[公告](https://towardsdatascience.com/announcing-pycaret-an-open-source-low-code-machine-learning-library-in-python-4a1f1aad8d46)。

本文将演示如何将PyCaret与Tableau Desktop和Tableau Prep集成，为分析师和数据科学家在他们的仪表板、报告和可视化中添加机器学习层面打开新的途径。通过减少编码所需的时间以及购买额外软件的需求，现在可以在组织中已经熟悉和可用的环境中进行快速原型设计。

### 学习目标

* 在PyCaret中训练一个监督式机器学习模型并创建一个机器学习流程
* 将训练好的机器学习流程加载到Tableau Desktop和Tableau Prep中
* 创建一个仪表板，传达模型的洞察
* 了解如何使用Tableau将模型部署到生产环境中

### 直销业务背景

这个例子将重点介绍如何设置一个基本的直销倾向模型，该模型使用分类算法来预测哪些客户在收到短信或电子邮件优惠后最有可能发起访问。

然后将创建一个仪表板，可以使用训练好的模型预测新的营销活动的成功程度，这对于设计促销计划的营销人员非常有价值。

通过使用PyCaret和Tableau，企业可以快速设置报告产品，使用现有软件和最少的开发时间持续生成预测视图。

### 开始之前

需要以下软件才能跟随操作：

**1 — Tableau Desktop**

[Tableau Desktop](https://www.tableau.com/products/desktop) 是一款可视化分析工具，用于连接数据、构建交互式仪表板并在整个组织中共享洞察。

![](https://cdn-images-1.medium.com/max/2604/1\*XVjP6x5eMCCoEAlv\_gb6-Q.png)

**2 — Tableau Prep**

[Tableau Prep](https://www.tableau.com/products/prep) 提供了一个可视化界面，通过设置流程和计划来组合、清理和整理数据。

![](https://cdn-images-1.medium.com/max/3126/1\*TEG93kgXECyKsgYqXTJ5gw.png)

**3 — Python 3.7 或更高版本**

Anaconda是一个免费的、开源的Python编程语言的数据科学发行版。如果您之前没有使用过它，可以在[这里](https://www.anaconda.com/products/individual)下载。

![https://www.anaconda.com/distribution/](https://cdn-images-1.medium.com/max/2612/0\*78DWA4r55DRuMs2P.png)

**4 — PyCaret Python库**

要安装[PyCaret](http://pycaret.org)库，请在Jupyter笔记本或Anaconda提示符中使用以下代码。

```
pip install pycaret
```

这可能需要15分钟的时间。如果遇到任何问题，请参阅项目的[GitHub](https://www.github.com/pycaret/pycaret)页面以了解已知问题。

**5 — TabPy Python库**

TabPy是Tableau支持的库，需要运行Python脚本。

从[GitHub](https://github.com/tableau/TabPy)页面：

> TabPy（Tableau Python Server）是一个扩展Tableau功能的分析扩展实现，它允许用户通过Tableau的表计算执行Python脚本和保存的函数。

要安装TabPy，请在Anaconda提示符或终端中使用以下代码。

```
pip install tabpy
```

安装完成后，使用以下代码使用默认设置启动本地服务器。

```
tabpy
```

要将Tableau连接到TabPy服务器，请转到帮助 > 设置和性能 > 管理分析扩展连接。选择TabPy并输入localhost、端口9004（默认值）并测试连接。

![](https://cdn-images-1.medium.com/max/2000/1\*E9s7\_2uOGuraAcz7lRf6-A.png)

现在可以通过计算字段在Tableau中运行Python脚本，并将其输出为表计算。

有关自定义服务器选项，请参阅TabPy的[GitHub](https://github.com/tableau/TabPy)页面。本文不涵盖在外部服务器和/或云上运行TabPy以及配置Tableau Server的内容，但请在[这里](https://help.tableau.com/current/server/en-us/config\_r\_tabpy.htm)获取更多信息。

### 直销数据
将要使用的数据集包含通过短信和电子邮件发送给客户的各种营销提供的信息。数据集包含 64000 条记录，组织成一个 ID 列，10 个与客户或发送消息相关的特征，以及一个二进制目标，指示是否发生了访问。可以在[这里](https://github.com/andrewcowannagora/PyCaret-Tableau/blob/master/direct\_marketing.csv)下载数据。

![](https://cdn-images-1.medium.com/max/2000/1\*nixMpbPMN5\_aW0IGKOCuxw.png)

### **事先训练模型**

虽然可以在 Tableau 中执行模型训练过程，但这通常不是首选方法，因为每次数据刷新或用户与视图交互时，脚本都会重新运行。这是有问题的，因为：

* 当使用新数据重新训练模型时，预测可能会意外更改。
* 不断重新运行脚本会影响仪表板的性能。

更合适的方法是在 Tableau 中使用预训练模型对新数据生成预测。本示例将使用 Jupyter 笔记本演示如何使用 PyCaret 使此过程变得简单。

### 在 PyCaret 中构建模型

在 Jupyter Notebook 中运行以下代码将训练一个朴素贝叶斯分类模型，并创建一个保存为 pickle 文件的 ML 管道。

请注意，设置和保存模型只需 4 行代码。可以在[这里](https://github.com/andrewcowannagora/PyCaret-Tableau/blob/master/TabPy%20Direct%20Marketing.ipynb)下载完整的笔记本。

![](https://cdn-images-1.medium.com/max/2834/1\*EP9lfNBQ53CYV80nO7yAkA.png)

![包含训练模型和管道的 pickle 文件](https://cdn-images-1.medium.com/max/2000/1\*EoKanuGm98zMnLuURG7JHg.png)

未见数据将用于模拟尚未收到提供的新客户列表。当仪表板投入生产时，它将连接到包含新客户信息的数据库。

请注意，在设置阶段，PyCaret 执行自动预处理，本例中通过独热编码将特征数量从 10 扩展到 39。

这只是 PyCaret 内置功能的冰山一角，因此强烈建议查看 PyCaret 网站上的分类[模块](https://pycaret.org/classification/)和[教程](https://pycaret.org/clf101/)。此处不会涵盖所选模型的具体细节。

### 将模型加载到 Tableau Desktop

现在将未见数据传递给训练好的模型，并在 Tableau Desktop 中加以标记。

操作步骤：

1. 打开 Tableau 并连接到上述代码中创建的文本文件 new\_customer.csv。这只是一个示例，但理想情况下，新的或未标记的客户数据应存储在数据库中。

![](https://cdn-images-1.medium.com/max/3840/1\*YwmmK9bL3SFfnfemKF1agA.png)

1. 在新工作表中选择分析 > 创建计算字段，或者在数据窗格中右键单击。输入以下代码：

    SCRIPT\_INT(" import pandas as pd import pycaret.classification

    nb = pycaret.classification.load\_model('C:/Users/owner/Desktop/nb\_direct')

    X\_pred = pd.DataFrame({'recency':\_arg1, 'history\_segment':\_arg2, 'history':\_arg3, 'mens':\_arg4, 'womens':\_arg5,'zip\_code':\_arg6, 'newbie':\_arg7, 'channel':\_arg8, 'segment':\_arg9, 'DM\_category':\_arg10})

    pred = pycaret.classification.predict\_model(nb, X\_pred) return pred\['Label'.tolist()

    ", SUM(\[recency]), ATTR(\[history\_segment]), SUM(\[history]), SUM(\[mens]), SUM(\[womens]), ATTR(\[zip\_code]), SUM(\[newbie]), ATTR(\[channel]), ATTR(\[segment]), SUM(\[DM\_category]) )

![](https://cdn-images-1.medium.com/max/2386/1\*LmKKJVNSh8YS6aQZhWzkGg.png)

* 脚本函数指定计算返回的数据类型。在这种情况下，是访问的二进制预测标签。
* PyCaret 的 load\_model() 函数加载之前保存为 pickle 文件的模型和转换管道。
* X\_pred 是一个数据框，将通过 \_arg1、\_arg2、\_arg3… 表示法将连接到 Tableau 的数据作为输入映射。字段在脚本末尾列出。
* predict\_model() 使用训练好的模型对新数据输入进行预测。请注意，新数据通过 PyCaret 设置阶段创建的转换管道（编码）传递。
* 然后将标签作为列表返回，可在 Tableau 中查看。

1. 将 ID 和 Label 列拖动到视图中，可以查看模型预测。

![](https://cdn-images-1.medium.com/max/2498/1\*lPZGQtE89stX1SjWqSiyTQ.png)

重要的是要了解，输出是一个表计算，具有一些限制：

* 脚本仅在拉入视图时运行。
* 除非两者都在视图中，否则不能用作进一步计算的基础。
* Python 生成的数据无法附加到 Tableau 抽取中。
* 脚本在每次视图更改时运行，这可能导致长时间等待。

这些缺点相当显著，因为当每条记录必须包含在视图中时，仪表板选项变得有限，而在这种情况下脚本运行大约需要 4 分钟，记录数为 3200 条。

可行的应用包括生成得分列表，可以导出，或者像下面这样的摘要视图。

![](https://cdn-images-1.medium.com/max/2400/1\*DiaEihO8vjKM\_16\_zEYrvA.png)

从中可以得出的一个例子是，更高消费的客户最有可能造访，从商业角度来看是合理的，但也许可能是不必要折扣的一个指标。

### 将模型加载到 Tableau Prep 中

一个很好的替代方法，可以避开直接在 Tableau Desktop 中运行脚本的限制，就是使用 Tableau Prep。新数据可以连接，然后传递给模型，不同之处在于这次预测的标签会附加到输出中。当连接到 Tableau 时，新列可以像普通列一样使用，而不是作为表计算。

操作步骤：

1. 打开 Tableau Prep 并连接到上述代码中创建的文本文件 new\_customer.csv。

![](https://cdn-images-1.medium.com/max/3540/1\*TxMr\_QQR6jGACJzmSstITg.png)

1. 在流程窗格中文件旁边选择‘+’按钮，添加脚本选项。与在 Tableau Desktop 中一样，连接到应该仍在后台运行的 TabPy 服务器，使用 localhost 和 9004。

![](https://cdn-images-1.medium.com/max/2064/1\*KaiZM\_JXvB6qfjtyjglBIA.png)

1. 接下来，需要创建以下 Python 脚本，并使用浏览选项连接到 prep。可以在[这里](https://github.com/andrewcowannagora/PyCaret-Tableau/blob/master/direct\_marketing\_prep.py)下载。

![](https://cdn-images-1.medium.com/max/2000/1\*HRl6t04aiKBP0eqTGys7Sw.png)

创建一个函数，加载保存模型和转换流水线的 pickle 文件。加载到 prep 中的数据会自动保存在 df 对象中，并传递给模型。

PyCaret 输出将返回初始数据集和两个新附加列；Label（预测）和 Score（预测概率）。输出模式确保列和数据类型被正确读入 prep。

然后必须在 prep 中输入函数名称。

![](https://cdn-images-1.medium.com/max/2000/1\*Uh3neQ8weSZm9LJ\_Ki6Qug.png)

1. 选择脚本图标旁边的‘+’号，并选择输出。可以将其发布为 .tde 或 .hyper 文件到 Tableau Server，在生产环境中这将是首选方法，但在此示例中，将发送 .csv 文件到桌面。

![](https://cdn-images-1.medium.com/max/2384/1\*mVe4CorNxcBRU7q\_dPqHOQ.png)

注意标签和分数列现在附加到原始数据集中。选择‘运行流’以生成输出。可以在[这里](https://github.com/andrewcowannagora/PyCaret-Tableau/blob/master/DM\_Model\_Flow.tflx)下载流文件。

在服务器环境中，可以安排流程运行的时间，并在数据到达实际 Tableau 仪表板之前自动化评分过程。

### 将流程输出加载到 Tableau 中

现在，新标记的数据可以连接到 Tableau Desktop，而无需表计算限制和减速。

可以创建聚合和任何其他所需的计算，设计一个显示各种预测指标的摘要仪表板：

![](https://cdn-images-1.medium.com/max/2000/1\*AhgzQQMLOfwe\_5\_\_eMMTIQ.png)

一旦建立了数据和 ML 流水线，营销人员和高管将能够快速跟踪即将推出的活动可能的表现，几乎不需要干预。包含示例仪表板和之前脚本的 Tableau 文件可以在[这里](https://github.com/andrewcowannagora/PyCaret-Tableau/blob/master/DM\_Dashboard.twbx)下载。

### 结语

本文演示了如何将 PyCaret 与 Tableau Desktop 和 Tableau Prep 集成，快速将机器学习层添加到现有工作流程中。

通过使用组织熟悉的工具和 PyCaret 库，可以在几分钟内建立整个 ML 流水线，从而快速启动预测分析原型。

### 有用链接

[PyCaret](https://pycaret.org/)

[PyCaret：用户指南和文档](https://pycaret.org/guide/)

[PyCaret：教程](https://pycaret.org/tutorial/)

[PyCaret：Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g/featured)

[LinkedIn](http://www.linkedin.com/in/andrewcowannagora)