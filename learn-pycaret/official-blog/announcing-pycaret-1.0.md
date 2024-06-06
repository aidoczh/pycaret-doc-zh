# 宣布 PyCaret 1.0

![](https://cdn-images-1.medium.com/max/2000/1\*Xtb7t4Rlxq8jFLXZn\_sdyQ.png)

### 宣布 PyCaret 1.0.0

#### 一个在 Python 中的开源 **低代码** 机器学习库。

#### 作者：Moez Ali

我们很高兴地宣布 [PyCaret](https://www.pycaret.org)，这是一个在 Python 中的开源机器学习库，可在 **低代码** 环境中训练和部署监督学习和无监督学习模型。PyCaret 可以让您从准备数据到在您选择的笔记本环境中部署模型，仅需几秒钟的时间。

与其他开源机器学习库相比，PyCaret 是一个替代的低代码库，可以用少量代码取代数百行代码。这使得实验速度指数级提升，效率也更高。PyCaret 本质上是围绕几个机器学习库和框架（如 [scikit-learn](https://scikit-learn.org/stable/)、[XGBoost](https://xgboost.readthedocs.io/en/latest/)、[Microsoft LightGBM](https://github.com/microsoft/LightGBM)、[spaCy](https://spacy.io/) 等）的 Python 封装。

PyCaret **简单** 且 **易于使用**。PyCaret 中执行的所有操作都按顺序存储在一个完全为 \*\*部署\*\* 而编排的 **Pipeline** 中。无论是填充缺失值、转换分类数据、特征工程甚至超参数调整，PyCaret 都可以自动化完成。欲了解更多关于 PyCaret 的信息，请观看这段 1 分钟的视频。

### 使用 PyCaret 入门

可以使用 pip 安装 PyCaret 的第一个稳定版本 1.0.0。在命令行界面或笔记本环境中，运行以下代码单元格来安装 PyCaret。

```
pip install pycaret
```

如果您使用的是 [Azure notebooks](https://notebooks.azure.com/) 或 [Google Colab](https://colab.research.google.com/)，请运行以下代码单元格来安装 PyCaret。

```
!pip install pycaret
```

安装 PyCaret 时，所有依赖项都会自动安装。[点击这里](https://github.com/pycaret/pycaret/blob/master/requirements.txt) 查看完整依赖项列表。

### 没有比这更简单的了 👇

![](https://cdn-images-1.medium.com/max/2560/1\*QG6SjFXOV6wqY\_00D1fsLw.gif)

### 📘 逐步教程

### 1. 获取数据

在这个逐步教程中，我们将使用 **‘diabetes’** 数据集，目标是根据诸如血压、胰岛素水平、年龄等因素预测患者结果（二进制 1 或 0）。数据集可在 PyCaret 的 [github 仓库](https://github.com/pycaret/pycaret) 上找到。直接从仓库导入数据集的最简单方法是使用 **pycaret.datasets** 模块中的 **get_data** 函数。

```
from **pycaret.datasets** import **get_data**
diabetes = **get_data**('diabetes')
```

![get\_data 的输出](https://cdn-images-1.medium.com/max/2658/1\*o1xpZeVNUfzm7yQ6f1IPvw.png)

💡 PyCaret 可直接与 **pandas** dataframe 一起使用。

### 2. 设置环境

PyCaret 中任何机器学习实验的第一步是通过导入所需模块并初始化 **setup**( ) 来设置环境。此示例中使用的模块是 [\*\*pycaret.classification](https://www.pycaret.org/classification).\*\*

导入模块后，通过定义数据框（_‘diabetes’_）和目标变量（_‘Class variable’_）来初始化 **setup()**。

```
from **pycaret.classification** import ***
**exp1 = **setup**(diabetes, target = 'Class variable')
```

![](https://cdn-images-1.medium.com/max/2000/1\*WaVNaMkfoHIrD0lKPHFvJA.png)

所有预处理步骤都在 **setup()** 中应用。PyCaret 有超过 20 个特性用于为机器学习准备数据，根据 **setup** 函数中定义的参数创建一个转换管道。它会自动在一个 **pipeline** 中编排所有依赖项，这样您就无需手动管理在测试或未知数据集上的转换顺序。PyCaret 的管道可以轻松在不同环境之间传输，以便规模运行或轻松部署到生产环境。以下是 PyCaret 在其首次发布中提供的预处理功能。

![PyCaret 的预处理能力](https://cdn-images-1.medium.com/max/2000/1\*jo9vPsQhQZmyXUhnrt9akQ.png)

💡 当初始化 **setup()** 时，会自动执行诸如缺失值填充、分类变量编码、标签编码（将是或否转换为 1 或 0）以及训练-测试分割等机器学习必备的数据预处理步骤。[点击这里](https://www.pycaret.org/preprocessing) 了解更多关于 PyCaret 预处理能力的信息。

### 3. 比较模型
这是在监督机器学习实验中推荐的第一步（[分类](https://www.pycaret.org/classification)或[回归](https://www.pycaret.org/regression)）。该函数训练模型库中的所有模型，并使用k折交叉验证（默认为10折）比较常见的评估指标。使用的评估指标有：

* **对于分类：**准确率、AUC、召回率、精确率、F1、Kappa
* **对于回归：**MAE、MSE、RMSE、R2、RMSLE、MAPE

    **compare_models**()

![compare\_models( ) 函数的输出](https://cdn-images-1.medium.com/max/2000/1\*WaaSiqUkIFMiKbYofBRo7Q.png)

💡 默认情况下，评估指标使用10折交叉验证进行评估。可以通过更改**fold**参数的值来改变。

💡 默认情况下，表格按照“准确率”（从高到低）的值进行排序。可以通过更改**sort**参数的值来改变。

### 4. 创建模型

在PyCaret的任何模块中创建模型就像编写**create\_model**一样简单。它只需要一个参数，即作为字符串输入的模型名称。该函数返回一个带有k折交叉验证分数和训练好的模型对象的表格。

```
adaboost = **create_model**('ada')
```

![](https://cdn-images-1.medium.com/max/2000/1\*1twQHlWEbUbEtVEas0NQDQ.png)

变量‘adaboost’存储了通过**create\_model**函数返回的训练好的模型对象，它是一个scikit-learn估计器。可以使用变量后面的**period ( . )**访问训练对象的原始属性。请参见下面的示例。

![训练好的模型对象的属性](https://cdn-images-1.medium.com/max/2560/1\*Vlh9B3l6tFwlCNzJBfQEcQ.gif)

💡 PyCaret提供了60多个开源的预训练算法。[点击这里](https://www.pycaret.org/create-model)查看PyCaret中可用的估计器/模型的完整列表。

### 5. 调整模型

**tune\_model**函数用于自动调整机器学习模型的超参数。PyCaret使用预定义的搜索空间进行随机网格搜索。该函数返回一个带有k折交叉验证分数和训练好的模型对象的表格。

```
tuned_adaboost = tune_model('ada')
```

![](https://cdn-images-1.medium.com/max/2000/1\*pqsFYecRxZ\_ruvwBXZWlQA.png)

💡 在无监督模块（如[pycaret.nlp](https://www.pycaret.org/nlp)、[pycaret.clustering](https://www.pycaret.org/clustering)和[pycaret.anomaly](https://www.pycaret.org/anomaly)）中，**tune\_model**函数可以与监督模块一起使用。例如，可以使用PyCaret的NLP模块通过评估监督ML模型（如‘准确率’或‘R2’）来调整“主题数”参数。

### 6. 集成模型

**ensemble\_model**函数用于集成训练好的模型。它只需要一个参数，即训练好的模型对象。该函数返回一个带有k折交叉验证分数和训练好的模型对象的表格。

```
# 创建一个决策树模型
dt = **create_model**('dt')

# 集成一个训练好的决策树模型
dt_bagged = **ensemble_model**(dt)
```

![](https://cdn-images-1.medium.com/max/2000/1\*uw2WmHc1oFeUfnnz-jYfhA.png)

💡 默认情况下，集成使用“Bagging”方法，可以通过在ensemble\_model函数中使用**method**参数将其更改为“Boosting”。

💡 PyCaret还提供了[blend\_models](https://www.pycaret.org/blend-models)和[stack\_models](https://www.pycaret.org/stack-models)功能，用于集成多个训练好的模型。

### 7. 绘制模型

可以使用**plot\_model**函数对训练好的机器学习模型进行性能评估和诊断。它接受训练好的模型对象和绘图类型作为字符串输入。

```
# 创建一个模型
adaboost = **create_model**('ada')

# AUC图
**plot_model**(adaboost, plot = 'auc')

# 决策边界
**plot_model**(adaboost, plot = 'boundary')

# 精确率-召回率曲线
**plot_model**(adaboost, plot = 'pr')

# 验证曲线
**plot_model**(adaboost, plot = 'vc')
```

![](https://cdn-images-1.medium.com/max/2376/1\*JnfDw9wwuGxTDS676\_hBXg.png)

[点击这里](https://www.pycaret.org/plot-model)了解PyCaret中不同可视化的更多信息。

或者，您可以使用**evaluate\_model**函数在笔记本中的用户界面中查看绘图。

```
**evaluate_model**(adaboost)
```

![](https://cdn-images-1.medium.com/max/2560/1\*TMuREzi-o7\_edYCj4yIZfA.gif)

💡 **plot\_model**函数在**pycaret.nlp**模块中可用于可视化_文本语料库_和_语义主题模型_。[点击这里](https://pycaret.org/plot-model/#nlp)了解更多信息。

### 8. 解释模型
当数据中的关系是非线性的时候，通常在现实生活中我们会发现基于树的模型比简单的高斯模型表现得更好。然而，这是以失去解释性为代价的，因为树模型不像线性模型那样提供简单的系数。PyCaret使用**interpret_model**函数来实现[SHAP（SHapley Additive exPlanations）](https://shap.readthedocs.io/en/latest/)。

```python
# 创建模型
xgboost = **create_model**('xgboost')

# 模型摘要图
**interpret_model**(xgboost)

# 相关性图
**interpret_model**(xgboost, plot='correlation')
```

![](https://cdn-images-1.medium.com/max/2000/1\*ct0UFJA2sxTpSTwSwO1-fQ.png)

可以使用“reason”图来评估测试数据集中特定数据点（也称为原因参数）的解释。在下面的示例中，我们正在检查测试数据集中的第一个实例。

```python
**interpret_model**(xgboost, plot='reason', observation=0) 
```

![](https://cdn-images-1.medium.com/max/2184/1\*hsM128hQ2sDk9TnTHBH9Bw.png)

### 9. 预测模型

到目前为止，我们看到的结果都是基于训练数据集（默认为70%）上的k折交叉验证。为了看到模型在测试/留出数据集上的预测和性能，我们使用**predict_model**函数。

```python
# 创建模型
rf = **create_model**('rf')

# 预测测试/留出数据集
rf_holdout_pred **= predict_model**(rf)
```

![](https://cdn-images-1.medium.com/max/2000/1\*e05Sd2KFexSjxORcaxAeFw.png)

**predict_model**函数还可以预测未见过的数据集。目前，我们将使用与训练相同的数据集作为新的未见过的数据集的代理。在实践中，**predict_model**函数将被迭代使用，每次使用一个新的未见过的数据集。

```python
predictions = **predict_model**(rf, data=diabetes)
```

![](https://cdn-images-1.medium.com/max/2200/1\*TZwr8fI9cNqluSwnDa4IfA.png)

💡 **predict_model**函数还可以预测由[stack_models](https://www.pycaret.org/stack-models)和[create_stacknet](https://www.pycaret.org/classification/#create-stacknet)函数创建的顺序模型链。

💡 **predict_model**函数还可以直接从托管在AWS S3上的模型进行预测，使用[deploy_model](https://www.pycaret.org/deploy-model)函数。

### 10. 部署模型

利用训练好的模型在未见过的数据集上生成预测的一种方法是在与训练模型的笔记本/IDE中使用**predict_model**函数。然而，在未见过的数据集上进行预测是一个迭代的过程；根据使用情况，进行预测的频率可以从实时预测到批量预测不等。PyCaret的**deploy_model**函数允许从笔记本环境将整个流程包括训练好的模型部署到云端。

```python
**deploy_model**(model=rf, model_name='rf_aws', platform='aws', 
             authentication={'bucket': 'pycaret-test'})
```

### 11. 保存模型/保存实验

一旦训练完成，包含所有预处理转换和训练好的模型对象的整个流程可以保存为二进制pickle文件。

```python
# 创建模型
adaboost = **create_model**('ada')

# 保存模型
save_model**(adaboost, model_name='ada_for_deployment')
```

![](https://cdn-images-1.medium.com/max/2000/1\*sW7Vn\_mPiH-TWaJ3cZgE8Q.png)

您还可以将包含所有中间输出的整个实验保存为一个二进制文件。

```python
**save_experiment**(experiment_name='my_first_experiment')
```

![](https://cdn-images-1.medium.com/max/2000/1\*GFLvTgyzESXgy1SytG45xQ.png)

💡 您可以使用PyCaret的所有模块中提供的**load_model**和**load_experiment**函数加载保存的模型和实验。

### 12. 下一个教程

在下一个教程中，我们将展示如何在Power BI中使用训练好的机器学习模型生成批量预测，以在实际生产环境中使用。

请还参阅我们的初学者级笔记本，了解这些模块的内容：

[回归](https://www.pycaret.org/reg101) [聚类](https://www.pycaret.org/clu101) [异常检测](https://www.pycaret.org/anom101) [自然语言处理](https://www.pycaret.org/nlp101) [关联规则挖掘](https://www.pycaret.org/arul101)

### 开发计划中有什么？

我们正在积极改进PyCaret。我们未来的开发计划包括一个新的**时间序列预测**模块，与**TensorFlow**的集成以及对PyCaret的可扩展性的重大改进。如果您想分享您的反馈并帮助我们进一步改进，您可以在网站上[填写此表单](https://www.pycaret.org/feedback)或在我们的[GitHub](http://www.github.com/pycaret/)或[LinkedIn](https://www.linkedin.com/company/pycaret/)页面上留言。

### 想了解特定模块的内容吗？
截至第一个版本1.0.0，PyCaret有以下模块可供使用。点击下面的链接查看文档和工作示例。

- [分类](https://www.pycaret.org/classification)
- [回归](https://www.pycaret.org/regression)
- [聚类](https://www.pycaret.org/clustering)
- [异常检测](https://www.pycaret.org/anomaly-detection)
- [自然语言处理](https://www.pycaret.org/nlp)
- [关联规则挖掘](https://www.pycaret.org/association-rules)

### 重要链接

- [用户指南/文档](https://www.pycaret.org/guide)
- [Github代码库](http://www.github.com/pycaret/pycaret)
- [安装PyCaret](https://www.pycaret.org/install)
- [Notebook教程](https://www.pycaret.org/tutorial)
- [为PyCaret做贡献](https://www.pycaret.org/contribute)

如果你喜欢PyCaret，请在我们的Github仓库上给我们一个⭐️。

要了解更多关于PyCaret的信息，请关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)和[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI_B3g)。