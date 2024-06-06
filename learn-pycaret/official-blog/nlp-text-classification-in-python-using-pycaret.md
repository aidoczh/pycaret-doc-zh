# 使用 PyCaret 在 Python 中进行 NLP 文本分类

### 在 Python 中进行 NLP 文本分类：PyCaret 方法与传统方法的比较

#### 传统方法与 PyCaret 方法的比较分析

#### 作者：Prateek Baghel

### I. 引言

在本文中，我们将演示使用两种不同方法解决 NLP 分类问题：

**1-传统方法**：在这种方法中，我们将：

* 使用不同的 NLP 技术预处理给定的文本数据
* 使用不同的嵌入技术嵌入处理后的文本数据
* 在嵌入的文本数据上构建来自多个 ML 家族的分类模型 - 查看不同模型的性能，然后调整一些选定模型的超参数
* 最后，查看调整后模型的性能。显然，在 Python 中这样做意味着编写数百行代码，可能需要至少两到三个小时的时间。

**2-PyCaret 方法**：一种新方法，我们在其中使用低代码 Python 库 PyCaret 来执行上述传统方法中的所有操作，但我们只需编写不到 30 行代码，就能在几分钟内获得结果和见解。

为了让您了解这两种方法之间的差异，请看下面这张粗略的比较表：

![](https://cdn-images-1.medium.com/max/2000/1\*ivuj02wvmHjBQ8prmQrirA.png)

您可以看到，PyCaret 方法提供了更多解决方案和功能，而且耗时和精力更少！

### II. NLP 分类问题

这里的任务是识别给定的短信是垃圾短信还是正常短信。以下是原始数据的简要概述，您可以从这个 [\*链接](https://github.com/prateek025/SMS\_Spam\_Ham/blob/master/SMS\_Spam\_Ham\_Raw.csv)\* 找到原始数据。数据集中有 5574 条待分类的短信。

![原始数据集的头部和尾部](https://cdn-images-1.medium.com/max/NaN/1\*Jg5TUgAG0NgawLEiRYgg6A.png)

您可能已经发现，这个问题是两阶段的：对原始文本数据进行 NLP 处理，然后对处理后的文本数据进行分类。

现在让我们开始看这两种方法！我将在本文底部分享我的代码链接。

### III. 传统方法

#### 阶段 1. 数据设置和文本数据预处理

在预处理之前，我们将把 \*Flag \*列从分类数据类型转换为数值数据类型。处理后的数据集如下所示：

![将 Flag 列转换为数值/虚拟值的数据集的头部和尾部](https://cdn-images-1.medium.com/max/2000/1\*f60BNHwh86hhufJOth3Dqg.png)

接下来，在预处理步骤中，我对文本数据执行了以下操作：-删除 HTTP 标签-将大小写转换为小写-删除所有标点符号和 Unicode-删除停用词-词形还原（考虑与单词相关联的相关词性，将单词转换为其根形式）

在执行以上 5 个操作后，数据集如下所示：

![在预处理操作后的数据集的头部和尾部](https://cdn-images-1.medium.com/max/2000/1\*1JDtR1AMZnyhp0UON1pA0w.png)

在开始嵌入之前，对最常见单词和最罕见单词进行快速探索分析可能让我们了解垃圾短信和正常短信之间的区别。

![15 个最常见单词：它们似乎主要出现在正常短信中](https://cdn-images-1.medium.com/max/2000/1\*uAyqUWFZzeQQrBNfTfAgWQ.png)

![15 个最罕见单词：它们似乎主要出现在垃圾短信中](https://cdn-images-1.medium.com/max/2000/1\*Cv5lCR18OhwudRMFS35ASg.png)

通常，这种探索性分析有助于我们识别并删除可能具有非常低预测能力的单词（因为这些单词出现频率很高）或者可能在模型中引入噪音的单词（因为这些单词出现频率很低）。但是，我没有从处理后的文本数据中删除任何更多单词，而是转向了嵌入阶段。

#### 阶段 2. 在处理后的文本数据上进行嵌入

我在这里使用了两种嵌入技术。a. _词袋模型_ 方法：该方法创建一个术语文档矩阵，其中每个唯一单词/术语都成为一列。在 Python 中，我们使用 \*CountVectorizer() \*函数进行 _BoW 嵌入_。

![使用 BoW 嵌入的转换数据集](https://cdn-images-1.medium.com/max/2388/1\*DrpB6TM-i4D2pGil4X2TfQ.png)

b. _词频-逆文档频率_ 方法：该方法创建一个术语文档矩阵，其中对矩阵中的每个术语应用一些权重。权重取决于单词在文档中和整个语料库中出现的频率。在 Python 中，我们使用 \*TfidfVectorizer() \*函数进行 TF-IDF 嵌入。

![使用 TF-IDF 嵌入的转换数据集](https://cdn-images-1.medium.com/max/2566/1\*5UHgqdqV2zc0qRRL-rOLvQ.png)

#### 阶段 3. 模型构建
在决定构建哪些模型之前，我已经将数据分成了85%的数据（4737行）作为_训练数据集_，剩下的15%（837行）作为_测试数据集_。测试数据集可以帮助我们评估模型在未见过数据上的表现。

![数据的训练/测试分割输出](https://cdn-images-1.medium.com/max/2000/1\*MAa3Z6FIDhEq17iB05C1MQ.png)

* 在这里，我从四个随机选择的机器学习家族中构建了分类模型：_随机森林分类器、Adaboost分类器、梯度提升分类器、朴素贝叶斯分类器_。
* 我首先在_词袋嵌入_数据集上构建了上述模型，然后在_TF-IDF嵌入_数据集上构建了这些模型。
* 使用的模型性能指标包括：_混淆矩阵、准确率、精确率、召回率和ROC-AUC得分_。
* 我在这里只分享了在使用_词袋嵌入_数据集构建的基础模型的结果（\*超参数未调整）。我已经在我的Github存储库中分享了在使用_TF-IDF嵌入_数据集上的模型性能结果。您可以在下面提供的链接中查看：

![1. 随机森林分类器结果](https://cdn-images-1.medium.com/max/3040/1\*PCZbMiNQyWPi4JFccJRyXQ.png)

![2. Adaboost分类器结果](https://cdn-images-1.medium.com/max/3190/1\*WPga77czNgNEYIzekbCwvA.png)

![3. 梯度提升分类器结果](https://cdn-images-1.medium.com/max/3194/1\*PMNt\_3CouFENUIvVZwTsqA.png)

![4. 朴素贝叶斯分类器结果](https://cdn-images-1.medium.com/max/3102/1*AxWAiD277nuYuIiRLQwvxw.png)

#### 第四阶段. 超参数调优

为了方便起见，我已经对在_词袋嵌入_数据集上构建的模型进行了超参数调优。对于在_TF-IDF嵌入_数据集上进行相同操作，需要重复并添加大约30-40行代码。

我进一步决定继续调整_随机森林分类器_和_Adaboost分类器_模型的超参数，因为这两个模型似乎比其他两个模型表现更好。在超参数调整中，我使用了网格搜索方法。

![这是用于超参数调整的代码片段，完整代码请参见本文底部的Github代码存储库链接。](https://cdn-images-1.medium.com/max/2148/1*CaRuyN7g7rt4XUj251PrvA.png)

与之前一样，使用了相同的模型性能指标：\*混淆矩阵、准确率、精确率、召回率、ROC-AUC得分。\*结果还显示了_调整后的超参数_值，以及调整后模型的_准确率_的10折交叉验证值。

以下是调整后模型的性能结果：

![1. 调整后的Adaboost分类器模型结果](https://cdn-images-1.medium.com/max/3520/1*2LMSESB_nbtkDNx7LW8Akw.png)

![2. 调整后的随机森林分类器模型结果](https://cdn-images-1.medium.com/max/3516/1*r3Oq4lxFmcYnMatCb6xt4w.png)

比较两个调整后的模型，_Adaboost分类器_在_交叉验证准确率_得分上表现更好。

现在让我们探索PyCaret方法..！

### IV. PyCaret方法

我们将重复传统方法下的所有步骤，但您会注意到这种方法有多么快速和简便。

#### 第一阶段. 在文本数据上进行数据设置和预处理

在PyCaret中执行任何ML实验之前，您必须设置PyCaret模块环境。这样可以确保ML实验可以重复、扩展和部署多次。您会发现只需2行命令即可完成此操作。

![设置PyCaret的NLP模块输出](https://cdn-images-1.medium.com/max/2054/1*5DK96jC_oBQLC_-MCF3xOw.png)

这个函数的优点在于它会自动对原始文本数据执行所有NLP预处理操作（转换为小写、删除所有标点符号和停用词、词干提取、词形还原等）。整个步骤只用了21秒！

#### 第二阶段. 在处理后的文本数据上进行嵌入

PyCaret目前仅支持主题建模嵌入技术。在本例中，我们将使用_潜在狄利克雷分配（LDA）_技术和\*非负矩阵分解（NMF）\*技术进行嵌入。因此，这不是一个苹果到苹果的比较，因为我们在传统方法中使用了_词袋嵌入和TF-IDF嵌入_。

在PyCaret中，嵌入过程要简单得多。您可以看到下面的代码片段，我们只需要2行代码即可对处理后的数据进行嵌入。默认情况下，_nlp模块的create_model()_会创建4个主题。您可以通过在此函数中传递所需的数字值来更改主题数量。

![LDA嵌入和结果数据集的代码片段](https://cdn-images-1.medium.com/max/2394/1*180D4bhkXQK2UY4aJkwZZQ.png)

使用相同的2行代码，但更改_model参数_，您可以创建一个具有_NMF_嵌入的数据集。
此外，PyCaret 在这个阶段还提供了多个图表选项，用于探索性数据分析。您只需一行代码即可完成。需要注意的是，探索性数据分析是基于嵌入阶段创建的\*主题\*。

![\*evaluate\_model() 命令的输出。单击其中的任意 5 个选项卡，并从下拉菜单中选择 4 个主题中的任意一个，以获取额外的探索性分析和见解。](https://cdn-images-1.medium.com/max/2014/1\*O5Q\_rG8NFRULGi55k4BYCQ.png)

#### 第三阶段. 模型构建

在 NLP 之后，整个问题的第二部分是分类。因此，我们需要一个不同的设置环境来执行分类实验。

我们将在_LDA 嵌入_数据集和_NMF 嵌入_数据集上构建模型。然而，我们需要从两个嵌入数据集中删除不必要的变量（\*SMS，主题\*等），以便可以在其上构建分类模型。

![删除三个不必要变量后的 LDA 嵌入数据集](https://cdn-images-1.medium.com/max/2000/1\*h6YjjxvOxYJ3WSTQprsexA.png)

我们使用 PyCaret 的\*nlp\*模块进行 NLP 操作，同样，我们使用 PyCaret 的_classification_模块处理问题的分类部分。要设置_分类模块_，只需要 2 行代码。此外，我们还必须指定_目标变量_和_训练-测试分割_比例。

![分类设置命令的输出。](https://cdn-images-1.medium.com/max/2000/1\*ABfkCw-evo-BRXvb-cmGng.png)

就像在\*nlp\*设置中自动执行预处理操作一样，在\*classification\*设置中，根据数据，PyCaret 自动创建新特征并执行其他预处理步骤！

如果您认为设置 PyCaret 环境并获得自动化特征工程很容易，那么模型构建就更容易了！您只需输入一条命令，就能看到结果！

![比较模型命令的输出。这里使用了 LDA 嵌入数据集](https://cdn-images-1.medium.com/max/2000/1\*WnA-NEHIHmKaUqcDgRH-XQ.png)

您可以看到，PyCaret 自动从 18 种不同的 ML 分类家族中构建了基本模型，并按_准确度得分_降序排列了 15 个最佳模型。它进一步指出，对于特定性能指标，哪个模型表现最佳（突出显示的指标得分为黄色）。

所有这些只需一条命令就能完成，结果在大约 1 分钟内呈现！

我们看到_随机森林分类器模型_在\*准确度\*方面表现最佳。让我们调整_随机森林分类器模型_。

#### 第四阶段. 超参数调整

这是 PyCaret 中的一个 3 步过程：创建模型，调整模型，评估其性能。每个步骤只需要 1 行代码！

调整_在 LDA 嵌入数据集上构建的随机森林分类器：_

* 在下面的输入-输出片段中，每个步骤只需要 1 行代码。
* 要创建_随机森林分类器_模型，您必须传递_'rf'_值
* 您可以观察到，调整后的模型指标优于基本模型指标
* PyCaret 提供了 15 个评估图。单击其中的任何 15 个选项卡，选择您想要使用的评估图以获取进一步见解。

![调整模型和评估其性能结果的 3 个步骤](https://cdn-images-1.medium.com/max/4054/1\*PXvktJa\_-1d5Fa23fmb0Aw.png)

我重复了在_NMF 嵌入_数据上调整构建的模型的相同过程。这次我用一些变化调整了_Extra Trees 分类器_模型。

* 在设置新环境后，_compare\_models_() 的结果显示_Extra Trees 分类器_模型表现最佳，因此我决定对其进行调整。

![在 NMF 嵌入数据上设置() 和 compare\_model() 命令的输出](https://cdn-images-1.medium.com/max/3118/1\*8Wxt-e5bXGHyHlN-CxjMMw.png)

* 这里我正在重复我在 LDA 嵌入数据中遵循的相同步骤：\*create\_model()\*，然后\*tune\_model()\*，然后 _evaluate\_model()_。您可以观察到，要创建和调整_Extra Trees 分类器_模型，您必须传递_'et'_值。这次我决定在调整超参数时优化\*AUC 值\*而不是_准确度得分_。为此，我必须传递\*'AUC'\*值。

![在 NMF 嵌入数据上创建、调整和评估 Extra-Tress 分类器模型](https://cdn-images-1.medium.com/max/3400/1\*sLiMNkki4J4uGkkFWEq1mQ.png)

### V. 两种方法的比较

可以看出，与传统方法相比，PyCaret 在更少的代码行和更短的执行时间内提供了更多选项和功能的解决方案。
我想进一步指出，比较传统方法模型的性能结果和 PyCaret 方法模型的性能结果并不是一种简单的对苹果和橙子的比较，因为这两种方法在文本数据上使用了不同的嵌入技术。我们将不得不等待 PyCaret 的 _nlp-module_ 的更新版本，以支持传统方法中使用的嵌入技术。

然而，根据业务问题的不同，重要的是要看到在 PyCaret 方法下节省的时间和精力，以及获得的见解选项，远比在传统方法下将评估指标值增加一些小数值更有价值。

以下是粗略的比较表，以再次突出这两种方法之间的关键差异。

![](https://cdn-images-1.medium.com/max/2000/1\*ivuj02wvmHjBQ8prmQrirA.png)

### VI. 重要链接

* [此比较的完整代码存储库](https://github.com/prateek025/SMS\_Spam\_Ham/blob/master/Spam-Ham.ipynb)
* [PyCaret：用户指南和文档](https://pycaret.org/guide/)
* [PyCaret：教程](https://pycaret.org/tutorial/)

感谢阅读本文。祝学习愉快！