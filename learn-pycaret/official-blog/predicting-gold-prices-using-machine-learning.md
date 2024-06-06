# 用机器学习预测黄金价格

#### [黄金预测](https://towardsdatascience.com/tagged/gold-price-prediction)

### 用机器学习预测黄金价格

#### 作者：Mohammad Riazuddin

#### 第一部分：导入和准备数据

### 简介

我已经在金融市场领域学习了十多年，研究不同资产类别及它们在不同经济条件下的行为。很难找到比黄金更具极端分化的资产类别。有人热爱黄金，有人憎恨黄金，而且往往他们会永远留在同一阵营。由于黄金本身基本面很少（再次引发分歧），在这个多部分系列中，我将尝试使用几种机器学习技术来预测黄金价格回报。以下是我（目前）设想系列的方式：

_**第一部分：定义方法，收集和准备数据**_

_**第二部分：使用 PyCaret 进行回归建模**_

_**第三部分：使用 PyCaret 进行分类建模**_

_**第四部分：使用 Prophet（Facebook）进行时间序列建模**_

_**第五部分：评估方法的整合**_

> “请注意，黄金是一个在极其竞争激烈的市场中广泛交易的资产。从任何策略中长期稳定赚钱是极其困难，几乎不可能的。本文仅分享我的经验，不是投资或交易的建议或倡导。然而，对于像我这样的领域学生，这个想法可以被扩展和发展成为具有个人努力的交易算法。”

### 背景

黄金一直是人类的原始价值储存和交换媒介，直到几个世纪前纸币取代了它。然而，直到1971年，大多数可持续的纸币都是以黄金为支撑的，当时布雷顿森林协议被废除，世界货币成为真正的‘法定’货币。

然而，黄金继续引起人们的兴趣，不仅作为首选珠宝金属，还作为价值储存和经济困难时的避风港，通常被建议作为多元化投资组合的一部分，因为它往往是有效的通胀对冲工具。

### 方法

在这个系列中，我们将采用不同的方法来预测使用机器学习突出的黄金价格回报

首先，我们将通过回归路线预测未来2周和3周内黄金的回报。我们将利用我认为影响对黄金前景看法的不同工具的历史回报来做到这一点。基本原因是，我将黄金称为‘反应性’资产。它本身基本面很少，价格波动往往是投资者如何看待其他资产类别（股票、商品等）的衍生物。

### 导入数据

为此和后续练习，我们需要过去10年几种工具的收盘价。有各种付费（路透社、彭博社）和免费资源（IEX、Quandl、Yahoo Finance、Google Finance）可用于导入数据。由于这个项目需要不同类型的资产类别（股票、商品、债务和贵金属），我发现**‘**[**yahoofinancials**](https://pypi.org/project/yahoofinancials/)**’**包非常有帮助且简单明了。

```
***#导入库***
import pandas as pd
from datetime import datetime
import matplotlib.pyplot as plt
from yahoofinancials import YahooFinancials
```

我已经准备了一个需要导入数据的工具列表。_**yahoofinancials**_包需要雅虎股票代码。该列表包含股票代码及其描述。包含该列表的 Excel 文件可以在[这里](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Ticker%20List.xlsx)找到，名为‘Ticker List’。我们导入该文件并将股票代码和名称提取为单独的列表。([\*查看笔记本](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Regression/Gold%20Prediction%20Experiment%20%20Regression-%20PyCaret.ipynb)\*)

```
ticker_details = pd.read_excel(“Ticker List.xlsx”)
ticker = ticker_details['Ticker'].to_list()
names = ticker_details['Description'].to_list()
ticker_details.head(20)
```

![](https://cdn-images-1.medium.com/max/2000/1\*8j73ugtxNcnvJt4iwFvnLg.png)

一旦我们有了列表，我们需要定义我们需要导入数据的日期范围。我选择的时间段是2010年1月至2020年3月1日。我之所以没有拉取此前的数据是因为2008-09年的全球金融危机极大地改变了经济和市场格局。那段时期之前的关系现在可能不那么相关。

我们创建一个日期范围，并将其写入一个名为_values_的空数据框，我们将从_yahoofinancials_中提取并粘贴数据。
```
***#创建日期范围并将其添加到值表中***
end_date = "2020-03-01"
start_date = "2010-01-01"
date_range = pd.bdate_range(start=start_date, end=end_date)
values = pd.DataFrame({'日期': date_range})
values['日期'] = pd.to_datetime(values['日期'])
```
一旦我们在数据框中有了日期范围，我们需要使用股票代码从 API 中提取数据。***yahoofinancials*** 以 JSON 格式返回输出。以下代码循环遍历股票代码列表，并仅提取所有历史日期的收盘价，并将其水平合并到数据框中，以日期为键。由于这些资产类别可能具有不同的地区和交易假日，每次数据提取的日期范围可能不同。通过合并，我们最终会有几个 _NAs_，稍后我们将对其进行 _frontfill_。

```
***#从雅虎财经提取数据并使用日期作为键将其添加到 Values 表中
***for i in ticker:
 raw_data = YahooFinancials(i)
 raw_data = raw_data.get_historical_price_data(start_date, end_date, “daily”)
 df = pd.DataFrame(raw_data[i][‘prices’])[[‘formatted_date’,’adjclose’]]
 df.columns = [‘Date1’,i]
 df[‘Date1’]= pd.to_datetime(df[‘Date1’])
 values = values.merge(df,how=’left’,left_on=’Date’,right_on=’Date1')
 values = values.drop(labels=’Date1',axis=1)

***#将列重命名为工具名称，而不是它们的股票代码，以便更容易阅读***
names.insert(0,’Date’)
values.columns = names
print(values.shape)
print(values.isna().sum())


***#填充数据集中的 NaN 值***
values = values.fillna(method="ffill",axis=0)
values = values.fillna(method="bfill",axis=0)
values.isna().sum()

***#将除日期外的所有列强制转换为数值类型***
cols=values.columns.drop('Date')
values[cols] = values[cols].apply(pd.to_numeric,errors='coerce').round(decimals=1)
values.tail()
```

![values 表的尾部](https://cdn-images-1.medium.com/max/2000/1\*n9DrEpRxVpV2rDzJBtsDhg.png)

### 准备数据

在上述方法中，我们强调将使用上市工具的滞后收益来预测黄金的未来收益。在这里，我们继续计算所有工具的短期历史收益和少数选定工具的长期历史收益。

其背后的基本思想是，如果某种资产表现出高度的超额表现或低于预期表现，那么重新平衡投资组合的可能性更大，这将影响其他资产类别的未来收益。*例如：如果股票市场（比如标普500指数）在过去6个月表现出色，资产管理人员可能希望获利并将一些资金分配给贵金属，以应对股票市场的调整。*下图显示了黄金和标普500在不同市场条件下的价格走势和相关性。

![](https://cdn-images-1.medium.com/max/2334/1\*YIsV9sx5qw\_GqWVjA4QF\_Q.png)

![来源：彭博社，ICE基准管理，世界黄金协会。链接](https://cdn-images-1.medium.com/max/2000/1\*x6xtsbW5IqY9m1O1ymhYBQ.jpeg)

从上面可以看出，当标普500指数出现极端下跌时，黄金呈现负相关性。最近股市的大幅下跌也突显了类似的关系，黄金在预期下跌中上涨，年初至今涨幅为11%，而标普500指数年初至今亏损11%。

然而，我们将使用机器学习来评估这个假设。您可以直接从我的 [git-hub 仓库](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Training%20Data\_Values.csv) 下载值数据，文件名为 "Training Data\_Values"。

```
imp = [‘Gold’,’Silver’, ‘Crude Oil’, ‘S&P500’,’MSCI EM ETF’]

***# 计算短期历史收益***
change_days = [1,3,5,14,21]

data = pd.DataFrame(data=values[‘Date’])
for i in change_days:
 print(data.shape)
 x= values[cols].pct_change(periods=i).add_suffix(“-T-”+str(i))
 data=pd.concat(objs=(data,x),axis=1)
 x=[]
print(data.shape)

***# 计算长期历史收益***
change_days = [60,90,180,250]

for i in change_days:
 print(data.shape)
 x= values[imp].pct_change(periods=i).add_suffix(“-T-”+str(i))
 data=pd.concat(objs=(data,x),axis=1)
 x=[]
print(data.shape)
```

除了滞后收益之外，我们还可以看到当前黄金价格与其不同窗口的移动平均线的距离有多远。这是技术分析中非常常用的指标，其中移动平均线为资产价格提供支撑和阻力。我们使用简单移动平均线和指数移动平均线的组合。然后将这些移动平均线添加到现有的特征空间中。
```
***#计算黄金的移动平均线***
moving_avg = pd.DataFrame(values['Date'], columns=['Date'])
moving_avg['Date'] = pd.to_datetime(moving_avg['Date'], format='%Y-%b-%d')
***#添加简单移动平均线***
moving_avg['Gold/15SMA'] = (values['Gold'] / (values['Gold'].rolling(window=15).mean())) - 1
moving_avg['Gold/30SMA'] = (values['Gold'] / (values['Gold'].rolling(window=30).mean())) - 1
moving_avg['Gold/60SMA'] = (values['Gold'] / (values['Gold'].rolling(window=60).mean())) - 1
moving_avg['Gold/90SMA'] = (values['Gold'] / (values['Gold'].rolling(window=90).mean())) - 1
moving_avg['Gold/180SMA'] = (values['Gold'] / (values['Gold'].rolling(window=180).mean())) - 1

***#添加指数移动平均线***
moving_avg['Gold/90EMA'] = (values['Gold'] / (values['Gold'].ewm(span=90, adjust=True, ignore_na=True).mean())) - 1
moving_avg['Gold/180EMA'] = (values['Gold'] / (values['Gold'].ewm(span=180, adjust=True, ignore_na=True).mean())) - 1
moving_avg = moving_avg.dropna(axis=0)
print(moving_avg.shape)
moving_avg.head(20)
```
![移动平均数据框的输出](https://cdn-images-1.medium.com/max/2000/1\*nPd7K-2XXRt\_e9m3K4pP4g.png)

```
***#将移动平均值合并到特征空间中***
data['Date'] = pd.to_datetime(data['Date'], format='%Y-%b-%d')
data = pd.merge(left=data, right=moving_avg, how='left', on='Date')
print(data.shape)
data.isna().sum()
```

这些都是关于特征的内容。现在我们需要创建目标，即我们想要预测的内容。由于我们正在预测回报率，我们需要选择一个预测回报率的时间段。我选择了14天和22天的时间段，因为其他较小的时间段往往非常波动且缺乏预测能力。然而，您也可以尝试其他时间段。

```
***#计算目标的前向回报***
y = pd.DataFrame(data=values['Date'])
y['Gold-T+14'] = values["Gold"].pct_change(periods=-14)
y['Gold-T+22'] = values["Gold"].pct_change(periods=-22)
print(y.shape)
y.isna().sum()

***#删除缺失值***

data = data[data['Gold-T-250'].notna()]
y = y[y['Gold-T+22'].notna()]

***#添加目标变量***
data = pd.merge(left=data, right=y, how='inner', on='Date', suffixes=(False, False))
print(data.shape)
```

现在我们已经准备好完整的数据集来开始建模。在下一部分中，我们将使用极具创新性和高效的 PyCaret 库尝试不同的算法。我还将展示如何创建一个流水线来持续导入新数据并使用训练好的模型生成预测。

### 使用机器学习预测黄金价格

#### 第二部分：使用 PyCaret 进行回归建模

在第一部分中，我们讨论了从开源免费 API 导入数据并将其准备为适合我们预期的机器学习练习的方式。您可以参考第一部分的代码，或者从[github 仓库](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Training%20Data.csv)中导入名为“Training Data”的最终数据集。

PyCaret 是一个开源的 Python 机器学习库，可在任何笔记本环境中使用，并大大减少编码工作量，使过程极其高效和高产。在下面的部分中，我们将看到如何使用\*\*\*[PyCaret](https://pycaret.org/)\*\*\*来加速任何机器学习实验。首先，您需要使用以下命令安装 PyCaret：

```
!pip install pycaret
```

#### 22 天模型

我们选择 22 天的时间段作为目标。这意味着，根据历史数据，我们将尝试预测未来三周黄金的回报率。

```
***#如果您正在导入已下载的数据集***
data = pd.read_csv("Training Data.csv")

from pycaret.regression import *

***#我们有两个目标列。我们将删除 T+14 天的目标
***data_22 = data.drop(['Gold-T+14'], axis=1)
```

**设置**

在 PyCaret 中开始任何建模练习的第一步是使用“setup”函数。此处的必需变量是数据集和数据集中的目标标签。所有基本和必要的数据转换，如删除 ID，对分类因子进行独热编码和缺失值插补，都在幕后自动完成。PyCaret 还提供了超过 20 种预处理选项。在本例中，我们将使用基本的设置，并在后续实验中尝试不同的预处理技术。

```
a = setup(data_22, target='Gold-T+22',
          ignore_features=['Date'], session_id=11,
          silent=True, profile=False);
```

在上面的代码中，数据集传递为“data_22”，目标指向标记为“Gold-T+22”的列。我特意提到要忽略“Date”列，以防止 PyCaret 在日期列上创建基于时间的特征，这在其他情况下可能非常有用，但我们现在不评估这一点。如果您想查看变量之间的分布和相关性，可以保留参数“profile=True”，这将显示一个 Panda 分析器输出。我有意地提供了“session_id=11”以便能够重新创建结果。

**神奇的命令....**_**compare\_models( )**_

在下一步中，我将使用 PyCaret 的一个我最喜欢的功能，它可以将数百行代码缩减为只有 2 个单词——“compare\_models”。该函数使用所有算法（目前有 25 个）并将它们拟合到数据中，运行 10 折交叉验证，并为每个模型生成 6 个评估指标。所有这些只需 2 个单词。在函数中可以使用的两个额外参数是为了节省时间：

**a. turbo=False** — 默认为 True。当 turbo=True 时，compare models 不评估一些更耗时的算法，即 Kernel Ridge (kr)、Automatic Relevance Determination (ard) 和 Multi-level Perceptron (mlp)

**b. blacklist** — 在这里，可以传递算法缩写的列表（请参阅文档字符串），这些算法已知需要更长的时间，并且性能提升很小。例如：下面我已经将 Theilsen Regressor (tr) 加入黑名单

```
compare_models(blacklist=['tr'], turbo=True)
```
![compare_models的输出](https://cdn-images-1.medium.com/max/2000/1\*qyHVCx4o4J7DfvHByLWcvA.png)

在这里，我们将使用R-Square（R2）作为首选的度量标准。我们看到ET、Catboost和KNN是排名前三的模型。接下来，我们将调整这三个模型的超参数。

**调整模型超参数**

PyCaret为每个算法都预定义了一个网格，_**tune\_model()**_ 函数使用随机网格搜索来找到优化度量选择的参数集（这里是Rsquare），并显示优化模型的交叉验证分数。它不接受训练好的模型，并且需要传递作为字符串的估计器的缩写。我们将调整Extra Tree（et）、K最近邻（knn）和CatBoost（catboost）回归器。

```
et_tuned = tune_model('et')
```

![](https://cdn-images-1.medium.com/max/2000/1\*Uyr9_ZMPPj1ymb7qQLVEVw.png)

```
catb_tuned = tune_model('catboost')
```

![](https://cdn-images-1.medium.com/max/2000/1\*n_Ss1NREh9Bgjcmveb3jIw.png)

```
knn_tuned = tune_model('knn',n_iter=150)

*#我增加了knn中的迭代次数，因为增加迭代次数已经显示出对knn性能的改善，而不会显著增加训练时间。*
```

![](https://cdn-images-1.medium.com/max/2000/1*jNwXyUhJAiWNr2BTb63eEA.png)

我们可以看到，调整后knn的R2显著提高到了87.86%，远高于et和catboost，在调整后没有改善。这可能是由于网格搜索过程中的随机性。在一些非常高的迭代次数下，它们可能会有所改善。

我还会创建一个基本的Extra Tree（et）模型，因为它的原始性能（调整前）与调整后的knn非常接近。我们将使用PyCaret中的_**create\_model()**_ 函数来创建模型。

```
et = create_model('et')
```

**评估模型**

对训练好的模型进行一些模型诊断是很重要的。我们将使用PyCaret中的\*\*\*evaluate\_model() \*\*\*函数来查看一系列图表和其他诊断信息。它接受一个训练好的模型，返回一系列模型诊断图和模型定义。我们将对我们的前两个模型knn\_tuned和et进行模型诊断。

![Cook's Distance Plot knn\_tuned](https://cdn-images-1.medium.com/max/2000/1*vecSoo26snJEaae8bi8OzA.png)

从上图中，我们可以清楚地看到在前500个观测中有很多异常值，这不仅影响了模型性能，还可能影响未来模型的泛化能力。因此，删除这些异常值可能是值得的。但在我们这样做之前，我们将通过et（knn不提供特征重要性）来查看特征重要性。

![](https://cdn-images-1.medium.com/max/2000/1*98uU-bVryZsx-ew8k52bJg.png)

我们看到，白银回报和新兴市场ETF的特征重要性很高，这强调了白银和黄金经常成对运动，而投资组合配置确实在新兴市场股票和黄金之间转移。

**删除异常值**

要删除异常值，我们需要回到设置阶段，并使用PyCaret内置的异常值移除器，然后再次创建模型以查看影响。

```
b=setup(data_22,target='Gold-T+22', ignore_features=['Date'], session_id=11,silent=True,profile=False,remove_outliers=True);
```

如果\*\*\*'remove\_outliers' \*\*\*参数设置为true，PyCaret将通过使用奇异值分解（SVD）技术进行PCA线性降维来识别异常值。默认的异常水平为5%。这意味着它将删除5%的观测，它认为是异常值。

删除异常值后，我们再次运行我们的顶级模型，看是否有性能改善，显然是有的。

![](https://cdn-images-1.medium.com/max/2000/1*XV-SkGw8rAQgNGZZc9p_CA.png)

![删除异常值后的et和knn\_tuned结果](https://cdn-images-1.medium.com/max/2000/1*Iq5k-FAxyNo9NC_FxB74nw.png)

我们看到，et的性能从85.43%提高到了86.16%，而knn\_tuned的性能从87.86%提高到了88.3%。在交叉验证中，折叠间的标准差也有所减少。

**集成模型**

我们还可以尝试看看装袋/提升是否可以提高模型性能。我们可以使用PyCaret中的\*\*\*ensemble\_model() \*\*\*函数快速查看集成方法如何通过以下代码改善结果：

```
et_bagged = ensemble_model(et,method='Bagging')
knn_tuned_bagged = ensemble_model(knn_tuned, method='Bagging')
```

上述代码将显示类似的交叉验证分数，没有显示出太大的改善。结果可以在存储库中的笔记本链接中查看。

**混合模型**
我们可以将前两个模型（et和knn\_tuned）进行混合，看看混合模型是否能够表现得更好。通常情况下，混合模型往往能够学习到不同的模式，并且它们共同具有更好的预测能力。我将使用PyCaret的blend\_models()函数来实现这一点。它接受一个训练好的模型列表，并返回一个混合模型和10折交叉验证的得分。

```
blend_knn_et = blend_models(estimator_list=[knn_tuned,et])
```

![混合模型的结果](https://cdn-images-1.medium.com/max/2000/1\*KBFj0LhJQrw8tI1frwPntg.png)

从上表中我们可以看到，knn\_tuned和et的混合模型的平均R2值比两者都要好。平均R2值增加了1.9%，R2的标准差减小了，这意味着在不同的折叠中性能更好且更一致。

平均R2值为90.2%，这意味着我们的模型能够从我们提供的特征中平均捕捉到90.2%的黄金回报率的变化。

**堆叠模型**

虽然混合模型的结果很好，但我想看看是否有可能从数据中提取出更多的R2基点。为了做到这一点，我们将构建一个多层次的堆叠模型。这与混合模型不同，因为模型的层次是按顺序堆叠的，即来自一层模型的预测与原始特征一起传递给下一层模型（如果restack = True）。一组模型的预测对后续模型的预测有很大帮助。链的最后是元模型（默认为线性模型）。PyCaret指南对此主题有更多详细信息。在笔记本中，我尝试了几种架构。下面是表现最好的架构：

```
stack2 = create_stacknet(estimator_list=[[catb,et,knn_tuned],[blend_knn_et]], restack=True)
```

![stack2（多层堆叠）的结果](https://cdn-images-1.medium.com/max/2000/1\*C0bE6lZdqnuM1sQ4N1idpg.png)

如上所示，stack2模型的R2值比blend\_knn\_et模型高1%，我们将选择stack2作为最佳模型并保存它以进行预测。

**保存模型**

模型训练完成后，我们需要保存模型以便在新数据上使用它进行预测。我们可以通过save\_model()来实现这一点。这将模型保存在当前目录或任何定义的路径中。下面的代码将保存模型和预处理流程，并将其命名为'22Day Regressor'。

```
save_model(model=stack2, model_name='22Day Regressor')
```

**在新数据上进行预测**

一旦我们保存了模型，我们就希望能够在新数据上进行预测。我们可以依赖yahoofinancials包来获取所有工具的收盘价格，但是我们需要再次准备新数据以便使用模型。准备新数据的步骤与准备训练数据时类似，唯一的区别是我们将导入最新的数据，并且不会创建标签（因为我们没有未来的价格）。以下代码块应该导入和整理数据，使其准备好进行预测。
```python
***#导入库***
import pandas as pd
from datetime import datetime
import matplotlib.pyplot as plt
from yahoofinancials import YahooFinancials

ticker_details = pd.read_excel("Ticker List.xlsx")
ticker = ticker_details['Ticker'].to_list()
names = ticker_details['Description'].to_list()

***#准备日期范围***
end_date= datetime.strftime(datetime.today(),'%Y-%m-%d')
start_date = "2019-01-01"
date_range = pd.bdate_range(start=start_date,end=end_date)
values = pd.DataFrame({ 'Date': date_range})
values['Date']= pd.to_datetime(values['Date'])

***#从Yahoo Finance提取数据，并使用日期作为键将其添加到Values表中***
for i in ticker:
    raw_data = YahooFinancials(i)
    raw_data = raw_data.get_historical_price_data(start_date, end_date, "daily")
    df = pd.DataFrame(raw_data[i]['prices'])[['formatted_date','adjclose']]
    df.columns = ['Date1',i]
    df['Date1']= pd.to_datetime(df['Date1'])
    values = values.merge(df,how='left',left_on='Date',right_on='Date1')
    values = values.drop(labels='Date1',axis=1)

***#将列名重命名为工具名称，而不是它们的股票代码，以便更容易阅读***
names.insert(0,'Date')
values.columns = names

***#前向填充数据集中的NaN值***
values = values.fillna(method="ffill",axis=0)
values = values.fillna(method="bfill",axis=0)

***#将所有列的数据类型强制转换为数值型，除了日期列***
cols=values.columns.drop('Date')
values[cols] = values[cols].apply(pd.to_numeric,errors='coerce').round(decimals=1)
imp = ['Gold','Silver', 'Crude Oil', 'S&P500','MSCI EM ETF']

***#计算短期历史收益率***
change_days = [1,3,5,14,21]

data = pd.DataFrame(data=values['Date'])
for i in change_days:
    x= values[cols].pct_change(periods=i).add_suffix("-T-"+str(i))
    data=pd.concat(objs=(data,x),axis=1)
    x=[]

***#计算长期历史收益率***
change_days = [60,90,180,250]

for i in change_days:
    x= values[imp].pct_change(periods=i).add_suffix("-T-"+str(i))
    data=pd.concat(objs=(data,x),axis=1)
    x=[]

***#计算黄金的移动平均线***
moving_avg = pd.DataFrame(values['Date'],columns=['Date'])
moving_avg['Date']=pd.to_datetime(moving_avg['Date'],format='%Y-%b-%d')
moving_avg['Gold/15SMA'] = (values['Gold']/(values['Gold'].rolling(window=15).mean()))-1
moving_avg['Gold/30SMA'] = (values['Gold']/(values['Gold'].rolling(window=30).mean()))-1
moving_avg['Gold/60SMA'] = (values['Gold']/(values['Gold'].rolling(window=60).mean()))-1
moving_avg['Gold/90SMA'] = (values['Gold']/(values['Gold'].rolling(window=90).mean()))-1
moving_avg['Gold/180SMA'] = (values['Gold']/(values['Gold'].rolling(window=180).mean()))-1
moving_avg['Gold/90EMA'] = (values['Gold']/(values['Gold'].ewm(span=90,adjust=True,ignore_na=True).mean()))-1
moving_avg['Gold/180EMA'] = (values['Gold']/(values['Gold'].ewm(span=180,adjust=True,ignore_na=True).mean()))-1
moving_avg = moving_avg.dropna(axis=0)

***#将移动平均值合并到特征空间***
data['Date']=pd.to_datetime(data['Date'],format='%Y-%b-%d')
data = pd.merge(left=data,right=moving_avg,how='left',on='Date')
data = data[data['Gold-T-250'].notna()]
prediction_data = data.copy()
```
一旦数据准备就绪，我们需要加载模型并进行预测。要加载模型，我们将再次使用 PyCaret 的回归模块。下面的代码将加载模型，在新数据上进行预测，并为数据集中的每个日期提供历史价格、预期收益和未来3周的预测价格。

```python
from pycaret.regression import *

***#加载已存储的模型
***regressor_22 = load_model("22Day Regressor");

***#进行预测
***predicted_return_22 = predict_model(regressor_22, data=prediction_data)
predicted_return_22 = predicted_return_22[['Date','Label']]
predicted_return_22.columns = ['Date','Return_22']

***#将预测收益添加到金价数值中***
predicted_values = values[['Date','Gold']]
predicted_values = predicted_values.tail(len(predicted_return_22))
predicted_values = pd.merge(left=predicted_values, right=predicted_return_22, on=['Date'], how='inner')
predicted_values['Gold-T+22'] = (predicted_values['Gold'] * (1 + predicted_values['Return_22'])).round(decimals=1)

***#添加 T+22 日期
***from datetime import datetime, timedelta

predicted_values['Date-T+22'] = predicted_values['Date'] + timedelta(days=22)
predicted_values.tail()
```

![](https://cdn-images-1.medium.com/max/2000/1\*OKAIYzGtwmhzagdQ1rIXlQ.png)

上表显示，2020年4月17日金价收盘价为1,694.5美元，模型预测在接下来的22天内，收益率将为-2.3%，导致到2020年5月9日金价目标为1,655美元。我为预测创建了一个名为 _**“Gold Prediction New Data — Regression”**_ 的单独笔记本，可以在此处的存储库中找到 [链接](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Regression/Gold%20Prediction%20New%20Data%20-%20Regression.ipynb)。

我们可以使用相同的概念和技术进行 T+14 天的预测。代码和输出可以在 Jupyter 笔记本标题为 _**“Gold Prediction Experiment Regression — PyCaret”**_ 的存储库中找到 [链接](https://github.com/Riazone/Gold-Return-Prediction/blob/master/Regression/Gold%20Prediction%20Experiment%20%20Regression-%20PyCaret.ipynb)。

### 重要链接

_**第三部分链接 —**_ [_**预测金价崩盘**_](https://towardsdatascience.com/predicting-crashes-in-gold-prices-using-machine-learning-5769f548496)

_**Github 存储库链接**_ [_**Github Repository**_](https://github.com/Riazone/Gold-Return-Prediction)

_**关注我**_ [_**LinkedIn**_](https://www.linkedin.com/in/riazuddin-mohammad/)

_**PyCaret 指南**_ [_**Guide to PyCaret**_](https://pycaret.org/guide/)