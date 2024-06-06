---
description: PyCaret 中初始化实验的函数
---

# 初始化

## setup

此函数在 PyCaret 中初始化实验，并根据传递给函数的所有参数创建转换流水线。在执行任何其他函数之前，必须先调用设置函数。它有两个必填参数：`data` 和 `target`。所有其他参数都是可选的。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')
```

一旦运行设置函数，PyCaret 将自动推断数据集中所有变量的数据类型。如果这些数据类型被正确推断，您可以按 Enter 键继续。

![](<../../.gitbook/assets/image (390).png>)

当您按 Enter 键继续时，您将看到如下输出：

![输出被截断以显示](<../../.gitbook/assets/image (362).png>)

所有预处理和数据转换都在设置函数中配置。有许多选项可供选择，从数据清洗到特征工程。要了解更多关于所有可用预处理的信息，[请查看此页面](../preprocessing/)。

{% hint style="info" %}
**注意：** 如果您不想看到数据类型确认，可以在设置中传递 `silent=True` 以在没有任何中断的情况下运行它。&#x20;
{% endhint %}

### 必填参数

设置函数中有许多参数，但只有两个是非可选的。

* **data: pandas.DataFrame**\
  ****形状为 (n\_samples, n\_features)，其中 n\_samples 是样本数量，n\_features 是特征数量。
* **target: str**\
  ****要传递的目标列的名称，以字符串形式。&#x20;

{% hint style="info" %}
**注意：** 对于无监督模块（如 `clustering`、`anomaly detection` 或 `NLP`），目标参数是不需要的。
{% endhint %}

### 默认转换

设置中的所有预处理步骤只是一个 `True` 或 `False` 的标志。例如，如果您想要对特征进行缩放，您将需要在设置函数中传递 `normalize=True`。然而，有三件事会默认发生：

* [缺失值填充](../preprocessing/data-preparation.md#missing-values)
* [独热编码](../preprocessing/data-preparation.md#one-hot-encoding)
* [训练-测试分割](../preprocessing/other-setup-parameters.md)

### 实验日志记录

PyCaret 使用 [MLflow](https://mlflow.org/) 进行实验跟踪。设置中的一个参数可以启用自动跟踪所有有关您机器学习模型的指标、超参数和其他重要信息。&#x20;

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target = 'Class variable', log_experiment = True, experiment_name = 'diabetes1')

# 模型训练
best_model = compare_models() 
```

要初始化 `MLflow` 服务器，您必须从笔记本内或命令行中运行以下命令。一旦服务器初始化完成，您可以在 `https://localhost:5000` 上跟踪您的实验。

```
# 初始化服务器
!mlflow ui
```

![](<../../.gitbook/assets/image (215).png>)

要了解更多关于 PyCaret 中实验跟踪的信息，[请查看此页面](../preprocessing/other-setup-parameters.md#experiment-logging)。

### 模型验证

设置函数中有一些与预处理或数据转换无直接关系，但作为模型验证和选择策略的一部分使用的参数，如 `train_size`、`fold_strategy` 或交叉验证的 `fold` 数量。要了解设置中所有模型验证和选择设置的更多信息，请参阅[此页面](../preprocessing/other-setup-parameters.md#model-selection)。

### GPU 支持&#x20;

使用 PyCaret，您可以在 GPU 上训练模型，将工作流程加速 10 倍。要在 GPU 上训练模型，只需在设置函数中传递 `use_gpu = True`。API 的使用没有变化，但在某些情况下，必须安装额外的库，因为它们没有与默认版本或完整版本一起安装。要了解更多关于 GPU 支持的信息，请参阅[此页面](../installation.md#gpu)。

### 示例

要查看在 PyCaret 的其他模块中使用 `setup` 的示例，请参见以下内容：

* [分类](../quickstart.md#classification)
* [回归](../quickstart.md#regression)
* [聚类](../quickstart.md#clustering)
* [异常检测](../quickstart.md#anomaly-detection)
* [自然语言处理](../quickstart.md#natural-language-processing)
* [关联规则挖掘](../quickstart.md#association-rules-mining)

{% hint style="warning" %}
**注意：** 在 Python 中，`setup` 函数使用全局环境变量。因此，如果在同一个脚本中两次运行 `setup` 函数，它将覆盖先前的实验。PyCaret 的下一个重大更新将包括一个新的面向对象的 API，通过类实例可以创建多个实例。  
{% endhint %}