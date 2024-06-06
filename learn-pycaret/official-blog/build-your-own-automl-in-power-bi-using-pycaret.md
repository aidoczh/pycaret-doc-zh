# 在 Power BI 中使用 PyCaret 构建您自己的 AutoML

### 在 Power BI 中使用 PyCaret 2.0 构建您自己的 AutoML

#### 作者：Moez Ali

![PyCaret — 一个开源的、低代码的 Python 机器学习库](https://cdn-images-1.medium.com/max/2664/1\*Kx9YUt0hWPhU\_a6h2vM5qA.png)

### **PyCaret 2.0**

上周我们宣布了 [PyCaret 2.0](https://towardsdatascience.com/announcing-pycaret-2-0-39c11014540e)，这是一个在 Python 中自动化机器学习工作流程的开源、**低代码**机器学习库。它是一个端到端的机器学习和模型管理工具，可以加快机器学习实验周期，帮助数据科学家提高效率和生产力。

在本文中，我们将介绍一个关于如何使用 PyCaret 构建一个自动化机器学习解决方案在 [Power BI](https://powerbi.microsoft.com/en-us/) 中的**逐步教程**，从而使数据科学家和分析师能够在他们的仪表板中添加一层机器学习，而无需额外的许可证或软件成本。PyCaret 是一个开源且**免费使用**的 Python 库，具有广泛的功能，专为在 Power BI 中使用而设计。

通过本文，您将学会如何在 Power BI 中实现以下内容：

- 设置 Python conda 环境并安装 pycaret==2.0。
- 将新创建的 conda 环境与 Power BI 连接起来。
- 在 Power BI 中构建您的第一个 AutoML 解决方案，并在仪表板上展示性能指标。
- 在 Power BI 中将您的 AutoML 解决方案投入生产/部署。

### Microsoft Power BI

Power BI 是一款业务分析解决方案，可让您可视化数据并在组织中共享见解，或将它们嵌入到您的应用程序或网站中。在本教程中，我们将通过将 PyCaret 库导入 Power BI，使用 [Power BI Desktop](https://powerbi.microsoft.com/en-us/downloads/) 进行机器学习。

### 什么是自动化机器学习？

自动化机器学习（AutoML）是自动化机器学习中耗时且迭代的任务的过程。它允许数据科学家和分析师以高效的方式构建机器学习模型，同时保持模型质量。任何 AutoML 解决方案的最终目标是根据一些性能标准确定最佳模型。

传统的机器学习模型开发过程资源密集，需要大量领域知识和时间来生成和比较数十个模型。通过自动化机器学习，您将加快准备生产就绪的 ML 模型所需的时间，而且更加轻松高效。

### **PyCaret 是如何工作的？**

PyCaret 是一个用于监督和无监督机器学习的工作流自动化工具。它分为六个模块，每个模块都有一组可用于执行某些特定操作的函数。每个函数都接受一个输入并返回一个输出，在大多数情况下是一个经过训练的机器学习模型。截至第二版发布，可用的模块有：

- [分类](https://www.pycaret.org/classification)
- [回归](https://www.pycaret.org/regression)
- [聚类](https://www.pycaret.org/clustering)
- [异常检测](https://www.pycaret.org/anomaly-detection)
- [自然语言处理](https://www.pycaret.org/nlp)
- [关联规则挖掘](https://www.pycaret.org/association-rules)

PyCaret 中的所有模块都支持数据准备（超过 25 种基本预处理技术，提供了大量未经训练的模型集合和对自定义模型的支持，自动超参数调整，模型分析和可解释性，自动模型选择，实验日志记录以及简单的云部署选项。

![https://www.pycaret.org/guide](https://cdn-images-1.medium.com/max/2066/1\*wT0m1kx8WjY\_P7hrM6KDbA.png)

要了解更多关于 PyCaret 的信息，[点击这里](https://towardsdatascience.com/announcing-pycaret-2-0-39c11014540e) 阅读我们的官方发布公告。

如果您想在 Python 中开始使用，请[点击这里](https://github.com/pycaret/pycaret/tree/master/examples) 查看示例笔记本库以开始。

> ## “PyCaret 通过为业务分析师、领域专家、公民数据科学家和经验丰富的数据科学家提供免费、开源和低代码的机器学习解决方案，实现了机器学习和高级分析的民主化”。

### 开始之前

如果您是第一次使用 Python，安装 Anaconda Distribution 是最简单的入门方式。[点击这里](https://www.anaconda.com/distribution/) 下载带有 Python 3.7 或更高版本的 Anaconda Distribution。

![https://www.anaconda.com/products/individual](https://cdn-images-1.medium.com/max/2612/1\*sMceDxpwFVHDtdFi528jEg.png)

#### 设置环境

在我们开始在 Power BI 中使用 PyCaret 的机器学习功能之前，我们需要创建一个虚拟环境并安装 pycaret。这是一个三步过程：

[✅](https://fsymbols.com/signs/tick/) **步骤 1 — 创建 anaconda 环境**
打开 **Anaconda Prompt**，从开始菜单中执行以下代码：

```
conda create --name **myenv** python=3.7
```

![Anaconda Prompt — 创建环境](https://cdn-images-1.medium.com/max/2194/1\*2D9jKJPM4eAy1-7lvcLlJQ.png)

[✅](https://fsymbols.com/signs/tick/) **第二步 — 安装 PyCaret**

在 Anaconda Prompt 中执行以下代码：

```
pip install **pycaret==2.0**
```

安装可能需要 15–20 分钟。如果安装出现问题，请查看我们的 [GitHub](https://www.github.com/pycaret/pycaret) 页面以获取已知问题和解决方案。

[✅](https://fsymbols.com/signs/tick/)**第三步 — 在 Power BI 中设置 Python 目录**

创建的虚拟环境必须与 Power BI 相关联。这可以通过 Power BI Desktop 中的全局设置完成（文件 → 选项 → 全局 → Python 脚本）。Anaconda 环境默认安装在：

C:\Users\***username**\*\AppData\Local\Continuum\anaconda3\envs\myenv

![文件 → 选项 → 全局 → Python 脚本](https://cdn-images-1.medium.com/max/2000/1\*zQMKuyEk8LGrOPE-NByjrg.png)

### **👉 让我们开始吧**

### 设定业务背景

一家保险公司希望通过在患者住院时利用人口统计信息和基本患者健康风险指标更好地预测患者费用，从而改进现金流预测。

![](https://cdn-images-1.medium.com/max/2000/1\*qM1HiWZ\_uigwdcZ0\_ZL6yA.png)

_(_[_数据来源_](https://www.kaggle.com/mirichoi0218/insurance#insurance.csv)_)_

### 目标

训练并选择最佳的回归模型，根据数据集中的其他变量（年龄、性别、BMI、子女数、吸烟者和地区）预测患者费用。

### 👉 步骤 1 — 加载数据集

您可以通过转到 Power BI Desktop → 获取数据 → Web 直接从我们的 GitHub 加载数据集

数据集链接：[https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/insurance.csv](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/insurance.csv)

![Power BI Desktop → 获取数据 → Web](https://cdn-images-1.medium.com/max/2000/1\*zZjZzF\_TJudoThDCBGK3fQ.png)

在 Power Query 中创建一个数据集副本：

![Power Query → 创建数据集副本](https://cdn-images-1.medium.com/max/3436/1\*mU8tl4P89WKMC\_\_k6rM-Vw.png)

### 👉 步骤 2 — 作为 Python 脚本运行 AutoML

在 Power Query 中运行以下代码（转换 → 运行 Python 脚本）：

```
**# 导入回归模块**
from pycaret.regression import *

**# 初始化设置**
reg1 = setup(data=dataset, target = 'charges', silent = True, html = False)

**# 比较模型**
best_model = compare_models()

**# 完成最佳模型
**best = finalize_model(best_model)

**# 保存最佳模型**
save_model(best, 'c:/users/moezs/best-model-power')

**# 返回性能指标 df
**dataset = pull()
```

![Power Query 中的脚本](https://cdn-images-1.medium.com/max/2000/1\*FOxy83SH1uy8pFLJT6sa3w.png)

代码的前两行是用于导入相关模块和初始化设置函数。设置函数执行机器学习中需要的几个必要步骤，如清理缺失值（如果有的话）、将数据拆分为训练集和测试集、设置交叉验证策略、定义度量标准、执行特定于算法的转换等。

训练多个模型、比较和评估性能指标的神奇函数是 **compare\_models**。它基于 'sort' 参数返回最佳模型，该参数可以在 **compare\_models** 中定义。默认情况下，它在回归用例中使用 'R2'，在分类用例中使用 '准确性'。

其余行是用于完成通过 **compare\_models** 返回的最佳模型并将其保存为 pickle 文件在您的本地目录中。最后一行返回训练模型及其性能指标的详细数据框。

输出：

![Python 脚本的输出](https://cdn-images-1.medium.com/max/3822/1\*6CSYQDLfQUZeTtYwNllFSw.png)

仅需几行代码，我们就训练了超过 20 个模型，表格显示了基于 10 折交叉验证的性能指标。

表现最佳的模型 **Gradient Boosting Regressor** 将与整个转换管道一起保存为 pickle 文件在您的本地目录中。稍后可以使用此文件在新数据集上生成预测（请参阅下面的第 3 步）。

![转换管道和模型保存为 pickle 文件](https://cdn-images-1.medium.com/max/2000/1\*euQRJQVAVvP2X5ASNWjjOg.png)

PyCaret 的工作原理是模块化自动化。因此，如果您有更多资源和时间进行训练，可以扩展脚本以执行超参数调整、集成和其他可用的建模技术。请参见下面的示例：
```
**# 导入回归模块**
from pycaret.regression import *

**# 初始化设置**
reg1 = setup(data=dataset, target='charges', silent=True, html=False)

**# 比较模型**
top5 = compare_models(n_select=5)
results = pull()

**# 调整前五个模型**
tuned_top5 = [tune_model(i) for i in top5]

**# 选择最佳模型**
best = automl()

**# 保存最佳模型**
save_model(best, 'c:/users/moezs/best-model-power')

**# 返回性能指标数据框**
dataset = results
```
我们现在返回了前5个模型，而不是最佳性能的单个模型。然后，我们使用列表推导式（循环）来调整候选模型的超参数，最后\*\*automl函数\*\*选择了性能最佳的单个模型，并将其保存为pickle文件（请注意，这次我们没有使用\*\*finalize\_model\*\*，因为automl函数返回了最终的模型）。

### **示例仪表盘**

创建了示例仪表盘。PBIX文件在[这里上传](https://github.com/pycaret/pycaret-powerbi-automl)。

![使用PyCaret AutoML结果创建的仪表盘](https://cdn-images-1.medium.com/max/2664/1\*Kx9YUt0hWPhU\_a6h2vM5qA.png)

### 👉 第三步 - 部署模型以生成预测

一旦我们将最终模型保存为pickle文件，我们就可以使用它来预测新数据集上的费用。

### **加载新数据集**

为了演示目的，我们将再次加载相同的数据集，并从数据集中删除'charges'列。在Power Query中将以下代码作为Python脚本执行以获取预测结果：

```
**# 从回归模块加载函数**
from pycaret.regression import load_model, predict_model

**# 将模型加载到变量中**
model = load_model('c:/users/moezs/best-model-powerbi')

**# 预测费用**
dataset = predict_model(model, data=dataset)
```

输出：

![在Power Query中的predict\_model函数输出](https://cdn-images-1.medium.com/max/3840/1\*ZYWjwtu4njS7f7XMp90ofg.png)

### **在Power BI Service上部署**

当您将带有Python脚本的Power BI报告发布到服务时，这些脚本也将在通过本地数据网关刷新数据时执行。

为了实现这一点，您必须确保在托管个人网关的计算机上安装了具有相关Python包的Python运行时。请注意，不支持在多个用户共享的本地数据网关上执行Python脚本。[点击这里](https://powerbi.microsoft.com/en-us/blog/python-visualizations-in-power-bi-service/)阅读更多信息。

本教程中使用的PBIX文件已上传到GitHub存储库：[https://github.com/pycaret/pycaret-powerbi-automl](https://github.com/pycaret/pycaret-powerbi-automl)

如果您想了解更多关于PyCaret 2.0的信息，请阅读这个[公告](https://towardsdatascience.com/announcing-pycaret-2-0-39c11014540e)。

如果您之前使用过PyCaret，您可能对当前版本的[发布说明](https://github.com/pycaret/pycaret/releases/tag/2.0)感兴趣。

使用这个轻量级的Python工作流自动化库，您可以实现无限的可能。如果您觉得有用，请不要忘记在我们的GitHub仓库上给我们一个⭐️。

要了解更多关于PyCaret的信息，请关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)和[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)。

### **您可能还对以下内容感兴趣：**

[在Power BI中使用PyCaret进行机器学习](https://towardsdatascience.com/machine-learning-in-power-bi-using-pycaret-34307f09394a) [使用PyCaret在Power BI中构建您的第一个异常检测器](https://towardsdatascience.com/build-your-first-anomaly-detector-in-power-bi-using-pycaret-2b41b363244e) [如何在Power BI中使用PyCaret实现聚类](https://towardsdatascience.com/how-to-implement-clustering-in-power-bi-using-pycaret-4b5e34b1405b) [在Power BI中使用PyCaret进行主题建模](https://towardsdatascience.com/topic-modeling-in-power-bi-using-pycaret-54422b4e36d6)

### 重要链接

[博客](https://medium.com/@moez\_62905) [PyCaret 2.0发布说明](https://github.com/pycaret/pycaret/releases/tag/2.0) [用户指南/文档](https://www.pycaret.org/guide)[ ](https://github.com/pycaret/pycaret/releases/tag/2.0)[Github](http://www.github.com/pycaret/pycaret) [Stackoverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装PyCaret](https://www.pycaret.org/install) [Notebook教程](https://www.pycaret.org/tutorial) [为PyCaret做贡献](https://www.pycaret.org/contribute)

### 想了解特定模块的信息吗？

点击下面的链接查看文档和工作示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression)[聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection)[自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)