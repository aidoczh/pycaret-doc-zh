# 在 SQL 中使用 PyCaret 进行机器学习 第一部分

### 在 SQL 中使用 PyCaret 进行机器学习

#### 通过在 SQL Server 中集成 PyCaret 将您的机器学习代码部署到数据

#### 作者：Umar Farooque

![](https://cdn-images-1.medium.com/max/8064/1\*CxB6I95fPzYv\_z9C6RRzVg.png)

本文是关于如何在 SQL Server 中使用 **PyCaret（Python 中的低代码机器学习库）** 训练和部署无监督机器学习聚类模型的**逐步教程**。

**本文将涵盖以下内容：**

1. 如何免费下载并安装 SQL Server
2. 如何创建新数据库并将数据导入数据库
3. 如何启用并使用数据库中的 Python 脚本
4. 如何训练聚类算法以为数据集中的每个观测分配聚类标签

### **I. 将代码带到数据中 —— 使用数据库进行机器学习的案例**

进行机器学习实验的首选工具/环境是命令行、集成开发环境或笔记本。然而，当数据量非常大或需要将机器学习模型投入生产时，这些工具/环境可能存在限制。迫切需要在数据所在地编程和训练模型。MS SQL Server 在其 SQL Server 2019 版本中引入了这一功能。使用 SQL Server 进行机器学习的明显优势包括：

i. 从系统中提取大量数据是费时费力的。在服务器上进行机器学习实验将代码带到数据，而不是将数据带到代码

ii. 机器学习实验主要在计算机/CPU 内存中执行。当在大型数据集上训练机器学习算法时，大多数计算机会达到性能上限。在 SQL Server 数据库上进行机器学习可以避免这种情况

iii. 容易集成和部署机器学习流水线以及其他 ETL 过程

### **II. SQL Server**

SQL Server 是 Microsoft 的关系数据库管理系统。作为数据库服务器，它的主要功能是根据不同应用程序的请求存储和检索数据。在本教程中，我们将使用 **SQL Server 2019 Developer** 通过将 PyCaret 库导入 SQL Server 进行机器学习。

### **III. 下载软件**

如果您以前使用过 SQL Server，很可能已经安装了它并可以访问数据库。如果没有，请[**点击这里**](https://www.microsoft.com/en-ca/sql-server/sql-server-downloads)下载 SQL Server 2019 Developer 版或其他版本。

![](https://cdn-images-1.medium.com/max/2000/1\*lt9GPAvrhixDQAP6iatTIQ.png)

### **IV. 设置环境**

在将 PyCaret 功能引入 SQL Server 之前，您需要安装 SQL Server 和 PyCaret。这是一个多步骤的过程：

#### 步骤 1 — 安装 SQL Server

下载 SQL Server 2019 Developer Edition 文件 “**SQL2019-SSEI-Dev.exe**”

![](https://cdn-images-1.medium.com/max/2000/1\*WYIssA8f1vpYIPced1miYA.png)

打开文件并按照说明进行安装（建议使用自定义安装选项）

![](https://cdn-images-1.medium.com/max/2000/1\*H2mr4UeI3q2DaidWtgldxQ.png)

选择新的 SQL Server 独立安装

![](https://cdn-images-1.medium.com/max/2000/1\*bxqUK4NIzdW7DW1LBKObiQ.png)

在实例特性选项中，选择包括“**Python**”在内的特性，位于**机器学习服务和语言扩展**和**机器学习服务器（独立）**下

![](https://cdn-images-1.medium.com/max/2000/1\*OCXaVsXQmnGSBa5hPUh4GQ.png)

点击“**接受**”以同意安装 Python

![](https://cdn-images-1.medium.com/max/2000/1\*rYm00TFsLe70EzW1503h1A.png)

安装可能需要 15–20 分钟

#### 步骤 2 — 安装 Microsoft SQL Server Management Studio (SSMS)

[**点击这里**](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?redirectedfrom=MSDN\&view=sql-server-ver15)或打开 SQL Server 安装中心下载“SQL Server Management Tools” 文件 “**SSMS-Setup-ENU.exe**”

![](https://cdn-images-1.medium.com/max/2000/1\*VydYoU\_rbKtzFKgsJ7fPpw.png)

打开“**SSMS-Setup-ENU.exe**” 文件开始安装

![](https://cdn-images-1.medium.com/max/2000/1\*Fnt\_PmyaEh8AitkuQIy7og.png)

安装可能需要 5–10 分钟

#### 步骤 3 — 为机器学习创建数据库

安装完成后，您需要启动服务器实例。要这样做，请启动 SSMS。在登录阶段，您将被要求输入 SQL Server 的名称，您可以从下拉菜单中选择。建立连接后，您可以看到来自服务器的所有对象。如果您是第一次下载 SQL Server 并且没有要处理的数据库，您需要首先创建一个新数据库。

在对象资源管理器面板中，右键单击数据库，然后选择新建数据库

![](https://cdn-images-1.medium.com/max/2000/1\*4pzZ6-fk48V3ujaAIUhI2A.png)

输入数据库名称和其他信息
设置可能需要2-3分钟，包括创建数据库、用户和设置所有权。

#### 第四步 - 导入CSV文件

现在，您需要使用SQL Server管理工具将CSV文件导入数据库。

在数据库中创建一个名为“jewellery”的表

![](https://cdn-images-1.medium.com/max/2000/1\*-kkwdIAo4GDkzZ1013GYog.png)

右键单击数据库，选择**任务**->**导入数据**

![](https://cdn-images-1.medium.com/max/2000/1\*d07HzD7rwEr\_SHkGsR\_m-Q.png)

对于数据源，选择**扁平文件源**。然后使用**浏览**按钮选择CSV文件。在点击**下一步**按钮之前，花一些时间配置数据导入。

![](https://cdn-images-1.medium.com/max/2000/1\*zNU8RuLdlIDNmoh93Mlu1w.png)

对于目标，选择正确的数据库提供程序（例如SQL Server Native Client 11.0）。输入**服务器名称**；勾选**使用SQL Server身份验证**，输入**用户名**、**密码**和**数据库**，然后点击**下一步**按钮。

![](https://cdn-images-1.medium.com/max/2000/1\*jz9FIU8o-98-H\_3vinmUfA.png)

在选择源表和视图窗口中，您可以在点击**下一步**按钮之前编辑映射。

![](https://cdn-images-1.medium.com/max/2000/1\*q1RuWU3dVZmr0zvoVWrBLg.png)

勾选立即运行，然后点击**下一步**按钮

![](https://cdn-images-1.medium.com/max/2000/1\*P9yzJy0imJkl85yGgE6C7g.png)

点击完成按钮运行包

![数据加载结果](https://cdn-images-1.medium.com/max/2000/1\*jBBq5kfftYJNR2DKWYmRuQ.png)

#### 第五步 - 启用SQL Server的Python脚本

我们将通过使用**sp\_execute\_external\_script**系统存储过程在SQL Server中运行Python。首先，您需要打开一个“**新查询**”。在您的实例中执行以下查询以启用远程脚本执行的过程：

```
EXEC sp_configure ‘external scripts enabled’, 1

RECONFIGURE WITH OVERRIDE
```

**注意：**在继续下一步之前重新启动实例。

可以执行以下SQL语句来检查Python路径和列出已安装的包。

检查Python路径：

```
EXECUTE sp_execute_external_script

@language =N’Python’,

@script=N’import sys; print(“\n”.join(sys.path))’
```

![脚本执行结果](https://cdn-images-1.medium.com/max/2000/1\*WDzLc0EXLpH1Zu3TEbfsiw.png)

列出已安装的包：

```
EXECUTE sp_execute_external_script

@language = N’Python’,

@script = N’

import pkg_resources

import pandas as pd

installed_packages = pkg_resources.working_set

installed_packages_list = sorted([“%s==%s” % (i.key, i.version) for i in installed_packages])

df = pd.DataFrame(installed_packages_list)

OutputDataSet = df’

WITH RESULT SETS (( PackageVersion nvarchar (150) ))
```

![脚本执行结果](https://cdn-images-1.medium.com/max/2000/1\*1bSpU8-L-KYpzR\_dDPaTPQ.png)

#### 第六步 - 将PyCaret Python包添加到SQL Server

要安装PyCaret包，请打开命令提示符并浏览到安装了SQL Server的Python包的位置。默认位置为：

```
C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES
```

导航到“**Scripts**”目录并使用pip命令安装**PyCaret**包

```
pip.exe install pycaret
```

![命令提示符 - PyCaret安装](https://cdn-images-1.medium.com/max/2000/1\*EZ4cZxBOb6sJ8sTaObM5bA.png)

![命令提示符 - PyCaret安装结束](https://cdn-images-1.medium.com/max/2000/1\*fAL6HpAkkuweGH\_q90KU-Q.png)

**注意：**确保您有访问SQL Server目录以安装包和/或更改配置的权限。否则，包安装将失败。

安装可能需要5-10分钟

**注意：**如果在运行SQL脚本时遇到缺少“_lightgbm_”模块的问题，请按照以下说明操作：

i. 卸载“_lightgbm_”

```
pip.exe uninstall lightgbm
```

ii. 重新安装“_lightgbm_”

```
pip.exe install lightgbm
```

执行以下SQL以从SQL Server验证PyCaret的安装：

```
EXECUTE sp_execute_external_script

@language = N’Python’,

@script = N’

import pkg_resources

pckg_name = “pycaret”

pckgs = pandas.DataFrame([(i.key) for i in pkg_resources.working_set], columns = [“key”])

installed_pckg = pckgs.query(‘’key == @pckg_name’’)

print(“Package”, pckg_name, “is”, “not” if installed_pckg.empty else “”, “installed”) ’
```

![脚本执行结果](https://cdn-images-1.medium.com/max/2000/1\*evwN5FoXycMQJETJcjawgA.png)

### 五、机器学习实验示例 - 在SQL Server中进行聚类

聚类是一种机器学习技术，用于将具有相似特征的数据点分组。这些分组对于探索数据、识别模式和分析数据子集非常有用。一些常见的业务用例包括：

✔ 用于营销目的的客户细分。

✔ 用于促销和折扣的客户购买行为分析。
✔ 在流行病爆发中识别地理聚类，例如 COVID-19。

在本教程中，我们将使用 PyCaret 的 [Github 仓库](https://raw.githubusercontent.com/pycaret/pycaret/master/datasets/jewellery.csv) 上提供的 'jewellery.csv' 文件。

![jewellery 数据集的样本数据点](https://cdn-images-1.medium.com/max/2000/1\*itUa7b3dSjnzKFaaTJOkMg.png)

#### 1. K-Means 聚类

在 SQL Server 中运行以下 SQL 代码：

```
EXECUTE sp_execute_external_script

@language = N’Python’,

@script = N’dataset = InputDataSet

import pycaret.clustering as pc

dataset = pc.get_clusters(data = dataset)

OutputDataSet = dataset’,

@input_data_1 = N’SELECT [Age], [Income], [SpendingScore], [Savings] FROM [jewellery]’

WITH RESULT SETS(([Age] INT, [Income] INT, [SpendingScore] FLOAT, [Savings] FLOAT, [Cluster] varchar(15)));
```

#### 2. 输出

![SQL 语句结果](https://cdn-images-1.medium.com/max/2000/1\*AwXT-NmfgHJ9LDU6IWrPpA.png)

原始表格上附加了一个名为 'Cluster' 的新列，其中包含了标签。

默认情况下，PyCaret 使用 4 个聚类（即表中的所有数据点被分为 4 组）训练 K-Means 聚类模型。可以轻松更改默认值：

要更改聚类数目，可以在 get_clusters() 函数中使用 num_clusters 参数。

要更改模型类型，可以在 get_clusters() 函数中使用 model 参数。

#### 3. K-Modes

以下代码展示了如何使用 6 个聚类训练 K-Modes 模型：

```
EXECUTE sp_execute_external_script

@language = N’Python’,

@script = N’dataset = InputDataSet

import pycaret.clustering as pc

dataset = pc.get_clusters(data = dataset, model=”kmodes”, num_clusters = 6)

OutputDataSet = dataset’,

@input_data_1 = N’SELECT [Age], [Income], [SpendingScore], [Savings] FROM [jewellery]’

WITH RESULT SETS(([Age] INT, [Income] INT, [SpendingScore] FLOAT, [Savings] FLOAT, [Cluster] varchar(15)));
```

按照这些步骤，您可以为 jewellery 数据集中的每个观测点分配聚类值。您也可以在其他数据集上使用类似的步骤进行聚类分析。

### VI. 结论

在本文中，我们学习了如何在 SQL Server 中使用 Python 库（PyCaret）构建聚类模型。同样，您可以根据业务问题的需要构建和运行其他类型的监督学习和无监督学习模型。

您可以进一步查看 [PyCaret](http://pycaret.org/) 网站，了解在 SQL Server 中以类似方式实施的其他监督学习和无监督学习实验的文档。

我的未来文章将是关于使用 Python 和 Pycaret 在 SQL Server 中探索监督学习技术（回归/分类）的教程。

### VII. 重要链接

[PyCaret](https://pycaret.org/)

[PyCaret：用户指南和文档](https://pycaret.org/guide/)

[PyCaret：教程](https://pycaret.org/tutorial/)

[我的 LinkedIn 个人资料](https://www.linkedin.com/in/umarfarooque/)