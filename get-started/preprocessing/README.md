---
description: >-
  本页面列出了设置中的所有数据预处理和转换参数
---

# ⚙ 预处理

## 选择标签 :point\_down:

{% tabs %}
{% tab title="数据准备" %}
#### [缺失值](data-preparation.md#missing-values)

由于各种原因，数据集中可能存在缺失值或空记录，通常编码为空白或 `NaN`。大多数机器学习算法无法处理缺失值。[了解更多。](data-preparation.md#missing-values)



#### [数据类型](data-preparation.md#data-types)

数据集中的每个特征都有一个关联的数据类型，如数值、分类或日期时间。PyCaret会自动检测每个特征的数据类型。[了解更多。](data-preparation.md#data-types)



#### [独热编码](data-preparation.md#one-hot-encoding)

数据集中的分类特征包含标签值（有序或无序），而不是连续数字。大多数机器学习算法无法处理未经编码的分类数据。[了解更多。](data-preparation.md#one-hot-encoding)



#### [序数编码](data-preparation.md#ordinal-encoding)

当数据集中的分类特征包含具有固有自然顺序的变量，例如“低、中、高”时，必须对其进行与无序变量（例如男性或女性没有固有顺序的情况）不同的编码。[了解更多。](data-preparation.md#ordinal-encoding)



#### [基数编码](data-preparation.md#cardinal-encoding)

当数据集中的分类特征包含具有许多级别的变量（也称为高基数特征）时，典型的独热编码会导致创建大量新特征。[了解更多。](data-preparation.md#cardinal-encoding)



#### [处理未知级别](data-preparation.md#handle-unknown-levels)

当数据集中的分类特征在预测时包含未见过的变量时，可能会导致训练模型出现问题，因为这些级别在训练时不存在。[了解更多。](data-preparation.md#handle-unknown-levels)



#### [目标不平衡](data-preparation.md#target-imbalance)

当训练数据集中目标类别的分布不均匀时，可以使用设置中的 `fix_imbalance` 参数来解决。[了解更多。](data-preparation.md#target-imbalance)



#### [移除异常值](data-preparation.md#remove-outliers)

PyCaret 中的 `remove_outliers` 函数允许您在训练模型之前识别并移除数据集中的异常值。[了解更多。](data-preparation.md#remove-outliers)
{% endtab %}

{% tab title="缩放和转换" %}
#### [归一化](scale-and-transform.md#normalize)

归一化是机器学习数据准备的常用技术之一。归一化的目标是重新缩放数据集中数值列的值，而不扭曲值范围的差异。[了解更多。](scale-and-transform.md#normalize)



#### [特征转换](scale-and-transform.md#feature-transform)

虽然归一化会将数据重新缩放到新的限制范围以减少方差中的数量影响，但特征转换是一种更激进的技术。转换会改变分布的形状。[了解更多。](scale-and-transform.md#feature-transform)



#### [目标变换](scale-and-transform.md#target-transform)

目标变换类似于特征变换，它将改变目标变量的分布形状，而不是特征。[了解更多。](scale-and-transform.md#target-transform)
{% endtab %}

{% tab title="特征工程" %}
#### [特征交互](feature-engineering.md#feature-interaction)

在机器学习实验中经常看到，通过算术运算组合的两个特征在解释数据中的差异方面比单独使用这两个特征更重要。[了解更多。](feature-engineering.md#feature-interaction)



#### [多项式特征](feature-engineering.md#polynomial-features)

在机器学习实验中，依赖变量和独立变量之间的关系通常被假定为线性，但并非总是如此。有时，依赖变量和独立变量之间的关系更为复杂。[了解更多。](feature-engineering.md#polynomial-features)



#### [组特征](feature-engineering.md#group-features)

当数据集包含某种方式相关的特征时，例如在某些固定时间间隔记录的特征，那么可以为这些特征组创建新的统计特征，如均值、中位数、方差和标准差。[了解更多。](feature-engineering.md#group-features)



#### [数值特征分箱](feature-engineering.md#bin-numeric-features)
```
特征分箱是一种将连续变量转化为分类值的方法，使用预定义的箱数。当连续特征具有过多的唯一值或者在预期范围之外存在极端值时，这种方法非常有效。[了解更多](feature-engineering.md#bin-numeric-features)

#### [合并稀有级别](feature-engineering.md#combine-rare-levels)

有时候数据集中的分类特征（或多个分类特征）具有非常多的级别（即高基数特征）。如果将这样的特征（或特征）编码为数值，那么结果矩阵就是一个稀疏矩阵。[了解更多](feature-engineering.md#combine-rare-levels)

#### [创建聚类](feature-engineering.md#create-clusters)

使用数据中的现有特征创建聚类是一种无监督机器学习技术，用于构建和创建新特征。[了解更多](feature-engineering.md#create-clusters)

#### [特征选择](feature-selection.md#feature-selection)

特征选择是一种用于选择数据集中对目标变量预测最有贡献的特征的过程。使用选定的特征而不是所有特征可以减少过拟合的风险，提高准确性并减少训练时间。[了解更多](feature-selection.md#feature-selection)

#### [去除多重共线性](feature-selection.md#remove-multicollinearity)

多重共线性（也称为共线性）是指数据集中的一个特征变量与同一数据集中的另一个特征变量高度线性相关的现象。[了解更多](feature-selection.md#remove-multicollinearity)

#### [主成分分析](feature-selection.md#principal-component-analysis)

主成分分析（PCA）是一种无监督技术，用于降低数据的维度。它通过压缩特征空间来实现这一目的。[了解更多](feature-selection.md#principal-component-analysis)

#### [忽略低方差](feature-selection.md#ignore-low-variance)

有时候数据集中可能存在一个具有多个级别的分类特征，这些级别的分布是倾斜的，其中一个级别可能主导其他级别。[了解更多](feature-selection.md#ignore-low-variance)

#### [必需参数](other-setup-parameters.md#mandatory-parameters)

在设置函数中，只有两个非可选参数，即数据和目标变量的名称。[了解更多](other-setup-parameters.md#mandatory-parameters)

#### [实验记录](other-setup-parameters.md#experiment-logging)

PyCaret使用MLflow进行实验跟踪。设置中的一个参数可以自动跟踪所有指标、超参数和其他模型工件。[了解更多](other-setup-parameters.md#experiment-logging)

#### [模型选择](other-setup-parameters.md#model-selection)

设置中的参数可用于设置模型选择过程的参数。这些参数与数据预处理无关，但可以影响模型选择过程。[了解更多](other-setup-parameters.md#model-selection)

#### [其他杂项](other-setup-parameters.md#other-miscellaneous)

设置中的其他杂项参数用于控制实验设置，例如使用GPU进行训练或设置实验的详细程度。[了解更多](other-setup-parameters.md#other-miscellaneous)