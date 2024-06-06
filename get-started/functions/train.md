---
description: PyCaret 中的训练函数
---

# 训练

## compare\_models

该函数使用交叉验证训练和评估模型库中所有可用的估计器的性能。该函数的输出是一个带有平均交叉验证分数的评分表。可以使用 `get_metrics` 函数访问 CV 过程中评估的指标。可以使用 `add_metric` 和 `remove_metric` 函数添加或删除自定义指标。

### 示例

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 比较模型
best = compare_models()
```

![compare\_models 的输出](<../../.gitbook/assets/image (245).png>)

`compare_models` 仅返回基于 `sort` 参数定义的标准的表现最佳模型。对于分类实验，标准是 `Accuracy`，对于回归实验，标准是 `R2`。您可以通过传递要进行模型选择的基于哪个指标的名称来更改 `sort` 顺序。&#x20;

### 更改排序顺序

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 比较模型
best = compare_models(sort = 'F1')
```

![compare\_models(sort = 'F1') 的输出](<../../.gitbook/assets/image (213).png>)

注意，现在评分表的排序顺序已更改，而且此函数返回的最佳模型是基于 `F1` 选择的。

```
print(best)
```

![print(best) 的输出](<../../.gitbook/assets/image (71).png>)

### 仅比较少数模型

如果您不想在整个模型库上进行比赛，可以通过使用 `include` 参数仅比较您选择的少数模型。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 比较模型
best = compare_models(include = ['lr', 'dt', 'lightgbm'])
```

![compare\_models(include = \['lr', 'dt', 'lightgbm'\]) 的输出](<../../.gitbook/assets/image (393).png>)

或者，您也可以使用 `exclude` 参数。这将比较除在 `exclude` 参数中传递的模型之外的所有模型。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 比较模型
best = compare_models(exclude = ['lr', 'dt', 'lightgbm'])
```

![compare\_models(exclude = \['lr', 'dt', 'lightgbm'\]) 的输出](<../../.gitbook/assets/image (545).png>)

### 返回多个模型

默认情况下，`compare_models` 仅返回表现最佳模型，但如果需要，您可以获得前 N 个模型而不仅仅是一个模型。&#x20;

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 比较模型
best = compare_models(n_select = 3)
```

![compare\_models(n\_select = 3) 的输出](<../../.gitbook/assets/image (342).png>)

请注意，结果没有变化，但是如果检查变量 `best`，现在它将包含前 3 个模型的列表，而不仅仅是之前看到的一个模型。&#x20;

```
type(best)
>>> list

print(best)
```

![print(best) 的输出](<../../.gitbook/assets/image (79).png>)

### 设置预算时间

如果时间紧迫，您想为此函数设置固定的预算时间，可以通过设置 `budget_time` 参数来实现。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 比较模型
best = compare_models(budget_time = 0.5)
```

![compare\_models(budget\_time = 0.5) 的输出](<../../.gitbook/assets/image (262).png>)

### 设置概率阈值

在执行二元分类时，您可以更改硬标签的概率阈值或截断值。默认情况下，所有分类器都使用 `0.5` 作为默认阈值。&#x20;

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 比较模型
best = compare_models(probability_threshold = 0.25)
```

![compare\_models(probability\_threshold = 0.25) 的输出](<../../.gitbook/assets/image (92) (1).png>)

请注意，除 `AUC` 外，现在所有指标都不同。AUC 不会更改，因为它不依赖于硬标签，其他所有指标都依赖于使用 `probability_threshold=0.25` 获得的硬标签。

{% hint style="info" %}
**注意：** 此参数仅在 PyCaret 的 [Classification](../modules.md) 模块中可用。
{% endhint %}

### 禁用交叉验证

如果您不想使用交叉验证来评估模型，而是只想训练它们并在测试/留出集上查看指标，您可以设置 `cross_validation=False`。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=diabetes, target='Class variable')

# 比较模型
best = compare_models(cross_validation=False)
```

![compare\_models(cross\_validation=False) 的输出](<../../.gitbook/assets/image (291).png>)

输出看起来相似，但如果您仔细观察，指标现在不同了，这是因为这些指标不再是平均交叉验证分数，而是测试/留出集上的指标。

{% hint style="info" %}
**注意：** 此函数仅在 [Classification](../modules.md) 和 [Regression](../modules.md) 模块中可用。
{% endhint %}

### 集群上的分布式训练

为了在大型数据集上扩展，您可以使用一个名为 `parallel` 的参数，在分布式模式下在集群上运行 `compare_models` 函数。它利用 [Fugue](https://github.com/fugue-project/fugue/) 抽象层在 Spark 或 Dask 集群上运行 `compare_models`。&#x20;

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=diabetes, target='Class variable', n_jobs=1)

# 创建 pyspark 会话
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()

# 导入并行后端
from pycaret.parallel import FugueBackend

# 比较模型
best = compare_models(parallel=FugueBackend(spark))
```

![compare\_models(parallel=FugueBackend(spark)) 的输出](<../../.gitbook/assets/image (378).png>)

{% hint style="info" %}
请注意，我们需要在设置中设置 `n_jobs = 1`，以便在本地 Spark 上进行测试，因为某些模型已经尝试使用所有可用核心，并行运行这些模型可能会导致资源争用而发生死锁。&#x20;
{% endhint %}

对于 Dask，我们可以在 `FugueBackend` 中指定 `"dask"`，它将调用可用的 Dask 客户端。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=diabetes, target='Class variable', n_jobs=1)

# 导入并行后端
from pycaret.parallel import FugueBackend

# 比较模型
best = compare_models(parallel=FugueBackend("dask"))
```

有关分布式执行的完整示例和其他功能，请查看[此示例](https://github.com/pycaret/pycaret/blob/master/examples/PyCaret%202%20Fugue%20Integration.ipynb)。该示例还展示了如何实时获取排行榜。在分布式设置中，这涉及设置一个 RPCClient，但 Fugue 简化了这一过程。

## create\_model

此函数训练并评估给定估算器的性能，使用交叉验证。此函数的输出是一个按折叠计算的评分表。可以使用 `get_metrics` 函数访问 CV 期间评估的指标。可以使用 `add_metric` 和 `remove_metric` 函数添加或删除自定义指标。可以使用 `models` 函数访问所有可用模型。

### **示例**&#x20;

```
# 加载数据集 
from pycaret.datasets import get_data 
diabetes = get_data('diabetes') 

# 初始化设置
from pycaret.classification import * 
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练逻辑回归
lr = create_model('lr')
```

![create\_model('lr') 的输出](<../../.gitbook/assets/image (85).png>)

此函数按折叠显示性能指标，以及每个指标的平均值和标准差，并返回训练好的模型。默认情况下，它使用 `10` 折，可以在 [setup](initialize.md#setup) 函数中全局更改，或在 `create_model` 中局部更改。

### 更改折叠参数

```
# 加载数据集 
from pycaret.datasets import get_data 
diabetes = get_data('diabetes') 

# 初始化设置
from pycaret.classification import * 
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练逻辑回归
lr = create_model('lr', fold = 5)
```

![create\_model('lr', fold = 5) 的输出](<../../.gitbook/assets/image (507).png>)

此处返回的模型与上述相同，但是性能评估是使用 5 折交叉验证进行的。&#x20;

### 模型库

要查看任何模块中可用模型的列表，可以使用 `models` 函数。

```
# 加载数据集 
from pycaret.datasets import get_data 
diabetes = get_data('diabetes') 

# 初始化设置
from pycaret.classification import * 
clf1 = setup(data = diabetes, target = 'Class variable')

# 检查可用模型
models()
```
![models() 的输出](<../../.gitbook/assets/image (304).png>)

### 使用自定义参数的模型

当您只运行 `create_model('dt')` 时，它将使用所有默认的超参数设置来训练决策树。如果您想要更改这些设置，只需在 `create_model` 函数中传递属性即可。

```
# 加载数据集
from pycaret.datasets import get_data 
diabetes = get_data('diabetes') 

# 初始化设置
from pycaret.classification import * 
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练决策树
dt = create_model('dt', max_depth = 5)
```

![create_model('dt', max_depth = 5) 的输出](<../../.gitbook/assets/image (90).png>)

```
# 查看模型参数
print(dt)
```

![](<../../.gitbook/assets/image (52).png>)

### 访问评分表

在 `create_model` 后看到的性能指标/评分表仅用于显示，并不会返回。因此，如果您想要将该表格作为 `pandas.DataFrame` 访问，您需要在 `create_model` 后使用 `pull` 命令。

```
# 加载数据集
from pycaret.datasets import get_data 
diabetes = get_data('diabetes') 

# 初始化设置
from pycaret.classification import * 
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练决策树
dt = create_model('dt', max_depth = 5)

# 访问评分表
dt_results = pull()
print(dt_results)
```

![print(dt_results) 的输出](<../../.gitbook/assets/image (26).png>)

```
# 检查类型
type(dt_results)
>>> pandas.core.frame.DataFrame

# 仅选择均值和标准差
dt_results.loc[['Mean', 'SD']]
```

![dt_results.loc\[\['Mean', 'SD'\]\] 的输出](<../../.gitbook/assets/image (150).png>)

### 禁用交叉验证

如果您不想使用交叉验证评估模型，而是只训练它们并在测试/留出集上查看指标，您可以设置 `cross_validation=False`。

```
# 加载数据集
from pycaret.datasets import get_data 
diabetes = get_data('diabetes') 

# 初始化设置
from pycaret.classification import * 
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练模型无交叉验证
lr = create_model('lr', cross_validation = False)
```

![create_model('lr', cross_validation = False) 的输出](<../../.gitbook/assets/image (5).png>)

这些是测试/留出集上的指标。这就是为什么您只看到一行，而不是原始输出中的 12 行。当您禁用 `cross_validation` 时，模型仅在整个训练数据集上训练一次，并使用测试/留出集进行评分。

{% hint style="info" %}
**注意：** 此功能仅适用于[分类](../modules.md)和[回归](../modules.md)模块。
{% endhint %}

### 返回训练分数

默认的评分表显示了按折叠在验证集上的性能指标。如果您想要查看按折叠在训练集上的性能指标，以检查过拟合/欠拟合，您可以使用 `return_train_score` 参数。

```
# 加载数据集
from pycaret.datasets import get_data 
diabetes = get_data('diabetes') 

# 初始化设置
from pycaret.classification import * 
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练模型无交叉验证
lr = create_model('lr', return_train_score = True)
```

![createmodel('lr', return_train_score = True) 的输出](<../../.gitbook/assets/image (379).png>)

### 设置概率阈值

在执行二元分类时，您可以更改硬标签的概率阈值或截断值。默认情况下，所有分类器都使用 `0.5` 作为默认阈值。

```
# 加载数据集
from pycaret.datasets import get_data 
diabetes = get_data('diabetes') 

# 初始化设置
from pycaret.classification import * 
clf1 = setup(data = diabetes, target = 'Class variable')

# 使用 0.25 阈值训练模型
lr = create_model('lr', probability_threshold = 0.25)
```

![create_model('lr', probability_threshold = 0.25) 的输出](<../../.gitbook/assets/image (172).png>)

```
# 查看模型
print(lr)
```

![print(lr) 的输出](<../../.gitbook/assets/image (407).png>)

### 循环训练模型

您可以在循环中使用 `create_model` 函数来训练多个模型，甚至是具有不同配置的相同模型，并比较它们的结果。

```
# 加载数据集
from pycaret.datasets import get_data 
diabetes = get_data('diabetes') 

# 初始化设置
from pycaret.classification import * 
clf1 = setup(data = diabetes, target = 'Class variable')

# 在循环中训练模型
lgbs  = [create_model('lightgbm', learning_rate = i) for i in np.arange(0,1,0.1)]
```

![](<../../.gitbook/assets/image (476) (1).png>)

```
type(lgbs)
>>> list

len(lgbs)
>>> 9
```

如果您想要跟踪指标，就像大多数情况下一样，这就是您可以做到的方式。
```markdown
# 载入数据集
从 pycaret.datasets 中导入 get_data 
diabetes = get_data('diabetes') 

# 初始化设置
从 pycaret.classification 中导入 * 
clf1 = setup(data = diabetes, target = 'Class variable')

# 开始循环
models = []
results = []

for i in np.arange(0.1,1,0.1):
    model = create_model('lightgbm', learning_rate = i)
    model_results = pull().loc[['Mean']]
    models.append(model)
    results.append(model_results)
    
results = pd.concat(results, axis=0)
results.index = np.arange(0.1,1,0.1)
results.plot()
```
![results.plot() 的输出结果](<../../.gitbook/assets/image (46).png>)

### 训练自定义模型

您可以使用自己的自定义模型进行训练，或者使用其他不属于 pycaret 的库中的模型。只要它们的 API 与 `sklearn` 一致，就可以轻松使用。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=diabetes, target='Class variable')

# 导入自定义模型
from gplearn.genetic import SymbolicClassifier
sc = SymbolicClassifier()

# 训练自定义模型
sc_trained = create_model(sc)
```

![create_model(sc) 的输出结果](<../../.gitbook/assets/image (300).png>)

```
type(sc_trained)
>>> gplearn.genetic.SymbolicClassifier

print(sc_trained)
```

![print(sc_trained) 的输出结果](<../../.gitbook/assets/image (426).png>)

### 编写自己的模型

您还可以编写自己的类，其中包含 `fit` 和 `predict` 函数。PyCaret 与此兼容。以下是一个简单的示例：

```
# 加载数据集
from pycaret.datasets import get_data
insurance = get_data('insurance')

# 初始化设置
from pycaret.regression import *
reg1 = setup(data=insurance, target='charges')

# 创建自定义估计器
import numpy as np
from sklearn.base import BaseEstimator
class MyOwnModel(BaseEstimator):
    
    def __init__(self):
        self.mean = 0
        
    def fit(self, X, y):
        self.mean = y.mean()
        return self
    
    def predict(self, X):
        return np.array(X.shape[0]*[self.mean])
        
# 创建一个实例
my_own_model = MyOwnModel()

# 训练模型
my_model_trained = create_model(my_own_model)
```

![create_model(my_own_model) 的输出结果](<../../.gitbook/assets/image (39).png>)