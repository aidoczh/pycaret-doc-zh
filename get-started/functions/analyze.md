---
description: 在 PyCaret 中的分析和模型可解释性函数
---

# 分析

## plot\_model

该函数分析了在留出集上训练的模型的性能。在某些情况下，可能需要重新训练模型。

### **示例**

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
lr = create_model('lr')

# 绘制模型
plot_model(lr, plot = 'auc')
```

![plot\_model(lr, plot = 'auc') 的输出](<../../.gitbook/assets/image (235).png>)

### **更改比例**

可以使用 `scale` 参数更改图像的分辨率比例。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
lr = create_model('lr')

# 绘制模型
plot_model(lr, plot = 'auc', scale = 3)
```

![plot\_model(lr, plot = 'auc', scale = 3) 的输出](<../../.gitbook/assets/image (307).png>)

### 保存绘图

可以使用 `save` 参数将绘图保存为 `png` 文件。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
lr = create_model('lr')

# 绘制模型
plot_model(lr, plot = 'auc', save = True)
```

![plot\_model(lr, plot = 'auc', save = True) 的输出](<../../.gitbook/assets/image (467).png>)

### 自定义绘图

PyCaret 大部分绘图工作使用 [Yellowbrick](https://www.scikit-yb.org/en/latest/) 完成。可以将任何适用于 Yellowbrick 可视化器的参数传递给 `plot_kwargs` 参数。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
lr = create_model('lr')

# 绘制模型
plot_model(lr, plot = 'confusion_matrix', plot_kwargs = {'percent' : True})
```

![plot\_model(lr, plot = 'confusion\_matrix', plot\_kwargs = {'percent' : True}) 的输出](<../../.gitbook/assets/image (27).png>)

{% tabs %}
{% tab title="自定义前" %}
![](<../../.gitbook/assets/image (163).png>)
{% endtab %}

{% tab title="自定义后" %}
![](<../../.gitbook/assets/image (295).png>)
{% endtab %}
{% endtabs %}

### 使用训练数据

如果要在训练数据上评估模型绘图，可以在 `plot_model` 函数中传递 `use_train_data=True`。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
lr = create_model('lr')

# 绘制模型
plot_model(lr, plot = 'auc', use_train_data = True)
```

![plot\_model(lr, plot = 'auc', use\_train\_data = True) 的输出](<../../.gitbook/assets/image (323).png>)

#### 在训练数据和留出数据上绘制

{% tabs %}
{% tab title="训练数据" %}
![](<../../.gitbook/assets/image (321).png>)
{% endtab %}

{% tab title="留出数据" %}
![](<../../.gitbook/assets/image (492).png>)
{% endtab %}
{% endtabs %}

### **按模块的示例**

#### **分类**

| **绘图名称**                | **绘图**            |
| --------------------------- | ------------------- |
| 曲线下面积                  | ‘auc’               |
| 判别阈值                    | ‘threshold’         |
| 精确率-召回率曲线           | ‘pr’                |
| 混淆矩阵                    | ‘confusion\_matrix’ |
| 分类预测错误                | ‘error’             |
| 分类报告                    | ‘class\_report’     |
| 决策边界                    | ‘boundary’          |
| 递归特征选择                | ‘rfe’               |
| 学习曲线                    | ‘learning’          |
| 流形学习                    | ‘manifold’          |
| 校准曲线                    | ‘calibration’       |
| 验证曲线                    | ‘vc’                |
| 维度学习                    | ‘dimension’         |
| 特征重要性（前 10 个）       | ‘feature’           |
| 特征重要性（全部）          | 'feature\_all'      |
| 模型超参数                  | ‘parameter’         |
| 提升曲线                    | 'lift'              |
| 增益曲线                    | 'gain'              |
| KS 统计图                   | 'ks'                |



{% tabs %}
{% tab title="auc" %}
![](<../../.gitbook/assets/image (186).png>)
{% endtab %}

{% tab title="confusion_matrix" %}
![](<../../.gitbook/assets/image (248).png>)
{% endtab %}

{% tab title="threshold" %}
![](<../../.gitbook/assets/image (101).png>)
{% endtab %}

{% tab title="pr" %}
![](<../../.gitbook/assets/image (408).png>)
{% endtab %}

{% tab title="error" %}
![](<../../.gitbook/assets/image (537).png>)
{% endtab %}

{% tab title="class_report" %}
![](<../../.gitbook/assets/image (41).png>)
{% endtab %}

{% tab title="rfe" %}
![](<../../.gitbook/assets/image (404).png>)
{% endtab %}

{% tab title="learning" %}
![](<../../.gitbook/assets/image (200) (1).png>)
{% endtab %}

{% tab title="vc" %}
![](<../../.gitbook/assets/image (23).png>)
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="feature" %}
![](<../../.gitbook/assets/image (102).png>)
{% endtab %}

{% tab title="manifold" %}
![](<../../.gitbook/assets/image (35) (1).png>)
{% endtab %}

{% tab title="calibration" %}
![](<../../.gitbook/assets/image (296).png>)
{% endtab %}

{% tab title="dimension" %}
![](<../../.gitbook/assets/image (12).png>)
{% endtab %}

{% tab title="boundary" %}
![](<../../.gitbook/assets/image (185).png>)
{% endtab %}

{% tab title="lift" %}
![](<../../.gitbook/assets/image (277) (1).png>)
{% endtab %}

{% tab title="gain" %}
![](<../../.gitbook/assets/image (340).png>)
{% endtab %}

{% tab title="ks" %}
![](<../../.gitbook/assets/image (148).png>)
{% endtab %}

{% tab title="parameter" %}
![](<../../.gitbook/assets/image (7).png>)
{% endtab %}
{% endtabs %}

#### 回归

| **名称**                    | **图表**       |
| --------------------------- | -------------- |
| 残差图                      | ‘residuals’    |
| 预测误差图                  | ‘error’        |
| Cook's 距离图               | ‘cooks’        |
| 递归特征选择                | ‘rfe’          |
| 学习曲线                    | ‘learning’     |
| 验证曲线                    | ‘vc’           |
| 流形学习                    | ‘manifold’     |
| 特征重要性（前10个）        | ‘feature’      |
| 特征重要性（全部）          | 'feature\_all' |
| 模型超参数                  | ‘parameter’    |

{% tabs %}
{% tab title="residuals" %}
![](<../../.gitbook/assets/image (455).png>)
{% endtab %}

{% tab title="error" %}
![](<../../.gitbook/assets/image (63).png>)
{% endtab %}

{% tab title="cooks" %}
![](<../../.gitbook/assets/image (217) (1).png>)
{% endtab %}

{% tab title="rfe" %}
![](<../../.gitbook/assets/image (103).png>)
{% endtab %}

{% tab title="feature" %}
![](<../../.gitbook/assets/image (142) (3).png>)
{% endtab %}

{% tab title="learning" %}
![](<../../.gitbook/assets/image (314).png>)
{% endtab %}

{% tab title="vc" %}
![](<../../.gitbook/assets/image (377).png>)
{% endtab %}

{% tab title="manifold" %}
![](<../../.gitbook/assets/image (167) (2).png>)
{% endtab %}
{% endtabs %}

#### 聚类

| **名称**              | **图表**       |
| --------------------- | -------------- |
| 聚类 PCA 图（2D）     | ‘cluster’      |
| 聚类 t-SNE 图（3D）   | ‘tsne’         |
| Elbow 图              | ‘elbow’        |
| 轮廓系数图            | ‘silhouette’   |
| 距离图               | ‘distance’     |
| 分布图               | ‘distribution’ |

{% tabs %}
{% tab title="cluster" %}
![](<../../.gitbook/assets/image (273).png>)
{% endtab %}

{% tab title="tsne" %}
![](<../../.gitbook/assets/image (330) (1).png>)
{% endtab %}

{% tab title="elbow" %}
![](<../../.gitbook/assets/image (203).png>)
{% endtab %}

{% tab title="silhouette" %}
![](<../../.gitbook/assets/image (348).png>)
{% endtab %}

{% tab title="distance" %}
![](<../../.gitbook/assets/image (205).png>)
{% endtab %}

{% tab title="distribution" %}
![](<../../.gitbook/assets/image (180).png>)
{% endtab %}
{% endtabs %}

#### 异常检测

| **名称**                  | **图表** |
| ------------------------- | -------- |
| t-SNE（3D）维度图         | ‘tsne’   |
| UMAP 降维图               | ‘umap’   |

{% tabs %}
{% tab title="tsne" %}
![](<../../.gitbook/assets/image (306).png>)
{% endtab %}

{% tab title="umap" %}
![](<../../.gitbook/assets/image (104).png>)
{% endtab %}
{% endtabs %}

#### 自然语言处理

| **名称**                  | **图表**              |
| ------------------------- | --------------------- |
| 词频分布图                | ‘frequency’           |
| 词分布图                  | ‘distribution’        |
| 二元词频图                | ‘bigram’              |
| 三元词频图                | ‘trigram’             |
| 情感极性分布图            | ‘sentiment’           |
| 词性频率图                | ‘pos’                 |
| t-SNE（3D）维度图         | ‘tsne’                |
| 主题模型（pyLDAvis）      | ‘topic\_model’        |
| 主题推断分布图            | ‘topic\_distribution’ |
| 词云图                    | ‘wordcloud’           |
| UMAP 降维图               | ‘umap’                |

{% tabs %}
{% tab title="frequency" %}
![](<../../.gitbook/assets/image (238).png>)
{% endtab %}

{% tab title="distribution" %}
![](<../../.gitbook/assets/image (429).png>)
{% endtab %}

{% tab title="bigram" %}
![](<../../.gitbook/assets/image (192) (2).png>)
{% endtab %}

{% tab title="trigram" %}
![](<../../.gitbook/assets/image (6).png>)
{% endtab %}

{% tab title="sentiment" %}
![](<../../.gitbook/assets/image (352).png>)
{% endtab %}

{% tab title="pos" %}
![](<../../.gitbook/assets/image (364).png>)
## 关联规则挖掘

`evaluate_model` 函数显示了一个用户界面，用于分析训练模型的性能。它在内部调用了 [plot_model](analyze.md#plot_model) 函数。

```python
# 加载数据集
from pycaret.datasets import get_data
juice = get_data('juice')

# 初始化设置
from pycaret.classification import *
exp_name = setup(data = juice,  target = 'Purchase')

# 创建模型
lr = create_model('lr')

# 启动评估小部件
evaluate_model(lr)
```

![evaluate_model(lr) 的输出](<../../.gitbook/assets/image (72).png>)

{% hint style="info" %}
**注意：**此函数仅在 Jupyter Notebook 或等效环境中可用。
{% endhint %}

## interpret\_model

此函数分析训练模型生成的预测结果。该函数中的大多数图表都是基于 SHAP（Shapley Additive exPlanations）实现的。有关更多信息，请参见 [https://shap.readthedocs.io/en/latest/](https://shap.readthedocs.io/en/latest/)

### 示例

```python
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost)
```

![interpret_model(xgboost) 的输出](<../../.gitbook/assets/image (482).png>)

### 保存图表

您可以使用 `save` 参数将图表保存为 `png` 文件。

```python
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost, save = True)
```

{% hint style="info" %}
**注意：**当 `save=True` 时，在 Notebook 中不会显示任何图表。
{% endhint %}

### 更改图表类型

可以通过 `plot` 参数更改可用的几种不同图表类型。

#### 相关性

```python
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost, plot = 'correlation')
```

![interpret_model(xgboost, plot = 'correlation') 的输出](<../../.gitbook/assets/image (502).png>)

默认情况下，PyCaret 使用数据集中的第一个特征，但可以使用 `feature` 参数进行更改。

```python
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost, plot = 'correlation', feature = 'Age (years)')
```

![interpret_model(xgboost, plot = 'correlation', feature = 'Age (years)') 的输出](<../../.gitbook/assets/image (271) (1).png>)

#### 偏依赖图

```python
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost, plot = 'pdp')
```

![interpret_model(xgboost, plot = 'pdp') 的输出](<../../.gitbook/assets/image (544).png>)

默认情况下，PyCaret 使用数据集中的第一个可用特征，但可以使用 `feature` 参数进行更改。

```python
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost, plot = 'pdp', feature = 'Age (years)')
```

![interpret_model(xgboost, plot = 'pdp', feature = 'Age (years)') 的输出](<../../.gitbook/assets/image (8).png>)

#### Morris 敏感性分析
```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data=diabetes, target='Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost, plot='msa')
```

以上代码是使用 PyCaret 进行机器学习的示例。首先，我们使用 `get_data` 函数加载了一个名为 "diabetes" 的数据集。然后，我们使用 `setup` 函数初始化了一个分类模型，并指定了目标变量为 "Class variable"。接下来，我们使用 `create_model` 函数创建了一个名为 "xgboost" 的模型。最后，我们使用 `interpret_model` 函数对模型进行解释，并选择了 "msa" 的可视化方式。
![interpret_model(xgboost, plot = 'msa') 的输出](<../../.gitbook/assets/image (370) (1).png>)

#### 排列特征重要性

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost, plot = 'pfi')
```

![interpret_model(xgboost, plot = 'pfi') 的输出](<../../.gitbook/assets/image (500).png>)

#### 原因图

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost, plot = 'reason')
```

![interpret_model(xgboost, plot = 'reason') 的输出](<../../.gitbook/assets/image (131).png>)

当你生成 `reason` 图时，如果不传递特定的测试数据索引，你将得到一个交互式图表，可以选择 x 轴和 y 轴。这只在使用 Jupyter Notebook 或等效环境时才可能。如果你想查看特定观察的图表，你需要在 `observation` 参数中传递索引。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost, plot = 'reason', observation = 1)
```

![](<../../.gitbook/assets/image (268).png>)

这里的 `observation = 1` 表示测试集中的索引 1。

### 使用训练数据

默认情况下，所有的图表都是在测试数据集上生成的。如果你想使用训练数据集生成图表（不推荐），你可以使用 `use_train_data` 参数。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 解释模型
interpret_model(xgboost, use_train_data = True)
```

![interpret_model(xgboost, use_train_data = True) 的输出](<../../.gitbook/assets/image (136).png>)

## 仪表盘

`dashboard` 函数为训练好的模型生成交互式仪表盘。该仪表盘使用 ExplainerDashboard 实现（[explainerdashboard.readthedocs.io](https://explainerdashboard.readthedocs.io)）。

#### 仪表盘示例

```
# 加载数据集
from pycaret.datasets import get_data
juice = get_data('juice')

# 初始化设置
from pycaret.classification import *
exp_name = setup(data = juice,  target = 'Purchase')

# 训练模型
lr = create_model('lr')

# 启动仪表盘
dashboard(lr)
```

![仪表盘（分类指标）](<../../.gitbook/assets/image (64).png>)

![仪表盘（个体预测）](<../../.gitbook/assets/image (118).png>)

![仪表盘（假设分析）](<../../.gitbook/assets/image (293).png>)

#### 视频：

{% embed url="https://www.youtube.com/watch?v=FZ5-GtdYez0" %}

## deep\_check

该函数使用 [deepchecks](https://github.com/deepchecks/deepchecks) 库对训练好的模型进行全面检查。

#### Deep Check 示例

```
# 加载数据集
from pycaret.datasets import get_data
juice = get_data('juice')

# 初始化设置
from pycaret.classification import *
exp_name = setup(data = juice,  target = 'Purchase')

# 训练模型
lr = create_model('lr')

# 深度检查模型
deep_check(lr)
```

![Deep Check 仪表盘（1/3）- 输出被截断](<../../.gitbook/assets/image (385).png>)

![Deep Check 仪表盘（2/3）- 输出被截断](<../../.gitbook/assets/image (319).png>)

![Deep Check 仪表盘（3/3）- 输出被截断](<../../.gitbook/assets/image (524).png>)

{% hint style="warning" %}
该函数处于实验模式。`deepchecks` 需要 `scikit-learn>1.0` 版本，而 pycaret 需要 scikit-learn==0.23.2。因此，在安装 deepchecks 后，你必须卸载 scikit-learn 并重新安装 scikit-learn==0.23.2，否则会出现与 pycaret 的错误。未来版本的 pycaret 将与 scikit-learn>1.0 兼容。&#x20;
{% endhint %}

## eda

该函数使用 AutoViz 库生成自动的探索性数据分析（EDA）。你必须单独安装 Autoviz `pip install autoviz` 才能使用该函数。

#### EDA 示例

```
# 加载数据集
from pycaret.datasets import get_data
juice = get_data('juice')

# 初始化设置
from pycaret.classification import *
exp_name = setup(data = juice,  target = 'Purchase')

# 启动 eda
eda(display_format = 'bokeh')
```

![使用 display\_format = 'bokeh' 的输出](<../../.gitbook/assets/image (346).png>)

你也可以使用 `display_format = 'svg'` 运行该函数。
```
# 载入数据集
from pycaret.datasets import get_data
juice = get_data('juice')

# 初始化设置
from pycaret.classification import *
exp_name = setup(data=juice, target='Purchase')

# 启动探索性数据分析（EDA）
eda(display_format='svg')
```
![display\_format = 'svg' 的输出](<../../.gitbook/assets/image (231).png>)

#### 视频：

{% embed url="https://www.youtube.com/watch?v=Pm5VOuYqU4Q" %}

## check\_fairness

有很多方法来概念化公平性。`check_fairness` 函数遵循了被称为群体公平性的方法，即询问：哪些群体的个体面临受害的风险。`check_fairness` 提供了不同群体（也称为子群体）之间的公平性相关指标。

#### 检查公平性示例

```
# 加载数据集
from pycaret.datasets import get_data
income = get_data('income')

# 初始化设置
from pycaret.classification import *
exp_name = setup(data = income,  target = 'income >50K')

# 训练模型
lr = create_model('lr')

# 检查模型公平性
lr_fairness = check_fairness(lr, sensitive_features = ['sex', 'race'])
```

![](<../../.gitbook/assets/image (317).png>)

![](<../../.gitbook/assets/image (509).png>)

#### 视频：

{% embed url="https://www.youtube.com/watch?v=mjhDKuLRpM0" %}

## get\_leaderboard

此函数返回当前设置中训练的所有模型的排行榜。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 比较模型
top3 = compare_models(n_select = 3)

# 调整前3个模型
tuned_top3 = [tune_model(i) for i in top3]

# 集成前3个调整后的模型
ensembled_top3 = [ensemble_model(i) for i in tuned_top3]

# 混合器
blender = blend_models(tuned_top3)

# 堆叠器
stacker = stack_models(tuned_top3)

# 检查排行榜
get_leaderboard()
```

![get\_leaderboard() 的输出](<../../.gitbook/assets/image (287).png>)

您还可以使用此方法访问训练过的 Pipeline。

```
# 检查排行榜
lb = get_leaderboard()

# 选择顶部模型
lb.iloc[0]['Model']
```

![lb.iloc\[0\]\['Model'\] 的输出](<../../.gitbook/assets/image (534).png>)

## assign\_model

此函数使用训练过的模型为训练数据集分配标签。它适用于[聚类](../modules.md)、[异常检测](../modules.md)和[NLP](../modules.md)模块。

#### 聚类

```
# 加载数据集
from pycaret.datasets import get_data
jewellery = get_data('jewellery')

# 初始化设置
from pycaret.clustering import *
clu1 = setup(data = jewellery)

# 训练模型
kmeans = create_model('kmeans')

# 分配模型
assign_model(kmeans)
```

![assign\_model(kmeans) 的输出](<../../.gitbook/assets/image (164).png>)

#### 异常检测

```
# 加载数据集
from pycaret.datasets import get_data
anomaly = get_data('anomaly')

# 初始化设置
from pycaret.anomaly import *
ano1 = setup(data = anomaly)

# 训练模型
iforest = create_model('iforest')

# 分配模型
assign_model(iforest)
```

![assign\_model(iforest) 的输出](<../../.gitbook/assets/image (403).png>)

#### 自然语言处理

```
# 加载数据集
from pycaret.datasets import get_data
kiva = get_data('kiva')

# 初始化设置
from pycaret.nlp import *
nlp1 = setup(data = kiva, target = 'en')

# 训练模型
lda = create_model('lda')

# 分配模型
assign_model(lda)
```

![assign\_model(lda) 的输出](<../../.gitbook/assets/image (138).png>)