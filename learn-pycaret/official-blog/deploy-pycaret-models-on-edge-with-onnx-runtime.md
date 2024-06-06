# 使用 ONNX Runtime 在边缘设备上部署 PyCaret 模型

### 使用 ONNX Runtime 在边缘设备上部署 PyCaret 模型的逐步教程

#### 一步一步教你如何将使用 PyCaret 训练的机器学习模型转换为 ONNX 格式，以实现高性能评分（CPU 或 GPU）

![Photo by Austin Distel on Unsplash](https://cdn-images-1.medium.com/max/12668/0\*X79bEMfw0xAW7nKT)

### 简介

在本教程中，我将向您展示如何使用 [PyCaret](https://www.pycaret.org/) 这个 Python 中的开源低代码机器学习库来训练机器学习模型，并将其转换为 ONNX 格式，以便在边缘设备或其他非 Python 环境上部署。例如，您可以在 Python 中使用 PyCaret 训练机器学习模型，并在 R、Java 或 C 中部署它们。本教程的学习目标包括：

👉 什么是 PyCaret 以及如何入门？

👉 不同类型的模型格式（pickle、onnx、pmml 等）

👉 ONNX 是什么（发音为 ONEX）以及其好处是什么？

👉 使用 PyCaret 训练机器学习模型并将其转换为 ONNX 以在边缘设备上部署。

### PyCaret

[PyCaret](https://www.pycaret.org/) 是一个开源的低代码机器学习库和端到端模型管理工具，用于自动化机器学习工作流程。PyCaret 以其易用性、简洁性和快速高效地构建和部署端到端机器学习流水线的能力而闻名。要了解更多关于 PyCaret 的信息，请访问他们的 [GitHub](https://www.github.com/pycaret/pycaret)。

**特点：**

![PyCaret — 一个开源的低代码机器学习库](https://cdn-images-1.medium.com/max/2084/1\*sESpLOGhMa2U1FsFdxxzIQ.png)

### skl2onnx

[skl2onnx](https://github.com/onnx/sklearn-onnx) 是一个将 scikit-learn 模型转换为 ONNX 的开源项目。一旦转换为 ONNX 格式，您可以使用 ONNX Runtime 等工具进行高性能评分。这个项目是由 Microsoft 的工程师和数据科学家于 2017 年发起的。要了解更多关于该项目的信息，请访问他们的 [GitHub](https://github.com/onnx/sklearn-onnx)。

### 安装

您需要为本教程安装以下库。安装只需几分钟时间。

```
# 安装 pycaret
pip install pycaret

# 安装 skl2onnx
pip install skl2onnx

# 安装 onnxruntime
pip install onnxruntime
```

### 不同的模型格式

在介绍 ONNX 和其优势之前，让我们看看当前可用于部署的不同模型格式。

### 👉**Pickle**

这是最常见的格式，也是许多 Python 库（包括 PyCaret）将模型对象保存到文件的默认方式。[Pickle](https://docs.python.org/3/library/pickle.html) 将 Python 对象转换为位流，并允许将其存储到磁盘并在以后重新加载。它提供了一个很好的格式来存储机器学习模型，前提是推理应用程序也是在 Python 中构建的。

### 👉PMML

预测模型标记语言（PMML）是另一种机器学习模型的格式，相对于 Pickle 来说较少常见。PMML 自 1997 年以来一直存在，并且有很多应用程序使用该格式。诸如 SAP 和 PEGA CRM 等应用程序能够利用某些版本的 PMML。有一些开源库可以将 scikit-learn 模型（PyCaret）转换为 PMML。PMML 格式的最大缺点是它不支持所有的机器学习模型。

### 👉ONNX

[ONNX](https://github.com/onnx)（Open Neural Network Exchange）是一种开放格式，支持在不同库和语言之间存储和移植机器学习模型。这意味着您可以使用任何语言中的任何框架训练机器学习模型，然后将其转换为 ONNX 格式，以在任何环境中生成推理（例如 Java、C、.Net、Android 等）。ONNX 的这种与语言无关的能力使其与其他格式相比非常强大（例如，您无法在 Python 以外的任何其他语言中使用保存为 Pickle 文件的模型）。

### ONNX 是什么？

[ONNX](https://onnx.ai/) 是一种用于表示深度学习和传统模型的开放格式。使用 ONNX，AI 开发人员可以更轻松地在先进工具之间移动模型，并选择最适合自己的组合。ONNX 由 Microsoft、Facebook 和 AWS 等合作伙伴社区开发和支持。

ONNX 得到广泛支持，并且可以在许多框架、工具和硬件中找到。在不同框架之间实现互操作性，并简化从研究到生产的路径，有助于增加 AI 社区的创新速度。ONNX 有助于解决与 AI 模型相关的硬件依赖性挑战，并使相同的 AI 模型能够部署到多个硬件加速目标上。

_**来源：Microsoft**_

![https://microsoft.github.io/ai-at-edge/docs/onnx/](https://cdn-images-1.medium.com/max/2000/0\*9WvPLwTrLDynzQGM.PNG)
有许多优秀的机器学习库，涵盖了各种语言，比如 PyTorch、TensorFlow、scikit-learn、PyCaret 等。其核心理念在于，你可以使用任何工具、语言或框架训练模型，然后使用另一种语言或应用程序进行部署，进行推理和预测。举个例子，假设你有一个使用 .Net 构建的 Web 应用、一个 Android 应用，甚至是一个边缘设备，你想要将机器学习模型的预测集成到这些下游系统中。你可以通过将模型转换为 ONNX 格式来实现这一点。_使用 Pickle 或 PMML 格式是无法做到这一点的。_

### **主要优势：**

#### 👉 互操作性

在你喜欢的框架中开发，无需担心下游推理的影响。ONNX 使你能够在你选择的推理引擎中使用你喜欢的框架。

#### 👉 硬件访问

ONNX 使得访问硬件优化变得更加容易。使用兼容 ONNX 的运行时和库，旨在最大程度地提高硬件性能。这意味着，即使是在关注延迟的情况下，你也可以在 GPU 上使用 ONNX 模型进行推理。

![兼容性与互操作性](https://cdn-images-1.medium.com/max/2000/0\*CNFZ8AKtAPwDYki3.png)

### 👉 让我们开始吧

### 数据集

在本教程中，我使用了 PyCaret 仓库中的一个回归数据集，名为 _**insurance**_。你可以从[这里](https://github.com/pycaret/pycaret/blob/master/datasets/insurance.csv)下载数据。

![样本数据集](https://cdn-images-1.medium.com/max/2000/0\*AlNXvwqZitdNOLUJ.png)

```
**# 加载数据集
**from pycaret.datasets import get_data
data = get_data('insurance')

**# 初始化设置 / 数据准备
**from pycaret.regression import *
s = setup(data, target = 'charges')
```

![设置函数的输出结果（为了显示目的而压缩）](https://cdn-images-1.medium.com/max/2000/1\*wRI5YKWljqvtzKHNnc4osQ.png)

### 👉 模型训练与选择

现在数据已经准备好进行建模，让我们通过使用 compare\_models 函数开始训练过程。它将训练模型库中的所有可用算法，并使用 k 折交叉验证评估多个性能指标。

```
**# 比较所有模型
**best = compare_models()
```

![compare\_models 的输出结果](https://cdn-images-1.medium.com/max/2000/1\*7aZp9Tt2oPIyw6xbdzlnLQ.png)

基于交叉验证指标，最佳模型是 \*\*\*Gradient Boosting Regressor。\*\*\*你可以使用 save\_model 函数将模型保存为 Pickle 文件。

```
**# 将模型保存到驱动器
**save_model(best, 'c:/users/models/insurance')
```

这将以 Pickle 格式保存模型。

### 👉 使用 Pickle 格式生成预测

你可以使用 load\_model 函数将保存的模型加载回 Python 环境，并使用 predict\_model 函数生成推理。

```
**# 加载模型
**from pycaret.regression import load_model
loaded_model = load_model('c:/users/models/insurance')

**# 生成预测 / 推理
**from pycaret.regression import predict_model
pred = predict_model(loaded_model, data=data) # 新数据
```

![在测试集上生成的预测](https://cdn-images-1.medium.com/max/2000/1\*vjO887TVlqS9H2utp2rY9A.png)

### 👉 ONNX 转换

到目前为止，我们看到了如何以 Pickle 格式保存和加载训练好的模型（这是 PyCaret 的默认格式）。然而，使用 skl2onnx 库，我们可以将模型转换为 ONNX：

```
**# 将最佳模型转换为 onnx
**from skl2onnx import to_onnx
X_sample = get_config('X_train')[:1]
model_onnx = to_onnx(best, X_sample.to_numpy())
```

我们也可以将 model\_onnx 保存到本地驱动器：

```
**# 将模型保存到驱动器
**with open("c:/users/models/insurance.onnx", "wb") as f:
    f.write(model_onnx.SerializeToString())
```

现在，为了从 insurance.onnx 生成推理，我们将在 Python 中使用 onnxruntime 库（仅为了演示）。基本上，你现在可以在任何其他平台或环境中使用这个 insurance.onnx。

```
**# 在 onnx 上生成推理
**from onnxruntime import InferenceSession
sess = InferenceSession(model_onnx.SerializeToString())
X_test = get_config('X_test').to_numpy()
predictions_onnx = sess.run(None, {'X': X_test})[0]

**# 打印 predictions_onnx
**print(predictions_onnx)
```

![predictions\_onnx](https://cdn-images-1.medium.com/max/2000/1\*GnEcu24SeB7y2--rYT0qyA.png)

请注意，predictions\_onnx 的输出是一个 numpy 数组，与我们使用 PyCaret 的 predict\_model 函数时得到的 pandas DataFrame 不同，但如果你比对数值，这些数字都是相同的（_使用 ONNX 时，有时会在第四位小数点后发现微小差异 — 非常罕见_）。

> **任务完成！**

### 即将推出！
下周我将深入探讨ONNX转换，并讨论如何将整个机器学习流程（包括输入器和转换器）转换为ONNX。如果您希望自动收到通知，可以关注我的[Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/)和[Twitter](https://twitter.com/moezpycaretorg1)。

![PyCaret — 作者提供的图片](https://cdn-images-1.medium.com/max/2412/0\*PLdJpNCTXdttEn8W.png)

![PyCaret — 作者提供的图片](https://cdn-images-1.medium.com/max/2402/0\*IvqhUYDstXqz55eF.png)

使用这个轻量级的Python工作流自动化库，您可以实现无限可能。如果您觉得这个工具有用，请不要忘记在我们的GitHub仓库上给我们 ⭐️。

想了解更多关于PyCaret的信息，请关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)和[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)。

加入我们的Slack频道。邀请链接在[这里](https://join.slack.com/t/pycaret/shared\_invite/zt-p7aaexnl-EqdTfZ9U\~mF0CwNcltffHg)。

### 重要链接

[文档](https://pycaret.readthedocs.io/en/latest/installation.html) [博客](https://medium.com/@moez\_62905) [GitHub](http://www.github.com/pycaret/pycaret) [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html) [笔记本教程](https://pycaret.readthedocs.io/en/latest/tutorials.html) [为PyCaret做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 更多与PyCaret相关的教程：

[**在Alteryx中使用PyCaret进行机器学习** _一篇关于在Alteryx Designer中使用PyCaret训练和部署机器学习模型的逐步教程_towardsdatascience.com](https://towardsdatascience.com/machine-learning-in-alteryx-with-pycaret-fafd52e2d4a) [**在KNIME中使用PyCaret进行机器学习** _一篇关于在KNIME中使用PyCaret训练和部署端到端机器学习流程的逐步指南_towardsdatascience.com](https://towardsdatascience.com/machine-learning-in-knime-with-pycaret-420346e133e2) [**使用PyCaret + MLflow进行简单的MLOps** _一篇适合初学者的、关于如何使用PyCaret在您的机器学习实验中集成MLOps的逐步教程_towardsdatascience.com](https://towardsdatascience.com/easy-mlops-with-pycaret-mlflow-7fbcbf1e38c6) [**编写和训练自定义机器学习模型使用PyCaret** towardsdatascience.com](https://towardsdatascience.com/write-and-train-your-own-custom-machine-learning-models-using-pycaret-8fa76237374e) [**使用PyCaret构建，使用FastAPI部署** _一篇逐步、适合初学者的教程，介绍如何使用PyCaret构建端到端机器学习流程并部署_towardsdatascience.com](https://towardsdatascience.com/build-with-pycaret-deploy-with-fastapi-333c710dc786) [**使用PyCaret进行时间序列异常检测** _一篇关于使用PyCaret对时间序列数据进行无监督异常检测的逐步教程_towardsdatascience.com](https://towardsdatascience.com/time-series-anomaly-detection-with-pycaret-706a6e2b2427) [**通过PyCaret和Gradio加速您的机器学习实验** _一篇逐步教程，快速开发和交互式地与机器学习流程交互_towardsdatascience.com](https://towardsdatascience.com/supercharge-your-machine-learning-experiments-with-pycaret-and-gradio-5932c61f80d9) [**使用PyCaret进行多时间序列预测** _一篇关于使用PyCaret预测多时间序列的逐步教程_towardsdatascience.com](https://towardsdatascience.com/multiple-time-series-forecasting-with-pycaret-bc0a779a22fe)