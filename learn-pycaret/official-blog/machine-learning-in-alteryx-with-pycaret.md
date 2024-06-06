# 在 Alteryx 中使用 PyCaret 进行机器学习

### 在 Alteryx 中使用 PyCaret 进行机器学习

#### 通过 PyCaret 在 Alteryx Designer 中训练和部署机器学习模型的逐步教程

![](https://cdn-images-1.medium.com/max/2000/1\*T6OjmWCOMcsm8wi0xQcjeQ.jpeg)

### 介绍

在本教程中，我将向您展示如何使用 PyCaret（一种Python中的开源、低代码机器学习库）在非常流行的ETL工具 [Alteryx](https://www.alteryx.com) 中训练和部署机器学习流程。本教程的学习目标包括：

👉 什么是 PyCaret 以及如何入门？

👉 什么是 Alteryx Designer 以及如何设置？

👉 在 Alteryx Designer 中训练端到端的机器学习流程，包括数据准备（如缺失值填充、独热编码、缩放、转换等）。

👉 部署训练好的流程，并在ETL过程中生成推断。

### PyCaret

[PyCaret](https://www.pycaret.org/) 是一个内置于 Python 中的开源、低代码机器学习库和端到端模型管理工具，用于自动化机器学习工作流程。PyCaret 以易用性、简单性以及快速高效地构建和部署端到端机器学习流程而闻名。要了解更多关于 PyCaret 的信息，请访问他们的 [GitHub](https://www.github.com/pycaret/pycaret)。

### Alteryx Designer

[Alteryx Designer](https://www.alteryx.com/products/alteryx-platform/alteryx-designer) 是由 [\*\*Alteryx](https://www.alteryx.com)\*\* 开发的专有工具，用于自动化分析的每一个步骤，包括数据准备、混合、报告、预测分析和数据科学。您可以访问任何数据源、文件、应用程序或数据类型，并体验到一个拥有260多个拖放构建块的自助服务平台的简单性和强大性。您可以从[这里](https://www.alteryx.com/designer-trial/alteryx-free-trial)下载 Alteryx Designer 的一个月免费试用版本。

![https://www.alteryx.com](https://cdn-images-1.medium.com/max/3648/1\*OeDHEH-vFx2u3nF69Wu3DQ.png)

### 教程前提条件：

在本教程中，您需要两样东西。第一样是 Alteryx Designer，这是一个桌面软件，您可以从[这里](https://www.alteryx.com/designer-trial/alteryx-free-trial)下载。第二样是 Python。获取 Python 最简单的方法是下载 Anaconda Distribution。要下载，请[点击这里](https://www.anaconda.com/distribution/)。

### 👉我们现在准备好了

打开 Alteryx Designer，然后点击 文件 → 新建工作流

![Alteryx Designer 中的新建工作流](https://cdn-images-1.medium.com/max/3818/1\*O7on438FoX76Ou9vjFDGpw.png)

在顶部，有一些工具，您可以将其拖放到画布上，并通过连接每个组件来执行工作流。

### 数据集

在本教程中，我使用了 PyCaret 仓库中名为 _**insurance**_ 的回归数据集。您可以从[这里](https://github.com/pycaret/pycaret/blob/master/datasets/insurance.csv)下载数据。

![示例数据集](https://cdn-images-1.medium.com/max/2000/0\*\_5ZOcQ4IBD55ADn6.png)

我将创建两个独立的 Alteryx 工作流。第一个用于**模型训练和选择**，第二个用于使用训练好的流程对**新数据进行评分**。

### 👉 模型训练和选择

让我们首先从\*\*输入数据工具\*\*中读取 CSV 文件，然后是一个\*\*Python 脚本\*\*。在 Python 脚本中执行以下代码：

```
**# 安装 pycaret
**from ayx import Package
Package.installPackages('pycaret')

**# 从输入数据工具中读取数据**
from ayx import Alteryx
data = Alteryx.read("#1")

**# 初始化设置，准备数据**
from pycaret.regression import *
s = setup(data, target = 'charges', silent=True)

**# 模型训练和选择
**best = compare_models()

**# 存储结果，打印并保存**
results = pull()
results.to_csv('c:/users/moezs/pycaret-demo-alteryx/results.csv', index = False)
Alteryx.write(results, 1)

**# 完成最佳模型并保存**
best_final = finalize_model(best)
save_model(best_final, 'c:/users/moezs/pycaret-demo-alteryx/pipeline')
```

该脚本从 pycaret 中导入回归模块，然后初始化设置函数，该函数自动处理 train_test_split 和所有数据准备任务，如缺失值填充、缩放、特征工程等。compare_models 使用 kfold 交叉验证训练和评估所有估算器，并返回最佳模型。

pull 函数调用模型性能指标作为一个数据框，然后将其保存为 results.csv 文件，并写入 Alteryx 中 Python 工具的第一个锚点（这样您就可以在屏幕上查看结果）。

最后，save_model 将整个转换流程（包括最佳模型）保存为 pickle 文件。

![训练工作流程](https://cdn-images-1.medium.com/max/3836/1\*2qny4Iy7SNePSpT7fZWSuw.png)
当您成功执行此工作流时，将生成pipeline.pkl和results.csv文件。您还可以在屏幕上查看最佳模型的输出以及它们的交叉验证指标。

![](https://cdn-images-1.medium.com/max/2000/1\*Vc6Pr88a6cxVfxxUGSp9yg.png)

results.csv文件包含以下内容：

![](https://cdn-images-1.medium.com/max/2000/0\*u9dRI79LDdDOrvw5.png)

这些是所有模型的交叉验证指标。在这种情况下，最佳模型是_**Gradient Boosting Regressor**_。

### 👉 模型评分

现在我们可以使用pipeline.pkl对新数据集进行评分。由于我没有一个单独的没有标签的insurance.csv数据集，我将删除目标列即_charges_，然后使用训练好的pipeline生成预测结果。

![评分工作流程](https://cdn-images-1.medium.com/max/3830/1\*ZVEhi6EdcXg\_dKINWisR0g.png)

我使用了**Select Tool**来删除目标列_charges_。在Python脚本中执行以下代码：

```
**# 从输入工具读取数据**
from ayx import Alteryx**
**data = Alteryx.read("#1")

**# 加载pipeline
**from pycaret.regression import load_model, predict_model
pipeline = load_model('c:/users/moezs/pycaret-demo-alteryx/pipeline')

**# 生成预测结果并保存为csv
**predictions = predict_model(pipeline, data)
predictions.to_csv('c:/users/moezs/pycaret-demo-alteryx/predictions.csv', index=False)

**# 在Alteryx中显示
**Alteryx.write(predictions, 1)
```

当您成功执行此工作流时，将生成predictions.csv文件。

![predictions.csv](https://cdn-images-1.medium.com/max/2000/0\*v6pthOCcVwNMww9S.png)

### 即将推出！

下周我将深入探讨PyCaret的更高级功能，您可以在Alteryx中使用这些功能来增强您的机器学习工作流程。如果您想自动收到通知，可以关注我的[Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/)和[Twitter](https://twitter.com/moezpycaretorg1)。

![PyCaret — 图片来自作者](https://cdn-images-1.medium.com/max/2412/0\*PLdJpNCTXdttEn8W.png)

![PyCaret — 图片来自作者](https://cdn-images-1.medium.com/max/2402/0\*IvqhUYDstXqz55eF.png)

使用这个轻量级的Python工作流自动化库，您可以实现无限的可能。如果您觉得有用，请不要忘记在我们的GitHub存储库上给我们一个⭐️。

要了解更多关于PyCaret的信息，请在[LinkedIn](https://www.linkedin.com/company/pycaret/)和[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)上关注我们。

加入我们的slack频道。邀请链接[在这里](https://join.slack.com/t/pycaret/shared\_invite/zt-p7aaexnl-EqdTfZ9U\~mF0CwNcltffHg)。

### 重要链接

[文档](https://pycaret.readthedocs.io/en/latest/installation.html) [博客](https://medium.com/@moez\_62905) [GitHub](http://www.github.com/pycaret/pycaret) [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html) [Notebook教程](https://pycaret.readthedocs.io/en/latest/tutorials.html) [为PyCaret做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 更多与PyCaret相关的教程：
[**在 KNIME 中使用 PyCaret 进行机器学习** _一步一步指南，使用 PyCaret 在 KNIME 中训练和部署端到端的机器学习流程_towardsdatascience.com](https://towardsdatascience.com/machine-learning-in-knime-with-pycaret-420346e133e2) [**使用 PyCaret + MLflow 实现简单的 MLOps** _一个适合初学者的、一步一步的教程，教你如何在机器学习实验中集成 MLOps_towardsdatascience.com](https://towardsdatascience.com/easy-mlops-with-pycaret-mlflow-7fbcbf1e38c6) [**使用 PyCaret 编写和训练自定义机器学习模型** towardsdatascience.com](https://towardsdatascience.com/write-and-train-your-own-custom-machine-learning-models-using-pycaret-8fa76237374e) [**使用 PyCaret 构建，使用 FastAPI 部署** _一个适合初学者的、一步一步的教程，教你如何使用 PyCaret 和 FastAPI 构建端到端的机器学习流程_towardsdatascience.com](https://towardsdatascience.com/build-with-pycaret-deploy-with-fastapi-333c710dc786) [**使用 PyCaret 进行时间序列异常检测** _一个逐步教程，使用 PyCaret 对时间序列数据进行无监督异常检测_towardsdatascience.com](https://towardsdatascience.com/time-series-anomaly-detection-with-pycaret-706a6e2b2427) [**使用 PyCaret 和 Gradio 加速机器学习实验** _一个逐步教程，快速开发和交互式地与机器学习流程进行交互_towardsdatascience.com](https://towardsdatascience.com/supercharge-your-machine-learning-experiments-with-pycaret-and-gradio-5932c61f80d9) [**使用 PyCaret 进行多时间序列预测** _一个逐步教程，使用 PyCaret 对多个时间序列进行预测_towardsdatascience.com](https://towardsdatascience.com/multiple-time-series-forecasting-with-pycaret-bc0a779a22fe)