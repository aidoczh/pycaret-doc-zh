# 缩放和转换

### 标准化

标准化是机器学习中常用的数据准备技术之一。标准化的目标是在不扭曲数值列的值范围差异或丢失信息的情况下，重新调整数据集中数值列的值。有几种可用于标准化的方法，默认情况下，PyCaret 使用 `zscore`。

#### **参数**

* **normalize: bool, 默认值 = False**\
  当设置为 `True` 时，使用 `normalized_method` 参数定义的方法对特征空间进行转换。&#x20;
* **normalize\_method: string, 默认值 = ‘zscore’**\
  定义用于标准化的方法。默认情况下，方法设置为 `zscore`。其他可用选项有：
  * **`z-score`** 标准 z 分数计算公式为 z = (x – u) / s
  * **`minmax`** 将每个特征单独缩放和平移，使其在 0 到 1 的范围内。
  * **`maxabs`** 将每个特征单独缩放和平移，使每个特征的最大绝对值为 1.0。它不会移动/居中数据，因此不会破坏任何稀疏性。
  * **`robust`** 根据四分位距缩放和平移每个特征。当数据集包含异常值时，鲁棒缩放器通常会得到更好的结果。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
pokemon = get_data('pokemon')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = pokemon, target = 'Legendary', normalize = True)
```

#### **转换前**

![](<../../.gitbook/assets/image (193).png>)

#### **转换后**

![](<../../.gitbook/assets/image (229).png>)

#### 标准化的效果：

![](<../../.gitbook/assets/image (210).png>)

### 特征转换

而 [**标准化**](scale-and-transform.md#normalize) **** 会在新的范围内重新缩放数据以减少方差中的数量影响，特征转换是一种更激进的技术。转换会改变分布的形状，使得转换后的数据可以用正态分布或近似正态分布来表示。有两种可用于转换的方法：`yeo-johnson` 和 `quantile`。

#### **参数**

* **transformation: bool, 默认值 = False**\
  当设置为 `True` 时，将应用功率变换器，使数据更加正态/类似于高斯分布。这对于与异方差性或其他需要正态性的建模问题非常有用。通过最大似然估计来估计稳定方差和最小偏斜的最佳参数。
* **transformation\_method: string, 默认值 = ‘yeo-johnson’**\
  定义转换的方法。默认情况下，转换方法设置为 `yeo-johnson`。另一个可用选项是 `quantile` 转换。这两种转换都将特征集转换为遵循高斯分布或正态分布的形式。分位数转换是非线性的，可能会扭曲在相同尺度上测量的变量之间的线性相关性。

#### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
pokemon = get_data('pokemon')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = pokemon, target = 'Legendary', transformation = True)
```

#### **转换前**

![转换前的数据框视图](<../../.gitbook/assets/image (359).png>)

#### **转换后**

![转换后的数据框视图](<../../.gitbook/assets/image (43).png>)

#### 特征转换的效果：

![](<../../.gitbook/assets/image (188) (1).png>)

### 目标变量转换

**目标变量转换** 类似于 [特征转换](scale-and-transform.md#feature-transform) ****，不同之处在于它会改变目标变量的分布形状，而不是特征。目标变量转换支持两种方法：`box-cox` **** 和 `yeo-johnson`。

#### **参数**

* **transform\_target: bool, 默认值 = False**\
  当设置为 `True` 时，将使用 `transform_target_method` 参数中定义的方法对目标变量进行转换。目标变量的转换与特征转换分开应用。
* **transform\_target\_method: string, 默认值 = ‘box-cox’**\
  `box-cox` 要求输入数据严格为正值，而 `yeo-johnson` 支持正值和负值的数据。当 `transform_target_method` 为 `box-cox` 且目标变量包含负值时，内部会强制将方法设置为 `yeo-johnson`，以避免任何异常情况。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
diamond = get_data('diamond')

# 初始化设置
from pycaret.regression import *
reg1 = setup(data = diamond, target = 'Price', transform_target = True)
```

#### 转换前

![目标变量转换前的数据框视图](<../../.gitbook/assets/image (383).png>)

#### 转换后

![目标变量转换后的数据框视图](<../../.gitbook/assets/image (333).png>)

{% hint style="success" %}
**注意：** 这个功能只在 `pycaret.classification` 模块中可用。{% endhint %}