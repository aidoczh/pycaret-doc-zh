---
description: 一个开源的、低代码的 Python 机器学习库
cover: >-
  https://images.unsplash.com/photo-1620641788421-7a1c342ea42e?crop=entropy&cs=tinysrgb&fm=jpg&ixid=MnwxOTcwMjR8MHwxfHNlYXJjaHw1fHxncmFkaWVudHxlbnwwfHx8fDE2NzY4NTQ4Mjk&ixlib=rb-4.0.3&q=80
coverY: 0
---

# 欢迎来到 PyCaret

{% hint style="info" %}
**PyCaret 3.0-rc 现已推出**。`pip install --pre pycaret` 来尝试。查看这个示例 [Notebook](https://colab.research.google.com/drive/1\_H0sHYhzKGZDmgzrQLosuZAR3nOaL6CN?usp=sharing)。
{% endhint %}

PyCaret 是一个开源的、低代码的 Python 机器学习库，可以自动化机器学习工作流程。它是一个端到端的机器学习和模型管理工具，极大加快了实验周期，提高了工作效率。

与其他开源机器学习库相比，PyCaret 是一个替代性的低代码库，可以用几行代码替代数百行代码。这使得实验变得极其快速和高效。PyCaret 本质上是围绕几个机器学习库和框架（如 scikit-learn、XGBoost、LightGBM、CatBoost、spaCy、Optuna、Hyperopt、Ray 等）的 Python 封装。

PyCaret 的设计和简洁性受到了 Gartner 首次使用的公民数据科学家这一新兴角色的启发。公民数据科学家是能够执行简单和中等复杂分析任务的高级用户，这些任务以前需要更多的技术专长。

## 快速链接

| ⭐ [**教程**](get-started/tutorials.md)****                             | 查看我们的官方笔记本！        |
| --------------------------------------------------------------------------- | --------------------------------------- |
| 📋 [**示例**](learn-pycaret/examples.md)****                            | 示例笔记本。                      |
| 📙 [**博客**](learn-pycaret/official-blog/)****                             | 由贡献者撰写的教程和文章。 |
| 📚 [**API 参考**](https://pycaret.readthedocs.io/en/latest/index.html) | PyCaret 的详细 API 文档        |
| 📺 [**视频教程**](learn-pycaret/videos.md)****                       | 我们来自各种活动的视频教程。 |
| 📢 [**讨论**](https://github.com/pycaret/pycaret/discussions)        | 与社区和贡献者互动。 |
| 🛠️ [**更新日志**](get-started/release-notes.md)                       | 变更和版本历史。            |
| :question:[**常见问题**](learn-pycaret/faqs.md)                                 | 常见问题              |
| 🌳 [**路线图**](https://github.com/pycaret/pycaret/issues/1756)            | PyCaret 的发展计划。             |
| :m: [**聚会**](https://www.meetup.com/pycaret-user-group/)****            | 加入我们的聚会用户组。             |

## 特点

PyCaret 是一个**开源**、**低代码**的 Python 机器学习库，旨在缩短 ML 实验中的假设到洞察的周期时间。它使数据科学家能够快速高效地执行端到端实验。与其他开源机器学习库相比，PyCaret 是一个替代性的低代码库，可以用几行代码执行复杂的机器学习任务。PyCaret **简单**且**易于使用**。&#x20;

#### PyCaret 适用于公民数据科学家

PyCaret 的设计和简洁性受到了**公民数据科学家**这一新兴角色的启发，这一术语首次被 _Gartner_ 使用。公民数据科学家是能够执行简单和中等复杂分析任务的“高级用户”，这些任务以前需要更多的专业知识。经验丰富的数据科学家往往难以找到且昂贵，但公民数据科学家可以是弥补这一差距、解决商业环境中的数据科学挑战的有效途径。

#### PyCaret 部署能力

PyCaret 是一个准备好部署的 Python 库，这意味着在 ML 实验中执行的所有步骤都可以使用可复制且可保证用于生产的流水线来重现。可以将流水线保存为可在不同环境中传输的二进制文件格式。

#### PyCaret 与 BI 无缝集成

PyCaret 及其机器学习功能与支持 Python 的环境（如 Microsoft Power BI、Tableau、Alteryx 和 KNIME 等）无缝集成。这为这些 BI 平台的用户赋予了巨大的力量，他们现在可以轻松将 PyCaret 集成到其现有工作流程中，并轻松添加一层机器学习。

![](<.gitbook/assets/image (506).png>)

#### PyCaret 适用于：

- 希望提高生产力的经验丰富数据科学家。
- 偏好低代码机器学习解决方案的公民数据科学家。
- 希望构建快速原型的数据科学专业人士。
* 数据科学和机器学习的学生和爱好者。

## PyCaret 简介

### 分类

![](<.gitbook/assets/image (86).png>)

### 回归

![](<.gitbook/assets/image (216).png>)

### 聚类

![](<.gitbook/assets/image (98).png>)

### 异常检测

![](<.gitbook/assets/image (243).png>)

### 时间序列

PyCaret **新的时间序列模块** 现已在 `3.0-rc` 版本中推出。保持了 PyCaret 的简单性，与现有 API 一致，并且功能齐全。包括统计测试、模型训练和选择（30+ 算法）、模型分析、自动超参数调整、实验记录、云端部署等功能。所有这些只需几行代码即可完成。如果您想尝试一下，请查看我们官方的[快速入门](https://nbviewer.org/github/pycaret/pycaret/blob/master/examples/time\_series/forecasting/time\_series\_101.ipynb)笔记本。

* 📚 [时间序列文档](https://pycaret.readthedocs.io/en/latest/api/time\_series.html)
* ❓ [时间序列常见问题](https://github.com/pycaret/pycaret/discussions/categories/faqs?discussions\_q=category%3AFAQs+label%3Atime\_series)
* 🚀 [功能和路线图](https://github.com/pycaret/pycaret/issues/1648)

要使用 PyCaret 的时间序列模块，您可以安装 `PyCaret 3.0-rc`。

```
pip install --pre pycaret
```

## 核心团队

以下人员目前是 PyCaret 开发和维护的核心贡献者：

{% embed url="https://github.com/moezali1" %}

{% embed url="https://github.com/Yard1" %}

{% embed url="https://github.com/ngupta23" %}

{% embed url="https://github.com/tvdboom" %}

{% embed url="https://github.com/wkuopt" %}

{% embed url="https://github.com/mfahadakbar" %}

{% embed url="https://github.com/PabloJMoreno" %}

{% embed url="https://github.com/reza1615" %}

{% hint style="info" %}
**为 PyCaret 做贡献！** 如果您想为这个项目做贡献，请查看我们的[贡献指南](https://github.com/pycaret/pycaret/blob/master/CONTRIBUTING.md)。
{% endhint %}

## 文档

这份文档由以下人员开发和维护：

* [Moez Ali](https://www.linkedin.com/in/profile-moez/)
* [Pablo Moreno](https://www.linkedin.com/in/pablo-moreno-363188150/)

{% hint style="info" %}
**贡献！** 如果您想加入我们的文档团队，帮助我们为社区构建和维护这份出色的文档，请加入我们的 Slack 上的 **#docs-revamp** 频道（https://join.slack.com/t/pycaret/shared\_invite/zt-row9phbm-BoJdEVPYnGf7\_NxNBP307w）。
{% endhint %}

## 贡献者

![](https://contributors-img.web.app/image?repo=pycaret/pycaret)

## 获取帮助

* 获取帮助的最快途径是通过[我们的 Slack 群](https://join.slack.com/t/pycaret/shared\_invite/zt-row9phbm-BoJdEVPYnGf7\_NxNBP307w)。
* 您也可以查看我们的[问题日志](./#pycaret-at-a-glance)或[讨论区](https://github.com/pycaret/pycaret/discussions)，查看其他成员过去提出的问题的答案，或者如果以前没有提出过，提出新问题。
* 您还可以在[Stack Overflow](https://stackoverflow.com/questions/tagged/pycaret)上寻找常见问题的答案。
* 我们还有一个非常活跃的[LinkedIn](https://www.linkedin.com/company/pycaret/)页面。
* 查看我们的[常见问题](learn-pycaret/faqs.md)（FAQs）页面。
* 快讯即将推出。

## 引用

```
@Manual {PyCaret, 
    title = {PyCaret: An open source, low-code machine learning library in Python}, 
    author = {Moez Ali}, 
    year = {2020}, 
    month = {April}, 
    note = {PyCaret version 1.0}, 
    url = {https://www.pycaret.org
    }
```

## 支持我们

#### :star: 在 GitHub 上给 **PyCaret** 点星 <a href="#star-fastapi-in-github" id="star-fastapi-in-github"></a>

在我们的[GitHub 仓库](https://github.com/pycaret/pycaret)上给我们点个星（点击右上角的星按钮）

#### :link: 在 LinkedIn 上关注我们

我们有一个非常活跃的[LinkedIn 页面](https://www.linkedin.com/company/36103360/admin/)。关注我们获取 PyCaret 的更新和学习内容。

#### :person\_tipping\_hand: 在 GitHub 上帮助他人解决问题

您可以查看现有的[开放问题](https://github.com/pycaret/pycaret/issues)和[讨论](https://github.com/pycaret/pycaret/discussions)，通过回答其他社区成员的问题来帮助他们。

#### :tv: 订阅我们的 YouTube 频道

订阅我们的[YouTube 频道](https://www.youtube.com/channel/UCxA1YTYJ9BEeo50lxyI\_B3g)，获取与 PyCaret 相关的学习内容。

#### :speaking\_head: 在 Twitter 上发推关于 PyCaret

帮助我们传播信息。我们喜欢听到成功故事和用例。

#### :writing\_hand: 在 PyCaret 上写博客

喜欢写作吗？您可以撰写一篇中等博客并与我们分享。查看这位作者的[博客](https://moez-62905.medium.com/)。 

#### :m: 加入我们的聚会 <a href="#blog-on-pycaret" id="blog-on-pycaret"></a>
想要保持联系并了解社区中的最新动态吗？加入我们的[Meetup](https://www.meetup.com/pycaret-user-group/)吧。

## 赞助商

占位符

## 让我们开始吧

一切准备就绪了吗？让我们来探索 PyCaret，从[**安装**](get-started/installation.md#dockerfile)开始吧。