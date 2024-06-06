---
description: 本页面列出了 PyCaret 的所有函数
---

# 💡 函数

## 选择标签 :point\_down:

{% tabs %}
{% tab title="初始化" %}
#### [setup](initialize.md#setting-up-environment)

此函数在 PyCaret 中初始化实验，并根据传递的所有参数准备转换流水线。在执行任何其他函数之前，必须调用 setup 函数。它只需要两个参数：`data` 和 `target`。所有其他参数都是可选的。[了解更多。](initialize.md#setting-up-environment)
{% endtab %}

{% tab title="训练" %}
#### [compare\_models](train.md#compare\_models)

此函数使用交叉验证训练和评估模型库中所有可用模型的性能。此函数的输出是带有平均交叉验证分数的评分表。[了解更多。](train.md#compare\_models)



#### [create\_model](train.md#create\_model)&#x20;

此函数使用交叉验证训练和评估给定模型的性能。此函数的输出是带有交叉验证分数以及均值和标准差的评分表。[了解更多。](train.md#create\_model)
{% endtab %}

{% tab title="优化" %}
#### [tune\_model](optimize.md#tune\_model)

此函数调整给定模型的超参数。此函数的输出是最佳模型的交叉验证分数表。搜索空间预先定义，可以灵活提供自定义。搜索算法可以是随机的、贝叶斯的，以及其他一些可以在大型集群上扩展的算法。[了解更多。 ](optimize.md#tune\_model)



#### [ensemble\_model](optimize.md#ensemble\_model)

此函数对给定模型进行集成。此函数的输出是集成模型的交叉验证分数表。可以使用两种方法进行集成：`Bagging` 或 `Boosting`。[了解更多](optimize.md#ensemble\_model).



#### [blend\_models](optimize.md#blend\_models)

此函数为列表中的给定模型训练 Soft Voting / Majority Rule 分类器。此函数的输出是投票分类器或回归器的交叉验证分数表。[了解更多。](optimize.md#blend\_models)



#### [stack\_models](optimize.md#stack\_models)

此函数在列表中的给定模型上训练元模型。此函数的输出是堆叠分类器或回归器的交叉验证分数表。[了解更多。](optimize.md#stack\_models)



#### [optimize\_threshold](optimize.md#optimize\_threshold)

此函数优化给定模型的概率阈值。它在不同概率阈值下迭代性能指标，并返回一个绘图，其中性能指标在 y 轴上，阈值在 x 轴上。[了解更多。](optimize.md#optimize\_threshold)



#### [calibrate\_model](optimize.md#calibrate\_model)

此函数使用等温或逻辑回归校准给定模型的概率。此函数的输出是校准分类器的交叉验证分数表。[了解更多。](optimize.md#calibrate\_model)
{% endtab %}

{% tab title="分析" %}
#### [plot\_model](analyze.md#plot\_model)

此函数分析在留出集上训练模型的性能。在某些情况下，可能需要重新训练模型。[了解更多。](analyze.md#plot\_model)



#### [evaluate\_model](analyze.md#evaluate\_model)

此函数使用 `ipywidgets` 显示用于分析训练模型性能的基本用户界面。[了解更多。](analyze.md#evaluate\_model)



#### [interpret\_model](analyze.md#interpret\_model)

此函数分析从训练模型生成的预测。此函数中的大多数图表是基于 SHAP（Shapley Additive exPlanations）实现的。[了解更多。](analyze.md#interpret\_model)



#### [dashboard](analyze.md#dashboard)

此函数为训练模型生成交互式仪表板。该仪表板使用 ExplainerDashboard 项目实现。[了解更多。](analyze.md#dashboard)



#### [deep\_check](analyze.md#deep\_check)

此函数使用 deepchecks 库对训练模型运行完整套件检查。此函数处于实验模式。[了解更多](analyze.md#deep\_check).



#### [eda](analyze.md#eda)

此函数使用 AutoViz 项目生成自动化的探索性数据分析（EDA）。完全交互式且可导出。[了解更多。](analyze.md#eda)



#### [check\_fairness](analyze.md#check\_fairness)

此函数为给定模型的数据集中不同组之间提供与公平性相关的指标。评估公平性有许多方法，但此函数使用称为组公平性的方法，即询问：哪些个体组有遭受伤害的风险。[了解更多。](analyze.md#check\_fairness)

####

#### [get\_leaderboard](analyze.md#get\_leaderboard)
这个函数返回当前设置中所有训练模型的排行榜。[了解更多](analyze.md#get_leaderboard)

#### [assign_model](analyze.md#assign_model)

该函数使用训练好的模型为训练数据集分配标签。仅适用于无监督模型。[了解更多](analyze.md#assign_model)

#### [predict_model](deploy.md#predict_model)

该函数使用训练好的模型生成标签。当未传入未见过的数据时，它会在留置集上预测标签和得分。[了解更多](deploy.md#predict_model)

#### [finalize_model](deploy.md#finalize_model)

该函数在整个数据集上重新拟合给定的模型。[了解更多](deploy.md#finalize_model)

#### [save_model](deploy.md#save_model)

该函数将机器学习流程保存为 pickle 文件以供以后使用。[了解更多](deploy.md#save_model)

#### [load_model](deploy.md#load_model)

该函数加载先前保存的流程。[了解更多](deploy.md#load_model)

#### [save_config](deploy.md#save_config)

该函数将所有全局变量保存为 pickle 文件，以便以后恢复而无需重新运行设置函数。[了解更多](deploy.md#save_config)

#### [load_config](deploy.md#load_config)

该函数从 pickle 文件中加载全局变量到 Python。[了解更多](deploy.md#load_config)

#### [deploy_model](deploy.md#deploy_model)

该函数在云上部署整个机器学习流程。[了解更多](deploy.md#deploy_model)

#### [convert_model](deploy.md#convert_model)

该函数将训练好的机器学习模型的决策函数转换为不同的编程语言，如 Python、C、Java、Go、C# 等。[了解更多](deploy.md#convert_model)

#### [create_api](deploy.md#create_api)

该函数接受一个输入模型，并为推理创建一个 POST API。它只创建 API，不会自动运行。要运行 API，您必须使用 `!python` 运行 Python 文件。[了解更多](deploy.md#create_api)

#### [create_docker](deploy.md#create_docker)

该函数为部署 API 创建一个 Dockerfile 和 requirements.txt 文件。[了解更多](deploy.md#create_docker)

#### [create_app](deploy.md#create_app)

该函数为推理创建一个基本的 gradio 应用程序。[了解更多](deploy.md#create_app)

#### [pull](others.md#pull)

在任何训练函数之后使用 `pull` 函数返回最后一个打印的评分网格。以 `pandas.DataFrame` 的形式返回指标。[了解更多](others.md#pull)

#### [models](others.md#models)

返回包含导入的模型库中所有可用模型的表格。[了解更多](others.md#models)

#### [get_config](others.md#get_config)

该函数检索由 [setup](initialize.md#setup) 函数创建的全局变量。[了解更多](others.md#get_config)

#### [set_config](others.md#set_config)

该函数重置全局变量。[了解更多](others.md#set_config)

#### [get_metrics](others.md#get_metrics)

返回所有可用于交叉验证的指标表格。[了解更多](others.md#get_metrics)

#### [add_metric](others.md#add_metric)

将自定义指标添加到指标容器中以进行交叉验证。[了解更多](others.md#add_metric)

#### [remove_metric](others.md#remove_metric)

从指标容器中删除自定义指标。[了解更多](others.md#remove_metric)

#### [automl](others.md#automl)

该函数返回当前设置中所有模型中的最佳模型。[了解更多](others.md#automl)

#### [get_logs](others.md#get_logs)

返回实验日志表格。仅在初始化设置函数时 log_experiment = True 时有效。[了解更多](others.md#get_logs)

#### [get_system_logs](others.md#get_system_logs)

从当前活动目录中读取和打印 logs.log 文件。[了解更多](others.md#get_system_logs)