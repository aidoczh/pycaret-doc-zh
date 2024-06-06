# 特征工程

### 特征交互

在机器学习实验中，经常会发现通过**算术运算**将两个特征组合起来，比起单独使用这两个特征，更能解释数据中的方差。通过现有特征的交互创建新特征被称为**特征交互**。在 PyCaret 中，可以使用 [setup](https://www.pycaret.org/setup) 中的 `feature_interaction` 和 `feature_ratio` 参数来实现特征交互。特征交互通过将两个变量相乘（a \* b）来创建新特征，而特征比率通过计算现有特征的比率（a / b）来创建新特征。

#### **参数**

* **feature\_interaction: bool, 默认值 = False**\
  当设置为 True 时，它将通过交互（a \* b）为数据集中的所有数值变量创建新特征，包括多项式和三角函数特征（如果已创建）。该特性不可扩展，并且在具有大量特征空间的数据集上可能无法按预期工作。
* **feature\_ratio: bool, 默认值 = False**\
  当设置为 True 时，它将通过计算数据集中所有数值变量的比率（a / b）来创建新特征。该特性不可扩展，并且在具有大量特征空间的数据集上可能无法按预期工作。
* **interaction\_threshold: bool, 默认值 = 0.01**\
  类似于 polynomial\_threshold，它用于通过交互创建的稀疏矩阵进行压缩。基于随机森林、AdaBoost 和线性相关性的组合，其重要性落在定义的阈值百分位数范围内的特征将保留在数据集中。在进一步处理之前，其余特征将被丢弃。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
insurance = get_data('insurance')

# 初始化设置
from pycaret.regression import *
reg1 = setup(data = insurance, target = 'charges', feature_interaction = True, feature_ratio = True)
```

#### **特征交互前**

![特征交互前的数据框视图](<../../.gitbook/assets/image (351).png>)

#### **特征交互后**

![特征交互后的数据框视图](<../../.gitbook/assets/image (76).png>)

### 多项式特征

在机器学习实验中，通常假设因变量和自变量之间的关系是线性的，但并不总是如此。有时，因变量和自变量之间的关系更加复杂。通过创建新的多项式特征，有时可以捕捉到这种关系，否则可能会被忽视。PyCaret 可以使用 [setup](https://www.pycaret.org/setup) 中的 `polynomial_features` 参数从现有特征中创建多项式特征。

#### **参数**

* **polynomial\_features: bool, 默认值 = False**\
  当设置为 True 时，将基于数据集中的所有数值特征中存在的所有多项式组合创建新特征，其度数由 polynomial\_degree 参数定义。
* **polynomial\_degree: int, 默认值 = 2**\
  多项式特征的度数。例如，如果输入样本是二维的，形式为 \[a, b]，则度数为 2 的多项式特征为：\[1, a, b, a^2, ab, b^2]。
* **polynomial\_threshold: float, 默认值 = 0.1**\
  用于压缩多项式和三角函数特征的稀疏矩阵。基于随机森林、AdaBoost 和线性相关性的组合，其特征重要性落在定义的阈值百分位数范围内的多项式和三角函数特征将保留在数据集中。在进一步处理之前，其余特征将被丢弃。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
juice = get_data('juice')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = juice, target = 'Purchase', polynomial_features = True)
```

#### **多项式特征前**

![多项式特征前的数据框视图](<../../.gitbook/assets/image (99).png>)

#### **多项式特征后**

![多项式特征后的数据框视图](<../../.gitbook/assets/image (301).png>)

注意，新特征是从现有特征空间中创建的。要扩展或压缩多项式特征空间，可以使用 `polynomial_threshold` 参数，该参数使用基于随机森林、AdaBoost 和线性相关性的特征重要性来过滤掉不重要的多项式特征。`polynomial_degree` 参数用于定义要考虑的度数。

### 三角函数特征

与[**多项式特征**](https://www.pycaret.org/polynomial-features)类似，PyCaret 还允许从现有特征中创建新的**三角函数特征**。可以使用 [setup](https://www.pycaret.org/setup) 中的 `trigonometry_features` 参数来实现。

**参数**

* **trigonometry\_features: bool, 默认值 = False**\
当设置为True时，将基于数据集中的数值特征中存在的所有三角函数组合创建新特征，其次数由多项式度参数定义。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
insurance = get_data('insurance')

# 初始化设置
from pycaret.regression import *
reg1 = setup(data = insurance, target = 'charges', trigonometry_features = True)
```

#### 变换前

![三角函数特征添加前的数据框视图](<../../.gitbook/assets/image (250).png>)

#### 变换后

![三角函数特征添加后的数据框视图](<../../.gitbook/assets/image (1).png>)

### 分组特征

当数据集包含某种方式相关的特征时，例如：以一定时间间隔记录的特征，那么可以使用`group_features`参数在[setup](https://www.pycaret.org/setup)中从现有特征中创建新的统计特征，如**均值**、**中位数**、**方差**和**标准差**。

#### **参数**

* **group\_features: list or list of list, default = None**\
  当数据集包含具有相关特征的特征时，可以使用group\_features参数进行统计特征提取。例如，如果数据集具有彼此相关的数值特征（即'Col1'、'Col2'、'Col3'），则可以传递包含列名的列表到group\_features下，以提取均值、中位数、众数和标准差等统计信息。
* **group\_names: list, default = None**\
  当传递group\_features时，可以将组名作为包含字符串的列表传递到group\_names参数中。group\_names列表的长度必须等于group\_features的长度。当长度不匹配或未传递名称时，新特征将按顺序命名，如group\_1、group\_2等。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
credit = get_data('credit')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = credit, target = 'default', group_features = ['BILL_AMT1', 'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6'])
```

#### **变换前**

![分组特征添加前的数据框视图](<../../.gitbook/assets/image (343).png>)

#### **变换后**

![分组特征添加后的数据框视图](<../../.gitbook/assets/image (508) (1).png>)

### 数值特征分箱

特征分箱是一种将连续变量转换为分类值的方法，使用预定义数量的**箱子**。当连续特征具有太多唯一值或在预期范围之外有少量极端值时，这种方法非常有效。这些极端值会影响训练模型，从而影响模型的预测准确性。在PyCaret中，可以使用`bin_numeric_features`参数在[setup](https://www.pycaret.org/setup)中将连续数值特征分箱为区间。PyCaret使用_'sturges'_规则确定箱子数量，并使用K-Means聚类将连续数值特征转换为分类特征。

#### **参数**

* **bin\_numeric\_features: list, default = None**\
  当传递数值特征列表时，它们将使用K-Means转换为分类特征，其中每个箱中的值具有相同的1D k-means聚类中心。箱的数量基于_'sturges'_方法确定。这仅适用于高斯数据，并且低估了大型非高斯数据集的箱数。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
income = get_data('income')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = income, target = 'income >50K', bin_numeric_features = ['age'])
```

#### 变换前

![数值特征分箱前的数据框视图](<../../.gitbook/assets/image (160).png>)

#### 变换后

![数值特征分箱后的数据框视图](<../../.gitbook/assets/image (456).png>)

### 合并稀有水平

有时数据集可能具有具有非常多水平（即高基数特征）的分类特征（或多个分类特征）。如果将这种特征（或特征）编码为数值值，则结果矩阵是**稀疏矩阵**。这不仅会使实验变慢，因为特征数量和数据集大小大幅增加，还会引入实验中的噪音。可以通过在PyCaret中使用`combine_rare_levels`参数在[setup](https://www.pycaret.org/setup)中合并具有高基数的特征（或特征）中的稀有水平来避免稀疏矩阵。

#### **参数**

* **combine\_rare\_levels: bool, default = False**
当设置为True时，分类特征中低于rare_level_threshold参数定义的阈值的所有级别将合并为一个级别。在此情况下，阈值下必须至少有两个级别才能生效。rare_level_threshold表示级别频率的百分位分布。通常，这种技术被应用于限制由于分类特征中级别数量过多而导致的稀疏矩阵。

* **rare_level_threshold: float, 默认值 = 0.1**\
  低于此百分位分布的稀有类别将被合并。仅在combine_rare_levels设置为True时生效。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
income = get_data('income')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = income, target = 'income >50K', combine_rare_levels = True)
```

#### **合并稀有级别之前**

![合并稀有级别之前的数据框视图](<../../.gitbook/assets/image (438).png>)

#### 合并稀有级别之后

![合并稀有级别之后的数据框视图](<../../.gitbook/assets/image (524) (1).png>)

#### 合并稀有级别的效果

![](<../../.gitbook/assets/image (417).png>)

### 创建聚类

使用数据中的现有特征来创建聚类是一种无监督机器学习技术，用于工程化和创建新特征。它使用迭代方法使用Calinski-Harabasz和Silhouette准则的组合来确定聚类的数量。将具有原始特征的每个数据点分配给一个聚类。然后，将分配的聚类标签用作预测目标变量的**新特征**。在PyCaret中，可以使用[setup](https://www.pycaret.org/setup)中的`create_clusters`参数来实现这一点。

#### 参数

* **create_clusters: bool, 默认值 = False**\
  当设置为True时，将创建一个额外的特征，其中每个实例被分配到一个聚类中。聚类的数量是使用Calinski-Harabasz和Silhouette准则的组合来确定的。
* **cluster_iter: int, 默认值 = 20**\
  用于创建聚类的迭代次数。每次迭代表示聚类大小。仅在create_clusters参数设置为True时生效。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
insurance = get_data('insurance')

# 初始化设置
from pycaret.regression import *
reg1 = setup(data = insurance, target = 'charges', create_clusters = True)
```

#### **创建聚类之前**

![创建聚类之前的数据框视图](<../../.gitbook/assets/image (257).png>)

#### **创建聚类之后**

![创建聚类之后的数据框视图](<../../.gitbook/assets/image (75).png>)