# 特征选择

### 特征选择

**特征重要性**是一种用于选择数据集中对预测目标变量最有贡献的特征的过程。使用选定的特征而不是所有特征可以降低过拟合的风险，提高准确性并减少训练时间。在 PyCaret 中，可以使用 `feature_selection` 参数来实现这一目标。它使用多种监督特征选择技术的组合来选择最重要的特征子集进行建模。可以使用 [setup](https://www.pycaret.org/setup) 中的 `feature_selection_threshold` 参数来控制子集的大小。

#### **参数**

* **feature\_selection: bool, 默认值 = False**\
  当设置为 True 时，使用包括随机森林、Adaboost 和与目标变量的线性相关性在内的多种排列重要性技术选择特征子集。子集的大小取决于 feature\_selection\_param。通常，这用于限制特征空间以提高建模效率。当使用 polynomial\_features 和 feature\_interaction 时，强烈建议使用较低的 feature\_selection\_threshold 值。
* **feature\_selection\_threshold: float, 默认值 = 0.8**\
  用于特征选择（包括新创建的多项式特征）的阈值。较高的值将导致较大的特征空间。建议在使用 polynomial\_features 和 feature\_interaction 的情况下，尤其是在多次试验中使用不同的 feature\_selection\_threshold 值。设置一个非常低的值可能是高效的，但可能导致欠拟合。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.regression import *
clf1 = setup(data = diabetes, target = 'Class variable', feature_selection = True)
```

#### **特征选择前**

![特征选择前的数据框](<../../.gitbook/assets/image (312).png>)

#### **特征选择后**

![特征选择后的数据框](<../../.gitbook/assets/image (388).png>)

### 去除多重共线性

**多重共线性**（也称为 _共线性_）是指数据集中的一个特征变量与同一数据集中的另一个特征变量高度线性相关的现象。多重共线性会增加系数的方差，使其对线性模型不稳定且嘈杂。解决多重共线性的一种方法是删除两个高度相关的特征中的一个。在 PyCaret 中，可以使用 [setup](https://www.pycaret.org/setup) 中的 `remove_multicollinearity` 参数来实现这一目标。

#### 参数

* **remove\_multicollinearity: bool, 默认值 = False**\
  当设置为 True 时，将删除与 multicollinearity\_threshold 参数定义的阈值以上的互相关变量。当两个特征彼此高度相关时，将删除与目标变量相关性较低的特征。
* **multicollinearity\_threshold: float, 默认值 = 0.9**\
  用于删除相关特征的阈值。仅在 remove\_multicollinearity 设置为 True 时生效。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
concrete = get_data('concrete')

# 初始化设置
from pycaret.regression import *
reg1 = setup(data = concrete, target = 'strength', remove_multicollinearity = True, multicollinearity_threshold = 0.6)
```

#### **去除多重共线性前**

![去除多重共线性前的数据框](<../../.gitbook/assets/image (387).png>)

#### **去除多重共线性后**

![去除多重共线性后的数据框](<../../.gitbook/assets/image (168) (1).png>)

### 主成分分析

**主成分分析（PCA）**是一种无监督技术，用于在机器学习中降低数据的维度。它通过识别捕获完整特征矩阵中大部分信息的子空间来压缩特征空间。它将原始特征空间投影到较低的维度。在 PyCaret 中，可以使用 [setup](https://www.pycaret.org/setup) 中的 `pca` 参数来实现这一目标。

**参数**

* **pca: bool, 默认值 = False**\
  当设置为 True 时，将应用降维以使用 pca\_method 参数中定义的方法将数据投影到较低的维度空间。在监督学习中，当处理高维特征空间且内存受限时，通常执行 PCA。请注意，并非所有数据集都可以使用线性 PCA 技术有效地进行分解，应用 PCA 可能会导致信息丢失。因此，建议使用不同的 pca\_methods 运行多个实验以评估影响。
* **pca\_method: string, 默认值 = ‘linear’**\
‘线性’方法使用奇异值分解进行线性降维。其他可用的选项包括：
* **kernel：** 使用RVF核进行降维。
* **incremental：** 当要分解的数据集太大无法放入内存时，用于替代‘线性’pca的方法。
* **pca_components：int/float，默认值=0.99**\
  要保留的主成分数量。如果pca_components是浮点数，则视为信息保留的目标百分比。当pca_components是整数时，视为要保留的特征数量。pca_components必须严格小于数据集中原始特征的数量。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
income = get_data('income')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=income, target='income >50K', pca=True, pca_components=10)
```

#### **降维前**

![降维前的数据框视图](<../../.gitbook/assets/image (493).png>)

#### **降维后**

![降维后的数据框视图](<../../.gitbook/assets/image (472).png>)

### 忽略低方差

有时，数据集可能具有具有多个级别的**分类特征**，其中这些级别的分布是倾斜的，并且一个级别可能主导其他级别。这意味着该特征提供的信息变化不大。对于机器学习模型来说，这样的特征可能不会添加很多信息，因此可以在建模过程中忽略它们。在PyCaret中，可以使用[setup](https://www.pycaret.org/setup)中的_ignore_low_variance_参数来实现。要满足以下两个条件，才能将特征视为低方差特征。

特征中唯一值的数量 / 样本大小 < 10%

最常见值的数量 / 第二常见值的数量 > 20倍。

#### **参数**

* **ignore_low_variance：bool，默认值=False**\
  当设置为True时，将从数据集中删除所有具有统计上不显著方差的分类特征。方差是使用唯一值与样本数的比率以及最常见值与第二最常见值的频率之比来计算的。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
mice = get_data('mice')

# 过滤数据集
mice = mice[mice['Genotype']] = 'Control'

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=mice, target='class', ignore_low_variance=True)
```

#### **忽略低方差前**

![忽略低方差前的数据框视图](<../../.gitbook/assets/image (242).png>)

#### **忽略低方差后**

![忽略低方差后的数据框视图](<../../.gitbook/assets/image (28).png>)