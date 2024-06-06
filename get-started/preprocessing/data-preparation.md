# 数据准备

### **缺失值**

由于各种原因，数据集中可能存在缺失值或空记录，通常编码为空白或 `NaN`。大多数机器学习算法无法处理缺失或空白值。删除具有缺失值的样本是一种基本策略，有时会导致丢失可能有价值的数据以及相关信息或模式。更好的策略是对缺失值进行填充。PyCaret 默认通过 `mean`（对于数值特征）和 `constant`（对于分类特征）来填充数据集中的缺失值。要更改填充方法，可以在设置中使用 `numeric_imputation` 和 `categorical_imputation` 参数。

#### **参数**

* **imputation\_type: string, default = 'simple'**\
  使用的填充类型。可以是 `simple` 或 `iterative`。
* **numeric\_imputation: string, default = ‘mean’**\
  数值特征中的缺失值将使用训练数据集中该特征的 `mean` 值进行填充。其他可用选项包括 `median` 或 `zero`。
* **categorical\_imputation: string, default = ‘constant’**\
  分类特征中的缺失值将使用常量 `not_available` 值进行填充。其他可用选项包括 `mode`。
* **iterative\_imputation\_iters: int = 5**\
  迭代次数。当 `imputation_type` 不是 `iterative` 时忽略。
* **numeric\_iterative\_imputer: Union\[str, Any] = 'lightgbm'**\
  用于数值特征中缺失值的迭代填充估算器。当 `imputation_type` 设置为 `simple` 时忽略。
* **categorical\_iterative\_imputer: Union\[str, Any] = 'lightgbm**'\
  用于分类特征中缺失值的迭代填充估算器。当 `imputation_type` 不是 `iterative` 时忽略。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
hepatitis = get_data('hepatitis')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = hepatitis, target = 'Class')
```

#### **之前**

![](<../../.gitbook/assets/image (483).png>)

#### **之后**

![](<../../.gitbook/assets/image (144) (1).png>)

#### 简单填充器与迭代填充器的比较

![](<../../.gitbook/assets/image (413).png>)

要了解更多关于这个实验的信息，您可以阅读[这篇文章](https://www.linkedin.com/pulse/iterative-imputation-pycaret-22-antoni-baum/)。

{% hint style="info" %}
**注意：** 在 `setup` 函数中不需要显式参数来填充缺失值，因为 PyCaret 默认处理此任务。
{% endhint %}

### 数据类型

数据集中的每个特征都有一个关联的数据类型，如数值、分类或日期时间。PyCaret 的推断算法会自动检测每个特征的数据类型。然而，有时 PyCaret 推断的数据类型是错误的。确保数据类型正确很重要，因为几个下游过程依赖于特征的数据类型。一个例子是数据集中数值和分类特征的[缺失值](data-preparation.md#missing-values)会有不同的填充方式。要覆盖推断的数据类型，可以在设置函数中使用 `numeric_features`、`categorical_features` 和 `date_features` 参数。您还可以使用 `ignore_features` 来忽略某些特征进行模型训练。

**参数**

* **numeric\_features: list of string, default = None**\
  如果推断的数据类型不正确，可以使用 `numeric_features` 来覆盖推断的数据类型。
* **categorical\_features: list of string, default = None**\
  如果推断的数据类型不正确，可以使用 `categorical_features` 来覆盖推断的数据类型。
* **date\_features: list of string, default = None**\
  如果数据中有 `Datetime` 列在运行设置时没有自动推断出来，可以使用 `date_features` 强制数据类型。它可以用于多个日期列。与建模不同，Datetime 相关特征不会用于模型训练，而是执行特征提取，并在模型训练期间忽略原始 `Datetime` 列。如果 `Datetime` 列包含时间戳，则还将提取与时间相关的特征。
* **ignore\_features: list of string, default = None**\
  `ignore_features` 可用于在模型训练期间忽略特征。它接受一个字符串列表，其中包含要忽略的列名。

#### **示例 1 - 分类特征**

```
# 加载数据集
from pycaret.datasets import get_data
hepatitis = get_data('hepatitis')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = hepatitis, target = 'Class', categorical_features = ['AGE'])
```

#### **之前**

![](<../../.gitbook/assets/image (353).png>)

#### **之后**

![](<../../.gitbook/assets/image (373).png>)

#### 示例 2 - 忽略特征
```
# 载入数据集
from pycaret.datasets import get_data
pokemon = get_data('pokemon')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=pokemon, target='Legendary', ignore_features=['#', 'Name'])
```
#### **之前**

![](<../../.gitbook/assets/image (294).png>)

#### **之后**

![](<../../.gitbook/assets/image (82).png>)

### 一位编码

数据集中的分类特征包含标签值（有序或无序），而不是连续数字。大多数机器学习算法无法直接处理分类特征，因此在训练模型之前，它们必须转换为数值。最常见的分类编码类型是一位编码（也称为虚拟编码），其中数据集中的每个分类级别都变成一个包含二进制值（1或0）的单独特征。

由于这是执行机器学习实验的必要步骤，PyCaret将使用一位编码转换数据集中的所有分类特征。这对于具有无序分类数据的特征非常理想，即数据无法排序。在其他不同的情况下，必须使用其他编码方法。例如，当数据是有序的，即数据具有内在级别时，必须使用[Ordinal Encoding](data-preparation.md#ordinal-encoding)。一位编码适用于所有被推断为分类或使用`setup`函数中的`categorical_features`强制为分类的特征。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
pokemon = get_data('pokemon')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=pokemon, target='Legendary')
```

#### **之前**

![](<../../.gitbook/assets/image (50).png>)

#### **之后**

![](<../../.gitbook/assets/image (129).png>)

{% hint style="info" %}
注意：在`setup`函数中不需要传递任何额外参数进行一位编码。默认情况下，它应用于所有`categorical_features`，除非您明确定义`ordinal_encoding`或`high_cardinality_features`。
{% endhint %}

### 有序编码

当数据集中的分类特征包含具有内在自然顺序的变量，例如“低、中、高”时，必须对其进行与无序变量（例如男性或女性）不同的编码。可以通过`setup`函数中的`ordinal_features`参数实现这一点，该参数接受一个字典，其中包含特征名称和从最低到最高的顺序递增的级别。

#### **参数**

* **ordinal\_features：字典，默认值=无**\
  当数据包含有序特征时，必须使用`ordinal_features`进行不同编码。如果数据具有值为`low`、`medium`、`high`的分类变量，并且已知`low` < `medium` < `high`，则可以传递为`ordinal_features = {'column_name' : ['low', 'medium', 'high']}`。列表顺序必须从最低到最高递增。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
employee = get_data('employee')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=employee, target='left', ordinal_features={'salary': ['low', 'medium', 'high']})
```

#### **之前**

![](<../../.gitbook/assets/image (395).png>)

#### **之后**

![](<../../.gitbook/assets/image (526).png>)

### 基数编码

当数据集中的分类特征包含许多级别的变量（也称为高基数特征）时，典型的一位编码会导致创建大量新特征，从而使实验变慢。可以使用`setup`中的`high_cardinality_features`处理具有高基数的特征。它支持基数编码的两种方法（1）频率和（2）聚类。这些方法可以在`setup`函数中定义。

#### **参数**

* **high\_cardinality\_features：字符串，默认值=无**\
  当数据包含高基数特征时，可以将其作为具有高基数的列名列表传递，通过在`high_cardinality_method`参数中定义的方法对其进行压缩。
* **high\_cardinality\_method：字符串，默认值=‘frequency’**\
  当方法设置为`frequency`时，它将用特征的频率分布替换特征的原始值，并将特征转换为数值。另一种可用的方法是`clustering`，它对数据的统计属性进行聚类，并用聚类标签替换特征的原始值。聚类数是使用Calinski-Harabasz和Silhouette标准的组合确定的。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
income = get_data('income')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=income, target='income >50K', high_cardinality_features=['native-country'])
```

#### **之前**

![](<../../.gitbook/assets/image (315).png>)

#### **之后**

![](<../../.gitbook/assets/image (247).png>)

### 处理未知级别
当数据集中的分类特征在预测时包含未见过的变量时，可能会给训练模型带来问题，因为这些水平在训练时并不存在。处理这种情况的一种方法是重新分配这些水平。在 PyCaret 中，可以通过设置 `handle_unknown_categorical` 和 `unknown_categorical_method` 参数来实现这一点。

#### **参数**

* **handle\_unknown\_categorical: bool, 默认值 = True**\
  当设置为 `True` 时，未知的分类水平将被替换为训练数据集中学习到的最常见或最不常见的水平。
* **unknown\_categorical\_method: string, 默认值 = ‘least\_frequent’**\
  可以设置为 `least_frequent` 或 `most_frequent`。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
insurance = get_data('insurance')

# 初始化设置
from pycaret.regression import *
reg1 = setup(data = insurance, target = 'charges', handle_unknown_categorical = True, unknown_categorical_method = 'most_frequent')
```

### 目标类别不平衡

当训练数据集中目标类别的分布不均匀时，可以使用设置中的 `fix_imbalance` 参数来解决。当设置为 `True` 时，默认使用 SMOTE（Synthetic Minority Over-sampling Technique）作为重新采样的方法。可以通过设置 `fix_imbalance_method` 来更改重新采样的方法。

#### **参数**

* **fix\_imbalance: bool, 默认值 = False**\
  当设置为 `True` 时，使用在设置中定义的算法对训练数据集进行重新采样。当为 `None` 时，默认使用 SMOTE。
* **fix\_imbalance\_method: obj, 默认值 = None**\
  此参数接受 [imblearn](https://imbalanced-learn.org/stable/) 中支持 `fit_resample` 方法的任何算法。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
credit = get_data('credit')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = credit, target = 'default', fix_imbalance = True)
```

#### SMOTE 前后对比

![](<../../.gitbook/assets/image (547).png>)

### 移除异常值

PyCaret 中的 `remove_outliers` 函数允许您在训练模型之前从数据集中识别和移除异常值。异常值是通过使用奇异值分解技术进行 PCA 线性降维来识别的。可以通过 [setup](https://www.pycaret.org/setup) 中的 `remove_outliers` 参数来实现。通过 `outliers_threshold` 参数控制异常值的比例。

#### **参数**

**remove\_outliers: bool, 默认值 = False**\
当设置为 True 时，使用奇异值分解技术进行 PCA 线性降维来移除训练数据中的异常值。

**outliers\_threshold: float, 默认值 = 0.05**\
可以使用 outliers\_threshold 参数定义数据集中异常值的百分比/比例。默认值为 0.05，这意味着在分布尾部的每一侧的值的 0.025 将从训练数据中删除。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
insurance = get_data('insurance')

# 初始化设置
from pycaret.regression import *
reg1 = setup(data = insurance, target = 'charges', remove_outliers = True)
```

![](<../../.gitbook/assets/image (419).png>)

#### 移除异常值前后对比

![](<../../.gitbook/assets/image (29).png>)