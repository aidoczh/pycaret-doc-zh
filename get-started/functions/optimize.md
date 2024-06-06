# 优化

## 调参模型

这个函数用于调整模型的超参数。该函数的输出是一个通过交叉验证得到的得分网格。根据`optimize`参数定义的评估指标选择最佳模型。可以使用`get_metrics`函数访问交叉验证期间评估的指标。可以使用`add_metric`和`remove_metric`函数添加或删除自定义指标。

### **示例**

```
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(data = boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 调参模型
tuned_dt = tune_model(dt)
```

![tune\_model(dt)的输出](<../../.gitbook/assets/image (427).png>)

比较超参数。

```
# 默认模型
print(dt)

# 调参后的模型
print(tuned_dt)
```

![调参前后的模型超参数](<../../.gitbook/assets/image (166).png>)

### 增加迭代次数

调参最终受到迭代次数的限制，而迭代次数最终取决于您可用的时间和资源。迭代次数由`n_iter`定义，默认设置为`10`。

```
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(data = boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 调参模型
tuned_dt = tune_model(dt, n_iter = 50)
```

![tune\_model(dt, n\_iter = 50)的输出](<../../.gitbook/assets/image (183).png>)

#### 10次和50次迭代的比较

{% tabs %}
{% tab title="n_iter = 10" %}
![](<../../.gitbook/assets/image (139).png>)
{% endtab %}

{% tab title="n_iter = 50" %}
![](<../../.gitbook/assets/image (540).png>)
{% endtab %}
{% endtabs %}

### 选择评估指标

在调整模型的超参数时，必须知道要优化的评估指标。可以在`optimize`参数下定义。默认情况下，分类实验为`Accuracy`，回归为`R2`。

```
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(data = boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 调参模型
tuned_dt = tune_model(dt, optimize = 'MAE')
```

![tune\_model(dt, optimize = 'MAE')的输出](<../../.gitbook/assets/image (246).png>)

### 使用自定义网格

超参数的调整网格已经由PyCaret为库中的所有模型定义。然而，如果您希望，可以通过使用`custom_grid`参数传递自定义网格来定义自己的搜索空间。

```
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 定义搜索空间
params = {"max_depth": np.random.randint(1, (len(boston.columns)*.85),20),
          "max_features": np.random.randint(1, len(boston.columns),20),
          "min_samples_leaf": [2,3,4,5,6]}
          
# 调参模型
tuned_dt = tune_model(dt, custom_grid = params)
```

![tune\_model(dt, custom\_grid = params)的输出](<../../.gitbook/assets/image (177).png>)

### 更改搜索算法

PyCaret与许多不同的超参数调整库无缝集成。这使您可以访问许多不同类型的搜索算法，包括随机搜索、贝叶斯搜索、optuna、TPE等。只需更改一个参数即可实现。默认情况下，PyCaret使用sklearn的`RandomGridSearch`，您可以通过在`tune_model`函数中使用`search_library`和`search_algorithm`参数来更改。

```
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 调参模型 sklearn
tune_model(dt)

# 调参模型 optuna
tune_model(dt, search_library = 'optuna')

# 调参模型 scikit-optimize
tune_model(dt, search_library = 'scikit-optimize')

# 调参模型 tune-sklearn
tune_model(dt, search_library = 'tune-sklearn', search_algorithm = 'hyperopt')
```

{% tabs %}
{% tab title="scikit-learn" %}
![](<../../.gitbook/assets/image (324).png>)
{% endtab %}

{% tab title="optuna" %}
![](<../../.gitbook/assets/image (57) (1).png>)
{% endtab %}

{% tab title="scikit-optimize" %}
![](<../../.gitbook/assets/image (233).png>)
{% endtab %}

{% tab title="tune-sklearn" %}
![](<../../.gitbook/assets/image (252).png>)
{% endtab %}
{% endtabs %}

### 访问调参器
默认情况下，PyCaret的`tune_model`函数只返回调整器选择的最佳模型。有时候你可能需要访问调整器对象，因为它可能包含重要的属性，你可以使用`return_tuner`参数。

```python
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 调整模型并返回调整器
tuned_model, tuner = tune_model(dt, return_tuner=True)
```

![tune\_model(dt, return\_tuner=True)的输出](<../../.gitbook/assets/image (329).png>)

```python
type(tuned_model), type(tuner)
```

![type(tuned\_model), type(tuner)的输出](<../../.gitbook/assets/image (225).png>)

```python
print(tuner)
```

![print(tuner)的输出](<../../.gitbook/assets/image (463).png>)

### 自动选择更好的模型

通常情况下，`tune_model`不会改善模型的性能。事实上，它可能会使性能比使用默认超参数的模型更差。当你不是在笔记本中进行主动实验，而是有一个运行`create_model` --> `tune_model`或`compare_models` --> `tune_model`的Python脚本时，这可能会成为一个问题。为了解决这个问题，你可以使用`choose_better`。当设置为`True`时，它将始终返回性能更好的模型，这意味着如果超参数调整不能改善性能，它将返回输入的模型。

```python
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 调整模型
dt = tune_model(dt, choose_better = True)
```

![tune\_model(dt, choose\_better = True)的输出](<../../.gitbook/assets/image (261).png>)

{% hint style="info" %}
**注意：**`choose_better`不会影响屏幕上显示的评分表。评分表始终会显示调整器选择的最佳模型的性能，而不管输出性能 < 输入性能的事实。
{% endhint %}

## ensemble\_model

此函数对给定的估计器进行集成。此函数的输出是一个带有交叉验证分数的评分表。可以使用`get_metrics`函数访问在交叉验证期间评估的指标。可以使用`add_metric`和`remove_metric`函数添加或删除自定义指标。

### **示例**

```python
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 集成模型
bagged_dt = ensemble_model(dt)
```

![ensemble\_model(dt)的输出](<../../.gitbook/assets/image (376).png>)

```python
type(bagged_dt)
>>> sklearn.ensemble._bagging.BaggingRegressor

print(bagged_dt)
```

![print(bagged\_dt)的输出](<../../.gitbook/assets/image (33).png>)

### **更改fold参数**

```python
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 集成模型
bagged_dt = ensemble_model(dt, fold = 5)
```

![ensemble\_model(dt, fold = 5)的输出](<../../.gitbook/assets/image (536).png>)

此返回的模型与上面的模型相同，但是性能评估是使用5折交叉验证进行的。

### **方法：Bagging**

Bagging，也称为_自助聚合_，是一种机器学习集成元算法，旨在提高统计分类和回归中使用的机器学习算法的稳定性和准确性。它还可以减少方差并帮助避免过拟合。尽管它通常应用于决策树方法，但它可以与任何类型的方法一起使用。Bagging是模型平均方法的特例。

![](<../../.gitbook/assets/image (178).png>)

### **方法：Boosting**

Boosting是一种主要用于减少有监督学习中的偏差和方差的集成元算法。Boosting属于将弱学习器转化为强学习器的机器学习算法家族。弱学习器被定义为与真实分类略微相关的分类器（它可以比随机猜测更好地标记示例）。相比之下，强学习器是与真实分类任意相关的分类器。

![](<../../.gitbook/assets/image (17).png>)

### 选择方法

使用`ensemble_model`可以有两种可能的方式来集成你的机器学习模型。你可以在`method`参数中定义。

```python
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 集成模型
boosted_dt = ensemble_model(dt, method = 'Boosting')
```

![ensemble\_model(dt, method = 'Boosting') 的输出结果](<../../.gitbook/assets/image (347).png>)

```
type(boosted_dt)
>>> sklearn.ensemble._weight_boosting.AdaBoostRegressor

print(boosted_dt)
```

![print(boosted\_dt) 的输出结果](<../../.gitbook/assets/image (152).png>)

### 增加估计器数量

默认情况下，PyCaret 对于 `Bagging` 或 `Boosting` 使用 10 个估计器。您可以通过更改 `n_estimators` 参数来增加估计器数量。

```
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(boston, target = 'medv')

# 训练模型
dt = create_model('dt')

# 集成模型
ensemble_model(dt, n_estimators = 100)
```

![ensemble\_model(dt, n\_estimators = 100) 的输出结果](<../../.gitbook/assets/image (441).png>)

### 自动选择更好的模型

通常情况下，`ensemble_model` 并不会提高模型的性能。事实上，它可能会使性能比不使用集成的模型更差。当您不是在笔记本中进行实验，而是有一个运行 `create_model` --> `ensemble_model` 或 `compare_models` --> `ensemble_model` 工作流程的 Python 脚本时，这可能会成为一个问题。为了解决这个问题，您可以使用 `choose_better`。当设置为 `True` 时，它将始终返回性能更好的模型，这意味着如果超参数调整不能提高性能，它将返回输入的模型。

```
# 加载数据集
from pycaret.datasets import get_data 
boston = get_data('boston') 

# 初始化设置
from pycaret.regression import * 
reg1 = setup(boston, target = 'medv')

# 训练模型
lr = create_model('lr')

# 集成模型
ensemble_model(lr, choose_better = True)
```

![ensemble\_model(lr, choose\_better = True) 的输出结果](<../../.gitbook/assets/image (299).png>)

请注意，使用 `choose_better = True`，`ensemble_model` 返回的模型是一个简单的 `LinearRegression`，而不是 `BaggedRegressor`。这是因为在集成后模型的性能没有改善，因此返回输入的模型。

## blend\_models

该函数对传入 `estimator_list` 参数的选择模型训练一个软投票/多数规则分类器。该函数的输出是一个带有交叉验证得分的评分网格。可以使用 `get_metrics` 函数访问 CV 期间评估的指标。可以使用 `add_metric` 和 `remove_metric` 函数添加或删除自定义指标。

### 示例

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 混合模型
blender = blend_models([lr, dt, knn])
```

![blend\_models(\[lr, dt, knn\]) 的输出结果](<../../.gitbook/assets/image (479).png>)

```
type(blender)
>>> sklearn.ensemble._voting.VotingClassifier

print(blender)
```

![print(blender) 的输出结果](<../../.gitbook/assets/image (276).png>)

### 更改 fold 参数

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 混合模型
blender = blend_models([lr, dt, knn], fold = 5)
```

![blend\_models(\[lr, dt, knn\], fold = 5) 的输出结果](<../../.gitbook/assets/image (32).png>)

该模型返回的结果与上面相同，但是使用 5 折交叉验证进行性能评估。

### 动态输入估计器

您还可以使用 [compare\_models](train.md#compare\_models) 函数自动生成输入估计器列表。这样做的好处是您不需要更改脚本。每次使用前 N 个模型作为输入列表。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 混合模型
blender = blend_models(compare_models(n_select = 3))
```

![blend\_models(compare\_models(n\_select = 3)) 的输出结果](<../../.gitbook/assets/image (157).png>)

请注意这里发生了什么。我们将 `compare_models(n_select = 3` 作为输入传递给 `blend_models`。内部发生的是首先执行 `compare_models` 函数，然后将前 3 个模型作为输入传递给 `blend_models` 函数。

```
print(blender)
```

![print(blender) 的输出结果](<../../.gitbook/assets/image (464).png>)

在这个例子中，由 `compare_models` 评估的前 3 个模型是 `LogisticRegression`、`LinearDiscriminantAnalysis` 和 `RandomForestClassifier`。

### 更改方法
当 `method = 'soft'` 时，它根据预测概率的和的 argmax 来预测类标签，这对于一组校准良好的分类器的集成是推荐的。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 混合模型
blender_soft = blend_models([lr,dt,knn], method = 'soft')
```

![blend\_models(\[lr,dt,knn\], method = 'soft') 的输出](<../../.gitbook/assets/image (223).png>)

当 `method = 'hard'` 时，它使用输入模型的预测（硬标签）而不是概率。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 混合模型
blender_hard = blend_models([lr,dt,knn], method = 'hard')
```

![blend\_models(\[lr,dt,knn\], method = 'hard') 的输出](<../../.gitbook/assets/image (81).png>)

默认方法设置为 `auto`，这意味着它将尝试使用 `soft` 方法，如果前者不受支持，则回退到 `hard` 方法，当其中一个输入模型不支持 `predict_proba` 属性时，可能会发生这种情况。

{% hint style="info" %}
**注意：**方法参数仅在 [Classification](../modules.md) 模块中可用。
{% endhint %}

### 更改权重

默认情况下，混合模型在混合它们时给予所有输入模型相等的权重，但您可以明确传递要给予每个输入模型的权重。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 混合模型
blender_weighted = blend_models([lr,dt,knn], weights = [0.5,0.2,0.3])
```

![blend\_models(\[lr,dt,knn\], weights = \[0.5,0.2,0.3\]) 的输出](../../.gitbook/assets/image.png)

您还可以使用 `tune_model` 调整混合器的权重。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 混合模型
blender_weighted = blend_models([lr,dt,knn], weights = [0.5,0.2,0.3])

# 调整混合器
tuned_blender = tune_model(blender_weighted)
```

![tune\_model(blender\_weighted) 的输出](<../../.gitbook/assets/image (234).png>)

```
print(tuned_blender)
```

![print(tuned\_blender) 的输出](<../../.gitbook/assets/image (196).png>)

### 自动选择更好的模型

通常情况下，`blend_models` 不会改善模型性能。实际上，它可能会使性能比混合模型更差。当您不是在笔记本中进行主动实验而是有一个运行 `compare_models` --> `blend_models` 工作流程的 Python 脚本时，这可能会成为问题。为了解决这个问题，您可以使用 `choose_better`。当设置为 `True` 时，它将始终返回性能更好的模型，这意味着如果混合模型不改善性能，它将返回单个性能最好的输入模型。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 混合模型
blend_models([lr,dt,knn], choose_better = True)
```

![blend\_models(\[lr,dt,knn\], choose\_better = True) 的输出](<../../.gitbook/assets/image (141).png>)

请注意，由于 `choose_better=True`，此函数返回的最终模型是 `LogisticRegression`，而不是 `VotingClassifier`，因为 Logistic Regression 的性能是所有给定输入模型中最优化的，再加上混合器。

## stack\_models

此函数在 `estimator_list` 参数中传递的选择估计器上训练元模型。此函数的输出是一个带有 CV 分数的评分网格。可以使用 `get_metrics` 函数访问 CV 期间评估的指标。可以使用 `add_metric` 和 `remove_metric` 函数添加或删除自定义指标。

### 示例
```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 堆叠模型
stacker = stack_models([lr, dt, knn])
```


![stack_models 函数的输出（\[lr, dt, knn\]）](<../../.gitbook/assets/image (436).png>)

### 更改 fold 参数

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 堆叠模型
stacker = stack_models([lr, dt, knn], fold = 5)
```

![stack_models 函数的输出（\[lr, dt, knn\]，fold = 5）](<../../.gitbook/assets/image (363).png>)

通过这种方式返回的模型与上述相同，但是性能评估是使用 5 折交叉验证完成的。

### 动态输入估计器

您还可以使用 [compare_models](train.md#compare_models) 函数自动生成输入估计器列表。这样做的好处是您根本不需要更改脚本。每次使用前 N 个模型作为输入列表。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 堆叠模型
stacker = stack_models(compare_models(n_select = 3))
```

![stack_models 函数的输出（compare_models(n_select = 3)）](<../../.gitbook/assets/image (440).png>)

请注意这里发生了什么。我们将 `compare_models(n_select = 3` 作为输入传递给 `stack_models`。内部发生的是首先执行了 `compare_models` 函数，然后将前 3 个模型作为输入传递给 `stack_models` 函数。

```
print(stacker)
```

![print(stacker) 的输出](<../../.gitbook/assets/image (236).png>)

在这个例子中，由 `compare_models` 评估的前 3 个模型是 `LogisticRegression`、`RandomForestClassifier` 和 `LGBMClassifier`。

### 更改方法

您可以明确选择几种不同的堆叠方法，或者传递 `auto` 以自动确定。当设置为 `auto` 时，它将按顺序调用每个模型的 `predict_proba`、`decision_function` 或 `predict` 函数。或者，您可以明确定义方法。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 堆叠模型
stacker = stack_models([lr, dt, knn], method = 'predict')
```

![stack_models 函数的输出（\[lr, dt, knn\]，method = 'predict'）](<../../.gitbook/assets/image (49).png>)

### 更改元模型

当没有显式传递 `meta_model` 时，对于分类实验会使用 `LogisticRegression`，对于回归实验会使用 `LinearRegression`。您还可以传递一个特定的模型作为元模型。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 训练元模型
lightgbm = create_model('lightgbm')

# 堆叠模型
stacker = stack_models([lr, dt, knn], meta_model = lightgbm)
```

![stack_models 函数的输出（\[lr, dt, knn\]，meta_model = lightgbm）](<../../.gitbook/assets/image (254).png>)

```
print(stacker.final_estimator_)
```

![print(stacker.final_estimator_) 的输出](<../../.gitbook/assets/image (439).png>)

### 重新堆叠

有两种堆叠模型的方式：(i) 仅使用输入模型的预测作为元模型的训练数据，(ii) 使用预测以及原始训练数据作为训练元模型的数据。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练几个模型
lr = create_model('lr')
dt = create_model('dt')
knn = create_model('knn')

# 堆叠模型
stacker = stack_models([lr, dt, knn], restack = False)
```

![stack_models 函数的输出（\[lr, dt, knn\]，restack = False）](<../../.gitbook/assets/image (397).png>)

## optimize_threshold

此函数优化训练模型的概率阈值。它在不同的 `probability_threshold` 上迭代性能指标，步长由 `grid_interval` 参数定义。此函数将显示每个概率阈值下的性能指标图，并根据 `optimize` 参数下定义的指标返回最佳模型。

### 示例
```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练模型
knn = create_model('knn')

# 优化阈值
optimized_knn = optimize_threshold(knn)
```

![optimize_threshold(knn)的输出](<../../.gitbook/assets/image (184).png>)

```
print(optimized_knn)
```

![print(optimized_knn)的输出](<../../.gitbook/assets/image (66).png>)

## calibrate_model

这个函数使用等温或逻辑回归来校准给定模型的概率。该函数的输出是一个通过交叉验证得到的评分表。可以使用`get_metrics`函数访问在交叉验证期间评估的指标。可以使用`add_metric`和`remove_metric`函数添加或删除自定义指标。

### 示例

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 训练模型
dt = create_model('dt')

# 校准模型
calibrated_dt = calibrate_model(dt)
```

![calibrate_model(dt)的输出](<../../.gitbook/assets/image (127).png>)

```
print(calibrated_dt)
```

![print(calibrated_dt)的输出](<../../.gitbook/assets/image (481).png>)

### 校准前后比较

{% tabs %}
{% tab title="校准前" %}
![](<../../.gitbook/assets/image (135).png>)
{% endtab %}

{% tab title="校准后" %}
![](<../../.gitbook/assets/image (320).png>)
{% endtab %}
{% endtabs %}