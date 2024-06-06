# 使用 PyCaret 预测黄金价格的崩盘

#### [黄金预测](https://towardsdatascience.com/tagged/gold-price-prediction)

### 使用机器学习预测黄金价格的崩盘

#### 作者：Mohammad Riazuddin

#### 黄金预测系列的第三部分。使用 PyCaret 逐步指南预测黄金价格的崩盘。

![黄金价格走势](https://cdn-images-1.medium.com/max/3666/0\*etQrkJsMtNUP9EWE.png)

### 方法

在黄金预测系列的前两部分中，我们讨论了如何从免费的 yahoofinancials API 导入数据，并构建回归模型来预测黄金在两个时间段内的回报，即14天和22天。

在本部分中，我们将尝试预测接下来的22天内黄金价格是否会出现“急剧下跌”或“崩盘”。我们将使用分类技术进行这个实验。我们还将学习如何使用训练好的模型每天对新数据进行预测。这个练习的步骤如下：

1. **导入和整理数据** - 这与第一部分中所解释的类似。您还可以从我的 git [repo](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Training%20Data.csv) 下载最终数据集。
2. **定义**“急剧下跌”的黄金价格。急剧下跌不是一个绝对的度量。我们将尝试客观地定义“急剧下跌”。
3. **创建标签** - 基于“急剧下跌”的定义，我们将在历史数据上创建标签。
4. 训练模型来预测“急剧下跌”，并使用训练好的模型对新数据进行预测。

### 准备数据

这部分与第一部分完全相同。[notebook](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Classification/Gold%20Prediction%20Experiment%20%20Classification-%20PyCaret.ipynb) 包含了数据导入和处理的完整代码块，或者您可以直接加载从这个链接[here](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Training%20Data.csv)下载的数据集。

### 定义“急剧下跌”

任何分类问题都需要标签。在这里，我们需要通过定义和量化“急剧下跌”来创建标签。

为了定义“急剧”，我定义了一个阈值，使得任何时间窗口（这里是22天和14天）的回报低于该阈值的概率为15%（基本上是正态分布的左尾，p=0.15）。为此，我需要假设回报的分布是正态的。从回报的分布来看，这是一个非常合理的假设。

![](https://cdn-images-1.medium.com/max/2000/1\*hjdmU6aYxcPQfqQ0QncN5w.png)

![14天和22天窗口的回报直方图。非常接近正态分布](https://cdn-images-1.medium.com/max/2000/1\*LiI4fq4eFHkKzvdXamWfxA.png)

为了得到14天和22天窗口的阈值回报水平，我首先定义了分布的左尾的 p 值，这个 p 值在这种情况下为15%。使用这个 p 值，我们从标准正态分布中得到了 -1.0364 的 z 值。以下代码将为我们完成这个操作。

```
import scipy.stats as st
#选择阈值 p（左尾概率）
p = 0.15
#获取 z 值
z = st.norm.ppf(p)
print(z)
```

现在，基于上述 z 值和每个窗口的回报的均值和标准差，我们将得到阈值回报水平。14天和22天期间的前瞻性回报在“data”中的列“Gold-T+14”和“Gold-T+22”中。

```
#计算每个 Y 的阈值（t）
t_14 = round((z * np.std(data["Gold-T+14"])) + np.mean(data["Gold-T+14"]), 5)
t_22 = round((z * np.std(data["Gold-T+22"])) + np.mean(data["Gold-T+22"]), 5)

print("t_14=", t_14)
print("t_22=", t_22)

t_14= -0.03733
t_22= -0.04636
```

因此，14天窗口的阈值回报水平为-0.0373或-3.73％，22天窗口的阈值回报水平为-0.0463或-4.63％。这意味着，只有15％的概率14天回报低于-3.73％，22天回报低于-4.63％。这是类似于风险价值（Value At Risk，VAR）计算中使用的概念。

### 创建标签

我们将使用上述阈值水平创建标签。任何两个窗口中低于相应阈值的回报将被标记为1，否则标记为0。

```
#创建标签
data['Y-14'] = (data['Gold-T+14'] < t_14) * 1
data['Y-22'] = (data['Gold-T+22'] < t_22) * 1
print("Y-14", sum(data['Y-14']))
print("Y-22", sum(data['Y-22']))

Y-14 338
Y-22 356
```

在总共的2,379个实例中，有338个实例的14天回报低于-3.73％的阈值，以及356个实例的22天回报低于-4.63％的阈值。

一旦我们有了这些标签，我们实际上不需要回报列，因此我们删除了实际的回报列。

```
data = data.drop(['Gold-T+14', 'Gold-T+22'], axis=1)
data.head()
```

### 使用 PyCaret 进行建模
#### 22天窗口

我们将从这里开始讨论22天窗口。在这里，我将使用PyCaret的分类模块进行实验。

我们从PyCaret中导入上述模块，然后删除14天的标签，因为我们在这里使用的是22天的窗口。就像在回归中一样，为了开始分类练习，我们需要运行_setup()_命令来指定数据和目标列。请记住，PyCaret会在后台处理所有基本的预处理工作。

```python
from pycaret.classification import *

data_22 = data.drop(['Y-14'],axis=1)
s22 = setup(data=data_22, target='Y-22', session_id=11, silent=True);
```

为了评估所有模型的集合，我们将运行_compare_models()_命令，将_turbo_设置为_False_，因为我想评估库中当前可用的所有模型。

```python
compare_models(turbo=False)
```

![Compare Models Output](https://cdn-images-1.medium.com/max/2832/1\*Wmvq7DeuVtmcMdkpz4B94w.png)

在选择模型之前，我们需要了解哪个指标对我们最有价值。在分类实验中选择指标取决于业务问题。精确率和召回率之间总是存在权衡。这意味着我们必须选择并偏向于真正例和假负例之间的平衡。

在这里，最终模型将用于为投资者/分析师创建一个警告标志，告诉他可能会发生崩盘的可能性。然后，投资者将决定对可能的下跌进行对冲。因此，非常重要的是模型能够预测出所有/大部分的剧烈下跌。换句话说，我们希望选择一个具有更好的真正例（更好的召回率）能力的模型，即使这意味着会有一些假正例（较低的精确率）。换句话说，我们不希望模型错过\*‘sharp fall’\*的可能性。我们可以承受一些假正例，因为如果模型预测将会有一个剧烈下跌，并且投资者对冲了他的头寸，但是下跌没有发生，投资者将损失保持投资的机会成本，或者最多是对冲成本（比如说，如果他购买了虚值看跌期权）。这个成本将低于假负例的成本，即模型预测没有_‘Sharp Fall’_，但是确实发生了一个大幅下跌。然而，我们需要注意\*\*精确率\*\*和\*\*AUC\*\*之间的权衡。

我们将继续创建四个模型，即MLP分类器（_mlp_）、Extra-Tree分类器（_et_）、Cat Boost分类器（_catb_）和Light Gradient Boosting Machine（_lgbm_），它们具有最佳的\*\*召回率\*\*和合理的\*\*AUC/精确率\*\*。

![](https://cdn-images-1.medium.com/max/2000/1\*OUz2ZUmbZLTYwgFn0m3Hvw.png)

![](https://cdn-images-1.medium.com/max/2000/1\*AFDkbDD6Mxv7YDz1G\_5rbg.png)

![](https://cdn-images-1.medium.com/max/2000/1\*kJUEp\_A989tlTGHiaFsz2g.png)

![](https://cdn-images-1.medium.com/max/2000/1\*LYnj\_G6F7yx4x5JmYOdcxA.png)

根据我们的结果，MLP分类器似乎是最佳选择，具有最高的召回率和非常不错的\*\*94.7%的AUC\*\*。

**超参数调整**

一旦我们有了我们想要进一步研究的前四个模型，我们需要找到这些模型的最佳超参数。PyCaret有一个非常方便的\*\*\*tune\_model()\*\*\*函数，它通过10折交叉验证循环遍历预定义的超参数网格，以找到我们模型的最佳参数。PyCaret使用标准的\*\*随机网格\*\*搜索来迭代参数。迭代次数（_n\_iter_）可以根据计算能力和时间限制指定为一个较高的数字。在\*\*tune\_function()\*\*中，PyCaret还允许我们指定要优化的指标。默认值是准确率，但我们也可以选择其他指标。在这里，我们选择**召回率**，因为这是我们想要增加/优化的指标。

![CatBoost Tuned Output](https://cdn-images-1.medium.com/max/2000/1\*D8\_Opw0chItS8Z1HB8OCVA.png)

上面的代码将Cat-Boost分类器调整为通过在定义的网格上迭代50次来优化\*\*‘Recall’\*\*，并显示每个折叠的6个指标。我们可以看到，平均召回率从基本Cat-Boost的\*\*58.2%\*\*提高到了这里的\*\*62.6%\*\*。这是一个巨大的提升，在调整阶段不太常见。然而，它仍然低于我们之前创建的基本\*\*‘mlp’\*\*的\*\*66.6%\*\*。对于其他三个模型，我们没有看到通过调整来提高性能的情况（在[notebook](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Classification/Gold%20Prediction%20Experiment%20%20Classification-%20PyCaret.ipynb)中有示例）。原因是参数迭代的随机性质。然而，肯定存在一个参数，它的性能等于或超过基本模型，但是为了达到这个目标，我们需要进一步增加_n\_iter_，这意味着更多的计算时间。

所以目前我们的前四个模型是：
![](https://cdn-images-1.medium.com/max/2000/1\*ok58GJ9EY2YGkhRrhBdpVg.png)

#### **评估模型**

在继续之前，让我们评估模型的性能。我们将利用 PyCaret 的 _**evaluate\_model()**_ 函数来评估并突出显示获胜模型的重要方面。

**混淆矩阵**

![](https://cdn-images-1.medium.com/max/2000/1\*W7dfGY1PnVEepgq9Y4LzgA.png)

**特征重要性**

由于 _mlp_ 和 _catb\_tuned_ 不提供特征重要性，我们将使用 \*lgbm\* 来查看我们预测中最重要的特征：

![](https://cdn-images-1.medium.com/max/2000/1\*k9IZNrOlsfgiT4jm27HXOA.png)

我们可以看到过去180天的黄金回报是最重要的因素。这也是直觉的，因为如果黄金价格在过去大幅上涨，它们下跌/修正的机会更高，反之亦然。接下来的3个特征是银的250、60和180天的回报。同样，银和黄金是两种最广泛交易且相关的贵金属，因此这种关系非常直观。

#### **集成模型**

在调整模型的超参数之后，我们可以尝试集成方法来提高性能。我们可以尝试两种集成方法 [‘Bagging’ 和 ‘Boosting’](https://towardsdatascience.com/ensemble-methods-bagging-boosting-and-stacking-c9214a10a205) 。不提供概率估计的模型不能用于 Boosting。因此，我们只能使用 ‘lgbm’ 和 ‘et’ 进行 Boosting。对于其他模型，我们尝试了 Bagging，以查看性能是否有所提高。以下是代码快照和10折交叉验证结果。

![](https://cdn-images-1.medium.com/max/2000/1\*lknUUSTHIjRxQ8s0\_BRohQ.png)

![集成结果](https://cdn-images-1.medium.com/max/2000/1\*aGIg3u79djyiWCE9poQ-BA.png)

如上所示，这两个模型的结果并没有改善。对于其他模型，性能也有所下降（查看 [notebook](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Classification/Gold%20Prediction%20Experiment%20%20Classification-%20PyCaret.ipynb)）。因此，我们的获胜模型仍然保持不变。

**混合模型**

混合模型基本上是在估计器之上构建投票分类器。对于提供预测概率的模型，我们可以使用软投票（使用它们的概率），而对于其他模型，我们将使用硬投票。_**blend\_model()**_ 函数默认使用硬投票，可以手动更改。我构建了两个混合模型，看看是否可以提取一些额外的性能。

![](https://cdn-images-1.medium.com/max/2000/1\*UwqjMhfu0JtQJKfCOV1VYw.png)

![](https://cdn-images-1.medium.com/max/2000/1\*dokZi-hH-1jL2ifHmyAjow.png)

尽管没有任何模型能够取代 _**‘mlp’**_ 的首位，但很有趣的是看到第二个混合模型，_**‘blend2’**_，它是 _**‘lgbm’**_ 和 _**‘et’**_ 的软组合。在 **Recall** 和 **AUC** 上的表现为 **62.25%** 和 **97.43%**，高于单独使用 _**‘lgbm’**_ 和 _**‘et’**_。这展示了混合模型的好处。现在我们的获胜模型将减少到3个。

![](https://cdn-images-1.medium.com/max/2000/1\*owcSPrycYFxoW9KrfrJ96g.png)

[\*\*创建 StackNet](https://www.coursera.org/lecture/competitive-data-science/stacknet-s8RLi)\*\*

[堆叠模型](https://www.geeksforgeeks.org/stacking-in-machine-learning/) 是一种方法，其中我们允许一个模型（或一组模型）在一层中的预测被用作后续层的特征，最终，元模型被允许在前一层的预测和原始特征（如果 _restack=True_）上进行训练以进行最终预测。PyCaret 对此有一个非常简单的实现，我们可以使用 _**stack\_model()**_ 构建一个具有一层和一个元模型的堆叠，或者使用 _**create\_stacknet()**_ 构建具有多层和一个元模型的堆叠。我使用了不同的组合来构建不同的堆叠并评估性能。

![堆叠-1 的结果](https://cdn-images-1.medium.com/max/2000/1\*d6PR6Hj2ZYrBFAifh-KSvw.png)
在第一个堆栈“stack1”中，我使用了性能较差的模型“catb_tuned”和“blend2”作为第一层的模型，将它们的预测结果传递给领导模型“mlp”，以帮助它进行预测，而“mlp”的预测结果则由元模型（默认为逻辑回归）用于最终预测。由于逻辑回归在完整数据上表现不佳（参见比较模型结果），我使用了“restack=False”，这意味着只有前面模型的预测结果传递给后续的估计器，而不是原始特征。我们在这里看到的简直就像是魔术一样。召回率从我们最好的模型“mlp”的66.63%跃升到了77.8%，AUC和精确度也是如此。显然，“stack1”绝对比我们之前构建的所有模型都要优秀。当然，我还尝试了其他配置，以查看是否可以获得更好的性能。而且我确实找到了一个更好的配置：

![Stack 3的结果](https://cdn-images-1.medium.com/max/2000/1\*n5ZY0N\_c4kBaSVpxn78siw.png)

上面的堆栈“stack3”在召回率方面取得了最大的成功，平均为80.7%，比我们的优胜模型“mlp”高出14%，而在准确度（95%，提高了3%）、AUC（97.34%，提高了2.5%）和精确度（86.28%，提高了7.7%）方面并没有牺牲。

我们可以使用这个模型对我们在设置阶段分离出来的测试数据（总观测量的30%）进行预测。我们可以通过使用“predict_model()”来实现。

![](https://cdn-images-1.medium.com/max/2000/1\*lJGUuf\_sa1xsaaCB58QG0w.png)

我们可以看到模型在测试数据上的表现甚至更好，召回率达到了86.9%。

由于我们将“stack3”作为领导模型，我们将在整个数据上拟合模型（包括测试数据）并保存模型以便对新数据进行预测。

```
classifier_22 = finalize_model(stack3)

save_model(classifier_22,”22D Classifier”)
```

上面的代码在整个数据上拟合模型，并使用“save_model()”函数将训练好的模型和预处理流程保存在一个名为“22D Classifier”的pkl文件中，该文件位于我的活动目录中。这个保存的模型可以并且将被用来对新数据进行预测。

#### 对新数据进行预测

为了进行预测，我们需要导入原始价格数据，就像我们在练习开始时所做的那样，以提取特征，然后加载模型进行预测。唯一的区别是，我们将导入数据直到最后一个交易日，以对最新数据进行预测，就像在现实生活中一样。名为“Gold Prediction New Data — Classification”的笔记本在仓库中展示了数据导入、准备和预测代码。

我们将跳过数据导入和准备部分（详细信息请查看笔记本），看一下预测过程。

```
from pycaret.classification import *
***#加载存储的模型***
classifier_22 = load_model(“22D Classifier”);
***#进行预测***
prediction = predict_model(classifier_22,data=prediction_data)
```

使用“predict_model()”，我们可以将加载的模型应用于新的数据集以生成预测（1或0）和得分（与预测相关的概率）。“prediction”数据框还将包含我们提取的所有特征。

![预测结果](https://cdn-images-1.medium.com/max/2000/1\*MW9Qm\_DdhnQFonTPD-meeg.png)

从预测的标签和得分列来看，该模型没有预测到任何一天的显著下跌。例如，根据4月29日的历史回报，模型预测在接下来的22天内黄金价格显著下跌的可能性不大，因此标签为0，概率仅为9.19%。

### 结论

所以在这里，我已经介绍了创建一个分类器的步骤，以预测黄金价格在接下来的22天内是否会显著下跌。笔记本中还包含了14天模型的代码。您可以尝试创建标签，并以类似的方式预测不同时间窗口内的类似下跌。到目前为止，我们已经创建了一个[回归模型](https://towardsdatascience.com/machine-learning-to-predict-gold-price-returns-4bdb0506b132)和一个分类模型。将来，我们将尝试使用分类模型的预测结果作为回归问题的特征，并查看是否可以提高回归的性能。

### 重要链接

[\*\*\*Git-hub 仓库](https://github.com/Riazone/Gold-Return-Prediction/tree/master/Classification)\*\*\*
[***第一部分和第二部分 — 回归***](https://towardsdatascience.com/machine-learning-to-predict-gold-price-returns-4bdb0506b132)

[***PyCaret***](https://pycaret.org/)

[***我的 LinkedIn 个人资料***](https://www.linkedin.com/in/riazuddin-mohammad/)