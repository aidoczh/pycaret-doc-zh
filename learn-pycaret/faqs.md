---
description: 常见问题解答！
---

# ❓ 常见问题解答

{% hint style="info" %}
**PyCaret 3.0-rc 现已发布**。使用 `pip install --pre pycaret` 来尝试。查看这个示例 [Notebook](https://colab.research.google.com/drive/1\_H0sHYhzKGZDmgzrQLosuZAR3nOaL6CN?usp=sharing)。
{% endhint %}

<details>

<summary>为什么选择 PyCaret？</summary>

简短的回答是，PyCaret 是一个开源的低代码机器学习库，构建在你喜爱的库和框架（如 _scikit-learn, xgboost, lightgbm 等）之上。机器学习实验需要大量迭代，PyCaret 的主要目标是让你能够以极快的速度进行迭代。与其他优秀的开源机器学习库相比，PyCaret 是一个替代低代码库，可以用几行代码取代数百行代码。试试看吧！

</details>

<details>

<summary>PyCaret 是否适用于所有操作系统和 Python 版本？</summary>

PyCaret 在以下 64 位系统上经过测试和支持：

* Python 3.6 – 3.8&#x20;
* Python 3.9 仅适用于 Ubuntu
* Ubuntu 16.04 或更高版本
* Windows 7 或更高版本

PyCaret 也可以在 Mac OS 上运行，但我们不能保证其性能，因为这些版本没有经过 Mac 的测试。要了解更多关于我们的测试工作流程的信息，[点击这里](https://github.com/pycaret/pycaret/blob/master/.github/workflows/test.yml)。

</details>

<details>

<summary>我可以在 Google Colab 或 Kaggle Notebooks 上使用 PyCaret 吗？</summary>

当然可以。只需执行 `pip install pycaret`。

由于这些平台上的基本安装不在我们的控制范围内，所以可能会因为一些依赖冲突而导致安装 PyCaret 时出现问题。这些问题及其临时解决方案在[这里](../get-started/installation.md#common-installation-issues)有所报道。

</details>

<details>

<summary>PyCaret 是否支持在 GPU 上进行模型训练？</summary>

是的。我们已经将 PyCaret 与令人惊叹的 [RAPIDS.AI](https://rapids.ai/) 项目集成在一起。要在 GPU 而不是 CPU 上使用，只需在 `setup` 函数中传递 `use_gpu=True`。

**这将使用 CPU 进行模型训练：**

```
from pycaret.classification import *
s = setup(data, target = 'target_name')
```

**这将使用 GPU 进行模型训练：**

```
from pycaret.classification import *
s = setup(data, target = 'target_name', use_gpu = True)
```

API 的使用方式没有变化，但在某些情况下，需要安装额外的库，因为它们没有与默认版本或完整版本的 PyCaret 一起安装。你可以在[这里](../get-started/installation.md#gpu)了解更多信息。

</details>

<details>

<summary>我可以在 Spark、Dask、Ray 等集群上使用 PyCaret 进行分布式训练吗？</summary>

可以。PyCaret 的所有函数都是普通的 Python 函数，而这些框架（如 Spark、Dask、Ray）都提供了在一组机器上分布任意代码的选项。在未来的版本中，我们计划将这些分布式框架集成到 PyCaret 中，但目前，如果你有兴趣这样做，[Fugue 项目团队](https://github.com/fugue-project/fugue)的[这篇文章](https://towardsdatascience.com/scaling-pycaret-with-spark-or-dask-through-fugue-60bdc3ce133f)展示了如何在 Spark 或 Dask 上分布 PyCaret 代码而无需对代码进行重大更改。

</details>

<details>

<summary>我如何为 PyCaret 做贡献？</summary>

感谢您选择为 PyCaret 做贡献。有很多优秀的开源项目，所以我们很感谢您对 PyCaret 的兴趣。请查看我们的[贡献指南](https://github.com/pycaret/pycaret/blob/master/CONTRIBUTING.md)。

</details>

<details>

<summary>如果我不会编程，如何支持 PyCaret？</summary>

当然可以。有很多方式可以支持我们。你可以加入我们的文档团队，帮助我们构建和维护这个令人惊叹的文档，这些文档每天都被成千上万的成员使用。[了解更多](../#support-us)关于你可以支持我们的其他方式。

</details>

<details>

<summary>PyCaret 是否支持深度学习或强化学习？</summary>

目前还不支持。将来可能会支持。

</details>

<details>

<summary>我可以将 PyCaret 与 Power BI、Tableau、Qlik 等 BI 工具集成吗？</summary>

是的，任何支持 Python 环境的工具都可以。你可以在 Power BI、Tableau、SQL、Alteryx、KNIME 中使用 PyCaret。如果你想了解更多，请阅读这些[官方教程](official-blog/#pycaret-and-bi-integrations)。

</details>

<details>

<summary>PyCaret 是否保证端到端实验的可重现性？</summary>
当然可以。如果没有可重现性的保证，任何框架都几乎没有用处。在任何机器学习工作流中，都有许多因素会导致随机性，比如`train_test_split`。有时，随机性也内置在算法中。一些例子包括随机森林(Random Forest)、极端随机树(Extra Trees)和梯度提升机(Gradient Boosting Machines)。为了确保您可以在以后的时间重现您的端到端实验，您必须在`setup`中传递`session_id`参数。

**示例：**

```
from pycaret.classification import *
s = setup(data, target='target_name', session_id=123)
```

无论您传递什么数字给`session_id`，只要您能记住它即可。在PyCaret中，`session_id`参数相当于scikit-learn模型中的`random_state`。这里的好处是我们从`setup`中获取`session_id`并将其传递给使用`random_state`的所有函数，这样您就不用担心了。

</details>

<details>

<summary>我可以在命令行或者除了Notebook之外的其他环境中运行PyCaret吗？</summary>

当然可以。PyCaret被设计和开发用于在Notebook环境中工作，但这并不意味着您不能在Notebook之外的其他IDE（如Visual Code、PyCharm或Spyder）中使用它。当您在Notebook环境之外使用PyCaret时，您必须在`setup`函数中传递`html=False`和`silent=True`。由于PyCaret在某些回调功能中使用IPython，如果不显式传递这两个参数，当您在Notebook环境之外时，您的代码将失败。

**注意：**这些参数的名称可能会在将来更改为类似于`mode='notebook'`的内容。

</details>

<details>

<summary>我如何在运行`setup`函数时绕过用户确认数据类型的对话框？</summary>

无论您在PyCaret的任何模块中运行`setup`，它都会生成一个对话框来确认数据类型，用户需要按回车键继续。当您在命令行或者作为Python脚本使用PyCaret时，您必须绕过确认对话框。您可以在`setup`函数中传递`silent=True`来实现。

**示例：**

```
from pycaret.classification import *
s = setup(data, target='target_name', silent=True)
```

</details>

<details>

<summary>我如何在PyCaret中更改详细程度？</summary>

PyCaret中的大多数函数都有`verbose`参数。只需在函数中设置`verbose=False`即可。

**示例：**

```
lr = create_model('lr', verbose=False)
```

</details>

<details>

<summary>有时候日志会在代码运行时打印在屏幕上。我可以关闭日志记录器吗？</summary>

我们注意到在某些情况下，PyCaret的日志记录器可能与环境中的其他库发生冲突，导致异常行为，结果是在代码运行时（Notebook或CLI）打印日志。虽然在下一个主要版本（3.0）中，我们计划使日志记录器更可配置，允许您完全禁用它。与此同时，有一种绕过的方法是使用环境变量。在Notebook的顶部运行以下代码：

```
import os
os.environ["PYCARET_CUSTOM_LOGGING_LEVEL"] = "CRITICAL"
```

**注意：**此命令将设置一个由PyCaret的日志记录器使用的环境变量。将其设置为`CRITICAL`意味着只记录关键消息，而PyCaret中并没有太多关键消息。

</details>

<details>

<summary>我在安装PyCaret时遇到问题，我该怎么办？</summary>

首先，请查看[常见安装问题](../get-started/installation.md#common-installation-issues)，然后查看我们在GitHub上的[问题](https://github.com/pycaret/pycaret/issues)。

</details>

<details>

<summary>我可以在PyCaret中添加自定义模型吗？</summary>

当然可以。PyCaret的目标是让您完全控制您的机器学习流程。要添加自定义模型，只有一个规则，它们必须与标准的`sklearn` API兼容。要了解如何做到这一点，您可以阅读Fahad Akbar的以下教程：

* [使用PyCaret添加自定义估计器-第一部分](https://towardsdatascience.com/custome-estimator-with-pycaret-part-1-by-fahad-akbar-839513315965)
* [使用PyCaret添加自定义估计器-第二部分](https://towardsdatascience.com/custom-estimator-with-pycaret-part-2-by-fahad-akbar-aee4dbdacbf)

</details>

<details>

<summary>我可以在PyCaret中添加自定义的交叉验证指标吗？</summary>

当然可以。PyCaret的目标是在抽象和灵活性之间取得平衡，到目前为止，我们做得相当不错。您可以使用PyCaret的`add_metric`和`remove_metric`函数来添加或删除用于交叉验证的指标。

</details>

<details>

<summary>我可以只使用PyCaret进行数据预处理吗？</summary>
是的，如果你愿意的话。你可以运行 `setup` 函数，该函数处理所有数据预处理，之后你可以使用 `get_config` 函数访问转换后的训练集和测试集。&#x20;

**示例:**

```
from pycaret.classification import *
s = setup(data, target = 'target_name')

X_train, y_train = get_config('X_train'), get_config('y_train')
X_test, y_test = get_config('X_test'), get_config('y_test')
```

</details>

<details>

<summary>我可以从 PyCaret 导出模型并在 PyCaret 之外进行操作吗？</summary>

当然可以。你可以使用 PyCaret 的 `save_model` 函数将整个 Pipeline 导出为 `pkl` 文件。关于这个函数，[了解更多](../get-started/functions/#save-model)。

</details>

<details>

<summary>为什么 PyCaret 没有面向对象的 API？以后会有吗？</summary>

PyCaret 的首个版本（1.0）在设计上做出了许多关键决策，这些决策很快成为社区中的共识做法。使用独立的函数式 API 是其中之一。然而，在随后的版本中，我们意识到了更传统的面向对象 API 的用例和需求，现在正在开发中。PyCaret 的默认 API 将继续是函数式 API，因为有大量用户依赖于它。但是，在下一个主要版本（3.0）中，我们将为那些有兴趣使用面向对象 API 的用户提供一个单独的 API。

**函数式 API 示例（当前）**

```
from pycaret.classification import *
s = setup(data, target = 'target_name')
best_model = compare_models()
```

**面向对象 API 示例（未来状态）**

```
from pycaret.classification import ClassificationExperiment
exp = ClassificationExperiment()
exp.setup(data, target = 'target_name')
best_model = exp.compare_models()
```

</details>

<details>

<summary>我可以使用 PyCaret 在云上部署机器学习流水线吗？</summary>

当然可以。PyCaret 是一个端到端的库，具有许多部署功能。有许多关于在 Azure、AWS 和 GCP 等不同云平台上部署的官方教程。你可以在这里查看这些[教程](official-blog/#pycaret-add-ml-deployment)。

</details>

<details>

<summary>我可以在没有安装 PyCaret 的情况下使用 PyCaret 训练的模型进行推断吗？</summary>

目前不行，但我们下一个主要版本（3.0）的目标是允许你在推断运行时使用纯 `sklearn`。目前，Pipeline 中有一些 PyCaret 的自定义功能，这些功能会强制你在推断时安装 `pycaret`，但我们致力于在未来版本中要么删除这些自定义功能，要么将其推送到基础库如 scikit-learn。我们的目标和愿景是成为一个用于训练和机器学习开发的超级抽象框架。我们不想重复造轮子。我们不希望在推断时让你承担 PyCaret 框架的巨大开销。

</details>

<details>

<summary>我运行了 setup 函数，但它卡住了，我该怎么办？</summary>

如果你的 setup 函数卡住了，首先要检查的是你是否在允许确认对话框的环境中，如果不是，你必须在 setup 中传入 `silent=True`。其次，如果你使用的是 Visual Code，对话框会出现在屏幕顶部，而不是像在 Jupyter Notebook 中看到的内联。最后，有时可能会花很长时间，特别是如果你的数据集具有许多级别的分类特征（1000+级别）。在这种情况下，你应该尝试合并级别，使特征不那么细粒度，然后再传入 PyCaret。如果所有这些都解决不了你的问题，而且你非常确定这是某种 bug 或者你可以改进代码，请随时在我们的 GitHub 上打开一个新的[问题](https://github.com/pycaret/pycaret/issues)。

</details>

<details>

<summary>我可以在 Apple M1 MacBook 上安装和运行 PyCaret 吗？</summary>

由于 PyCaret 底层依赖的一些问题，这并不直接。然而，如果你已经尝试了一切仍然找不到解决方案，Pareekshith Katti 的这篇[文章](https://pareekshithkatti.medium.com/setting-up-python-for-data-science-on-m1-mac-ced8a0d05911)可能会对你有所帮助。

</details>

<details>

<summary>我需要一台强大的计算机才能使用 PyCaret 吗？</summary>

不需要，只要你的数据可以放入内存，你就可以使用 PyCaret。不需要超级计算机。

</details>

<details>

<summary>为什么我的拉取请求没有得到任何关注？</summary>

审查过程可能需要一些时间。你不应该因为拉取请求的审查延迟而感到沮丧。我们收到了社区提出的许多功能请求，但我们的维护人员只有有限的时间来审查和批准这些拉取请求。由于每个功能都需要终身维护的成本，我们非常重视第一次就把事情做对。

</details>

<details>

<summary>PyCaret 是否可以与 scikit-learn 和机器学习库和框架相媲美？</summary>
PyCaret是建立在常见的机器学习库和框架（如scikit-learn、LightGBM、XGBoost等）之上的。使用PyCaret的好处是你不需要编写大量的代码。底层模型和评估框架与你所熟悉的相同。当我们首次创建PyCaret时，我们对使用PyCaret和不使用PyCaret执行一组给定任务进行了小规模比较，以下是结果：

![image (202)](<../.gitbook/assets/image (202).png>)