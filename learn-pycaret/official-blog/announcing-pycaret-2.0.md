# 宣布 PyCaret 2.0

### 宣布 PyCaret 2.0

#### 作者：Moez Ali

![https://www.pycaret.org](https://cdn-images-1.medium.com/max/2126/1\*oT-VYfpNDeKJ1L9vkpESdw.png)

我们很高兴地宣布今天发布了 PyCaret 的第二个版本。

PyCaret 是一个开源的、**低代码** Python 机器学习库，可以自动化机器学习工作流程。它是一个端到端的机器学习和模型管理工具，可以加快机器学习实验周期，提高工作效率。

与其他开源机器学习库相比，PyCaret 是一个替代的低代码库，可以用少量代码取代数百行代码。这使得实验速度指数级提升，效率更高。

查看 PyCaret 2.0 的详细 [发布说明](https://github.com/pycaret/pycaret/releases/tag/2.0)。

### **为什么使用 PyCaret?**

![PyCaret 2.0 特性](https://cdn-images-1.medium.com/max/2066/1\*wT0m1kx8WjY\_P7hrM6KDbA.png)

### 安装 PyCaret 2.0

安装 PyCaret 非常简单，只需几分钟即可完成。我们强烈建议使用虚拟环境，以避免与其他库可能发生的冲突。请参考以下示例代码，在 _**conda**_ 环境中创建并安装 pycaret：

```
**# 创建 conda 环境**
conda create --name yourenvname python=3.6  

**# 激活环境**
conda activate yourenvname  

**# 安装 pycaret**
pip install **pycaret==2.0  **

**# 创建与 conda 环境关联的 notebook 内核 python -m **ipykernel install --user --name yourenvname --display-name "display-name"
```

如果您使用 Azure notebooks 或 Google Colab，运行以下代码安装 PyCaret。

```
!pip install **pycaret==2.0**
```

使用 pip 安装 PyCaret 时，所有硬依赖项会自动安装。[点击这里](https://github.com/pycaret/pycaret/blob/master/requirements.txt) 查看完整的依赖项列表。

### 👉 开始使用 PyCaret 2.0

在 PyCaret 中进行任何机器学习实验的第一步是通过导入相关模块来设置环境，并通过传递数据框和目标变量的名称来初始化 \*\*setup 函数\*\*。参见示例代码：

**示例输出：**

![输出被截断](https://cdn-images-1.medium.com/max/2000/1\*di8zOe7rN7kWHO8t-6C6hg.png)

所有预处理转换都在 \*\*setup 函数\*\* 中应用。PyCaret 提供超过 20 种不同的预处理转换，可以在 setup 函数中定义。[点击这里](https://www.pycaret.org/preprocessing) 了解更多关于 PyCaret 预处理能力的信息。

![https://www.pycaret.org/preprocessing](https://cdn-images-1.medium.com/max/2000/1\*EcfstE4cOIhazduR4iUX0w.png)

### 👉 **比较模型**

这是我们在任何监督机器学习任务中推荐的第一步。此函数使用默认超参数训练模型库中的所有模型，并使用交叉验证评估性能指标。它返回训练好的模型对象类。使用的评估指标包括：

* \*\*对于分类：\*\*准确率、AUC、召回率、精确率、F1、Kappa、MCC
* \*\*对于回归：\*\*MAE、MSE、RMSE、R2、RMSLE、MAPE

以下是您可以使用 **compare\_models** 函数的几种方式：

**示例输出：**

![compare\_models 函数的示例输出](https://cdn-images-1.medium.com/max/2000/1\*gUjKx0cQbpaWl226CzAdyw.png)

### 👉 **创建模型**

Create Model 函数使用默认超参数训练模型，并使用交叉验证评估性能指标。此函数是 PyCaret 中几乎所有其他函数的基础。它返回训练好的模型对象类。以下是您可以使用此函数的几种方式：

**示例输出：**

![create\_model 函数的示例输出](https://cdn-images-1.medium.com/max/2000/1\*NDwHzljCyqQpkH55ogHzTA.png)

要了解更多关于 **create model** 函数的信息，[点击这里](https://www.pycaret.org/create-model)。

### 👉 调整模型

Tune Model 函数调整作为估计器传递的模型的超参数。它使用具有完全可定制化的预定义调整网格的随机网格搜索。以下是您可以使用此函数的几种方式：

要了解更多关于 **tune model** 函数的信息，[点击这里](https://www.pycaret.org/tune-model)。

### 👉 集成模型

有几个函数可用于集成基础学习器。**ensemble\_model**、\*\*blend\_models \*\*和 \*\*stack\_models \*\*是其中的三个。以下是您可以使用此函数的几种方式：

要了解更多关于 PyCaret 中集成模型的信息，[点击这里](https://www.pycaret.org/ensemble-model)。

### 👉 预测模型

顾名思义，此函数用于推断/预测。以下是如何使用它：

### 👉 绘制模型

Plot Model 函数用于评估训练好的机器学习模型的性能。以下是一个示例：

![plot\_model 函数的示例输出](https://cdn-images-1.medium.com/max/2000/1\*Do2ho2O\_fg8w62VPuwVm7g.png)
[点击这里](https://www.pycaret.org/plot-model) 了解 PyCaret 中不同可视化的更多信息。

或者，您可以使用 **evaluate_model** 函数通过笔记本中的用户界面查看图表。

![PyCaret 中的 evaluate_model 函数](https://cdn-images-1.medium.com/max/2560/1\*AGsJlbX6bhCOG2r\_uhvkKg.gif)

### 👉 实用函数

PyCaret 2.0 包含几个新的实用函数，可在使用 PyCaret 管理机器学习实验时派上用场。以下是其中一些示例：

要查看 PyCaret 2.0 中实施的所有新函数，请参阅 [发布说明](https://github.com/pycaret/pycaret/releases/tag/2.0)。

### 👉 实验记录

PyCaret 2.0 将 MLflow 跟踪组件嵌入为后端 API 和 UI，用于记录参数、代码版本、指标和输出文件，当运行机器学习代码时以及以后可视化结果。以下是如何在 PyCaret 中记录您的实验。

**输出（在 localhost:5000 上）**

![https://localhost:5000](https://cdn-images-1.medium.com/max/3788/1\*Z\_08utFByIK\_9nsps3rctA.png)

### 👉 将所有内容整合在一起 — 创建您自己的 AutoML 软件

使用所有函数，让我们创建一个简单的命令行软件，该软件将使用默认参数训练多个模型，调整前几个候选模型的超参数，尝试不同的集成技术，并返回/保存最佳模型。以下是命令行脚本：

此脚本将动态选择并保存最佳模型。只需几行代码，您就可以开发出具有完整记录系统甚至呈现美丽排行榜的自动机器学习软件。

使用 Python 中的轻量级工作流自动化库，您可以实现无限可能。如果您觉得这很有用，请不要忘记在我们的 GitHub 仓库上给我们 ⭐️。

要了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)。

### 重要链接

[PyCaret 2.0 发布说明](https://github.com/pycaret/pycaret/releases/tag/2.0) [用户指南 / 文档](https://www.pycaret.org/guide) [Github](http://www.github.com/pycaret/pycaret) [安装 PyCaret](https://www.pycaret.org/install) [笔记本教程](https://www.pycaret.org/tutorial) [为 PyCaret 做贡献](https://www.pycaret.org/contribute)

### 想了解特定模块吗？

点击下面的链接查看文档和工作示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)