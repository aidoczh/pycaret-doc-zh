# 使用 PyCaret 将 ML 模型部署到 SQL Server

### 使用 PyCaret 将机器学习模型部署到数据 — 第二部分

#### 二元分类

#### 作者：Umar Farooque

![Photo by Joshua Sortino on Unsplash](https://cdn-images-1.medium.com/max/8390/0\*3n2RWRuMocf7X67I)

我的上一篇文章 [\*\*在 SQL 中使用 PyCaret 1.0 进行机器学习](https://towardsdatascience.com/machine-learning-in-sql-using-pycaret-87aff377d90c)\*\* 详细介绍了如何将 [\*\*PyCaret](https://pycaret.org/)\*\* 与 [\*\*SQL Server](https://www.microsoft.com/en-ca/sql-server/sql-server-downloads)** 集成。本文将逐步介绍如何使用** [**\*\*PyCaret 2.0**](https://pycaret.org/) **（PyCaret 是 Python 中的低代码 ML 库）在 SQL Server 中训练和部署监督学习分类模型。**

**本文将涵盖以下内容：**

1. 如何将数据加载到 SQL Server 表中
2. 如何在 SQL Server 表中创建和保存模型
3. 如何使用保存的模型进行模型预测并将结果存储在表中

### **I. 导入/加载数据**

现在，您需要使用 SQL Server Management Studio 将 CSV 文件导入数据库。

在数据库中创建一个名为“**cancer**”的表

![](https://cdn-images-1.medium.com/max/2000/0\*js8AFIoUDjQZZSPa.png)

右键单击数据库，选择 **Tasks** **->** **Import Data**

![](https://cdn-images-1.medium.com/max/2000/0\*tZSAwhgQU9Oq8-uc.png)

对于数据源，选择 **Flat File Source**。然后使用 **Browse** 按钮选择 CSV 文件。在单击 \*\*Next \*\*按钮之前，花些时间配置数据导入。

![](https://cdn-images-1.medium.com/max/2000/1\*F4b5xric6n9Mc5sjWT4tHg.png)

对于目标，选择正确的数据库提供程序（例如 SQL Server Native Client 11.0）。输入 **服务器名称**；勾选 **使用 SQL Server 身份验证**，输入 **用户名**，**密码** 和 \*\*数据库 \*\*，然后单击 \*\*Next \*\*按钮。

![](https://cdn-images-1.medium.com/max/2000/0\*c514MMvjt3DavCyI.png)

在选择源表和视图窗口中，您可以在单击 \*\*Next \*\*按钮之前编辑映射。

![](https://cdn-images-1.medium.com/max/2000/0\*I4VhN7PyYrQfipU5.png)

勾选 **立即运行** 并单击 \*\*Next \*\*按钮

![](https://cdn-images-1.medium.com/max/2000/0\*uBqcVcjEyaUQsKNq.png)

单击完成按钮运行包

### **II. 创建 ML 模型并保存在数据库表中**

\*\*分类 **是一种监督学习类型，用于预测分类类别标签，这些标签是离散且无序的。** [**\*\*PyCaret**](https://pycaret.org/) **包中提供的模块可用于** 二元 **或** 多类 **问题。**

在本示例中，我们将使用一个“**乳腺癌数据集**”。在数据库表中创建并保存模型是一个多步骤的过程。让我们逐步进行：

i. 创建一个存储过程，用于创建一个经过训练的模型，这里是一个 Extra Trees Classifier 算法。该过程将从上一步创建的 cancer 表中读取数据。

以下是用于创建该过程的代码：

```
*-- 生成使用癌症数据使用 Extra Trees Classifier 算法生成 PyCaret 模型的存储过程*

DROP PROCEDURE IF EXISTS generate_cancer_pycaret_model;

Go

CREATE PROCEDURE generate_cancer_pycaret_model (@trained_model varbinary(max) OUTPUT) AS

BEGIN

EXECUTE sp_execute_external_script

@language = N'Python'

, @script = N'

import pycaret.classification as cp

import pickle

trail1 = cp.setup(data = cancer_data, target = "Class", silent = True, n_jobs=None)

*# 创建模型*
et = cp.create_model("et", verbose=False)


*# 为了进一步改进我们的模型，我们可以使用 tune_model 函数调整超参数。
# 我们还可以基于评估指标优化调整。由于我们选择的指标是 F1 分数，让我们优化我们的算法！*

tuned_et = cp.tune_model(et, optimize = "F1", verbose=False)


*# finalize_model() 函数将模型拟合到完整数据集。
# 该函数的目的是在将模型部署到生产环境之前在完整数据集上训练模型*

final_model = cp.finalize_model(tuned_et)

*# 在将模型保存到 DB 表之前，将其转换为二进制对象*

trained_model = []
prep = cp.get_config("prep_pipe")
trained_model.append(prep)
trained_model.append(final_model)
trained_model = pickle.dumps(trained_model)'

, @input_data_1 = N'select "Class", "age", "menopause", "tumor_size", "inv_nodes", "node_caps", "deg_malig", "breast", "breast_quad", "irradiat" from dbo.cancer'

, @input_data_1_name = N'cancer_data'

, @params = N'@trained_model varbinary(max) OUTPUT'

, @trained_model = @trained_model OUTPUT;

END;

GO
```

ii. 创建一个表，用于存储训练好的模型对象
```
如果存在则删除表 dbo.pycaret_models;

执行

创建表 dbo.pycaret_models (
model_id  INT NOT NULL PRIMARY KEY,
dataset_name VARCHAR(100) NOT NULL DEFAULT('default dataset'),
model_name  VARCHAR(100) NOT NULL DEFAULT('default model'),
model   VARBINARY(MAX) NOT NULL
);

执行
```
### iii. 调用存储过程创建模型对象并保存到数据库表中

```
DECLARE @model VARBINARY(MAX);
EXECUTE generate_cancer_pycaret_model @model OUTPUT;
INSERT INTO pycaret_models (model_id, dataset_name, model_name, model) VALUES(2, 'cancer', 'Extra Trees Classifier algorithm', @model);
```

此执行的输出为：

![控制台输出](https://cdn-images-1.medium.com/max/2000/1\*rMPx1cdPMqxZ2xAZDpqzsA.png)

保存模型后表格结果的展示

![SQL Server 表格结果](https://cdn-images-1.medium.com/max/2000/1\*8DcdhzXZupB\_fqiO6SofhQ.png)

### **III. 运行预测**

下一步是根据保存的模型对测试数据集进行预测。这同样是一个多步骤的过程。让我们再次逐步进行。

i. 创建一个存储过程，将使用测试数据集来检测测试数据点的癌症

以下是创建数据库过程的代码：

```
DROP PROCEDURE IF EXISTS pycaret_predict_cancer;
GO

CREATE PROCEDURE pycaret_predict_cancer (@id INT, @dataset varchar(100), @model varchar(100))
AS

BEGIN

DECLARE @py_model varbinary(max) = (select model

from pycaret_models

where model_name = @model

and dataset_name = @dataset

and model_id = @id

);

EXECUTE sp_execute_external_script

@language = N'Python',

@script = N'

# Import the scikit-learn function to compute error.

import pycaret.classification as cp

import pickle

cancer_model = pickle.loads(py_model)

*# Generate the predictions for the test set.*

predictions = cp.predict_model(cancer_model, data=cancer_score_data)

OutputDataSet = predictions

print(OutputDataSet)

'

, @input_data_1 = N'select "Class", "age", "menopause", "tumor_size", "inv_nodes", "node_caps", "deg_malig", "breast", "breast_quad", "irradiat" from dbo.cancer'

, @input_data_1_name = N'cancer_score_data'

, @params = N'@py_model varbinary(max)'

, @py_model = @py_model

with result sets (("Class" INT, "age" INT, "menopause" INT, "tumor_size" INT, "inv_nodes" INT,

"node_caps" INT, "deg_malig" INT, "breast" INT, "breast_quad" INT,

"irradiat" INT, "Class_Predict" INT, "Class_Score" float ));

END;

GO
```

ii. 创建一个表格，将预测结果与数据集一起保存

```
DROP TABLE IF EXISTS [dbo].[pycaret_cancer_predictions];

GO

CREATE TABLE [dbo].[pycaret_cancer_predictions](

[Class_Actual] [nvarchar] (50) NULL,

[age] [nvarchar] (50) NULL,

[menopause] [nvarchar] (50) NULL,

[tumor_size] [nvarchar] (50) NULL,

[inv_nodes] [nvarchar] (50) NULL,

[node_caps] [nvarchar] (50) NULL,

[deg_malig] [nvarchar] (50) NULL,

[breast] [nvarchar] (50) NULL,

[breast_quad] [nvarchar] (50) NULL,

[irradiat] [nvarchar] (50) NULL,

[Class_Predicted] [nvarchar] (50) NULL,

[Class_Score] [float] NULL

) ON [PRIMARY]

GO
```

iii. 调用 pycaret\_predict\_cancer 过程将预测结果保存到表中

```
*--将测试集的预测结果插入表中*

INSERT INTO [pycaret_cancer_predictions]

EXEC pycaret_predict_cancer 2, 'cancer', 'Extra Trees Classifier algorithm';
```

iv. 执行以下 SQL 以查看预测结果

```
*-- 选择表格内容*

SELECT * FROM [pycaret_cancer_predictions];
```

![预测结果](https://cdn-images-1.medium.com/max/2000/1\*kboWnjA4K41DV6a\_\_l79wA.png)

### IV. 结论

在本文中，我们学习了如何在 SQL Server 中使用 PyCaret 构建分类模型。同样，您可以根据业务问题的需要构建和运行其他类型的监督和无监督机器学习模型。

![Tobias Fischer 摄于 Unsplash](https://cdn-images-1.medium.com/max/8064/0\*45eSsUjDsCd\_7\_-J)

您可以进一步查看 [\*\*PyCaret](http://pycaret.org/)\*\* 网站，了解其他可以在 SQL Server 中以类似方式实现的监督和无监督实验的文档。

我的未来文章将是关于使用 Python 和 **PyCaret** 在 **SQL Server** 中探索其他监督和无监督学习技术的教程。

### V. 重要链接

[PyCaret](https://pycaret.org/)

[我的 LinkedIn 主页](https://www.linkedin.com/in/umarfarooque/)