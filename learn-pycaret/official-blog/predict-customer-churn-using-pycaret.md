# 使用 PyCaret 预测客户流失

### 使用 PyCaret 正确地预测客户流失

#### 一步一步指南，教你如何使用 PyCaret 正确地预测客户流失，从而优化业务目标并提高投资回报率

![使用 PyCaret 正确地预测客户流失 — 图片由作者提供](https://cdn-images-1.medium.com/max/2630/1\*mu45A-psfPHTIM1F\_nUXBw.png)

### **介绍**

对于以订阅模式为基础的公司来说，客户保留率是主要的关键绩效指标之一。在 SaaS 市场，竞争尤为激烈，客户可以自由选择众多供应商。一次不好的体验可能导致客户转向竞争对手，从而导致客户流失。

### **什么是客户流失？**

客户流失是指在一定时间内停止使用公司产品或服务的客户所占的百分比。计算流失率的一种方法是将在给定时间间隔内失去的客户数量除以期初活跃客户的数量。例如，如果你有1000个客户，上个月失去了50个客户，那么你的月度流失率为5%。

预测客户流失是一个具有挑战性但极其重要的业务问题，特别是在客户获取成本（CAC）高的行业，如技术、电信、金融等。能够预测某个特定客户流失的风险，并在还有时间采取措施的情况下，为公司带来巨大的额外潜在收入。

### 客户流失机器学习模型在实践中如何使用？

客户流失预测模型的主要目标是通过与高风险流失客户积极互动来保留客户。例如：提供礼品券或任何促销定价，并将他们锁定一年或两年以延长他们对公司的生命周期价值。

这里有两个要理解的概念：

* 我们希望客户流失预测模型能够提前预测流失（比如提前一个月、三个月甚至六个月——这完全取决于使用情况）。这意味着你必须非常小心截止日期，即在机器学习模型中不应使用截止日期之后的任何信息作为特征，否则会造成信息泄漏。截止日期之前的时期被称为**事件**。
* 通常情况下，对于客户流失预测，你需要做一些工作来创建一个_**目标列**_，它通常不以你想要的形式提供。例如，你想预测客户是否会在下一个季度流失，因此你将遍历截止日期之后的所有活跃客户，并检查他们是否在下一个季度离开了公司（是为1，否为0）。在这种情况下，这个季度被称为**性能窗口**。

![如何创建客户流失数据集 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*yNaRKOY1ZjTF59U1LRnQ0g.png)

### 客户流失模型工作流程

现在你了解了数据的来源和如何创建流失目标（这是问题中最具挑战性的部分之一），让我们讨论一下这个机器学习模型在业务中的使用方式。从左到右阅读下面的图表：

* 在客户流失历史数据上训练模型（事件期间的X特征和目标变量的性能窗口）。
* 每个月将活跃客户基数传递给**机器学习预测模型**，以返回每个客户流失的概率（在业务术语中，有时称为流失分数）。
* 列表将按照概率值（或分数）从高到低排序，客户保留团队将开始与客户互动，以阻止流失，通常是通过提供某种促销或礼品卡来锁定更多年份。
* 预测模型对于流失概率非常低（或者基本上模型预测没有流失）的客户来说，他们是满意的客户。对他们不采取任何行动。

![客户流失模型工作流程 — 图片由作者提供](https://cdn-images-1.medium.com/max/2598/1\*V\_Yiyl5iWIC6mRXEiTC0Qg.png)

### 让我们从实际示例开始

在本节中，我将演示完整的机器学习模型训练和选择、超参数调整、分析和结果解释的端到端工作流程。我还将讨论可以优化的指标以及为什么传统指标如AUC、准确率、召回率可能不适用于客户流失模型。我将使用 [PyCaret](https://www.pycaret.org) —— 一个开源的、低代码的机器学习库来执行这个实验。本教程假设你具备基本的 PyCaret 知识。

### PyCaret
[PyCaret](https://www.pycaret.org/) 是一个开源的、低代码的机器学习库和用于自动化机器学习工作流的端到端模型管理工具，使用 Python 构建。PyCaret 以其易用性、简洁性和快速高效地构建和部署端到端机器学习流程的能力而闻名。要了解更多关于 PyCaret 的信息，请访问他们的 [GitHub](https://www.github.com/pycaret/pycaret)。

![PyCaret 的特点 — 图片由作者提供](https://cdn-images-1.medium.com/max/2084/0\*KAZzGooA90037WgZ.png)

### 安装 PyCaret

```
**# 安装 pycaret
**pip install pycaret
```

### 👉 数据集

在本教程中，我使用了一个来自 Kaggle 的 [电信客户流失](https://www.kaggle.com/blastchar/telco-customer-churn) 数据集。数据集已经包含了我们可以直接使用的目标列。你可以从这个 [GitHub](https://raw.githubusercontent.com/srees1988/predict-churn-py/main/customer\_churn\_data.csv) 链接直接读取这个数据集。（感谢 srees1988）

```
**# 导入库**
import pandas as pd
import numpy as np

**# 读取 csv 数据
**data **= **pd.read_csv('[https://raw.githubusercontent.com/srees1988/predict-churn-py/main/customer_churn_data.csv'](https://raw.githubusercontent.com/srees1988/predict-churn-py/main/customer_churn_data.csv'))
```

![样本数据集 — 图片由作者提供](https://cdn-images-1.medium.com/max/2702/1\*mN9rTN4VxjI5opbTbnSZmQ.png)

### **👉 探索性数据分析**

```
**# 检查数据类型
**data.dtypes
```

![数据类型 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*S7mP3s8pykG4EBCBnXlAGw.png)

注意，TotalCharges 的数据类型是对象类型而不是 float64。经过调查，我发现该列中有一些空格，导致 Python 将数据类型强制为对象。为了解决这个问题，我们需要在更改数据类型之前去除空格。

```
**# 用 np.nan 替换空格**
data['TotalCharges'] = data['TotalCharges'].replace(' ', np.nan)

**# 转换为 float64**
data['TotalCharges'] = data['TotalCharges'].astype('float64')
```

直观地说，合同类型、续约时长（客户停留时间）和定价计划在客户流失或保留方面非常重要。让我们探索一下它们之间的关系：

![客户流失与续约时长、费用和合同类型的关系（图片由作者提供）](https://cdn-images-1.medium.com/max/2406/1\*I-h0lJJnHADHpk7rOUlYOg.png)

注意，大多数流失发生在“月付”合同中。这是合理的。此外，我还可以看到随着续约时长和总费用的增加，具有高续约时长和低费用的客户的可能性较低，而具有高续约时长和高费用的客户的可能性较高。

**缺失值**

```
**# 检查缺失值
**data.isnull().sum()
```

![缺失值 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*HP-9ncnzxgURBYwl6cG9DA.png)

注意，由于我们用 np.nan 替换了空值，现在 TotalCharges 列中有 11 行缺失值。没问题 — 我将让 PyCaret 自动填充它。

### **👉 数据准备**

在 PyCaret 的所有模块中，setup 是在进行任何机器学习实验之前的第一个且唯一必需的步骤。这个函数负责在训练模型之前进行所有的数据准备工作。除了执行一些基本的默认处理任务外，PyCaret 还提供了各种预处理功能。要了解有关 PyCaret 中所有预处理功能的更多信息，可以查看这个 [链接](https://pycaret.org/preprocessing/)。

```
**# 初始化 setup**
from pycaret.classification import *
s = setup(data, target = 'Churn', ignore_features = ['customerID'])
```

![pycaret.classification 中的 setup 函数 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*w-tUxTQ4p0tDhgYv8DbNOg.png)

每当你在 PyCaret 中初始化 setup 函数时，它会对数据集进行分析，并推断出所有输入特征的数据类型。在这种情况下，你可以看到除了 tenure、MonthlyCharges 和 TotalCharges 之外，其他所有特征都是分类的，这是正确的，你现在可以按回车键继续。如果数据类型没有被正确推断（有时会发生），你可以使用 numeric\_feature 和 categorical\_feature 来覆盖数据类型。

此外，注意我在 setup 函数中传递了 ignore\_features = \['customerID']，这样在训练模型时它就不会被考虑。这样做的好处是 PyCaret 不会从数据集中删除该列，它只会在模型训练时在后台忽略它。因此，在生成最终的预测时，你不需要担心自己手动连接 ID。

![setup 的输出 — 为了显示而截断 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*Y2N6dU1qvJFwOyTmTbgoCw.png)

### 👉 模型训练与选择
现在数据准备工作已完成，让我们通过使用 compare_models 功能开始训练过程。该函数训练模型库中的所有算法，并使用交叉验证评估多个性能指标。

```
**# 比较所有模型**
best_model = compare_models(sort='AUC')
```

![compare_models 的输出 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*eHvFHPaXU0IQoshg2\_5rlw.png)

基于 AUC，最佳模型是梯度提升分类器。使用 10 折交叉验证的 AUC 为 0.8472。

```
**# 打印最佳模型参数**
print(best_model)
```

![最佳模型参数 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*6TQiy5iPNmCYM0DolQ8Mjg.png)

### 超参数调优

您可以使用 PyCaret 中的 tune_model 函数自动调整模型的超参数。

```
**# 调整最佳模型**
tuned_best_model = tune_model(best_model)
```

![tune_model 的结果 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*NY4wOQYZB0l3V2FkScz8xQ.png)

注意，AUC 从 0.8472 稍微提高到 0.8478。

### 模型分析

```
**# AUC 曲线**
plot_model(tuned_best_model, plot = 'auc')
```

![AUC 曲线 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*QJNvMQmXxWk4n78VHF0jHQ.png)

```
**# 特征重要性图**
plot_model(tuned_gbc, plot = 'feature')
```

![特征重要性图 — 图片由作者提供](https://cdn-images-1.medium.com/max/2440/1\*tahIWrTGqWG-KJTe3MvF5g.png)

```
**# 混淆矩阵**
plot_model(tuned_best_model, plot = 'confusion_matrix')
```

![梯度提升分类器的混淆矩阵 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*5qW6fHWqXi-BPkhxYzcQmg.png)

这个混淆矩阵是针对包含我们数据 30% 的测试集（2,113 行）的。我们有 309 个真正例（15%）- 这些是我们将能够延长寿命价值的客户。如果我们没有预测到，那么就没有干预的机会。

我们还有 138 个假正例（7%），我们将因为向这些客户提供的促销只是额外成本而亏钱。

1,388 个（66%）是真负例（好客户），278 个（13%）是假负例（这是一个被错过的机会）。

到目前为止，我们已经训练了多个模型，选择了给出最高 AUC 的最佳模型，然后调整了最佳模型的超参数，以在 AUC 方面获得更多性能。然而，最佳 AUC 不一定转化为最适合业务的模型。

在流失模型中，通常真正例的回报与假正例的成本迥然不同。让我们使用以下假设：

- 对所有被识别为流失的客户提供 1,000 美元代金券（真正例 + 假正例）；
- 如果我们能够阻止流失，我们将获得 5,000 美元的客户寿命价值。

利用这些假设和上面的混淆矩阵，我们可以计算该模型的美元影响：

![$ 模型对 2,113 位客户的影响 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*FcLFdEVYe3Y9XWYj144Dgw.png)

这是一个不错的模型，但问题在于它不是一个商业智能模型。与没有模型相比，它的表现相当不错，但我们如何训练和选择最大化业务价值的模型呢。为了实现这一点，我们必须使用业务指标而不是任何传统指标（如 AUC 或准确性）来训练、选择和优化模型。

### 👉 在 PyCaret 中添加自定义指标

感谢 PyCaret，使用 add_metric 函数可以非常容易地实现这一点。

```
**# 创建自定义函数
**def calculate_profit(y, y_pred):
    tp = np.where((y_pred==1) & (y==1), (5000-1000), 0)
    fp = np.where((y_pred==1) & (y==0), -1000, 0)
    return np.sum([tp,fp])

**# 将指标添加到 PyCaret
**add_metric('profit', 'Profit', calculate_profit)
```

现在让我们运行 compare_models 看看魔法。

```
**# 比较所有模型**
best_model = compare_models(sort='Profit')
```

![compare_models 的输出 — 图片由作者提供](https://cdn-images-1.medium.com/max/2046/1\*J1eZLSvWgk67Fe4EdxUwAQ.png)

注意，这次添加了一个名为 Profit 的新列，令人惊讶的是朴素贝叶斯在利润方面是最佳模型，尽管在 AUC 方面表现很差。让我们看看：

```
**# 混淆矩阵**
plot_model(best_model, plot = 'confusion_matrix')
```

![朴素贝叶斯的混淆矩阵 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*WKTgO9-KhST6KK4d9qdH4Q.png)

客户总数仍然相同（测试集中的 2,113 位客户），改变的是模型如何在假正例和假负例上出现错误。让我们用美元价值来衡量一下，使用相同的假设（如上所述）：

![$ 模型对 2,113 位客户的影响 — 图片由作者提供](https://cdn-images-1.medium.com/max/2000/1\*tqM7173iOXcLToPgy9WgUQ.png)
## \*\*\*砰！\*_我们刚刚通过一个比最佳模型低2%的模型增加了约40万美元的利润。这是怎么发生的呢？首先，AUC或其他开箱即用的分类指标（准确率、召回率、精确率、F1、Kappa等）并不是一个商业智能的指标，因此它没有考虑到风险和回报的问题。添加一个自定义指标并将其用于模型选择或优化是一个很好的想法，也是正确的做法。

我希望您能欣赏 PyCaret 的简单易用性。只需几行代码，我们就能训练多个模型并选择对业务有意义的模型。我是一名常驻博主，主要写关于 PyCaret 及其在实际世界中的应用案例。如果您希望自动收到通知，可以关注我的 [Medium](https://medium.com/@moez-62905)、[LinkedIn](https://www.linkedin.com/in/profile-moez/) 和 [Twitter](https://twitter.com/moezpycaretorg1)。

![PyCaret — Image by Author](https://cdn-images-1.medium.com/max/2412/0\*PLdJpNCTXdttEn8W.png)

![PyCaret — Image by Author](https://cdn-images-1.medium.com/max/2402/0\*IvqhUYDstXqz55eF.png)

使用这个轻量级的 Python 工作流自动化库，您可以实现无限的可能。如果您觉得这个工具有用，请不要忘记在我们的 GitHub 仓库上给我们一个⭐️。

要了解更多关于 PyCaret 的信息，请关注我们的 [LinkedIn](https://www.linkedin.com/company/pycaret/) 和 [Youtube](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)。

加入我们的 Slack 频道。邀请链接在[这里](https://join.slack.com/t/pycaret/shared\_invite/zt-p7aaexnl-EqdTfZ9U\~mF0CwNcltffHg)。

### 重要链接

[文档](https://pycaret.readthedocs.io/en/latest/installation.html) [博客](https://medium.com/@moez\_62905) [GitHub](http://www.github.com/pycaret/pycaret) [StackOverflow](https://stackoverflow.com/questions/tagged/pycaret) [安装 PyCaret ](https://pycaret.readthedocs.io/en/latest/installation.html)[Notebook 教程 ](https://pycaret.readthedocs.io/en/latest/tutorials.html)[为 PyCaret 做贡献](https://pycaret.readthedocs.io/en/latest/contribute.html)

### 更多与 PyCaret 相关的教程：

[**在 Alteryx 中使用 PyCaret 进行机器学习** _一步一步教程，使用 PyCaret 在 Alteryx Designer 中训练和部署机器学习模型_towardsdatascience.com](https://towardsdatascience.com/machine-learning-in-alteryx-with-pycaret-fafd52e2d4a) [**在 KNIME 中使用 PyCaret 进行机器学习** _一步一步指南，使用 PyCaret 在 KNIME 中训练和部署端到端的机器学习流程_towardsdatascience.com](https://towardsdatascience.com/machine-learning-in-knime-with-pycaret-420346e133e2) [**使用 PyCaret + MLflow 进行简单的 MLOps** _一个适合初学者的、一步一步的教程，介绍如何使用 PyCaret 在机器学习实验中集成 MLOps_towardsdatascience.com](https://towardsdatascience.com/easy-mlops-with-pycaret-mlflow-7fbcbf1e38c6) [**使用 PyCaret 编写和训练自定义机器学习模型** towardsdatascience.com](https://towardsdatascience.com/write-and-train-your-own-custom-machine-learning-models-using-pycaret-8fa76237374e) [**使用 PyCaret 构建，使用 FastAPI 部署** \*一步一步、适合初学者的教程，介绍如何使用 PyCaret 和 FastAPI 构建端到端的机器学习流程_towardsdatascience.com](https://towardsdatascience.com/build-with-pycaret-deploy-with-fastapi-333c710dc786) [**使用 PyCaret 进行时间序列异常检测** _一步一步的教程，介绍如何使用 PyCaret 对时间序列数据进行无监督的异常检测_towardsdatascience.com](https://towardsdatascience.com/time-series-anomaly-detection-with-pycaret-706a6e2b2427) [**使用 PyCaret 和 Gradio 提升机器学习实验** _一步一步的教程，快速开发和交互式地与机器学习流程进行交互_towardsdatascience.com](https://towardsdatascience.com/supercharge-your-machine-learning-experiments-with-pycaret-and-gradio-5932c61f80d9) [**使用 PyCaret 进行多时间序列预测** _一步一步的教程，使用 PyCaret 对多个时间序列进行预测_towardsdatascience.com](https://towardsdatascience.com/multiple-time-series-forecasting-with-pycaret-bc0a779a22fe)