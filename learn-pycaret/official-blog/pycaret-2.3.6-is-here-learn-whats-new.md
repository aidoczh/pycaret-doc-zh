---
description: >-
  PyCaret 2.3.6 来了！了解有什么新功能？从 EDA 到部署再到 AI 公平性 - PyCaret 最大的更新
---

# PyCaret 2.3.6 来了！了解有什么新功能？

### 🚀 简介 <a href="#261e" id="261e"></a>

PyCaret 是一个开源的、低代码的 Python 机器学习库，可以自动化机器学习工作流程。它是一个端到端的机器学习和模型管理工具，可以大大加快实验周期并提高工作效率。

在新功能和功能方面，PyCaret 2.3.6 是迄今为止最大的更新。本文演示了在最新版本的 [PyCaret 2.3.6](https://pycaret.gitbook.io/docs/get-started/release-notes#pycaret-2.3.6) 中添加的新功能的使用方法。

### 💻 安装 <a href="#90a4" id="90a4"></a>

安装很简单，只需要几分钟时间。PyCaret 的默认安装只会安装 [requirements.txt](https://github.com/pycaret/pycaret/blob/master/requirements.txt) 文件中列出的硬性依赖项。

```
pip install pycaret
```

要安装完整版本：

```
pip install pycaret[full]
```

### 📈 仪表盘 <a href="#5b8f" id="5b8f"></a>

这个函数将为训练好的模型生成交互式仪表盘。该仪表盘使用 [ExplainerDashboard](http://explainerdashboard.readthedocs.io/) 实现。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('iris')

# 初始化设置
from pycaret.classification import *
s = setup(data, target = 'species', session_id = 123)

# 训练模型
lr = create_model('lr')

# 生成仪表盘
dashboard(lr)
```

![](https://cdn-images-1.medium.com/max/800/1\*MlXSTs8BmiICexLfcajJKA.png)

**视频演示：**

{% embed url="https://www.youtube.com/watch?v=FZ5-GtdYez0" %}

### 📊 探索性数据分析（EDA） <a href="#3223" id="3223"></a>

这个函数将使用 [AutoViz](https://github.com/AutoViML/AutoViz) 集成生成自动化的 EDA。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('iris')

# 初始化设置
from pycaret.classification import *
s = setup(data, target = 'species', session_id = 123)

# 生成 EDA
eda()
```

![](https://cdn-images-1.medium.com/max/800/1\*lByuyZL-pR2eZ0rPc1qsxA.png)

**视频演示：**

{% embed url="https://www.youtube.com/watch?v=Pm5VOuYqU4Q" %}

### 🚊 转换模型 <a href="#2e61" id="2e61"></a>

这个函数将训练好的机器学习模型转换为不同编程语言（Python、C、Java、Go、JavaScript、Visual Basic、C#、PowerShell、R、PHP、Dart、Haskell、Ruby、F#）的本地推理脚本。如果您想将模型部署到无法安装正常 Python 环境以支持模型推理的环境中，这个功能非常有用。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('iris')

# 初始化设置
from pycaret.classification import *
s = setup(data, target = 'species', session_id = 123)

# 训练模型
lr = create_model('lr')

# 转换模型
lr_java = convert_model(lr, language = 'java')
print(lr_java)
```

![](<../../.gitbook/assets/image (520).png>)

**视频演示：**

{% embed url="https://www.youtube.com/watch?v=xwQgfNC7808" %}

### ☑️ 检查公平性 <a href="#95da" id="95da"></a>

公平性有很多概念化的方法。这个新函数遵循了称为 [group fairness](https://github.com/fairlearn/fairlearn) 的方法，它问：哪些个体群体有遭受伤害的风险。这个函数提供了不同群体（也称为子群体）之间的公平性相关指标。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('income')

# 初始化设置
from pycaret.classification import *
s = setup(data, target = 'income >50K', session_id = 123)

# 训练模型
lr = create_model('lr')

# 检查公平性
check_fairness(lr, sensitive_features = ['race'])
```

![](<../../.gitbook/assets/image (452).png>)

**视频演示：**

{% embed url="https://youtu.be/mjhDKuLRpM0" %}

### 📩 创建 Web API <a href="#ea0e" id="ea0e"></a>

这个函数将使用 [FastAPI](https://github.com/tiangolo/fastapi) 框架为 ML 管道创建一个用于推理的 POST API。它只创建 API，不会自动运行。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('iris')

# 初始化设置
from pycaret.classification import *
s = setup(data, target = 'species', session_id = 123)

# 训练模型
lr = create_model('lr')

# 创建 API
create_api(lr, 'my_first_api')

# 运行 API
!python my_first_api.py
```

![](<../../.gitbook/assets/image (162).png>)

![](<../../.gitbook/assets/image (170) (1).png>)

**视频演示：** <a href="#4653" id="4653"></a>

{% embed url="https://www.youtube.com/watch?t=1s&v=88M9c5Hc-k0" %}

### 🚢 创建 Docker <a href="#4653" id="4653"></a>

这个函数将为您的 API 端点创建一个 `Dockerfile` 和 `requirements` 文件。
```
# 载入数据集
from pycaret.datasets import get_data
data = get_data('iris')

# 初始化设置
from pycaret.classification import *
s = setup(data, target = 'species', session_id = 123)

# 训练模型
lr = create_model('lr')

# 创建 API
create_api(lr, 'my_first_api')

# 创建 Docker
create_docker('my_first_api')
```

![](<../../.gitbook/assets/image (514).png>)

**视频演示:**

{% embed url="https://www.youtube.com/watch?v=xMgwEJ57uxs" %}

### 💻 创建 Web 应用程序 <a href="#5897" id="5897"></a>

此函数为推断创建一个基本的 [Gradio](https://github.com/gradio-app/gradio) Web 应用程序。稍后将扩展为其他应用程序类型，如 Streamlit。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('iris')

# 初始化设置
from pycaret.classification import *
s = setup(data, target = 'species', session_id = 123)

# 训练模型
lr = create_model('lr')
```

![](<../../.gitbook/assets/image (54).png>)

**视频演示:**

{% embed url="https://www.youtube.com/watch?v=4JyYhbW6eCA" %}

### 🎰 监控机器学习模型的漂移 <a href="#b01d" id="b01d"></a>

`predict_model` 函数新增了一个名为 `drift_report` 的参数，使用 [Evidently AI](https://github.com/evidentlyai/evidently?) 框架生成漂移报告。目前此功能处于实验模式，仅适用于测试数据。稍后将扩展为生产使用。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('iris')

# 初始化设置
from pycaret.classification import *
s = setup(data, target = 'species', session_id = 123)

# 训练模型
lr = create_model('lr')

# 生成报告
preds = predict_model(lr, drift_report = True)
```

![](<../../.gitbook/assets/image (84).png>)

![](<../../.gitbook/assets/image (259).png>)

**视频演示:**

{% embed url="https://www.youtube.com/watch?v=C9TNq1bndRI" %}

### 🔨 绘制模型现在更加可配置 <a href="#ac70" id="ac70"></a>

PyCaret 中的 `plot_model` 函数现在更加可配置。例如，以前如果想在混淆矩阵中看到百分比而不是绝对数字是不可能的，或者如果想要更改可视化的颜色映射也是不可能的。现在在 `plot_model` 函数中使用新参数 `plot_kwargs` 可以实现。查看示例：

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('iris')

# 初始化设置
from pycaret.classification import *
s = setup(data, target = 'species', session_id = 123)

# 训练模型
lr = create_model('lr')

# 绘制模型（不使用 plot kwargs）
plot_model(lr, plot = 'confusion_matrix') 

# 绘制模型（使用 plot kwargs）
plot_model(lr, plot = 'confusion_matrix', plot_kwargs = {'percent' : True})
```

![](<../../.gitbook/assets/image (405).png>)

### 🏆 优化阈值 <a href="#bc52" id="bc52"></a>

这不是一个新功能，但在 2.3.6 版本中进行了彻底改进。此功能用于优化二元分类问题的概率阈值。以前，您必须在此函数中传递成本函数，如 `true_positive`、`false_positive`、`true_negative`、`false_negative`，现在它会自动选择所有指标，包括您活动实验运行中的自定义指标。

```
# 加载数据集
from pycaret.datasets import get_data
data = get_data('blood')

# 初始化设置
from pycaret.classification import *
s = setup(data, target = 'Class', session_id = 123)

# 训练模型
lr = create_model('lr')

# 优化阈值
optimize_threshold(lr)
```

![](<../../.gitbook/assets/image (16).png>)

### 📚 新文档 <a href="#c5b7" id="c5b7"></a>

最大、最艰难的是全新的文档。这是关于 PyCaret 的一切的真相之源，从官方教程到发布说明，从 API 参考到社区贡献。观看视频导览：

{% embed url="https://youtu.be/NpJiD5H0dJc" %}

最后，如果您想了解 2.3.6 版本中添加的所有新功能，请观看这段 10 分钟的视频：

{% embed url="https://www.youtube.com/watch?t=4s&v=Qr6Hu2t2gwY" %}

要了解 PyCaret 2.3.6 中的所有其他更改、错误修复和小更新，请查看详细的[发布说明](https://github.com/pycaret/pycaret/releases/tag/2.3.6)。

感谢阅读。

### :link: 重要链接 <a href="#b749" id="b749"></a>

* 📚 [官方文档:](https://pycaret.gitbook.io/) PyCaret 的圣经。这里面包含了一切。
* 🌐 [官方网站:](https://www.pycaret.org/) 查看我们的官方网站
* 😺 [GitHub](https://www.github.com/pycaret/pycaret) 查看我们的 Git
* ⭐ [教程](https://pycaret.gitbook.io/docs/get-started/tutorials) 初次接触 PyCaret？查看我们的官方笔记！
* 📋 [示例笔记本](https://github.com/pycaret/pycaret/tree/master/examples) 由社区创建。
* 📙 [博客](https://pycaret.gitbook.io/docs/learn-pycaret/official-blog) 贡献者撰写的教程和文章。
* ❓ [常见问题](https://pycaret.gitbook.io/docs/learn-pycaret/faqs) 查看常见问题。
* 📺 [视频教程](https://pycaret.gitbook.io/docs/learn-pycaret/videos) 我们来自各种活动的视频教程。
* 📢 [讨论](https://github.com/pycaret/pycaret/discussions) 有问题吗？与社区和贡献者互动。
* 🛠️ [更新日志](https://pycaret.gitbook.io/docs/get-started/release-notes) 变更和版本历史。
* 🙌 [用户群](https://www.meetup.com/pycaret-user-group/) 加入我们的 Meetup 用户群。