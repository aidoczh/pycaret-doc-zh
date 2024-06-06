---
description: 本页面显示版本 >= 2.0 的发布说明
---

# ⚒ 发布说明

### **PyCaret 2.3.10**

**发布日期：2022年4月10日（错误修复，新功能）**

* 修复了 `predict_model` 在加载的流水线中抛出异常的问题（[#2349](https://github.com/pycaret/pycaret/pull/2349)）
* 修复了 `ParallelBackend` 可能会泄漏参数的问题 - 感谢 [@goodwanghan](https://github.com/goodwanghan)（[#2339](https://github.com/pycaret/pycaret/pull/2339)）
* 重构了 `arules` 模块中的一部分逻辑 - 感谢 [@daikikatsuragawa](https://github.com/daikikatsuragawa)（[#2316](https://github.com/pycaret/pycaret/pull/2316)）
* 添加了两个中文教程 - 感谢 [@ryanxjhan](https://github.com/ryanxjhan)（[#2352](https://github.com/pycaret/pycaret/pull/2352)）
* 添加了中文教程 CLF101 - 感谢 [@ryanxjhan](https://github.com/ryanxjhan)（[#2353](https://github.com/pycaret/pycaret/pull/2353)）
* 添加了新的中文教程 - 感谢 [@ryanxjhan](https://github.com/ryanxjhan)（[#2375](https://github.com/pycaret/pycaret/pull/2375)）
* 在 `classification` 和 `regression` 模块中添加了新函数 `deep_check`。

### **PyCaret 2.3.9**

**发布日期：2022年3月27日（对2.3.8的热修复）**

* 使 `log_experiment` 更可配置（[#2334](https://github.com/pycaret/pycaret/pull/2334)，[#2335](https://github.com/pycaret/pycaret/pull/2335)）
* 使 `return_train_score=False` 使用旧的输出格式（[#2333](https://github.com/pycaret/pycaret/pull/2333)）

### **PyCaret 2.3.8**

**发布日期：2022年3月21日（对2.3.7的热修复）**

* 修复了 `setup` 过程中 `dashboard_logger` 的键错误（[#2311](https://github.com/pycaret/pycaret/pull/2311)）

### **PyCaret 2.3.7**

**发布日期：2022年3月21日（新功能，错误修复）**

* Fugue 集成 - 感谢 [@goodwanghan](https://github.com/goodwanghan)（[#2035](https://github.com/pycaret/pycaret/pull/2035)）
* 添加了 W\&B 实验记录器 - 感谢 [@AyushExel](https://github.com/AyushExel)（[#2231](https://github.com/pycaret/pycaret/pull/2231)）
* 修复了 `check_fairness` 在 **** 索引不是序数时抛出异常的问题 - 感谢 [@reza1615](https://github.com/reza1615)（[#2055](https://github.com/pycaret/pycaret/pull/2055)）
* 现在会替换数据框中的不支持字符 - 感谢 [@reza1615](https://github.com/reza1615)（[#2058](https://github.com/pycaret/pycaret/pull/2058)）
* 修复了分类列在漂移报告中的问题 - 感谢 [@reza1615](https://github.com/reza1615)（[#2063](https://github.com/pycaret/pycaret/pull/2063)）
* 添加了来自 UCI 的多变量时间序列数据集 - 感谢 [@reza1615](https://github.com/reza1615)（[#2094](https://github.com/pycaret/pycaret/pull/2094)）
* 修复了安装过程中的 UTF 错误 - 感谢 [@reza1615](https://github.com/reza1615)（[#2113](https://github.com/pycaret/pycaret/pull/2113)）
* MLFlow 跟踪 API 现在可以接受自定义标签 - 感谢 [@netoferraz](https://github.com/netoferraz)（[#1526](https://github.com/pycaret/pycaret/pull/1526)）
* 更新了 `create_api` 函数（[#2146](https://github.com/pycaret/pycaret/pull/2146)）
* `drift_report` 现在可以处理未见过的数据 - 感谢 [@reza1615](https://github.com/reza1615)（[#2183](https://github.com/pycaret/pycaret/pull/2183)）
* 添加了日文教程 - 感谢 [@hanaseleb](https://github.com/hanaseleb)（[#2215](https://github.com/pycaret/pycaret/pull/2215)）
* 添加了与交通和药物相关的违规数据集和示例 - 感谢 [@HaithemH](https://github.com/HaithemH)（[#2191](https://github.com/pycaret/pycaret/pull/2191)）
* 现在可以从各种监督学习函数中返回训练分数（`return_train_score=True`）。将带有标签列的未见过的数据集传递给 `predict_model` 将计算该数据集的指标 - 感谢 [@levelalphaone](https://github.com/levelalphaone)（[#2237](https://github.com/pycaret/pycaret/pull/2237)）
* 修复了函数文档字符串中的拼写错误 - 感谢 [@aadarshsingh191198](https://github.com/aadarshsingh191198)（[#2269](https://github.com/pycaret/pycaret/pull/2269)）
* 固定了 `numba<0.55`（[#2056](https://github.com/pycaret/pycaret/pull/2056)）

### **PyCaret 2.3.6**

**发布日期：2022年1月11日（新功能，错误修复）**

* 添加了新函数 `create_docker`（[https://github.com/pycaret/pycaret/pull/2005](https://github.com/pycaret/pycaret/pull/2005)）
* 添加了新函数 `create_api`（[https://github.com/pycaret/pycaret/pull/2000](https://github.com/pycaret/pycaret/pull/2000)）
* 添加了新函数 `check_fairness`（[https://github.com/pycaret/pycaret/pull/1997](https://github.com/pycaret/pycaret/pull/1997)）
* 添加了新函数 `eda`（[https://github.com/pycaret/pycaret/pull/1983](https://github.com/pycaret/pycaret/pull/1983)）
* 添加了新函数 `convert_model`（[https://github.com/pycaret/pycaret/pull/1959](https://github.com/pycaret/pycaret/pull/1959)）
* 在 `plot_model` 中添加了传递 kwargs 到绘图的功能 ([https://github.com/pycaret/pycaret/pull/19400](https://github.com/pycaret/pycaret/pull/19400))
* 在 `predict_model` 中添加了 `drift_report` 功能 ([https://github.com/pycaret/pycaret/pull/1935](https://github.com/pycaret/pycaret/pull/1935))
* 添加了新的函数 `dashboard` ([https://github.com/pycaret/pycaret/pull/1925](https://github.com/pycaret/pycaret/pull/1925))
* 在 `optimize_threshold` 中添加了 `grid_interval` 参数 - 感谢 @wolfryu ([https://github.com/pycaret/pycaret/pull/1938](https://github.com/pycaret/pycaret/pull/1938))
* 通过环境变量使日志级别可配置化 ([https://github.com/pycaret/pycaret/pull/2026](https://github.com/pycaret/pycaret/pull/2026))
* 使 AWS 中的可选路径可配置化 ([https://github.com/pycaret/pycaret/pull/2045](https://github.com/pycaret/pycaret/pull/2045))
* 修复了带有 PCA 的 TSNE 绘图问题 ([https://github.com/pycaret/pycaret/pull/2032](https://github.com/pycaret/pycaret/pull/2032))
* 修复了 streamlit 绘图的渲染问题 ([https://github.com/pycaret/pycaret/pull/2008](https://github.com/pycaret/pycaret/pull/2008))
* 修复了 `tree` 绘图中的类名 - 感谢 @yamasakih ([https://github.com/pycaret/pycaret/pull/1982](https://github.com/pycaret/pycaret/pull/1982))
* 修复了 NearZeroVariance 预处理器无法配置化的问题 - 感谢 @Flyfoxs ([https://github.com/pycaret/pycaret/pull/1952](https://github.com/pycaret/pycaret/pull/1952))
* 移除了重复的代码 - 感谢 @Flyfoxs ([https://github.com/pycaret/pycaret/pull/1882](https://github.com/pycaret/pycaret/pull/1882))
* 文档改进 - 感谢 @harsh204016, @khrapovs ([https://github.com/pycaret/pycaret/pull/1931/files](https://github.com/pycaret/pycaret/pull/1931/files), [https://github.com/pycaret/pycaret/pull/1956](https://github.com/pycaret/pycaret/pull/1956), [https://github.com/pycaret/pycaret/pull/1946](https://github.com/pycaret/pycaret/pull/1946), [https://github.com/pycaret/pycaret/pull/1949](https://github.com/pycaret/pycaret/pull/1949))
* 固定 `pyyaml<6.0.0` 版本以解决与 Google Colab 的问题

**发布：PyCaret 2.3.6 | 发布日期：2022年1月11日（新功能，错误修复）**

### **PyCaret 2.3.5**

**发布日期：2021年11月19日（新功能，错误修复）**

* 修复了 `Fix_multicollinearity` 在目标为浮点数时失败的问题 ([https://github.com/pycaret/pycaret/pull/1640](https://github.com/pycaret/pycaret/pull/1640))
* MLFlow 运行现在是嵌套的 - 感谢 @jfagn ([https://github.com/pycaret/pycaret/pull/1660](https://github.com/pycaret/pycaret/pull/1660))
* 修复了 REG102 教程中的拼写错误 - 感谢 @bobo-jamson ([https://github.com/pycaret/pycaret/pull/1684](https://github.com/pycaret/pycaret/pull/1684))
* 修复了 `interpret_model` 不始终遵守 `save_path` 的问题 ([https://github.com/pycaret/pycaret/pull/1707](https://github.com/pycaret/pycaret/pull/1707))
* 修复了某些绘图未被 MLFlow 记录的问题 ([https://github.com/pycaret/pycaret/pull/1769](https://github.com/pycaret/pycaret/pull/1769))
* 在 `compare_models` 中添加了虚拟模型以设定基线 - 感谢 @reza1615 ([https://github.com/pycaret/pycaret/pull/1739](https://github.com/pycaret/pycaret/pull/1739))
* 如果在数据集中指定的列在 `ignore_features` 中不存在，将改进错误消息 - 感谢 @reza1615 ([https://github.com/pycaret/pycaret/pull/1793](https://github.com/pycaret/pycaret/pull/1793))
* 通过各种方法中的 `probability_threshold` 参数，现在可以设置二元分类的自定义概率阈值 ([https://github.com/pycaret/pycaret/pull/1858](https://github.com/pycaret/pycaret/pull/1858))
* 将内部 CV 与验证 CV 从 `stack_models` 和 `calibrate_models` 中分离 ([https://github.com/pycaret/pycaret/pull/1849](https://github.com/pycaret/pycaret/pull/1849), [https://github.com/pycaret/pycaret/pull/1858](https://github.com/pycaret/pycaret/pull/1858))
* 如果安装了不正确版本的 `scikit-learn`，现在将引发 `RuntimeError` ([https://github.com/pycaret/pycaret/pull/1870](https://github.com/pycaret/pycaret/pull/1870))
* 改进了 readme、文档和存储库结构
* 取消了对 `numba` 的固定版本 ([https://github.com/pycaret/pycaret/pull/173](https://github.com/pycaret/pycaret/pull/173))

### **PyCaret 2.3.4**

#### **发布日期：2021年9月23日（新功能，错误修复）**

* 为分类和回归模块添加了 `get_leaderboard` 函数
* 现在可以使用 `plot_model` 和 `interpret_model` 的 save 参数指定绘图保存路径 - 感谢 @bhanuteja2001 ([https://github.com/pycaret/pycaret/pull/1537](https://github.com/pycaret/pycaret/pull/1537))
* 修复了 `interpret_model` 影响 `plot_model` 行为的问题 - 感谢 @naujgf ([https://github.com/pycaret/pycaret/pull/1600](https://github.com/pycaret/pycaret/pull/1600))
* 修复了conda构建的问题 - 感谢 @melonhead901 ([https://github.com/pycaret/pycaret/pull/1479](https://github.com/pycaret/pycaret/pull/1479))
* 文档改进 - 感谢 @caron14 和 @harsh204016 ([https://github.com/pycaret/pycaret/pull/1499](https://github.com/pycaret/pycaret/pull/1499), [https://github.com/pycaret/pycaret/pull/1502](https://github.com/pycaret/pycaret/pull/1502))
* 修复了在使用自定义估算器时`blend_models`和`stack_models`抛出异常的问题 ([https://github.com/pycaret/pycaret/pull/1500](https://github.com/pycaret/pycaret/pull/1500))
* 修复了“目标缺失”问题与****“移除多重共线性”选项 ([https://github.com/pycaret/pycaret/pull/1508)](https://github.com/pycaret/pycaret/pull/1508\))
* `compare_models`中的`errors="ignore"`参数现在可以正确忽略完整拟合过程中的错误 ([https://github.com/pycaret/pycaret/pull/1510](https://github.com/pycaret/pycaret/pull/1510))
* 修复了某些数据类型在设置过程中被错误编码为int64的问题 ([https://github.com/pycaret/pycaret/pull/1515](https://github.com/pycaret/pycaret/pull/1515))
* 固定了`numba<0.54` ([https://github.com/pycaret/pycaret/pull/1530](https://github.com/pycaret/pycaret/pull/1530))

### **PyCaret 2.3.3**

#### **发布日期：2021年7月24日（新功能，错误修复）**

* 通过固定`interpret<=0.2.4`来修复`[full]`安装的问题
* 在`deploy_model()`中添加了对AWS中S3文件夹路径的支持
* 启用了实验性的Optuna `TPESampler`选项以改善收敛性（在`tune_model()`中）

### **PyCaret 2.3.2**

#### **发布日期：2021年7月7日（新功能，错误修复）**

* 在`interpret_model`中实现了PDP、MSA和PFI图表 - 感谢 @IncubatorShokuhou ([https://github.com/pycaret/pycaret/pull/1415](https://github.com/pycaret/pycaret/pull/1415))
* 在`pycaret.classification`模块的`plot_model`下实现了Kolmogorov-Smirnov（KS）图表
* 修复了一个拼写错误“RVF”为“RBF” - 感谢 @baturayo ([https://github.com/pycaret/pycaret/pull/1220](https://github.com/pycaret/pycaret/pull/1220))
* 更新和改进了自述文件和许可证
* 修复了`remove_multicollinearity`考虑分类特征的问题
* 修复了PyCaret的cuML包装器的关键字问题
* 改进了迭代填充的性能
* 修复了`gain`和`lift`图表传入错误参数，创建误导性图表
* 在LightGBM上的`interpret_model`现在将显示蜂群图
* 在`pycaret.persistence`中对异常处理和文档进行了多项改进 ([https://github.com/pycaret/pycaret/pull/1324](https://github.com/pycaret/pycaret/pull/1324))
* `remove_perfect_collinearity`选项现在将显示在`setup()`摘要中 - 感谢 @mjkanji ([https://github.com/pycaret/pycaret/pull/1342](https://github.com/pycaret/pycaret/pull/1342))
* 修复了`IterativeImputer`设置错误的浮点精度
* 修复了在`tune_model`中使用自定义网格时由于列表组成而引发异常
* 改进了`pycaret.clustering`中的文档 - 感谢 @susmitpy ([https://github.com/pycaret/pycaret/pull/1372](https://github.com/pycaret/pycaret/pull/1372))
* 添加了对LightGBM CUDA版本的支持 - 感谢 @IncubatorShokuhou ([https://github.com/pycaret/pycaret/pull/1396](https://github.com/pycaret/pycaret/pull/1396))
* 在`get_data`中为替代数据源公开了`address` - 感谢 @IncubatorShokuhou ([https://github.com/pycaret/pycaret/pull/1416](https://github.com/pycaret/pycaret/pull/1416))

### **PyCaret 2.3.1**

#### **发布日期：2021年4月28日（修复了几个错误）**

* 修复了在load\_config()期间缺少变量（display\_container等）时的异常
* 修复了在GPU模式下使用Ridge和RF估算器时引发异常
* 修复了PyCaret的cuML包装器无法被pickle化
* 在内部方法get\_all\_object\_vars\_and\_properties中添加了额外检查，修复了某些估算器引发异常的问题
* save\_model()现在支持kwargs，这些kwargs将传递给joblib.dump()
* 修复了从AWS中load\_model()时的问题（重复的.pkl扩展名） - 感谢markgrujic ([https://github.com/pycaret/pycaret/pull/1128](https://github.com/pycaret/pycaret/pull/1128))
* 修复了文档中的一个拼写错误 - 感谢koorukuroo ([https://github.com/pycaret/pycaret/pull/1149](https://github.com/pycaret/pycaret/pull/1149))
* 优化了Fix\_multicollinearity转换器，大幅减少了保存的管道大小
* interpret\_model()现在支持作为参数传递的数据 - 感谢jbechtel ([https://github.com/pycaret/pycaret/pull/1184](https://github.com/pycaret/pycaret/pull/1184))
* 在`log_experiment=True`时，从MLflow日志记录中删除了`infer_signature`。
* 修复了一个罕见的情况，即binary\_multiclass\_score\_func无法被pickle化
* 修复了特征选择中的边缘情况异常
* 在使用GroupKFold CV时，修复了`finalize_model`引发的异常
* 固定了`mlxtend>=0.17.0`，`imbalanced-learn==0.7.0`和`gensim<4.0.0`

### PyCaret 2.3.0&#x20;
#### 发布日期：2021年2月21日

* **受影响的模块：** `pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly` `pycaret.arules`

#### 变更摘要

* 在 `pycaret.regression` 模块中添加了新的交互式残差图。现在，您可以通过在 `plot_model` 函数中使用 `residuals_interactive` 来生成交互式残差图。
* 增加了对 streamlit 应用程序的绘图支持。在 `plot_model` 函数中添加了一个新的参数 `display_format`。要在 streamlit 应用程序中渲染图形，请将其设置为 `streamlit`。
* 对 Boruta 特征选择算法进行了改进。（试试看吧！）
* `pycaret.classification` 和 `pycaret.regression` 中的 `tune_model` 现在与自定义模型兼容。
* 在关联规则模块中添加了对 low\_memory 和 max\_len 的支持 ([https://github.com/pycaret/pycaret/pull/1008](https://github.com/pycaret/pycaret/pull/1008))。
* 提高了 DataFrame 检查的鲁棒性 ([https://github.com/pycaret/pycaret/pull/1005](https://github.com/pycaret/pycaret/pull/1005))。
* 改进了从 AWS 加载模型的过程 ([https://github.com/pycaret/pycaret/pull/1005](https://github.com/pycaret/pycaret/pull/1005))。
* Catboost 和 XGBoost 现在是可选依赖项。它们不会自动安装在默认的精简安装中。要安装可选依赖项，请使用 `pip install pycaret[full]`。
* 在 `pycaret.classification` 模块的 `predict_model` 函数中添加了 `raw_score` 参数。当设置为 True 时，将分别返回每个类别的分数。
* PyCaret 现在在可能的情况下返回基本的 scikit-learn 对象。
* 当在 `setup` 函数中将 `handle_unknown_categorical` 设置为 False 时，如果数据中的分类特征包含未知级别，则在预测过程中将引发异常。
* 多类分类的 `predict_model` 现在将标签作为整数返回。
* 修复了 `pycaret.clustering` 和 `pycaret.anomaly` 中可能引发 IndexError 的边缘情况。
* 修复了 `pycaret.classification` 和 `pycaret.regression` 中某些图形的文本格式问题。
* 如果在初始化 `setup` 时无法创建 `logs.log` 文件，现在不会引发异常（未来将支持更多可配置的日志记录）。
* 用户添加的指标现在不会引发异常，而是返回 0.0。
* 与 tune-sklearn>=0.2.0 兼容。
* 修复了删除目标列中的 NaN 值的边缘情况。
* 修复了堆叠模型未正确调整的问题。
* 修复了在 fold\_shuffle=False 时 KFold 引发异常的问题。

### **PyCaret 2.2.3**

#### **发布日期：2020年12月22日（多个错误修复 | 重要的兼容性修复）**

* 修复了 `predict_model` 函数在数据列中包含非字符串字符时引发的异常。
* 修复了 `setup` 函数中 `remove_multicollinearity` 参数的一个罕见异常。
* 改进了将日期特征转换为分类特征的性能和鲁棒性。
* 修复了 `models` 函数在传递 `type` 参数时引发的异常。
* 现在可以使用 `pull` 函数访问 `setup` 后显示的数据框。
* 修复了 save\_config 引发的异常。
* 修复了目标列被视为 ID 列并因此被删除的一种罕见情况。
* 现在可以保存 SHAP 图（将 save 参数设置为 True）。
* **| 重要 |** 兼容性在 catboost、pyod（其他影响尚不清楚）与 sklearn=0.24（于2020年12月22日发布）中中断。临时解决方法是在 `requirements.txt` 中明确要求 0.23.2。

### **PyCaret 2.2.2**

#### **发布日期：2020年11月25日（多个错误修复）**

* 修复了 `pycaret.classification` 模块中 `optimize_threshold` 函数的问题。现在它返回一个浮点数而不是一个数组。
* 修复了 `predict_model` 函数的问题。它现在使用原始数据框来附加预测结果。因此，在返回预测结果时，不会删除推理时提供的任何额外列。而是在预测时内部忽略它们。
* 修复了 `pycaret.clustering` 中 `create_model` 函数的边缘情况异常。
* 修复了列名不是字符串时引发的异常。
* 修复了 `pycaret.regression` 中 `setup` 函数中 `transform_target` 设置为 True 时的异常。
* 修复了 `models` 函数在指定 `type` 参数时引发的异常。

### **PyCaret 2.2.1**

#### **发布日期：2020年11月09日（多个错误修复）**

在 `2.2` 版本发布后，已修复以下问题：

* 修复了 `plot_model = 'tree'` 引发的异常。
* 修复了 `predict_model` 在非连续索引时引发错误的问题。
* 修复了 `setup` 函数中 `remove_outliers` 参数的问题。它在训练数据中引入了额外的列。现在问题已经修复。
* 修复了 `pycaret.clustering` 中 `plot_model` 引发的异常，该异常在非连续索引时引发错误。
* 当 `setup` 函数中的 `imputation_type` 设置为 'iterative' 时，保存或记录模型时引发异常。
* `compare_models` 现在在 `html=False` 时打印中间输出。
* 二分类问题中 `pycaret.classification` 中的指标现在使用 `average='binary'` 进行计算。之前是正负类的加权平均，现在只计算正类。对于多分类问题，使用 `average='weighted'`。
* `optimize_threshold` 现在返回优化后的概率阈值值作为 numpy 对象。
* 修复了 `compare_models` 中的某些异常问题。
* 在 `setup` 函数中添加了 `profile_kwargs` 参数，用于将关键字参数传递给 Pandas Profiler。
* `plot_model`、`interpret_model` 和 `evaluate_model` 现在接受一个新参数 `use_train_data`，当设置为 True 时，会在训练数据上生成图表，而不是测试数据。

### PyCaret 2.2**.0**

#### 发布日期：2020年10月28日

#### 变更摘要

* **受影响模块：** `pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly`
* **分离训练集和测试集：** 在 `pycaret.classification` 和 `pycaret.regression` 的 `setup` 函数中添加了新参数 `test_data`。当将 DataFrame 传递给 `test_data` 时，它将被用作保留集，`train_size` 参数将被忽略。`test_data` 必须带有标签，并且其形状必须与 `data` 的形状相匹配。
* **禁用默认预处理：** 在 `setup` 函数中添加了一个新参数 `preprocess`。当 `preprocess` 设置为 `False` 时，除了 `train_test_split` 和在 `custom_pipeline` 参数中传递的自定义转换之外，不会应用任何转换。当 preprocess 设置为 False 时，数据必须准备好进行建模（没有缺失值，没有日期，分类数据编码）。
* **自定义指标：** 在 `pycaret.classification`、`pycaret.regression` 和 `pycaret.clustering` 中现在添加了新函数 `get_metric`、`add_metric` 和 `remove_metric`，可用于添加/删除模型评估中使用的指标。
* **自定义转换：** 在 `setup` 函数中添加了一个新参数 `custom_pipeline`。它接受一个 `(str, transformer)` 元组或元组列表。当传递时，它将在预处理流水线中附加自定义转换器，并分别应用于每个 CV 折叠以及最终拟合。所有自定义转换都应用于 `train_test_split` 之后和 pycaret 内部转换之前。
* **启用 GPU 训练：** 要在 `setup` 函数中使用 GPU 进行训练，可以将 `use_gpu` 参数设置为 `True` 或 `force`。设置为 True 时，它将使用支持 GPU 的算法，并对剩余算法使用 CPU。设置为 `force` 时，它将仅使用支持 GPU 的算法，并在不可用时引发异常。以下算法支持 GPU：
  * 极端梯度提升 `pycaret.classification` `pycaret.regression`
  * LightGBM `pycaret.classification` `pycaret.regression`
  * CatBoost `pycaret.classification` `pycaret.regression`
  * 随机森林 `pycaret.classification` `pycaret.regression`
  * K-最近邻 `pycaret.classification` `pycaret.regression`
  * 支持向量机 `pycaret.classification` `pycaret.regression`
  * 逻辑回归 `pycaret.classification`
  * 岭回归分类器 `pycaret.classification`
  * 线性回归 `pycaret.regression`
  * Lasso 回归 `pycaret.regression`
  * 岭回归 `pycaret.regression`
  * 弹性网络（回归） `pycaret.regression`
  * K-均值 `pycaret.clustering`
  * 基于密度的空间聚类 `pycaret.clustering`
* **超参数调优：** 在 `pycaret.classification` 和 `pycaret.regression` 的 `tune_model` 函数中添加了超参数调优的新方法。`tune_model` 函数中添加了新参数 `search_library` 和 `search_algorithm`。`search_library` 可以是 `scikit-learn`、`scikit-optimize`、`tune-sklearn` 和 `optuna`。`search_algorithm` 参数可以根据其 `search_library` 接受以下值：

    * scikit-learn：`random` `grid`
    * scikit-optimize：`bayesian`
    * tune-sklearn：`random` `grid` `bayesian` `hyperopt` `bohb`
    * optuna：`random` `tpe`

    除了 `scikit-learn`，所有其他搜索库不是 pycaret 的硬依赖项，必须单独安装。
* **提前停止：** 现在支持超参数调优的提前停止。在 `pycaret.classification` 和 `pycaret.regression` 的 `tune_model` 函数中添加了一个新参数 `early_stopping`。当 `search_library` 为 `scikit-learn` 时，或者估计器没有 'partial\_fit' 属性时，它将被忽略。它可以是搜索库接受的对象，或以下之一：
  * `asha` 用于异步连续减半算法
  * `hyperband` 用于 Hyperband
  * `median` 用于中位数停止规则
  * 当为 `False` 或 `None` 时，将不使用提前停止。
* **迭代插补：** 现在已经实现了数值和分类缺失值的迭代插补类型。在`setup`函数中新增了参数`imputation_type`、`iterative_imptutation_iters`、`categorical_iterative_imputer`和`numeric_iterative_imputer`。详细信息请阅读博客文章：[https://www.linkedin.com/pulse/iterative-imputation-pycaret-22-antoni-baum/?trackingId=Shg1zF%2F%2FR5BE7XFpzfTHkA%3D%3D](https://www.linkedin.com/pulse/iterative-imputation-pycaret-22-antoni-baum/?trackingId=Shg1zF%2F%2FR5BE7XFpzfTHkA%3D%3D)
* **新增绘图：** 新增了以下绘图：
  * 提升曲线 `pycaret.classification`
  * 增益曲线 `pycaret.classification`
  * 决策树 `pycaret.classification` `pycaret.regression`
  * 全部特征 `pycaret.classification` `pycaret.regression`
* **CatBoost兼容性：** `CatBoostClassifier` 和 `CatBoostRegressor` 现在与 `plot_model` 兼容。需要`catboost>=0.23.2`。
* **MLFlow服务器中的对数图：** 现在可以在`MLFlow`跟踪服务器中记录`plot_model`函数中可用的任何图。要记录特定图，请在`log_plots`参数中传递包含图 ID 的列表。查看`plot_model`的文档以查看所有可用的图。
* **数据分割分层：** 在`pycaret.classification`和`pycaret.regression`的`setup`函数中新增了一个名为`data_split_stratify`的参数。它控制`train_test_split`期间的分层。设置为True时，将按目标列进行分层。要按其他列进行分层，请传递列名的列表。
* **折叠策略：** 在`pycaret.classification`和`pycaret.regression`的`setup`函数中新增了一个名为`fold_strategy`的参数。默认情况下，对于`pycaret.classification`是'stratifiedkfold'，对于`pycaret.regression`是'kfold'。可能的值包括：
  * `kfold` 用于KFold交叉验证；
  * `stratifiedkfold` 用于分层KFold交叉验证；
  * `groupkfold` 用于组KFold交叉验证；
  * `timeseries` 用于时间序列分割交叉验证；或
  * 与scikit-learn兼容的自定义CV生成器对象。
* **全局折叠参数：** 在`pycaret.classification`和`pycaret.regression`的`setup`函数中新增了一个名为`fold`的参数。它控制交叉验证中要使用的折数。这是一个全局设置，可以通过在每个函数内部使用`fold`参数来覆盖。当`fold_strategy`是自定义对象时将被忽略。
* **折叠组：** 当`fold_strategy`为`groupkfold`时，可选的组标签。它接受一个形状为(n_samples, )的数组，其中n_samples是训练数据集中的行数。当传递字符串时，它被解释为数据集中包含组标签的列名。
* **转换流水线：** 现在所有转换都应用在`train_test_split`之后。
* **数据类型处理：** 所有内部数据类型处理已从`int64`和`float64`更改为分别为`int32`和`float32`，以改善内存使用和性能，以及与基于GPU的算法更好地兼容。
* **AutoML行为更改：** `pycaret.classification`和`pycaret.regression`中的`automl`函数不再在整个数据集上重新拟合模型。因此，如果需要在整个数据集（包括留出集）上拟合模型，则必须显式使用`finalize_model`。
* **默认调优网格：** `RandomForest`、`XGBoost`、`CatBoost`和`LightGBM`的默认超参数调优网格已更改，以删除`max_depth`等训练强度参数的极端值，以加快调优过程。
* **随机森林默认值：** `RandomForestClassifier`和`RandomForestRegressor`的`n_estimators`默认值已从`10`更改为`100`，以使其与`scikit-learn`的默认行为一致。
* **多类分类的AUC：** 多类目标的AUC现在在度量评估中可用。
* **Google Colab显示：** 现在屏幕上打印的所有输出（信息网格、分数网格）现在与Google Colab兼容的格式，从而提高语义。
* **已移除的抽样参数：** `pycaret.classification`和`pycaret.regression`的`setup`函数中现在已移除了`sampling`参数。
* **类型提示：** 为了使使用和开发更容易，所有更新后的pycaret函数都添加了类型提示，符合最佳实践。用户可以通过使用支持类型提示的IDE来利用这些功能。
* **文档：** 网站上的所有模块文档现已停用。更新后的文档在此处可用：[https://pycaret.readthedocs.io/en/latest/](https://pycaret.readthedocs.io/en/latest/)

#### 函数级别更改

#### PyCaret 2.2 中引入的新函数

* **get\_metrics：** 返回用于交叉验证的可用指标表。`pycaret.classification` `pycaret.regression` `pycaret.clustering`
* **ensemble: bool, default = False** When set to True, creates an ensemble of models using the best performing models based on the evaluation metric.
* **method: str, default = None** The method used for creating the ensemble. Possible values are:
  * 'Bagging'
  * 'Boosting'
  * 'Stacking'
  * 'Voting'
* **fold: int, default = 10** The number of folds to be used in cross-validation for creating the ensemble. Must be at least 2.
* **round: int, default = 4** The number of decimal places for rounding the average of the ensemble predictions.
* **verbose: bool, default = True** Controls the verbosity of the ensemble creation process.

#### tune\_model

`pycaret.classification` `pycaret.regression`

Following new parameters have been added:

* **n\_iter: int, default = 10** The number of iterations for the random search. Ignored when `optimize` is not 'random'.
* **optimize: str, default = 'accuracy'** The metric to optimize during hyperparameter tuning. Possible values are:
  * 'accuracy'
  * 'auc'
  * 'recall'
  * 'precision'
  * 'f1'
  * 'kappa'
  * 'mcc'
  * 'ap'
  * 'ndcg'
  * 'map'
  * 'mlogloss'
  * 'rmse'
  * 'mse'
  * 'mae'
  * 'r2'
  * 'rmsle'
  * 'mape'
  * 'pearson'
  * 'determination'
* **early\_stopping: bool, default = False** When set to True, early stopping is applied during hyperparameter tuning.
* **early\_stopping\_rounds: int, default = 50** The number of rounds without improvement before early stopping is triggered.
* **early\_stopping\_tolerance: float, default = 0.001** The minimum improvement required to consider it as an improvement and not trigger early stopping.
* **custom\_grid: dict, default = None** A dictionary containing custom hyperparameter grids for the models. The keys of the dictionary should be the model names and the values should be the hyperparameter grids.
* **search\_library: str, default = 'scikit-learn'** The library to use for hyperparameter tuning. Possible values are:
  * 'scikit-learn'
  * 'optuna'
  * 'tune-sklearn'
  * 'scikit-optimize'
  * 'hyperopt'
  * 'optunity'
  * 'genetic'
  * 'pyswarms'
  * 'skopt'
  * 'bayesian-optimization'
  * 'optunity'
  * 'goptuna'
  * 'hyperopt'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyperopt'
  * 'goptuna'
  * 'tpe'
  * 'annealing'
  * 'pysot'
  * 'dragonfly'
  * 'nevergrad'
  * 'optuna'
  * 'optunity'
  * 'hyper
* **cross\_validation: bool = True** 当设置为 False 时，将在留出集上评估指标。当 cross\_validation 设置为 False 时，fold 参数将被忽略。
* **fit\_kwargs: Optional\[dict] = None** 传递给模型的 fit 方法的参数字典。
* **groups: Optional\[Union\[str, Any]] = None** 在使用 'GroupKFold' 进行交叉验证时的可选组标签。它接受一个形状为 (n\_samples, ) 的数组，其中 n\_samples 是训练数据集中的行数。当传递一个字符串时，它被解释为数据集中包含组标签的列名。

以下参数已被移除：

* **ensemble** - 已弃用 - 直接使用 `ensemble_model` 函数。
* **method** - 已弃用 - 直接使用 `ensemble_model` 函数。
* **system** - 已移至私有 API。

#### tune\_model

`pycaret.classification` `pycaret.regression`

以下新参数已被添加：

*   **search\_library: str, default = 'scikit-learn'** 用于调整超参数的搜索库。可能的值为：

    'scikit-learn' - 默认值，无需进一步安装 [https://github.com/scikit-learn/scikit-learn](https://github.com/scikit-learn/scikit-learn)

    'scikit-optimize' - `pip install scikit-optimize` [https://scikit-optimize.github.io/stable/](https://scikit-optimize.github.io/stable/)

    'tune-sklearn' - `pip install tune-sklearn ray[tune]`[ https://github.com/ray-project/tune-sklearn](https://github.com/ray-project/tune-sklearn)

    'optuna' - `pip install optuna` [https://optuna.org/](https://optuna.org/)
*   **search\_algorithm: str, default = None** 搜索算法取决于 `search_library` 参数。某些搜索算法需要安装额外的库。当为 None 时，将使用搜索库特定的默认算法。

    `scikit-learn` 可能的值：- random（默认）- grid

    `scikit-optimize` 可能的值：- bayesian（默认）

    `tune-sklearn` 可能的值：- random（默认）- grid - bayesian `pip install scikit-optimize` - hyperopt `pip install hyperopt` - bohb `pip install hpbandster ConfigSpace`

    `optuna` 可能的值：- tpe（默认）- random
* **early\_stopping: bool or str or object, default = False** 使用早期停止来停止对超参数配置的拟合，如果表现不佳。当 `search_library` 是 scikit-learn 时被忽略，或者估计器没有 'partial\_fit' 属性时被忽略。如果为 False 或 None，则不会使用早期停止。可以是搜索库接受的对象，或以下之一：
  * 'asha' 代表异步连续减半算法
  * 'hyperband' 代表 Hyperband
  * 'median' 代表中位数停止规则
  * 如果为 False 或 None，则不会使用早期停止。
* **early\_stopping\_max\_iters: int, default = 10** 每个采样配置运行的最大迭代次数。如果 `early_stopping` 为 False 或 None，则被忽略。
* **fit\_kwargs: Optional\[dict] = None** 传递给模型的 fit 方法的参数字典。
* **groups: Optional\[Union\[str, Any]] = None** 在使用 'GroupKFold' 进行交叉验证时的可选组标签。它接受一个形状为 (n\_samples, ) 的数组，其中 n\_samples 是训练数据集中的行数。当传递一个字符串时，它被解释为数据集中包含组标签的列名。
* **return\_tuner: bool, default = False** 当设置为 True 时，将返回一个元组 (model, tuner\_object)。
* **tuner\_verbose: bool or in, default = True** 如果为 True 或大于 0，则会打印来自调整器的消息。值越高，打印的消息越多。当 `verbose` 参数为 False 时被忽略。

#### ensemble\_model

`pycaret.classification` `pycaret.regression`

以下新参数已被添加：

* **fit\_kwargs: Optional\[dict] = None** 传递给模型的 fit 方法的参数字典。
* **groups: Optional\[Union\[str, Any]] = None** 在使用 'GroupKFold' 进行交叉验证时的可选组标签。它接受一个形状为 (n\_samples, ) 的数组，其中 n\_samples 是训练数据集中的行数。当传递一个字符串时，它被解释为数据集中包含组标签的列名。

#### blend\_models

`pycaret.classification` `pycaret.regression`

以下新参数已被添加：

* **fit\_kwargs: Optional\[dict] = None** 传递给模型的 fit 方法的参数字典。
* **groups: Optional\[Union\[str, Any]] = None** 在使用 'GroupKFold' 进行交叉验证时的可选组标签。它接受一个形状为 (n\_samples, ) 的数组，其中 n\_samples 是训练数据集中的行数。当传递一个字符串时，它被解释为数据集中包含组标签的列名。
* **weights: list, default = None** 权重序列（浮点数或整数），用于对预测类标签的出现进行加权（硬投票）或在平均之前加权类概率（软投票）。当为 None 时使用均匀权重。
* `method` 参数的默认值已从 `hard` 更改为 `auto`。

#### stack\_models

`pycaret.classification` `pycaret.regression`

添加了以下新参数：

* **fit\_kwargs: Optional\[dict] = None** 传递给模型的 fit 方法的参数字典。
* **groups: Optional\[Union\[str, Any]] = None** 当使用 'GroupKFold' 进行交叉验证时，可选的组标签。它接受一个形状为 (n\_samples, ) 的数组，其中 n\_samples 是训练数据集中的行数。当传递一个字符串时，它被解释为包含组标签的数据集中的列名。

#### calibrate\_model

`pycaret.classification`

添加了以下新参数：

* **fit\_kwargs: Optional\[dict] = None** 传递给模型的 fit 方法的参数字典。
* **groups: Optional\[Union\[str, Any]] = None** 当使用 'GroupKFold' 进行交叉验证时，可选的组标签。它接受一个形状为 (n\_samples, ) 的数组，其中 n\_samples 是训练数据集中的行数。当传递一个字符串时，它被解释为包含组标签的数据集中的列名。

#### plot\_model

`pycaret.classification` `pycaret.regression`

添加了以下新参数：

* **fold: int or scikit-learn compatible CV generator, default = None** 控制交叉验证。如果为 None，则使用 `setup` 函数中 `fold_strategy` 参数中的 CV 生成器。当传递一个整数时，它被解释为 `setup` 函数中 CV 生成器的 'n\_splits' 参数。
* **fit\_kwargs: Optional\[dict] = None** 传递给模型的 fit 方法的参数字典。
* **groups: Optional\[Union\[str, Any]] = None** 当使用 'GroupKFold' 进行交叉验证时，可选的组标签。它接受一个形状为 (n\_samples, ) 的数组，其中 n\_samples 是训练数据集中的行数。当传递一个字符串时，它被解释为包含组标签的数据集中的列名。

#### evaluate\_model

`pycaret.classification` `pycaret.regression`

添加了以下新参数：

* **fold: int or scikit-learn compatible CV generator, default = None** 控制交叉验证。如果为 None，则使用 `setup` 函数中 `fold_strategy` 参数中的 CV 生成器。当传递一个整数时，它被解释为 `setup` 函数中 CV 生成器的 'n\_splits' 参数。
* **fit\_kwargs: Optional\[dict] = None** 传递给模型的 fit 方法的参数字典。
* **groups: Optional\[Union\[str, Any]] = None** 当使用 'GroupKFold' 进行交叉验证时，可选的组标签。它接受一个形状为 (n\_samples, ) 的数组，其中 n\_samples 是训练数据集中的行数。当传递一个字符串时，它被解释为包含组标签的数据集中的列名。

#### finalize\_model

`pycaret.classification` `pycaret.regression`

添加了以下新参数：

* **fit\_kwargs: Optional\[dict] = None** 传递给模型的 fit 方法的参数字典。
* **groups: Optional\[Union\[str, Any]] = None** 当使用 'GroupKFold' 进行交叉验证时，可选的组标签。它接受一个形状为 (n\_samples, ) 的数组，其中 n\_samples 是训练数据集中的行数。当传递一个字符串时，它被解释为包含组标签的数据集中的列名。
* **model\_only: bool, default = True** 当设置为 False 时，只重新训练模型对象，忽略 Pipeline 中的所有转换。

#### models

`pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly`

添加了以下新参数：

* **internal: bool, default = False** 当为 True 时，将返回内部使用的额外列和行。
* **raise\_errors: bool, default = True** 当为 False 时，将抑制所有异常，忽略无法创建的模型。

### **PyCaret 2.1.2**

#### **发布日期：2020年8月31日（错误修复）**

* 在发布 `2.1` 后，有一个报告的 bug 阻止了 `regression` 模块中的 `predict_model` 函数在新的笔记本会话中工作，当在模型训练期间将 `transform_target` 设置为 `False` 时。这个问题已在 PyCaret 发布 `2.1.2` 中修复。了解更多关于该问题的信息：[https://github.com/pycaret/pycaret/issues/525](https://github.com/pycaret/pycaret/issues/525)

### **PyCaret 2.1.1**

#### **发布日期：2020年8月30日（错误修复）**
* 在发布 `2.1` 版本后，发现了 MLFlow 后端的一个 bug。只有当 `setup` 函数中的 `log_experiment` 设置为 True 时才会出现错误，并且适用于所有模块。已经确定了错误的原因，并向 `MLFlow` 提交了一个问题。错误是由 `mlflow.sklearn.log_model` 中的 `infer_signature` 函数引起的，仅在数据集中存在缺失值时才会引发错误。这个问题已经在 PyCaret 发布的 `2.1.1` 版本中得到修复，通过在 `MLFlow` 引发异常时跳过签名来解决这个问题。

### **PyCaret 2.1**

#### **发布日期：2020年8月28日**

#### 变更摘要

* **模型部署** 在所有模块的 `deploy_model` 函数中增加了对 `gcp` 和 `azure` 的模型部署支持。详情请参阅 `文档`。
* **比较模型预算时间** 在 `compare_models` 函数中新增了参数 `budget_time`。可以使用 `budget_time` 参数设置 `compare_models` 的训练时间上限。
* **特征选择** 新增了特征选择方法 `boruta` 用于特征选择。默认情况下，`setup` 函数中的 `feature_selection_method` 参数设置为 `classic`，但可以将其设置为 `boruta` 以使用 boruta 算法进行特征选择。此更改适用于 `pycaret.classification` 和 `pycaret.regression`。
* **数值填充** 在 `setup` 函数的 `numeric_imputation` 中新增了方法 `zero`。当方法设置为 `zero` 时，缺失值将被替换为常数 0。`numeric_imputation` 的默认行为未更改。
* **绘制模型** 在所有模块的 `plot_model` 中新增了参数 `scale`，以生成适用于研究出版物的高质量图像。
* **用户定义损失函数** 现在可以在 `pycaret.classification` 和 `pycaret.regression` 的 `tune_model` 中传递 `custom_scorer` 以优化用户定义的损失函数。您必须使用 `sklearn` 中的 `make_scorer` 创建可传递给 `tune_model` 函数中的 `custom_scorer` 的自定义损失函数。
* **管道行为变更** 在使用 `save_model` 时，`model` 对象被追加到 `Pipeline` 中，因此 `Pipeline` 和 `predict_model` 的行为现已更改。现在，`save_model` 不再保存 `list`，而是保存 `Pipeline` 对象，其中训练模型位于最后位置。`predict_model` 的前端用户功能保持不变。
* **比较模型** 参数 `blacklist` 和 `whitelist` 现已更名为 `exclude` 和 `include`，功能未更改。
* **预测模型标签** `pycaret.classification` 中 `predict_model` 函数返回的 `Label` 列现在返回原始标签而不是编码值。此更改旨在使 `predict_model` 的输出更易读。新增了一个参数 `encoded_labels`，默认值为 `False`。当设置为 `True` 时，将返回编码标签。
* **模型记录** 当 `log_experiment` 设置为 `True` 时，后端中的模型持久性现已更改。现在不再使用内部的 `save_model` 功能，而是采用 `mlflow.sklearn.save_model`，以允许使用模型注册表和 `MLFlow` 的本机部署功能。
* **CatBoost 兼容性** `CatBoostClassifier` 现在与 `pycaret.classification` 中的 `blend_models` 兼容。因此，`blend_models` 在没有任何 `estimator_list` 的情况下现在将混合总共 `15` 个估算器，包括 `CatBoostClassifier`。
* **堆叠模型** `pycaret.classification` 和 `pycaret.regression` 中的 `stack_models` 现在采用来自 `sklearn` 的 `StackingClassifier()` 和 `StackingRegressor`。因此，`stack_models` 函数现在返回 `sklearn` 对象，而不是之前版本中的自定义 `list`。
* **创建 Stacknet** `pycaret.classification` 和 `pycaret.regression` 中的 `create_stacknet` 现已移除。
* **调整模型** `pycaret.classification` 和 `pycaret.regression` 中的 `tune_model` 现在继承输入 `estimator` 的参数。因此，如果您在 GPU 上训练了 `xgboost`、`lightgbm` 或 `catboost`，将不会继承自 `estimator` 的训练方法。
* **解释模型** 在 `interpret_model` 中现已添加 `**kwargs` 参数。
* **Pandas 分类类型** 所有模块现在兼容 `pandas.Categorical` 对象。在内部，它们被转换为对象，并且与 `object` 或 `bool` 一样对待。
* **use\_gpu** 在 `pycaret.classification` 和 `pycaret.regression` 的 `setup` 函数中新增了一个参数。在 `2.1` 版本中，它被添加以准备未来版本中所需的后端工作。因此，在 `2.1` 中使用 `use_gpu` 参数没有影响。
* **单元测试** 单元测试得到增强。持续改进进行中 [https://github.com/pycaret/pycaret/tree/master/pycaret/tests](https://github.com/pycaret/pycaret/tree/master/pycaret/tests)
* **自动化文档已添加** 现在已添加自动化文档。网站上的文档仅在`major`版本发布0.X时更新。对于所有次要的每月发布，文档将在以下网址提供：[https://pycaret.readthedocs.io/en/latest/](https://pycaret.readthedocs.io/en/latest/)

* **引入 GitHub Actions** CI/CD 构建测试现已从`travis-ci`迁移到`github-actions`。`pycaret-nightly`现在每24小时自动发布一次。

* **教程** 所有教程现已更新为使用`pycaret==2.0`。[https://github.com/pycaret/pycaret/tree/master/tutorials](https://github.com/pycaret/pycaret/tree/master/tutorials)

* **资源** 在`/pycaret/resources/`下添加了新资源。[https://github.com/pycaret/pycaret/tree/master/resources](https://github.com/pycaret/pycaret/tree/master/resources)

* **示例笔记本** 在`/pycaret/examples/`下添加了许多示例笔记本。[https://github.com/pycaret/pycaret/tree/master/examples](https://github.com/pycaret/pycaret/tree/master/examples)

### **PyCaret 2.0**

#### **发布日期：2020年7月31日**

#### 变更摘要

* **实验记录** 添加了 MLFlow 记录后端。`setup`中添加了新参数`log_experiment` `experiment_name` `log_profile` `log_data`。在`pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly` `pycaret.nlp`中可用\

* **保存/加载实验** 在 PyCaret 2.0 中，从`pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly` `pycaret.nlp`中移除了`save_experiment`和`load_experiment`函数\

* **系统记录** 在执行`setup`时现在会生成系统日志文件。`logs.log`文件保存在当前工作目录中。可以使用`get_system_logs`函数在笔记本中访问日志文件\

* **命令行支持** 在笔记本之外使用 PyCaret 2.0 时，`setup`中的`html`参数必须设置为 False\

* **不平衡数据集** 在`setup`中为`pycaret.classification`添加了`fix_imbalance`和`fix_imbalance_method`参数。当设置为 True 时，默认应用 SMOTE 来为少数类创建合成数据点。要更改方法，请在`fix_imbalance_method`参数中传递任何支持`fit_resample`方法的`imblearn`类\

* **保存图表** 在`plot_model`中添加了`save`参数。当设置为 True 时，将图表保存为`png`或`html`格式到当前工作目录\

* **kwargs** 在`pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly`的`create_model`中添加了`kwargs**`\

* **choose_better** 在`pycaret.classification`和`pycaret.regression`的`tune_model` `ensemble_model` `blend_models` `stack_models` `create_stacknet`中添加了`choose_better`和`optimize`参数。阅读下面的详细信息以了解更多关于此的内容\

* **训练时间** 在`pycaret.classification`和`pycaret.regression`的`compare_models`函数中添加了`TT (Sec)`\

* **新指标：MCC** 在`pycaret.classification`的得分网格中添加了`MCC`指标\

* **新函数：automl()** 在`pycaret.classification` `pycaret.regression`中添加了新函数`automl`\

* **新函数：pull()** 在`pycaret.classification` `pycaret.regression`中添加了新函数`pull`\

* **新函数：models()** 在`pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly` `pycaret.nlp`中添加了新函数`models`\

* **新函数：get_logs()** 在`pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly` `pycaret.nlp`中添加了新函数`get_logs`\

* **新函数：get_config()** 在`pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly` `pycaret.nlp`中添加了新函数`get_config`\

* **新函数：set_config()** 在`pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly` `pycaret.nlp`中添加了新函数`set_config`\

* **新函数：get_system_logs** 在`pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly` `pycaret.nlp`中添加了新函数`get_logs`\

* **行为变更：compare_models** `compare_models`现在返回由`n_select`参数定义的前`n`个模型，默认设置为1\

* **行为变更：tune_model** `pycaret.classification`和`pycaret.regression`中的`tune_model`函数现在需要传递训练好的模型对象作为`estimator`，而不是字符串缩写/ID\

* **移除的依赖项** 从`requirements.txt`中移除了`awscli`和`shap`。要在`pycaret.classification` `pycaret.regression`中使用`interpret_model`函数和在`pycaret.classification` `pycaret.regression` `pycaret.clustering` `pycaret.anomaly`中使用`deploy_model`函数，必须单独安装这些库\

#### setup

**`pycaret.classification`  `pycaret.regression`  `pycaret.clustering`  `pycaret.anomaly`  `pycaret.nlp`**
* 在 `setup()` 中添加了 **`remove_perfect_collinearity`** 参数。默认设置为 False。\
  当设置为 True 时，会从数据集中删除完全共线的特征（相关性为1）。当两个特征完全相关时，其中一个将随机从数据集中删除。

* 在 `setup()` 中添加了 **`fix_imbalance`** 参数。默认设置为 False。\
  当数据集的目标类别分布不平衡时，可以使用 fix_imbalance 参数进行修正。当设置为 True 时，默认情况下会应用 SMOTE（合成少数类过采样技术）来为少数类别创建合成数据点。

* 在 `setup()` 中添加了 **`fix_imbalance_method`** 参数。默认设置为 None。\
  当 fix_imbalance 设置为 True 且 fix_imbalance_method 为 None 时，默认情况下会在交叉验证期间使用 'smote' 来对少数类别进行过采样。该参数接受任何支持 'fit_resample' 方法的 'imblearn' 模块。

* 在 `setup()` 中添加了 **`data_split_shuffle`** 参数。默认设置为 True。\
  如果设置为 False，则在拆分数据时阻止行的洗牌。

* 在 `setup()` 中添加了 **`folds_shuffle`** 参数。默认设置为 False。\
  如果设置为 False，则在使用交叉验证时阻止行的洗牌。

* 在 `setup()` 中添加了 **`n_jobs`** 参数。默认设置为 -1。\
  并行运行的作业数（对支持并行处理的函数而言），-1 表示使用所有处理器。要在单个处理器上运行所有函数，请将 n_jobs 设置为 None。

* 在 `setup()` 中添加了 **`html`** 参数。默认设置为 True。\
  如果设置为 False，则阻止运行时显示监视器。在使用不支持 HTML 的环境时，必须将其设置为 False。

* 在 `setup()` 中添加了 **`log_experiment`** 参数。默认设置为 False。\
  当设置为 True 时，所有指标和参数都会记录在 MLFlow 服务器上。

* 在 `setup()` 中添加了 **`experiment_name`** 参数。默认设置为 None。\
  用于记录的实验名称。当设置为 None 时，默认使用 'clf' 作为实验名称的别名。

* 在 `setup()` 中添加了 **`log_plots`** 参数。默认设置为 False。\
  当设置为 True 时，特定的图表将作为 png 文件记录在 MLflow 中。

* 在 `setup()` 中添加了 **`log_profile`** 参数。默认设置为 False。\
  当设置为 True 时，数据概要也会作为 html 文件记录在 MLflow 中。

* 在 `setup()` 中添加了 **`log_data`** 参数。默认设置为 False。\
  当设置为 True 时，训练集和测试集将作为 csv 文件记录。

* 在 `setup()` 中添加了 **`verbose`** 参数。默认设置为 True。\
  当 verbose 设置为 False 时，不会打印信息网格。

#### compare_models

**`pycaret.classification`  `pycaret.regression`**

* 在 `compare_models` 中添加了 **`whitelist`** 参数。默认设置为 None。\
  为了仅运行比较中的某些模型，可以将模型 ID 作为字符串列表传递给白名单参数。

* 在 `compare_models` 中添加了 **`n_select`** 参数。默认设置为 1。\
  返回前 n 个模型。使用负数参数进行底部选择。例如，n_select = -3 表示底部 3 个模型。

* 在 `compare_models` 中添加了 **`verbose`** 参数。默认设置为 True。\
  当 verbose 设置为 False 时，不会打印得分网格。

#### create_model

**`pycaret.classification`  `pycaret.regression`  `pycaret.clustering`  `pycaret.anomaly`**

* 在 `create_model` 中添加了 **`cross_validation`** 参数。默认设置为 True。\
  当 cross_validation 设置为 False 时，忽略 fold 参数，并在整个训练数据集上训练模型。不返回任何度量评估结果。仅适用于 `pycaret.classification` 和 `pycaret.regression`。

* 在 `create_model` 中添加了 **`system`** 参数。默认设置为 True。\
  必须始终保持为 True。只能由内部函数更改。

* 在 `create_model` 中添加了 **`ground_truth`** 参数。默认设置为 None。\
  当提供 ground_truth 时，将评估同质性分数、Rand 指数和完整性分数，并将其与其他度量一起打印出来。仅适用于 **`pycaret.clustering`**。

* 在 `create_model` 中添加了 **`kwargs`** 参数。\
  传递给估计器的其他关键字参数。

#### tune_model

**`pycaret.classification`  `pycaret.regression`  `pycaret.clustering`  `pycaret.anomaly`  `pycaret.nlp`**

* 在 `tune_model` 中添加了 **`custom_grid`** 参数。默认设置为 None。\
  若要使用自定义超参数进行调优，请将参数名称和要迭代的值的字典传递给 custom_grid 参数。当设置为 None 时，使用预定义的调优网格。对于 `pycaret.clustering` `pycaret.anomaly` `pycaret.nlp`，custom_grid 参数必须是要迭代的值的列表。

* 在 `tune_model` 中添加了 **`choose_better`** 参数。默认设置为 False。\
  当设置为 True 时，如果 tune_model 的性能没有改善，则返回基础估计器。这确保返回的对象至少与使用 create_model 创建的基础估计器或由 compare_models 返回的模型具有相同的性能。

#### ensemble_model
**`pycaret.classification`  `pycaret.regression`**

* 在 `ensemble_model` 中添加了 **`choose_better`** 参数。默认设置为 False。\
  当设置为 True 时，如果通过 tune\_model 无法改善性能，则返回基础估计器。这确保返回的对象至少与使用 create\_model 创建的基础估计器或由 compare\_models 返回的模型性能相当。\

* 在 `ensemble_model` 中添加了 **`optimize`** 参数。对于 `pycaret.classification` 默认设置为 **`Accuracy`**，对于 `pycaret.regression` 默认设置为 **`R2`**。\
  仅当 choose\_better 设置为 True 时才使用。optimize 参数用于将集成模型与基础估计器进行比较。`pycaret.classification` 中 optimize 参数接受的值为 'Accuracy', 'AUC', 'Recall', 'Precision', 'F1', 'Kappa', 'MCC'，`pycaret.regression` 中接受的值为 'MAE', 'MSE', 'RMSE' 'R2', 'RMSLE' 和 'MAPE'。

#### blend\_models

**`pycaret.classification`  `pycaret.regression`**

* 在 `blend_models` 中添加了 **`choose_better`** 参数。默认设置为 False。\
  当设置为 True 时，如果通过 tune\_model 无法改善性能，则返回基础估计器。这确保返回的对象至少与使用 create\_model 创建的基础估计器或由 compare\_models 返回的模型性能相当。\

* 在 `blend_models` 中添加了 **`optimize`** 参数。对于 `pycaret.classification` 默认设置为 **`Accuracy`**，对于 `pycaret.regression` 默认设置为 **`R2`**。\
  仅当 choose\_better 设置为 True 时才使用。optimize 参数用于将集成模型与基础估计器进行比较。`pycaret.classification` 中 optimize 参数接受的值为 'Accuracy', 'AUC', 'Recall', 'Precision', 'F1', 'Kappa', 'MCC'，`pycaret.regression` 中接受的值为 'MAE', 'MSE', 'RMSE' 'R2', 'RMSLE' 和 'MAPE'。

#### stack\_models

**`pycaret.classification`  `pycaret.regression`**

* 在 `stack_models` 中添加了 **`choose_better`** 参数。默认设置为 False。\
  当设置为 True 时，如果通过 tune\_model 无法改善性能，则返回基础估计器。这确保返回的对象至少与使用 create\_model 创建的基础估计器或由 compare\_models 返回的模型性能相当。\

* 在 `stack_models` 中添加了 **`optimize`** 参数。对于 `pycaret.classification` 默认设置为 **`Accuracy`**，对于 `pycaret.regression` 默认设置为 **`R2`**。\
  仅当 choose\_better 设置为 True 时才使用。optimize 参数用于将集成模型与基础估计器进行比较。`pycaret.classification` 中 optimize 参数接受的值为 'Accuracy', 'AUC', 'Recall', 'Precision', 'F1', 'Kappa', 'MCC'，`pycaret.regression` 中接受的值为 'MAE', 'MSE', 'RMSE' 'R2', 'RMSLE' 和 'MAPE'。

#### create\_stacknet

**`pycaret.classification`  `pycaret.regression`**

* 在 `create_stacknet` 中添加了 **`choose_better`** 参数。默认设置为 False。\
  当设置为 True 时，如果通过 tune\_model 无法改善性能，则返回基础估计器。这确保返回的对象至少与使用 create\_model 创建的基础估计器或由 compare\_models 返回的模型性能相当。\

* 在 `create_stacknet` 中添加了 **`optimize`** 参数。对于 `pycaret.classification` 默认设置为 **`Accuracy`**，对于 `pycaret.regression` 默认设置为 **`R2`**。\
  仅当 choose\_better 设置为 True 时才使用。optimize 参数用于将集成模型与基础估计器进行比较。`pycaret.classification` 中 optimize 参数接受的值为 'Accuracy', 'AUC', 'Recall', 'Precision', 'F1', 'Kappa', 'MCC'，`pycaret.regression` 中接受的值为 'MAE', 'MSE', 'RMSE' 'R2', 'RMSLE' 和 'MAPE'。

#### predict\_model

**`pycaret.classification`  `pycaret.regression`**

* 在 `predict_model` 中添加了 **`verbose`** 参数。默认设置为 True。\
  当 verbose 设置为 False 时，不会打印留存数据集的分数表格。

#### plot\_model

**`pycaret.classification`  `pycaret.regression`  `pycaret.clustering`  `pycaret.anomaly`  `pycaret.nlp`**\

* 在 `plot_model` 中添加了 **`save`** 参数。默认设置为 False。\
  当设置为 True 时，图表将保存为当前工作目录中的 'png' 文件。\

* 在 `plot_model` 中添加了 **`verbose`** 参数。默认设置为 True。\
  当 verbose 设置为 False 时，不显示进度条。\

* 在 `plot_model` 中添加了 **`system`** 参数。默认设置为 True。\
  必须始终保持为 True。仅由内部函数更改。

#### 新功能: automl

**`pycaret.classification`  `pycaret.regression`**

* 此函数基于 optimize 参数中定义的指标，从当前活动环境中创建的所有模型中返回最佳模型。

#### 参数:

* **`optimize`** 字符串，默认值为 'Accuracy'（`pycaret.classification`）和 'R2'（`pycaret.regression`）\
  optimize 参数中可以传递的其他值为 'AUC', 'Recall', 'Precision', 'F1', 'Kappa' 和 'MCC'（`pycaret.classification`），'MAE', 'MSE', 'RMSE', 'R2', 'RMSLE' 和 'MAPE'（`pycaret.regression`）\

* **`use_holdout`** 布尔值，默认值为 False
当设置为True时，指标将在留置集上进行评估，而不是交叉验证。

#### 新功能：pull

**`pycaret.classification`  `pycaret.regression`**

* 此函数将最后打印的分数表格返回为 pandas 数据框。

#### 新功能：models

**`pycaret.classification`  `pycaret.regression`  `pycaret.clustering`  `pycaret.anomaly`  `pycaret.nlp`**

* 此函数返回模型库中可用模型的表格。

#### 参数：

* **`type`** 字符串，默认值为 None\
  linear：过滤并仅返回线性模型\
  tree：过滤并仅返回基于树的模型\
  ensemble：过滤并仅返回集成模型

`type` 参数仅在 `pycaret.classification` 和 `pycaret.regression` 中可用。

#### 新功能：get\_logs

**`pycaret.classification`  `pycaret.regression`  `pycaret.clustering`  `pycaret.anomaly`  `pycaret.nlp`**

* 此函数返回一个包含实验日志的表格，包括运行详细信息、参数、指标和标签。

#### 参数：

* **`experiment_name`** 字符串，默认值为 None\
  当设置为 None 时，将使用当前活动运行。\

* **`save`** 布尔值，默认值为 False\
  当设置为 True 时，在当前目录中保存 csv 文件。

#### 新功能：get\_config

**`pycaret.classification`  `pycaret.regression`  `pycaret.clustering`  `pycaret.anomaly`  `pycaret.nlp`**

* 此函数用于访问全局环境变量。查看文档字符串以获取可访问的全局变量列表。

#### 新功能：set\_config

**`pycaret.classification`  `pycaret.regression`  `pycaret.clustering`  `pycaret.anomaly`  `pycaret.nlp`**

* 此函数用于重置全局环境变量。查看文档字符串以获取可访问的全局变量列表。

#### 新功能：get\_system\_logs

**`pycaret.classification`  `pycaret.regression`  `pycaret.clustering`  `pycaret.anomaly`  `pycaret.nlp`**

* 此函数读取并打印当前活动目录中的 'logs.log' 文件。logs.log 是从任何模块中初始化 `setup` 生成的。