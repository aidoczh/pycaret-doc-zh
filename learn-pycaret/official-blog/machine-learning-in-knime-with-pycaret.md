# 在 KNIME 中使用 PyCaret 进行机器学习

### 在 KNIME 中使用 PyCaret 进行机器学习

#### 一步一步指南，使用 PyCaret 在 KNIME 中训练和评分机器学习模型

![PyCaret 是一个开源的 Python 库，而 KNIME 是一个开源的数据分析平台](https://cdn-images-1.medium.com/max/2000/1\*GCzo1\_0f0E9HyK9jm7B2-w.png)

### PyCaret

[PyCaret](https://www.pycaret.org) 是一个开源的、低代码的机器学习库和端到端模型管理工具，使用 Python 构建，用于自动化机器学习工作流程。其易用性、简单性以及快速高效地构建和部署端到端机器学习流水线的能力将使您惊叹不已。

PyCaret 是一个替代低代码库，可以用少量代码取代数百行代码。这使得实验周期指数级加快和更高效。

PyCaret **简单易用**。在 PyCaret 中执行的所有操作都按顺序存储在一个**Pipeline**中，该管道完全自动化用于**部署**。无论是填补缺失值、独热编码、转换分类数据、特征工程，甚至是超参数调整，PyCaret 都可以自动化完成。要了解更多关于 PyCaret 的信息，请观看这个 1 分钟的视频。

### KNIME

[KNIME Analytics Platform](https://www.knime.com/knime-analytics-platform) 是用于创建数据科学的开源软件。直观、开放，并不断整合新的发展，KNIME 使理解数据、设计数据科学工作流程和可重复使用组件对每个人都变得可访问。

KNIME Analytics 平台是数据科学中最流行的开源平台之一，用于自动化数据科学流程。KNIME 在节点存储库中有成千上万的节点，允许您将节点拖放到 KNIME 工作台中。一组相关节点创建一个工作流程，可以在本地执行，也可以在将工作流程部署到 KNIME 服务器后，在 KNIME Web 门户中执行。

![KNIME Analytics Platform — 创建数据科学](https://cdn-images-1.medium.com/max/2000/0\*ct-Ux9jTTyDYyYHZ)

### 安装

对于本教程，您需要两样东西。第一样是 KNIME Analytics Platform，这是一个桌面软件，您可以从[这里](https://www.knime.com/downloads)下载。第二样是 Python。

开始使用 Python 的最简单方法是下载 Anaconda Distribution。要下载，请[点击这里](https://www.anaconda.com/distribution/)。

安装了 KNIME Analytics Platform 和 Python 后，您需要创建一个单独的 conda 环境，在其中我们将安装 PyCaret。打开 Anaconda 提示符并运行以下命令：

```
# 创建一个 conda 环境
conda create --name knimeenv python=3.6

# 激活环境
conda activate knimeenv

# 安装 pycaret
pip install pycaret
```

现在打开 KNIME Analytics Platform，转到 文件 → 安装 KNIME 扩展 → KNIME & Extensions → 选择 KNIME Python Extension 并安装它。

安装完成后，转到 文件 → 首选项 → KNIME → Python 并选择您的 Python 3 环境。请注意，在我的情况下，环境的名称是“powerbi”。如果您遵循上述命令，环境的名称将是“knimeenv”。

![在 KNIME Analytics Platform 中设置 Python](https://cdn-images-1.medium.com/max/2000/1\*KmNfJY16OzVldEbgfXh8kQ.png)

### 👉我们现在准备好了

点击“新 KNIME 工作流”，将打开一个空白画布。

![KNIME 新工作流](https://cdn-images-1.medium.com/max/3830/1\*TdQQ1wfEMH487wd3zz9OJg.png)

在左侧，有一些工具，您可以将其拖放到画布上，并通过连接每个组件来执行工作流。左侧存储库中的所有操作都称为_节点_。

### **数据集**

对于本教程，我使用了 PyCaret 存储库中的回归数据集称为 'insurance'。您可以从[这里](https://github.com/pycaret/pycaret/blob/master/datasets/insurance.csv)下载数据。

![示例数据集](https://cdn-images-1.medium.com/max/2000/1\*mpP1hqC9HQ37WGQmdZAoFQ.png)

我将创建两个单独的工作流。第一个用于模型训练和选择，第二个用于使用训练好的流水线对新数据进行评分。

### 👉 **模型训练和选择**

首先从**CSV Reader**节点读取 CSV 文件，然后是一个**Python Script**。在 Python 脚本中执行以下代码：

```
# 初始化设置，准备数据
from pycaret.regression import *
s = setup(input_table_1, target = 'charges', silent=True)

# 模型训练和选择
best = compare_models()

# 存储结果，打印并保存
output_table_1 = pull()
output_table_1.to_csv('c:/users/moezs/pycaret-demo-knime/results.csv', index = False)

# 完成最佳模型并保存
best_final = finalize_model(best)
save_model(best_final, 'c:/users/moezs/pycaret-demo-knime/pipeline')
```
这个脚本正在从 pycaret 中导入回归模块，然后初始化 setup 函数，该函数会自动处理 train_test_split 以及所有数据准备任务，如缺失值填充、缩放、特征工程等。compare_models 会使用 kfold 交叉验证训练和评估所有的估计器，并返回最佳模型。pull 函数调用模型性能指标作为一个 Dataframe，然后将其保存为 results.csv 文件到本地驱动器。最后，save_model 会将整个转换流水线和模型保存为一个 pickle 文件。

![训练工作流程](https://cdn-images-1.medium.com/max/2000/1\*dgzEEn15t8NmEsKKKd9sBA.png)

当您成功执行这个工作流程时，您将在定义的文件夹中生成 pipeline.pkl 和 results.csv 文件。

![](https://cdn-images-1.medium.com/max/2000/1\*d1rh9V4BApHEqNwXOR796A.png)

results.csv 包含以下内容：

![](https://cdn-images-1.medium.com/max/2000/1\*8iQmxMyNmXW4NS5lzpOdWA.png)

这些是所有模型的交叉验证指标。在这种情况下，最佳模型是 _**Gradient Boosting Regressor**_。

### 👉 模型评分

现在我们可以使用我们的 pipeline.pkl 对新数据集进行评分。由于我没有单独的数据集用于 'insurance.csv'，我将从同一文件中删除目标列，只是为了演示。

![评分工作流程](https://cdn-images-1.medium.com/max/2000/1\*JK5Dmk1\_I7u7qa7Zrskv\_A.png)

我使用了 **Column Filter** 节点来移除目标列即 charges。在 Python 脚本中执行以下代码：

```
**# 加载流水线
**from pycaret.regression import load_model, predict_model
pipeline = load_model('c:/users/moezs/pycaret-demo-knime/pipeline')

**# 生成预测并保存为 csv**
output_table_1 = predict_model(pipeline, data = input_table_1)
output_table_1.to_csv('c:/users/moezs/pycaret-demo-knime/predictions.csv', index=False)
```

当您成功执行这个工作流程时，它将生成 predictions.csv。

![predictions.csv](https://cdn-images-1.medium.com/max/2000/1\*0NjNrFay0-93xe0pje8j\_g.png)

希望您能欣赏 PyCaret 的易用性和简单性。在像 KNIME 这样的分析平台中使用它，可以节省您大量编码时间，以及在生产中维护代码的时间。只需不到 10 行代码，我就使用 PyCaret 训练和评估了多个模型，并部署了一个 ML Pipeline 到 KNIME。

### 即将推出！

下周我将深入探讨 PyCaret 的更高级功能，您可以在 KNIME 中使用这些功能来增强您的机器学习工作流程。如果您希望自动收到通知，可以在 [Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/) 和 [Twitter](https://twitter.com/moezpycaretorg1) 上关注我。

![作者图片](https://cdn-images-1.medium.com/max/NaN/1\*-Ul7wtRGqybl3eBm58ELcA.png)

![PyCaret — 作者图片](https://cdn-images-1.medium.com/max/NaN/1\*WSZ6hqiO\_B3u5ReftUCGSA.png)

使用这个轻量级的 Python 工作流自动化库，您可以实现无限可能。如果您觉得这个工具有用，请不要忘记在我们的 GitHub 仓库上给我们 ⭐️。

要了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)。

加入我们的 Slack 频道。邀请链接 [在这里](https://join.slack.com/t/pycaret/shared\_invite/zt-p7aaexnl-EqdTfZ9U\~mF0CwNcltffHg)。

### 您可能还感兴趣：

[在 Power BI 中使用 PyCaret 2.0 构建您自己的 AutoML](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d) [使用 Docker 在 Azure 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01) [在 Google Kubernetes Engine 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b) [在 AWS Fargate 上部署机器学习管道](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507) [构建和部署您的第一个机器学习 Web 应用程序](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99) [使用 AWS Fargate 无服务器部署 PyCaret 和 Streamlit 应用](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2) [使用 PyCaret 和 Streamlit 构建和部署机器学习 Web 应用程序](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104) [在 GKE 上部署使用 Streamlit 和 PyCaret 构建的机器学习应用程序](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

### 重要链接
[文档](https://pycaret.readthedocs.io/en/latest/installation.html) [博客](https://medium.com/@moez\_62905) [GitHub](http://www.github.com/pycaret/pycaret) [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装 PyCaret](https://pycaret.readthedocs.io/en/latest/installation.html) [笔记本教程](https://pycaret.readthedocs.io/en/latest/tutorials.html) [为 PyCaret 做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 想了解特定模块吗？

点击下面的链接查看文档和示例。

[分类](https://pycaret.readthedocs.io/en/latest/api/classification.html) [回归](https://pycaret.readthedocs.io/en/latest/api/regression.html) [聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html) [异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html) [自然语言处理](https://pycaret.readthedocs.io/en/latest/api/nlp.html) [关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)

### 更多与 PyCaret 相关的教程：
# **用 PyCaret + MLflow 轻松实现 MLOps**
[**向数据科学进军网站**](https://towardsdatascience.com/easy-mlops-with-pycaret-mlflow-7fbcbf1e38c6)

# **使用 PyCaret 编写和训练自定义机器学习模型**
[**向数据科学进军网站**](https://towardsdatascience.com/write-and-train-your-own-custom-machine-learning-models-using-pycaret-8fa76237374e)

# **使用 PyCaret 构建，使用 FastAPI 部署**
[**向数据科学进军网站**](https://towardsdatascience.com/build-with-pycaret-deploy-with-fastapi-333c710dc786)

# **使用 PyCaret 进行时间序列异常检测**
[**向数据科学进军网站**](https://towardsdatascience.com/time-series-anomaly-detection-with-pycaret-706a6e2b2427)

# **用 PyCaret 和 Gradio 加速你的机器学习实验**
[**向数据科学进军网站**](https://towardsdatascience.com/supercharge-your-machine-learning-experiments-with-pycaret-and-gradio-5932c61f80d9)

# **使用 PyCaret 进行多时间序列预测**
[**向数据科学进军网站**](https://towardsdatascience.com/multiple-time-series-forecasting-with-pycaret-bc0a779a22fe)

# **使用 PyCaret 回归模块进行时间序列预测**
[**向数据科学进军网站**](https://towardsdatascience.com/time-series-forecasting-with-pycaret-regression-module-237b703a0c63)

# **在 PyCaret 中你做错的 5 件事**
[**向数据科学进军网站**](https://towardsdatascience.com/5-things-you-are-doing-wrong-in-pycaret-e01981575d2a)

# **GitHub 是你唯一需要的最佳 AutoML**
[**向数据科学进军网站**](https://towardsdatascience.com/github-is-the-best-automl-you-will-ever-need-5331f671f105)

# **在 Power BI 中使用 PyCaret 构建你自己的 AutoML**
[**向数据科学进军网站**](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d)

# **使用 AWS Fargate 部署 PyCaret 和 Streamlit 应用 —— 无服务器基础设施**
[**向数据科学进军网站**](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2)

# **在 Google Kubernetes Engine 上部署使用 Streamlit 和 PyCaret 构建的机器学习应用**
[**向数据科学进军网站**](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

# **使用 PyCaret 和 Streamlit 构建和部署机器学习 Web 应用**
[**向数据科学进军网站**](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104)

# **在 AWS Fargate 上部署机器学习管道**
[**向数据科学进军网站**](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507)

# **在 Power BI 中使用 PyCaret 进行主题建模**
[**向数据科学进军网站**](https://towardsdatascience.com/topic-modeling-in-power-bi-using-pycaret-54422b4e36d6)

# **在 Google Kubernetes Engine 上部署机器学习管道**
[**向数据科学进军网站**](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b)

# **在 Power BI 中使用 PyCaret 实现聚类**
[**向数据科学进军网站**](https://towardsdatascience.com/how-to-implement-clustering-in-power-bi-using-pycaret-4b5e34b1405b)

# **在 Power BI 中使用 PyCaret 构建你的第一个异常检测器**
[**向数据科学进军网站**](https://towardsdatascience.com/build-your-first-anomaly-detector-in-power-bi-using-pycaret-2b41b363244e)

# **使用 Docker 容器在云上部署机器学习管道**
[**向数据科学进军网站**](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01)

# **构建和部署你的第一个机器学习 Web 应用**
[**向数据科学进军网站**](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99)

# **在 Power BI 中使用 PyCaret 进行机器学习**
[**向数据科学进军网站**](https://towardsdatascience.com/machine-learning-in-power-bi-using-pycaret-34307f09394a)
在计算机科学中，有一种称为无损压缩的技术，它可以将文件的大小减小，而不会丢失任何数据。这种技术在许多应用中非常有用，例如在存储和传输文件时，可以节省空间和带宽。

无损压缩的原理是利用文件中存在的冗余信息来减小文件的大小。冗余信息是指文件中存在的重复、不必要或可预测的数据。通过识别和消除这些冗余信息，可以将文件的大小减小，而不会对文件的内容造成任何改变。

有许多无损压缩算法可供选择，每种算法都有其独特的优点和适用范围。其中一种常见的算法是基于字典的压缩算法，例如LZ77和LZ78。这些算法通过构建一个字典，将文件中的重复模式替换为对字典中的条目的引用来实现压缩。

另一种常见的无损压缩算法是基于算术编码的压缩算法，例如Huffman编码和算术编码。这些算法通过将文件中的符号映射到较短的编码来实现压缩，从而减小文件的大小。

无损压缩的一个重要特点是压缩后的文件可以完全恢复为原始文件，而不会丢失任何信息。这使得无损压缩在许多领域中得到广泛应用，包括图像压缩、音频压缩和文本压缩等。

然而，无损压缩并不是万能的，它对某些类型的文件可能效果不佳。例如，对于已经经过压缩的文件（例如JPEG图像或MP3音频），再次进行无损压缩可能不会显著减小文件的大小。此外，无损压缩通常无法达到与有损压缩相同的压缩比。有损压缩是另一种压缩技术，它可以更大程度地减小文件的大小，但会引入一定程度的数据损失。

总之，无损压缩是一种非常有用的技术，可以在不丢失任何数据的情况下减小文件的大小。它使用各种算法来识别和消除文件中的冗余信息，从而实现压缩。无损压缩在许多领域中得到广泛应用，并且是计算机科学中的重要研究领域。