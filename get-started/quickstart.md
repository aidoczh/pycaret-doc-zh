---
description: PyCaret快速入门指南
---

# 🚀 快速开始

{% hint style="info" %}
**PyCaret 3.0-rc 现已推出**。运行 `pip install --pre pycaret` 进行尝试。查看这个示例 [Notebook](https://colab.research.google.com/drive/1\_H0sHYhzKGZDmgzrQLosuZAR3nOaL6CN?usp=sharing)。
{% endhint %}

## 简介

选择您的用例：

- [分类](quickstart.md#classification)
- [回归](quickstart.md#regression)
- [聚类](quickstart.md#clustering)
- [异常检测](quickstart.md#anomaly-detection)
- [自然语言处理](quickstart.md#natural-language-processing)
- [关联规则挖掘](quickstart.md#association-rules-mining)
- [时间序列（测试版）](quickstart.md#time-series)

## 分类

PyCaret的**分类模块**是一个监督式机器学习模块，用于将元素分类到不同的组中。其目标是预测分类的类别**标签**，这些标签是离散且无序的。一些常见的用例包括预测客户违约（是或否）、预测客户流失（客户会离开还是留下）、检测到的疾病（阳性或阴性）。该模块可用于**二元**或**多类**问题。它提供了多种[预处理](preprocessing/)功能，通过[setup](functions/#setting-up-environment)函数为建模准备数据。它拥有超过18种现成的算法和[多个图表](functions/#plot-model)来分析训练模型的性能。&#x20;

### 设置

此函数初始化训练环境并创建转换流水线。在执行任何其他函数之前，必须先调用设置函数。它有两个必填参数：`data` 和 `target`。所有其他参数都是可选的。

```python
from pycaret.datasets import get_data
data = get_data('diabetes')
```

![](<../.gitbook/assets/image (494).png>)

```python
from pycaret.classification import *
s = setup(data, target = 'Class variable')
```

![](<../.gitbook/assets/image (530).png>)

当执行`setup`时，PyCaret的推理算法将根据某些属性自动推断所有特征的数据类型。数据类型应该被正确推断，但这并不总是情况。为了处理这个问题，PyCaret会显示一个提示，询问数据类型是否正确，一旦您执行了`setup`。如果所有数据类型都正确，您可以按回车键，或者输入`quit`退出设置。

在PyCaret中确保数据类型正确非常重要，因为它会自动执行多个特定于类型的预处理任务，这对机器学习模型至关重要。

或者，您也可以在`setup`中使用`numeric_features`和`categorical_features`参数预定义数据类型。

![为显示而截断输出](<../.gitbook/assets/image (288).png>)

### 比较模型

此函数使用交叉验证训练和评估模型库中所有可用的估计器的性能。此函数的输出是一个带有平均交叉验证分数的评分表。可以使用`get_metrics`函数访问CV期间评估的指标。可以使用`add_metric`和`remove_metric`函数添加或删除自定义指标。

```python
best = compare_models()
```

![](<../.gitbook/assets/image (208).png>)

```python
print(best)
```

![](<../.gitbook/assets/image (239).png>)

### 分析模型

此函数分析在测试集上训练模型的性能。在某些情况下，可能需要重新训练模型。

```python
evaluate_model(best)
```

![](<../.gitbook/assets/image (430).png>)

`evaluate_model`只能在Notebook中使用，因为它使用了`ipywidget`。您也可以使用`plot_model`函数单独生成图表。

```python
plot_model(best, plot = 'auc')
```

![](<../.gitbook/assets/image (80).png>)

```python
plot_model(best, plot = 'confusion_matrix')
```

![](<../.gitbook/assets/image (61).png>)

### 预测

此函数使用训练好的模型预测`Label`和`Score`（预测类别的概率）。当`data`为None时，它会在测试集上（在`setup`函数中创建）预测标签和分数。

```python
predict_model(best)
```

![](<../.gitbook/assets/image (263).png>)

评估指标是在测试集上计算的。第二个输出是包含测试集上预测的`pd.DataFrame`（请参阅最后两列）。要在未见过的（新的）数据集上生成标签，只需将数据集传递给`predict_model`函数

```python
predictions = predict_model(best, data=data)
predictions.head()
```

![](<../.gitbook/assets/image (528).png>)

{% hint style="info" %}
`Score`表示预测类别的概率（而不是正类）。如果标签为0且分数为0.90，则表示类别0的概率为90%。如果要查看两个类别的概率，只需在`predict_model`函数中传递`raw_score=True`。
{% endhint %}

```python
predictions = predict_model(best, data=data, raw_score=True)
predictions.head()
```
![](<../.gitbook/assets/image (36).png>)

### 保存模型

```
save_model(best, 'my_best_pipeline')
```

![](<../.gitbook/assets/image (201) (1).png>)

#### 将模型加载回环境：

```
loaded_model = load_model('my_best_pipeline')
print(loaded_model)
```

![](<../.gitbook/assets/image (521).png>)

## 回归

PyCaret的**回归模块**是一个监督式机器学习模块，用于估计**因变量**（通常称为“结果变量”或“目标”）与一个或多个**自变量**（通常称为“特征”、“预测变量”或“协变量”）之间的关系。回归的目标是预测连续值，比如预测销售额、预测数量、预测温度等。它提供了几种[预处理](preprocessing/)功能，通过[setup](functions/#setting-up-environment)函数为建模准备数据。它拥有超过25种可供使用的算法和[多个图表](functions/#plot-model)来分析训练模型的性能。&#x20;

### 设置

此函数初始化训练环境并创建转换流水线。在执行任何其他函数之前，必须调用设置函数。它有两个必填参数：`data` 和 `target`。所有其他参数都是可选的。

```
from pycaret.datasets import get_data
data = get_data('insurance')
```

![](<../.gitbook/assets/image (121).png>)

```
from pycaret.regression import *
s = setup(data, target = 'charges')
```

![](<../.gitbook/assets/image (130).png>)

当执行`setup`时，PyCaret的推断算法将根据某些属性自动推断所有特征的数据类型。数据类型应该被正确推断，但这并不总是如此。为了处理这个问题，PyCaret会显示一个提示，询问数据类型是否正确，一旦您执行`setup`。如果所有数据类型都正确，您可以按回车键，或者输入`quit`退出设置。

在PyCaret中确保数据类型正确非常重要，因为它会自动执行多个特定于类型的预处理任务，这对于机器学习模型至关重要。

或者，您也可以在`setup`中使用`numeric_features`和`categorical_features`参数预定义数据类型。

![为了显示而截断的输出](<../.gitbook/assets/image (475).png>)

### 比较模型

此函数使用交叉验证训练和评估模型库中所有可用估计器的性能。此函数的输出是一个带有平均交叉验证分数的得分表。可以使用`get_metrics`函数访问CV期间评估的指标。可以使用`add_metric`和`remove_metric`函数添加或删除自定义指标。

```
best = compare_models()
```

![](<../.gitbook/assets/image (375).png>)

```
print(best)
```

![](<../.gitbook/assets/image (412).png>)

### 分析模型

此函数分析训练模型在测试集上的性能。在某些情况下，可能需要重新训练模型。

```
evaluate_model(best)
```

![](<../.gitbook/assets/image (91).png>)

`evaluate_model`只能在笔记本中使用，因为它使用`ipywidget`。您也可以使用`plot_model`函数单独生成图表。

```
plot_model(best, plot = 'residuals')
```

![](<../.gitbook/assets/image (453).png>)

```
plot_model(best, plot = 'feature')
```

![](<../.gitbook/assets/image (18).png>)

### 预测

此函数使用训练好的模型预测`Label`。当`data`为None时，它会在测试集上（在`setup`函数期间创建）预测标签和分数。

```
predict_model(best)
```

![](<../.gitbook/assets/image (465).png>)

评估指标是在测试集上计算的。第二个输出是带有测试集上预测的`pd.DataFrame`（请参阅最后两列）。要在未见过的（新的）数据集上生成标签，只需将数据集传递给`predict_model`函数。

```
predictions = predict_model(best, data=data)
predictions.head()
```

![](<../.gitbook/assets/image (143).png>)

### 保存模型

```
save_model(best, 'my_best_pipeline')
```

![](<../.gitbook/assets/image (171).png>)

#### 将模型加载回环境：

```
loaded_model = load_model('my_best_pipeline')
print(loaded_model)
```

![](<../.gitbook/assets/image (70).png>)

## 聚类

PyCaret的**聚类模块**是一个无监督机器学习模块，执行将一组对象分组的任务，使得同一组中的对象（也称为**簇**）彼此之间比与其他组中的对象更相似。它提供了几种[预处理](preprocessing/)功能，通过[setup](functions/#setting-up-environment)函数为建模准备数据。它拥有超过10种可供使用的算法和[多个图表](functions/#plot-model)来分析训练模型的性能。&#x20;

### 设置
这个函数初始化训练环境并创建转换流水线。在执行任何其他函数之前，必须调用设置函数。它接受一个必填参数：`data`。所有其他参数都是可选的。

```
from pycaret.datasets import get_data
data = get_data('jewellery')
```

![](<../.gitbook/assets/image (322).png>)

```
from pycaret.clustering import *
s = setup(data, normalize = True)
```

![](<../.gitbook/assets/image (13).png>)

当执行`setup`时，PyCaret的推断算法将根据某些属性自动推断所有特征的数据类型。数据类型应该被正确推断，但这并不总是如此。为了处理这个问题，一旦执行`setup`，PyCaret会显示一个提示，询问数据类型是否正确。您可以按回车键如果所有数据类型都正确，或者输入`quit`退出设置。

在PyCaret中确保数据类型正确非常重要，因为它会自动执行多个特定于类型的预处理任务，这对机器学习模型至关重要。

或者，您也可以在`setup`中使用`numeric_features`和`categorical_features`参数预定义数据类型。

![Output truncated for display](<../.gitbook/assets/image (9).png>)

### 创建模型

这个函数训练并评估给定模型的性能。可以使用`get_metrics`函数访问评估的指标。可以使用`add_metric`和`remove_metric`函数添加或删除自定义指标。可以使用`models`函数访问所有可用的模型。

```
kmeans = create_model('kmeans')
```

![](<../.gitbook/assets/image (113).png>)

```
print(kmeans)
```

![](<../.gitbook/assets/image (108).png>)

### 分析模型

这个函数分析训练模型的性能。

```
evaluate_model(kmeans)
```

![](<../.gitbook/assets/image (173).png>)

`evaluate_model`只能在笔记本中使用，因为它使用`ipywidget`。您也可以使用`plot_model`函数单独生成图表。

```
plot_model(kmeans, plot = 'elbow')
```

![](<../.gitbook/assets/image (428).png>)

```
plot_model(kmeans, plot = 'silhouette')
```

![](<../.gitbook/assets/image (206).png>)

### 分配模型

这个函数给训练数据分配聚类标签，给定一个训练好的模型。

```
result = assign_model(kmeans)
result.head()
```

![](<../.gitbook/assets/image (531).png>)

### 预测

这个函数使用训练好的模型在新的/未见过的数据集上生成聚类标签。

```
predictions = predict_model(kmeans, data = data)
predictions.head()
```

![](<../.gitbook/assets/image (319) (1).png>)

### 保存模型

```
save_model(kmeans, 'kmeans_pipeline')
```

![](<../.gitbook/assets/image (516).png>)

#### 要将模型加载回环境中：

```
loaded_model = load_model('kmeans_pipeline')
print(loaded_model)
```

![](<../.gitbook/assets/image (491).png>)

## 异常检测

PyCaret的**异常检测**模块是一个无监督机器学习模块，用于识别与大多数数据明显不同而引起怀疑的**罕见项目**、**事件**或**观察**。通常，异常项目会导致一些问题，如银行欺诈、结构缺陷、医疗问题或错误。它提供了几个[预处理](preprocessing/)功能，通过[设置](functions/#setting-up-environment)函数为建模准备数据。它拥有超过10种现成可用的算法和[几种图表](functions/#plot-model)来分析训练模型的性能。

### 设置

这个函数初始化训练环境并创建转换流水线。在执行任何其他函数之前，必须调用`setup`函数。它只接受一个必填参数：`data`。所有其他参数都是可选的。

```
from pycaret.datasets import get_data
data = get_data('anomaly')
```

![](<../.gitbook/assets/image (380).png>)

```
from pycaret.anomaly import *
s = setup(data)
```

![](<../.gitbook/assets/image (93) (1).png>)

当执行`setup`时，PyCaret的推断算法将根据某些属性自动推断所有特征的数据类型。数据类型应该被正确推断，但这并不总是如此。为了处理这个问题，一旦执行`setup`，PyCaret会显示一个提示，询问数据类型是否正确。您可以按回车键如果所有数据类型都正确，或者输入`quit`退出设置。

在PyCaret中确保数据类型正确非常重要，因为它会自动执行多个特定于类型的预处理任务，这对机器学习模型至关重要。

或者，您也可以在`setup`中使用`numeric_features`和`categorical_features`参数预定义数据类型。

![Output truncated for display](<../.gitbook/assets/image (232).png>)

### 创建模型
该函数用于训练无监督的异常检测模型。可以使用`models`函数访问所有可用的模型。

```
iforest = create_model('iforest')
print(iforest)
```

![](<../.gitbook/assets/image (20) (1).png>)

```
models()
```

![](<../.gitbook/assets/image (302).png>)

### 分析模型

```
plot_model(iforest, plot = 'tsne')
```

![](<../.gitbook/assets/image (146).png>)

```
plot_model(iforest, plot = 'umap')
```

![](<../.gitbook/assets/image (486).png>)

### 分配模型

该函数为给定模型的数据集分配异常标签。（1 = 异常值，0 = 正常值）。

```
result = assign_model(iforest)
result.head()
```

![](<../.gitbook/assets/image (326) (1).png>)

### 预测

该函数使用训练好的模型在新的/未知的数据集上生成异常标签。

```
predictions = predict_model(iforest, data = data)
predictions.head()
```

![predict\_model(iforest, data = data)的输出](<../.gitbook/assets/image (374).png>)

### 保存模型

```
save_model(iforest, 'iforest_pipeline')
```

![save\_model(iforest, 'iforest\_pipeline')的输出](<../.gitbook/assets/image (59).png>)

要在环境中加载模型：

```
loaded_model = load_model('iforest_pipeline')
print(loaded_model)
```

![load\_model('iforest\_pipeline')的输出](<../.gitbook/assets/image (209).png>)

## 自然语言处理

PyCaret的自然语言处理是一个无监督机器学习模块，用于在文本数据上训练主题模型。有几种技术用于分析文本数据，主题建模是其中之一。主题模型是一种用于发现文档集合中抽象主题的统计模型。

### 设置

该函数初始化训练环境并创建文本转换流水线。在执行任何其他函数之前，必须调用setup函数。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('kiva')
```

![数据集的样本](<../.gitbook/assets/image (461).png>)

```
# 打印第一个文档
print(data['en'][0])
```

![print(data\['en'\]\[0\])的输出](<../.gitbook/assets/image (92).png>)

```
# 初始化设置
from pycaret.nlp import *
s = setup(data, target = 'en')
```

![setup(...)的输出](<../.gitbook/assets/image (370).png>)

### 创建模型

该函数训练一个无监督的主题模型。可以使用`models`函数访问所有可用的模型。

```
models()
```

![models()的输出](<../.gitbook/assets/image (518).png>)

#### 训练模型：

```
lda = create_model('lda')
print(lda)
```

![print(lda)的输出](<../.gitbook/assets/image (458).png>)

### 分析模型

```
plot_model(lda, plot = 'frequency')
```

![plot\_model(...)的输出](<../.gitbook/assets/image (159).png>)

```
plot_model(lda, plot = 'sentiment')
```

![plot\_model(...)的输出](<../.gitbook/assets/image (168).png>)

或者，您也可以使用`evaluate_model`函数。

```
evaluate_model(lda)
```

![evaluate\_model(lda)的输出](<../.gitbook/assets/image (57).png>)

### 分配模型

该函数为给定模型的数据集分配主题标签。

```
lda_results = assign_model(lda)
lda_results.head()
```

![assign\_model(lda)的输出](<../.gitbook/assets/image (310).png>)

### 保存模型

```
save_model(lda, 'my_lda_model')
```

![save\_model(..)的输出](<../.gitbook/assets/image (241).png>)

要在环境中加载模型：

```
loaded_model = load_model('my_lda_model')
```

## 关联规则挖掘

PyCaret的关联规则模块是一个监督机器学习模块，用于发现数据集中变量之间的有趣关系。该模块会自动将任何事务数据库转换为apriori算法可接受的形式。Apriori是一种用于频繁项集挖掘和关联规则学习的算法，适用于关系数据库。

### 设置

```
from pycaret.datasets import get_data
data = get_data('france')
```

![数据集的样本行](<../.gitbook/assets/image (201).png>)

```
from pycaret.arules import * 
arules = setup(data, transaction_id = 'InvoiceNo', item_id = 'Description')
```

![setup(...)的输出](<../.gitbook/assets/image (476).png>)

### 创建模型

```
create_model(metric = 'confidence', threshold = 0.3)
```

![create\_model(...)的输出](<../.gitbook/assets/image (543).png>)

### 分析模型

```
plot_model(model, plot = '3d')
```

![plot\_model(...)的输出](<../.gitbook/assets/image (508).png>)

## 时间序列
PyCaret 的新时间序列模块现已在 `3.0-rc` 版本中推出。保持了 PyCaret 的简单易用特性，与现有 API 保持一致，并且功能齐全。包括统计测试、模型训练和选择（30+ 种算法）、模型分析、自动超参数调整、实验记录、云端部署等功能。所有这些只需几行代码即可完成。

### 设置

此函数初始化训练环境并创建转换流水线。在执行任何其他函数之前，必须先调用 Setup 函数。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('airline')
```

![get\_data('airline') 的输出](<../.gitbook/assets/image (277).png>)

```
from pycaret.time_series import *
s = setup(data, fh = 3, fold = 5, session_id = 123)
```

![setup(...) 的输出](<../.gitbook/assets/image (107).png>)

### 比较模型

此函数使用交叉验证训练和评估模型库中所有可用的估计器的性能。该函数的输出是一个带有平均交叉验证分数的评分表。在 CV 过程中评估的指标可以使用 `get_metrics` 函数访问。可以使用 `add_metric` 和 `remove_metric` 函数添加或删除自定义指标。

```
best = compare_models()
```

![compare\_models() 的输出](<../.gitbook/assets/image (451).png>)

### 分析模型

```
plot_model(best, plot = 'forecast', data_kwargs = {'fh' : 24})
```

![plot\_model(...) 的输出](<../.gitbook/assets/image (176).png>)

```
plot_model(best, plot = 'diagnostics')
```

![plot\_model(best, plot = 'diagnostics') 的输出](<../.gitbook/assets/image (462).png>)

```
plot_model(best, plot = 'insample')
```

![plot\_model(best, plot = 'insample') 的输出](<../.gitbook/assets/image (170).png>)

### 预测

```
# 完成模型
final_best = finalize_model(best)
predict_model(best, fh = 24)
```

![predict\_model(best, fh = 24) 的输出](<../.gitbook/assets/image (207).png>)

### 保存模型

```
save_model(final_best, 'my_final_best_model')
```

![save\_model(...) 的输出](<../.gitbook/assets/image (161).png>)

#### 要将模型加载回环境中：

```
loaded_model = load_model('my_final_best_model')
print(loaded_model)
```

![load\_model(...) 的输出](<../.gitbook/assets/image (25).png>)