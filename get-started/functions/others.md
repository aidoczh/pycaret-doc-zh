# 其他函数

## pull

返回最后一次打印的评分表格。在任何训练函数之后使用`pull`函数将评分表格存储在`pandas.DataFrame`中。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target='Class variable')

# 比较模型
best_model = compare_models()

# 获取评分表格
results = pull()
```

![pull()的输出](<../../.gitbook/assets/image (290).png>)

```
type(results)
>>> pandas.core.frame.DataFrame
```

## models

返回包含导入的模型库模块中所有可用模型的表格。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target='Class variable')

# 检查模型库
models()
```

![models()的输出](<../../.gitbook/assets/image (241) (1).png>)

如果你想查看比这更多的信息，可以传递`internal=True`。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target='Class variable')

# 检查模型库
models(internal=True)
```

![models(internal=True)的输出](<../../.gitbook/assets/image (34).png>)

## get\_config

此函数检索在初始化[setup](initialize.md#setup)函数时创建的全局变量。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target='Class variable')

# 获取X_train
get_config('X_train')
```

![get\_config('X\_train')的输出](<../../.gitbook/assets/image (137).png>)

`get_config`函数可以访问的变量：

* X：转换后的数据集（X）
* y：转换后的数据集（y）
* X\_train：转换后的训练数据集（X）
* X\_test：转换后的测试/留置数据集（X）
* y\_train：转换后的训练数据集（y）
* y\_test：转换后的测试/留置数据集（y）
* seed：通过session\_id设置的随机状态
* prep\_pipe：转换流水线
* fold\_shuffle\_param：在Kfolds中使用的shuffle参数
* n\_jobs\_param：在模型训练中使用的n\_jobs参数
* html\_param：通过setup配置的html\_param
* create\_model\_container：结果网格存储容器
* master\_model\_container：模型存储容器
* display\_container：结果显示容器
* exp\_name\_log：实验名称
* logging\_param：log\_experiment参数
* log\_plots\_param：log\_plots参数
* USI：唯一会话ID参数
* fix\_imbalance\_param：fix\_imbalance参数
* fix\_imbalance\_method\_param：fix\_imbalance\_method参数
* data\_before\_preprocess：预处理之前的数据
* target\_param：目标变量的名称
* gpu\_param：通过setup配置的use\_gpu参数
* fold\_generator：在fold\_strategy中配置的CV分割器
* fold\_param：在setup中定义的fold参数
* fold\_groups\_param：在setup中定义的fold groups
* stratify\_param：在setup中定义的stratify参数

## set\_config

此函数重置全局变量。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target='Class variable', session_id=123)

# 重置环境种子
set_config('seed', 999) 
```

## get\_metrics

返回度量容器中所有可用度量的表格。所有这些度量都用于交叉验证。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target='Class variable', session_id=123)

# 获取度量
get_metrics()
```

![get\_metrics()的输出](<../../.gitbook/assets/image (153).png>)

## add\_metric

将自定义度量添加到度量容器中。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target='Class variable', session_id=123)

# 添加度量
from sklearn.metrics import log_loss
add_metric('logloss', 'Log Loss', log_loss, greater_is_better=False)
```

![add\_metric('logloss', 'Log Loss', log\_loss, greater\_is\_better=False)的输出](<../../.gitbook/assets/image (269).png>)

现在如果你检查度量容器：

```
get_metrics()
```

![get\_metrics()（添加了log loss度量后）的输出](<../../.gitbook/assets/image (442).png>)

## remove\_metric

从度量容器中删除度量。

```
# 删除度量
remove_metric('logloss')
```

没有输出。让我们再次检查度量容器。

```
get_metrics()
```

![get\_metrics()（删除了log loss度量后）的输出](<../../.gitbook/assets/image (535).png>)

## automl
这个函数根据`optimize`参数返回当前设置中所有训练模型中的最佳模型。可以使用`get_metrics`函数访问评估的指标。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data 
data = get_data('diabetes') 

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target = 'Class variable') 

# 比较模型
top5 = compare_models(n_select = 5) 

# 调整模型
tuned_top5 = [tune_model(i) for i in top5]

# 集成模型
bagged_top5 = [ensemble_model(i) for i in tuned_top5]

# 混合模型
blender = blend_models(estimator_list = top5) 

# 堆叠模型
stacker = stack_models(estimator_list = top5) 

# 自动机器学习 
best = automl(optimize = 'Recall')
print(best)
```

![从print(best)输出的结果](<../../.gitbook/assets/image (251).png>)

## get\_logs

返回实验日志表。仅在初始化[setup](initialize.md#setup)函数时`log_experiment = True`时有效。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target = 'Class variable', log_experiment = True, experiment_name = 'diabetes1')

# 比较模型
top5 = compare_models()

# 检查ML日志
get_logs()
```

![从get\_logs()输出的结果](<../../.gitbook/assets/image (356) (1).png>)

## **get\_system\_logs**

读取并打印当前活动目录中的`logs.log`文件。

#### 示例

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data, target = 'Class variable', session_id = 123)

# 检查系统日志
from pycaret.utils import get_system_logs
get_system_logs()
```

![](<../../.gitbook/assets/image (469).png>)