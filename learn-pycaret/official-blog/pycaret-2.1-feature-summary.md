# PyCaret 2.1 功能摘要

![PyCaret 2.1 现已提供 pip 下载。https://www.pycaret.org](https://cdn-images-1.medium.com/max/4800/1\*OYS6O-iLkoE88fBbd3IKcw.jpeg)

### PyCaret 2.1 来了 — 有什么新功能？

我们很高兴地宣布 PyCaret 2.1 — 这是 2020 年 8 月的更新。

PyCaret 是一个开源的、**低代码** Python 机器学习库，可以自动化机器学习工作流程。它是一个端到端的机器学习和模型管理工具，可以加快机器学习实验周期，让您的工作效率提高 10 倍。

与其他开源机器学习库相比，PyCaret 是一个替代性的低代码库，可以用少量代码取代数百行代码。这使得实验速度指数级提升，效率更高。

如果您之前没有听说过或使用过 PyCaret，请查看我们的[先前公告](https://towardsdatascience.com/announcing-pycaret-2-0-39c11014540e)，快速开始。

### 安装 PyCaret

安装 PyCaret 非常简单，只需几分钟时间。我们强烈建议使用虚拟环境，以避免与其他库可能发生的冲突。请参考以下示例代码，在一个 \*\*\*conda 环境 \*\*\*中创建并安装 pycaret：

```
**# 创建一个 conda 环境 **
conda create --name yourenvname python=3.6  

**# 激活环境 **
conda activate yourenvname  

**# 安装 pycaret **
pip install **pycaret==2.1  **

**# 创建与 conda 环境关联的 notebook 内核
**python -m ****ipykernel install --user --name yourenvname --display-name "display-name"
```

### **PyCaret 2.1 功能摘要**

### 👉 GPU 上的超参数调整

在 PyCaret 2.0 中，我们宣布了针对某些算法（XGBoost、LightGBM 和 Catboost）的 GPU 加速训练。在 2.1 中的新功能是，现在您还可以在 GPU 上调整这些模型的超参数。

```
**# 使用 GPU 训练 xgboost**
xgboost = create_model('xgboost', tree_method = 'gpu_hist')

**# 调整 xgboost 
**tuned_xgboost **= **tune_model(xgboost)
```

在 \*\*tune\_model \*\*函数内不需要额外的参数，因为它会自动继承自使用 \*\*create\_model \*\*函数创建的 xgboost 实例的 tree\_method。如果您对一点比较感兴趣，这里有一个例子：

> **在一个具有 8 个类别的多类问题中，有 8 个类别的 10 万行和 88 个特征**

![在 GPU 上训练 XGBoost（使用 Google Colab）](https://cdn-images-1.medium.com/max/2180/1\*1lAya7O3sEad9-epPH1sUw.jpeg)

### 👉 模型部署

自 2020 年 4 月首次发布 PyCaret 以来，您可以通过 Notebook 中简单地使用 \*\*deploy\_model \*\*将训练好的模型部署在 AWS 上。在最新版本中，我们添加了支持在 GCP 和 Microsoft Azure 上部署的功能。

#### **Microsoft Azure**

要在 Microsoft Azure 上部署模型，必须设置用于连接字符串的环境变量。连接字符串可以从 Azure 中存储帐户的“访问密钥”中获取。

![https:/portal.azure.com — 从存储帐户获取连接字符串](https://cdn-images-1.medium.com/max/3832/1\*XPH0ZtRmQkRxVHiqEMLaIw.png)

复制连接字符串后，您可以将其设置为环境变量。请参考以下示例：

```
**import os
**os.environ['AZURE_STORAGE_CONNECTION_STRING'] = 'your-conn-string'

**from pycaret.classification import load_model**
deploy_model(model = model, model_name = 'model-name', platform = 'azure', authentication = {'container' : 'container-name'})
```

就是这样。只需一行代码\*\*，\*\*您的整个机器学习流程现在已经部署在 Microsoft Azure 的容器中。您可以使用 **load\_model** 函数访问它。

```
**import os
**os.environ['AZURE_STORAGE_CONNECTION_STRING'] = 'your-conn-string'

**from pycaret.classification import load_model
**loaded_model = load_model(model_name = 'model-name', platform = 'azure', authentication = {'container' : 'container-name'})

**from pycaret.classification import predict_model
**predictions = predict_model(loaded_model, data = new-dataframe)
```

#### Google Cloud Platform

要在 Google Cloud Platform (GCP) 上部署模型，您必须首先使用命令行或 GCP 控制台创建一个项目。创建项目后，您必须创建一个服务帐户，并下载服务帐户密钥作为 JSON 文件，然后将其用于设置环境变量。

![在 GCP 控制台创建新服务帐户并下载 JSON](https://cdn-images-1.medium.com/max/3834/1\*nN6uslyOixxmYpFcVel8Bw.png)

要了解更多关于创建服务帐户的信息，请阅读[官方文档](https://cloud.google.com/docs/authentication/production)。一旦您创建了服务帐户并从 GCP 控制台下载了 JSON 文件，您就可以准备部署。
```
**导入 os 模块
**os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = 'c:/path-to-json-file.json'

**从 pycaret.classification 模块中导入 deploy_model 函数
**deploy_model(model = model, model_name = 'model-name', platform = 'gcp', authentication = {'project' : 'project-name', 'bucket' : 'bucket-name'})
```
模型已上传。您现在可以使用 **load\_model** 函数从 GCP 存储桶中访问该模型。

```
**import os
**os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = 'c:/path-to-json-file.json'

**from pycaret.classification import load_model
**loaded_model = load_model(model_name = 'model-name', platform = 'gcp', authentication = {'project' : 'project-name', 'bucket' : 'bucket-name'})

**from pycaret.classification import predict_model
**predictions = predict_model(loaded_model, data = new-dataframe)
```

### 👉 MLFlow 部署

除了使用 PyCaret 的原生部署功能外，您现在还可以使用所有 MLFlow 的部署功能。要使用这些功能，您必须在 \*\*setup \*\*函数中使用 **log\_experiment** 参数记录您的实验。

```
**# 初始化设置**
exp1 = setup(data, target = 'target-name', log_experiment = True, experiment_name = 'exp-name')

**# 创建 xgboost 模型
**xgboost = create_model('xgboost')

..
..
..

# 您的脚本的其余部分

**# 在本地主机上启动 mlflow 服务器**
!mlflow ui
```

现在在您喜爱的浏览器中打开 [https://localhost:5000](https://localhost:5000)。

![MLFlow UI on https://localhost:5000](https://cdn-images-1.medium.com/max/3838/1\*y0nMOMuDeMS1sdFepDngKw.png)

点击左侧的 **“Start Time”** 旁边的 **“Run Name”**，您可以查看运行的详细信息。在内部，您可以看到训练模型的所有超参数和评分指标，如果向下滚动一点，还会显示所有工件（如下所示）。

![MLFLow Artifacts](https://cdn-images-1.medium.com/max/3496/1\*NS7ifCnHHKRpLHCWeYhNZg.png)

训练模型以及其他元数据文件存储在目录 “/model” 下。MLFlow 遵循标准格式打包机器学习模型，可以在各种下游工具中使用，例如通过 REST API 进行实时服务或在 Apache Spark 上进行批量推断。如果您希望在本地提供此模型，可以使用 MLFlow 命令行来执行。

```
mlflow models serve -m local-path-to-model
```

然后，您可以使用 CURL 发送请求到模型以获取预测结果。

```
curl [http://127.0.0.1:5000/invocations](http://127.0.0.1:5000/invocations) -H 'Content-Type: application/json' -d '{
    "columns": ["age", "sex", "bmi", "children", "smoker", "region"],
    "data": [[19, "female", 27.9, 0, "yes", "southwest"]]
}'
```

（注：目前 Windows 操作系统不支持此 MLFlow 功能）。

MLFlow 还与 AWS Sagemaker 和 Azure 机器学习服务集成。您可以在 SageMaker 兼容环境中的 Docker 容器中本地训练模型，也可以远程在 SageMaker 上训练。要远程部署到 SageMaker，您需要设置环境和 AWS 用户帐户。

**使用 MLflow CLI 的示例工作流程**

```
mlflow sagemaker build-and-push-container 
mlflow sagemaker run-local -m <path-to-model>
mlflow sagemaker deploy <parameters>
```

要了解有关 MLFlow 所有部署功能的更多信息，请[点击这里](https://www.mlflow.org/docs/latest/models.html)。

### 👉 MLFlow 模型注册表

MLflow 模型注册表组件是一个集中的模型存储、一组 API 和用户界面，用于协作管理 MLflow 模型的完整生命周期。它提供模型血统（哪个 MLflow 实验和运行生成了模型）、模型版本控制、阶段转换（例如从暂存到生产）和注释。

如果要运行自己的 MLflow 服务器，您必须使用基于数据库的后端存储才能访问模型注册表。要了解更多信息，请[点击这里](https://www.mlflow.org/docs/latest/tracking.html#backend-stores)。但是，如果您使用的是 [Databricks](https://databricks.com/) 或任何托管的 Databricks 服务，如 [Azure Databricks](https://azure.microsoft.com/en-ca/services/databricks/)，您无需担心设置任何内容。它已经具备您所需的一切功能。

![https://databricks.com/blog/2020/06/25/announcing-mlflow-model-serving-on-databricks.html](https://cdn-images-1.medium.com/max/2048/1\*XlT58YrFuszGb-1PIXvKZw.gif)

### 👉 高分辨率绘图

这并非突破性，但对于使用 PyCaret 进行研究和出版的人来说确实是一个非常有用的补充。**plot\_model** 现在有一个名为 “scale” 的额外参数，通过它您可以控制分辨率，生成高质量的绘图用于出版。

```
**# 创建线性回归模型**
lr = create_model('lr')

**# 以高质量分辨率绘制
**plot_model(lr, scale = 5) # 默认为 1
```

![PyCaret 生成的高分辨率残差图](https://cdn-images-1.medium.com/max/3456/1\*O413K8IUvgYTgD3aTtcYjw.png)

### 👉 用户定义的损失函数
这是自首个版本发布以来最受欢迎的功能之一。允许使用自定义/用户定义函数调整模型的超参数，为数据科学家提供了极大的灵活性。现在可以使用 **tune_model** 函数中的 **custom_scorer** 参数来使用用户定义的自定义损失函数。

```
**# 定义损失函数**
def my_function(y_true, y_pred):
...
...

**# 使用 sklearn 创建评分器**
from sklearn.metrics import make_scorer**
**my_own_scorer = make_scorer(my_function, needs_proba=True)

**# 训练 catboost 模型
**catboost = create_model('catboost')

**# 使用自定义评分器调整 catboost
**tuned_catboost = tune_model(catboost, custom_scorer = my_own_scorer)
```

### 👉 特征选择

特征选择是机器学习中的基础步骤。您拥有大量特征，希望仅选择相关的特征并丢弃其他特征。其目的是通过去除无用特征简化问题，避免引入不必要的噪音。

在 PyCaret 2.1 中，我们在 Python 中引入了 Boruta 算法的实现（最初在 R 中实现）。Boruta 是一个相当智能的算法，可以自动对数据集执行特征选择。要使用它，您只需在 **setup** 函数中传递 **feature_selection_method**。

```
exp1 = setup(data, target = 'target-var', feature_selection = True, feature_selection_method = 'boruta')
```

要了解有关 Boruta 算法的更多信息，[点击这里。](https://towardsdatascience.com/boruta-explained-the-way-i-wish-someone-explained-it-to-me-4489d70e154a)

### 👉 其他变更

* 在 **compare_models** 函数中，黑名单和白名单参数现在更名为 exclude 和 include，功能保持不变。
* 在 **compare_models** 函数中，添加了新参数 budget_time，用于设置训练时间的上限。
* PyCaret 现在兼容 Pandas 的分类数据类型。在内部，它们被转换为对象，并与对象或布尔值的处理方式相同。
* 数值缺失值填充 新方法 zero 已添加到 **setup** 函数中的 **numeric_imputation**。当方法设置为 zero 时，缺失值将替换为常数 0。
* 为了使输出更易读，predict_model 函数返回的标签列现在返回原始值，而不是编码值。

要了解 PyCaret 2.1 中的所有更新，请查看[发布说明](https://github.com/pycaret/pycaret/releases/tag/2.1)。

使用 Python 中的轻量级工作流自动化库，您可以实现无限可能。如果您觉得这个工具有用，请不要忘记在我们的 [GitHub 仓库](https://www.github.com/pycaret/pycaret/) 上给我们 ⭐️。

要了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。

### 重要链接

[用户指南](https://www.pycaret.org/guide) [文档](https://pycaret.readthedocs.io/en/latest/) [官方教程](https://github.com/pycaret/pycaret/tree/master/tutorials) [示例笔记本](https://github.com/pycaret/pycaret/tree/master/examples) [其他资源](https://github.com/pycaret/pycaret/tree/master/resources)

### 想了解特定模块吗？

点击下面的链接查看文档和示例。

[分类](https://www.pycaret.org/classification) [回归](https://www.pycaret.org/regression) [聚类](https://www.pycaret.org/clustering) [异常检测](https://www.pycaret.org/anomaly-detection) [自然语言处理](https://www.pycaret.org/nlp) [关联规则挖掘](https://www.pycaret.org/association-rules)