---
description: 所有其他设置相关参数
---

# 其他设置参数

### 必填参数

设置函数中只有两个非可选参数。

#### 参数

* **data: pandas.DataFrame**\
  ****形状为 (n\_samples, n\_features) 的数据框，其中 n\_samples 是样本数量，n\_features 是特征数量。
* **target: str**\
  ****要传入的目标列的名称，以字符串形式传递。&#x20;

### 实验日志记录

PyCaret 可以自动记录整个实验，包括设置参数、模型超参数、性能指标和管道工件。默认设置使用 [MLflow](https://mlflow.org/) 作为日志记录后端。[wandb](https://wandb.ai/) 也可作为日志记录后端的选项。可以在设置中启用一个参数，自动跟踪有关您的机器学习模型的所有指标、超参数和其他重要信息。&#x20;

#### 参数

* **log\_experiment: bool, default = bool 或字符串 'mlflow' 或 'wandb'**\
  一个 (列表) PyCaret `BaseLogger` 或字符串 (其中一个 `mlflow`, `wandb`)，对应一个确定要使用哪个实验记录器的记录器。设置为 True 将默认使用 MLFlow 后端。
* **experiment\_name: str, default = None**\
  用于记录的实验名称。当设置为 `None` 时，将使用默认名称。
* **experiment\_custom\_tags: dict, default = None**\
  ****tag\_name: String -> value: (String，但如果不是则将被转换为字符串) 的字典，传递给 mlflow.set\_tags 以为实验添加新的自定义标签。
* **log\_plots: bool, default = False**\
  当设置为 `True` 时，适用的分析图将被记录为图像文件。
* **log\_profile: bool, default = False**\
  当设置为 `True` 时，数据概要将被记录为 HTML 文件。&#x20;
* **log\_data: bool, default = False**\
  当设置为 `True` 时，训练和测试数据集将被记录为 CSV 文件。

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

![](<../../.gitbook/assets/image (215) (1).png>)

#### 配置 MLflow 跟踪服务器

当未配置后端时，数据将存储在提供的文件中（或如果为空，则存储在 ./mlruns 中）。在执行设置函数之前，使用 `mlflow.set_tracking_uri` 来配置后端。

* 空字符串，或以 file:/ 开头的本地文件路径。数据将存储在提供的文件中（或如果为空，则存储在 ./mlruns 中）。
* 像 https://my-tracking-server:5000 这样的 HTTP URI。
* Databricks 工作区，提供为字符串 “databricks” 或，要使用 Databricks CLI 配置文件，则为 “databricks://\<profileName>”。

```
# 设置跟踪 uri 
import mlflow 
mlflow.set_tracking_uri('file:/c:/users/mlflow-server')

# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target = 'Class variable', log_experiment = True, experiment_name = 'diabetes1')
```

#### 在 Databricks 上使用 PyCaret

在 Databricks 上使用 PyCaret 时，设置中的 `experiment_name` 参数必须包括完整的存储路径。查看以下示例，了解在使用 Databricks 时如何记录实验：

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target = 'Class variable', log_experiment = True, experiment_name = '/Users/username@domain.com/experiment-name-here')
```

### 模型选择

设置中的以下参数可用于设置模型选择过程的参数。这些参数与数据预处理无关，但可能会影响您的模型选择过程。

#### 参数

* **train\_size: float, default = 0.7**\
  ****用于训练和验证的数据集比例。&#x20;
* **test\_data: pandas.DataFrame, default = None**\
  ****如果不是 `None`，则 `test_data` 将用作保留集，`train_size` 将被忽略。`test_data` 必须带有标签，并且 `data` 和 `test_data` 的形状必须匹配。
* **data\_split\_shuffle: bool, default = True**\
  当设置为 `False` 时，阻止在 `train_test_split` 过程中对行进行洗牌。
* **data\_split\_stratify: bool 或列表, default = False**\
  控制 `train_test_split` 过程中的分层。当设置为 `True` 时，将按目标列进行分层。要按其他列进行分层，请传递列名称的列表。当 `data_split_shuffle` 为 `False` 时忽略。
* **fold\_strategy: str 或 scikit-learn** **CV 生成器对象, default = ‘stratifiedkfold’**\
### 交叉验证策略选择

交叉验证策略的可能取值包括：
- ‘kfold’
- ‘stratifiedkfold’
- ‘groupkfold’
- ‘timeseries’
- 与 `scikit-learn` 兼容的自定义 CV 生成器对象。

* **fold: int, 默认值 = 10**\
  ****用于交叉验证的折数。必须至少为 2。这是一个全局设置，可以通过使用 `fold` 参数在函数级别进行覆盖。当 `fold_strategy` 是自定义对象时将被忽略。

* **fold\_shuffle: bool, 默认值 = False**\
  ****控制 CV 的 shuffle 参数。仅适用于 `fold_strategy` 是 `kfold` 或 `stratifiedkfold` 时。当 `fold_strategy` 是自定义对象时将被忽略。

* **fold\_groups: str 或 array-like，形状为 (n\_samples,)，默认值 = None**\
  
  在进行交叉验证时使用 'GroupKFold' 时的可选组标签。它接受一个形状为 (n\_samples,) 的数组，其中 n\_samples 是训练数据集中的行数。当传递字符串时，它被解释为数据集中包含组标签的列名。

### 其他杂项

设置中的以下参数可用于控制其他实验设置，例如在训练中使用 GPU 或设置实验的详细程度。它们不会以任何方式影响数据。

#### 参数

* **n\_jobs: int, 默认值 = -1**\
  ****并行运行的作业数（支持并行处理的函数）-1 表示使用所有处理器。要在单个处理器上运行所有函数，请将 n\_jobs 设置为 None。

* **use\_gpu: bool 或 str，默认值 = False**\
  ****当设置为 `True` 时，将使用支持 GPU 的算法进行训练，如果不可用，则回退到 CPU。当设置为 `force` 时，将仅使用支持 GPU 的算法，并在不可用时引发异常。当设置为 `False` 时，所有算法都将仅使用 CPU 进行训练。

* **html: bool, 默认值 = True**\
  当设置为 `False` 时，阻止运行时监视器的显示。当环境不支持 IPython 时，必须将其设置为 `False`。例如，命令行终端，Databricks，PyCharm，Spyder 和其他类似的 IDE。

* **session\_id: int, 默认值 = None**\
  ****控制实验的随机性。相当于 `scikit-learn` 中的 `random_state`。当为 `None` 时，生成一个伪随机数。这可用于以后重现整个实验。

* **silent: bool, 默认值 = False**\
  控制在执行 `setup` 时数据类型确认输入。在完全自动化模式下执行或在远程内核上执行时，必须将其设置为 `True`。

* **verbose: bool, 默认值 = True**\
  当设置为 `False` 时，不打印信息网格。

* **profile: bool, 默认值 = False**\
  当设置为 `True` 时，显示交互式 EDA 报告。

* **profile\_kwargs: dict, 默认值 = {}（空字典）**\
  传递给用于创建 EDA 报告的 ProfileReport 方法的参数字典。如果 `profile` 为 False，则将被忽略。

* **custom\_pipeline: (str, transformer) 或 (str, transformer) 列表，默认值 = None**\
  ****传递时，将在预处理流水线中附加自定义转换器，并分别应用于每个 CV 折叠以及最终拟合。所有自定义转换都将应用于 `train_test_split` 之后，并在 PyCaret 的内部转换之前应用。

* **preprocess: bool, 默认值 = True**\
  当设置为 `False` 时，除了 `train_test_split` 和传递给 `custom_pipeline` 参数的自定义转换之外，不应用任何转换。当 preprocess 设置为 `False` 时，数据必须准备好进行建模（没有缺失值，没有日期，分类数据编码）。