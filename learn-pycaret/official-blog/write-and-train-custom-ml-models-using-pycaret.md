# 使用 PyCaret 编写和训练自定义机器学习模型

### 使用 PyCaret 编写和训练您自己的自定义机器学习模型

#### 一份逐步指导，适合初学者的教程，教您如何在 PyCaret 中编写和训练自定义机器学习模型

![Rob Lambert 摄于 Unsplash](https://cdn-images-1.medium.com/max/10368/0\*dERI0tdhD\_Yay4OU)

### PyCaret

PyCaret 是一个开源的、低代码机器学习库和端到端模型管理工具，使用 Python 构建，用于自动化机器学习工作流程。它因易用性、简单性以及快速高效地构建和部署端到端机器学习原型而广受欢迎。

PyCaret 是一个替代低代码库，可以用少量代码行取代数百行代码。这使得实验周期指数级加快和更加高效。

PyCaret **简单且** **易于使用**。在 PyCaret 中执行的所有操作都按顺序存储在一个**Pipeline**中，该 Pipeline 完全自动化用于\*\*部署。\*\*无论是填补缺失值、独热编码、转换分类数据、特征工程，甚至是超参数调整，PyCaret 都可以自动化完成。

本教程假定您具有一些关于 PyCaret 的先前知识和经验。如果您以前没有使用过，没问题 — 您可以通过这些教程快速入门：

* [PyCaret 2.2 来了 — 有什么新功能](https://towardsdatascience.com/pycaret-2-2-is-here-whats-new-ad7612ca63b)
* [宣布 PyCaret 2.0](https://towardsdatascience.com/announcing-pycaret-2-0-39c11014540e)
* [关于 PyCaret 你不知道的五件事](https://towardsdatascience.com/5-things-you-dont-know-about-pycaret-528db0436eec)

### 安装 PyCaret

安装 PyCaret 非常简单，只需几分钟即可完成。我们强烈建议使用虚拟环境，以避免与其他库可能发生的冲突。

PyCaret 的默认安装是一个精简版本的 pycaret，只安装了硬依赖项[在此处列出](https://github.com/pycaret/pycaret/blob/master/requirements.txt)。

```
**# 安装精简版本（默认）
**pip install pycaret

**# 安装完整版本**
pip install pycaret[full]
```

当您安装 pycaret 的完整版本时，还会安装所有可选依赖项，如[此处列出](https://github.com/pycaret/pycaret/blob/master/requirements-optional.txt)。

### 👉 让我们开始吧

在开始讨论自定义模型训练之前，让我们快速演示一下 PyCaret 如何使用现成模型。我将使用 PyCaret 仓库中提供的 'insurance' 数据集。该数据集的目标是基于一些属性预测患者费用。

### 👉 **数据集**

```
**# 从 pycaret 仓库读取数据
**from pycaret.datasets import get_data
data = get_data('insurance')
```

![insurance 数据集中的样本行](https://cdn-images-1.medium.com/max/2000/1\*h4UQIEtUUmsP2ybJlsFKPA.png)

### 👉 **数据准备**

在 PyCaret 的所有模块中都是通用的，setup 是在 PyCaret 中执行的任何机器学习实验中的第一个且唯一必需的步骤。此函数负责在训练模型之前进行所有必要的数据准备。除了执行一些基本的默认处理任务外，PyCaret 还提供了各种预处理功能。要了解 PyCaret 中所有预处理功能的更多信息，您可以查看此[链接](https://pycaret.org/preprocessing/)。

```
**# 初始化 setup
**from pycaret.regression import *
s = setup(data, target = 'charges')
```

![pycaret.regression 模块中的 setup 函数](https://cdn-images-1.medium.com/max/2736/1\*s5bhY1IN1jOM11JDuwFaZw.png)

每当您在 PyCaret 中初始化 setup 函数时，它会对数据集进行分析，并推断所有输入特征的数据类型。如果所有数据类型都被正确推断，您可以按 Enter 继续。

![setup 的输出 — 为显示而截断](https://cdn-images-1.medium.com/max/2000/1\*Y9kIg0BfRfzG1WdZm6MnbQ.png)

### 👉 可用模型

要查看所有可用于训练的模型列表，可以使用名为 models 的函数。它显示一个包含模型 ID、名称和实际估计器引用的表格。

```
**# 检查所有可用模型
**models()
```

![models() 的输出 — 为显示目的而截断](https://cdn-images-1.medium.com/max/2000/1\*JVVVA2aUyVBJ9SyQjEC13A.png)

### 👉 模型训练与选择

在 PyCaret 中训练任何模型最常用的函数是 create\_model。它接受您想要训练的估计器的 ID。

```
**# 训练决策树
**dt = create_model('dt')
```

![create\_model(‘dt’) 的输出](https://cdn-images-1.medium.com/max/2000/1\*-txdluhP3Jl27ZUvoMzdow.png)

输出显示了 10 折交叉验证指标的均值和标准差。此函数的输出是一个训练好的模型对象，本质上是一个 scikit-learn 对象。

```
print(dt)
```
![打印出的 dt](https://cdn-images-1.medium.com/max/2000/1\*7z-sMZdcNVamc1U-PFzVXg.png)

要在循环中训练多个模型，您可以编写一个简单的列表推导式：

```
**# 训练多个模型**
multiple_models = [create_model(i) for i in ['dt', 'lr', 'xgboost']]

**# 检查 multiple_models
**type(multiple_models), len(multiple_models)
>>> (list, 3)

print(multiple_models)
```

![打印出的 multiple\_models](https://cdn-images-1.medium.com/max/2734/1\*OO\_I-wbH7h4PseoHOQ36Vg.png)

如果您想训练库中所有可用的模型而不是仅选择的几个，您可以使用 PyCaret 的 compare\_models 函数，而不是编写自己的循环（尽管结果将是相同的）。

```
**# 比较所有模型**
best_model = compare_models()
```

![compare\_models 函数的输出](https://cdn-images-1.medium.com/max/2000/1\*9XtwGLvLDmJ5ro2fq67HLQ.png)

compare\_models 返回的输出显示了所有模型的交叉验证指标。根据这个输出，Gradient Boosting Regressor 是最佳模型，在训练集上使用 10 折交叉验证的平均绝对误差（MAE）为 $2,702。

```
**# 检查最佳模型**
print(best_model)
```

![打印出的 best\_model](https://cdn-images-1.medium.com/max/2000/1\*pQSXoclIKREi-2U2uG3jhQ.png)

上面网格中显示的指标是交叉验证分数，要检查 best\_model 在留出集上的分数：

```
**# 在留出集上预测
**pred_holdout = predict_model(best_model)
```

![predict\_model(best\_model) 函数的输出](https://cdn-images-1.medium.com/max/2000/1\*G1l1bG1f\_Tzoeo7X\_ieixw.png)

要在未见过的数据集上生成预测，可以使用相同的 predict\_model 函数，只需传递一个额外的参数 data：

```
**# 创建数据副本并删除目标列
**data2 = data.copy()
data2.drop('charges', axis=1, inplace=True)

**# 生成预测
**predictions = predict_model(best_model, data=data2)
```

![predict\_model(best\_model, data=data2) 的输出](https://cdn-images-1.medium.com/max/2000/1\*jch\_dJNscn\_i2vNfgWpV5g.png)

### 👉 编写和训练自定义模型

到目前为止，我们已经看到了 PyCaret 中所有可用模型的训练和模型选择。然而，PyCaret 用于自定义模型的方式与此完全相同。只要您的估算器与 sklearn API 风格兼容，它将以相同的方式工作。让我们看几个例子。

在展示如何编写自定义类之前，我将首先演示如何处理自定义非 sklearn 模型（即不在 sklearn 或 pycaret 基础库中的模型）。

#### 👉 **GPLearn 模型**

虽然遗传编程（GP）可用于执行[各种各样的任务](http://www.genetic-programming.org/combined.php)，但 gplearn 专门用于解决符号回归问题。

符号回归是一种机器学习技术，旨在识别最能描述关系的基础数学表达式。它通过构建一组天真的随机公式来开始，以表示已知自变量与其因变量目标之间的关系，以预测新数据。然后，通过从种群中选择适应度最强的个体进行遗传操作，每一代程序都是从前一代进化而来的。

要使用 gplearn 中的模型，您首先必须安装它：

```
**# 安装 gplearn
**pip install gplearn
```

现在，您只需导入未经训练的模型并将其传递给 create\_model 函数：

```
**# 导入未经训练的估算器
**from gplearn.genetic import SymbolicRegressor
sc = SymbolicRegressor()

**# 使用 create_model 进行训练
**sc_trained = create_model(sc)
```

![create\_model(sc\_trained) 的输出](https://cdn-images-1.medium.com/max/2000/1\*WjHjXcM\_Q4w7zuM\_nfzVng.png)

```
print(sc_trained)
```

![print(sc\_trained) 的输出](https://cdn-images-1.medium.com/max/2000/1\*1glIGPLohn6bxElYSmgtuQ.png)

您还可以检查这个模型的留出分数：

```
**# 检查留出分数
**pred_holdout_sc = predict_model(sc_trained)
```

![predict\_model(sc\_trained) 的输出](https://cdn-images-1.medium.com/max/2000/1\*EoTHf4G1wm8Zh0xqScS\_Gg.png)

#### 👉 NGBoost 模型

ngboost 是一个实现自然梯度提升的 Python 库，如[“NGBoost: 自然梯度提升用于概率预测”](https://stanfordmlgroup.github.io/projects/ngboost/)中所述。它构建在[Scikit-Learn](https://scikit-learn.org/stable/)之上，旨在在选择适当的评分规则、分布和基础学习器方面具有可伸缩性和模块化性。关于 NGBoost 方法背后的教学介绍可在这个[幻灯片](https://drive.google.com/file/d/183BWFAdFms81MKy6hSku8qI97OwS\_JH\_/view?usp=sharing)中找到。

要使用 ngboost 中的模型，您首先必须安装 ngboost：
```
**# 安装 ngboost**
pip install ngboost
```
安装完成后，您可以从ngboost库中导入未经训练的估计器，并使用create_model来训练和评估模型：

```python
# 导入未经训练的估计器
from ngboost import NGBRegressor
ng = NGBRegressor()

# 使用create_model进行训练
ng_trained = create_model(ng)
```

![create_model(ng)的输出结果](https://cdn-images-1.medium.com/max/2000/1\*6GjuFIPOBN4f19Qj7YPMlw.png)

```python
print(ng_trained)
```

![print(ng_trained)的输出结果](https://cdn-images-1.medium.com/max/2000/1\*SSZoHUK4NnLE2Ri\_Uy985Q.png)

#### 👉 编写自定义类

上述两个例子gplearn和ngboost是pycaret的自定义模型，因为它们不在默认库中，但您可以像使用其他开箱即用的模型一样使用它们。然而，可能会有一种情况需要编写自己的算法（即算法背后的数学原理），在这种情况下，您可以继承sklearn的基类，并编写自己的数学方法。

让我们创建一个简单的估计器，在拟合阶段学习目标变量的均值，并对所有新数据点预测相同的均值，而不考虑X输入（在现实生活中可能没有用，只是为了演示功能）。

```python
# 创建自定义估计器
import numpy as np
from sklearn.base import BaseEstimator

class MyOwnModel(BaseEstimator):
    
    def __init__(self):
        self.mean = 0
        
    def fit(self, X, y):
        self.mean = y.mean()
        return self
    
    def predict(self, X):
        return np.array(X.shape[0]*[self.mean])
```

现在让我们使用这个估计器进行训练：

```python
# 导入MyOwnModel类
mom = MyOwnModel()

# 使用create_model进行训练
mom_trained = create_model(mom)
```

![create_model(mom)的输出结果](https://cdn-images-1.medium.com/max/2000/1\*ElBm8PRRTYgkCQ7J6tsf\_g.png)

```python
# 在数据上生成预测
predictions = predict_model(mom_trained, data=data)
```

![predict_model(mom, data=data)的输出结果](https://cdn-images-1.medium.com/max/2000/1\*SE8aSw-Rhj41PYzRQxHWQw.png)

请注意，Label列（即预测值）对于所有行来说都是相同的数字$13,225，这是因为我们以这种方式创建了这个算法，它从训练集的均值中学习并预测相同的值（只是为了保持简单）。

我希望您能欣赏PyCaret的易用性和简洁性。只需几行代码，您就可以进行端到端的机器学习实验，并编写自己的算法，而无需调整任何原生代码。

### 即将推出！

下周我将撰写一个进阶教程来扩展本教程。我们将编写一个更复杂的算法，而不仅仅是均值预测。我将在下一个教程中介绍一些复杂的概念。请在[Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/)和[Twitter](https://twitter.com/moezpycaretorg1)上关注我，以获取更多更新。

使用这个轻量级的Python工作流自动化库，您可以实现无限的可能。如果您觉得这个工具有用，请不要忘记在我们的GitHub存储库上给我们一个⭐️。

要了解更多关于PyCaret的信息，请关注我们的[LinkedIn](https://www.linkedin.com/company/pycaret/)和[Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)。

加入我们的slack频道。邀请链接在[这里](https://join.slack.com/t/pycaret/shared\_invite/zt-p7aaexnl-EqdTfZ9U\~mF0CwNcltffHg)。

### 您可能还对以下内容感兴趣：

[使用PyCaret 2.0在Power BI中构建自己的AutoML](https://towardsdatascience.com/build-your-own-automl-in-power-bi-using-pycaret-8291b64181d) [使用Docker在Azure上部署机器学习流程](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-cloud-using-docker-container-bec64458dc01) [在Google Kubernetes Engine上部署机器学习流程](https://towardsdatascience.com/deploy-machine-learning-model-on-google-kubernetes-engine-94daac85108b) [在AWS Fargate上部署机器学习流程](https://towardsdatascience.com/deploy-machine-learning-pipeline-on-aws-fargate-eb6e1c50507) [构建和部署您的第一个机器学习Web应用](https://towardsdatascience.com/build-and-deploy-your-first-machine-learning-web-app-e020db344a99) [使用AWS Fargate无服务器部署PyCaret和Streamlit应用](https://towardsdatascience.com/deploy-pycaret-and-streamlit-app-using-aws-fargate-serverless-infrastructure-8b7d7c0584c2) [使用PyCaret和Streamlit构建和部署机器学习Web应用](https://towardsdatascience.com/build-and-deploy-machine-learning-web-app-using-pycaret-and-streamlit-28883a569104) [在GKE上部署使用Streamlit和PyCaret构建的机器学习应用](https://towardsdatascience.com/deploy-machine-learning-app-built-using-streamlit-and-pycaret-on-google-kubernetes-engine-fd7e393d99cb)

### 重要链接
[文档](https://pycaret.readthedocs.io/en/latest/installation.html) [博客](https://medium.com/@moez_62905) [GitHub](http://www.github.com/pycaret/pycaret) [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装 PyCaret ](https://pycaret.readthedocs.io/en/latest/installation.html)[Notebook 教程](https://pycaret.readthedocs.io/en/latest/tutorials.html)[为 PyCaret 做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 想了解特定模块吗？

点击下面的链接查看文档和工作示例。

[分类 ](https://pycaret.readthedocs.io/en/latest/api/classification.html)[回归](https://pycaret.readthedocs.io/en/latest/api/regression.html) [聚类](https://pycaret.readthedocs.io/en/latest/api/clustering.html) [异常检测](https://pycaret.readthedocs.io/en/latest/api/anomaly.html) [自然语言处理 ](https://pycaret.readthedocs.io/en/latest/api/nlp.html)[关联规则挖掘](https://pycaret.readthedocs.io/en/latest/api/arules.html)