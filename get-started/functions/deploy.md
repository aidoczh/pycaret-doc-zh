---
description: PyCaret 中的 MLOps 和部署相关功能
---

# 部署

## predict\_model

此函数使用训练好的模型生成标签。当 `data` 为 None 时，它会在留置集上预测标签和得分。

### **留置集预测**

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 在留置集上预测
predict_model(xgboost)
```

![predict\_model(xgboost) 的输出](<../../.gitbook/assets/image (122).png>)

### **未知数据预测**

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 在新数据上预测
new_data = diabetes.copy()
new_data.drop('Class variable', axis = 1, inplace = True)
predict_model(xgboost, data = new_data)
```

![predict\_model(xgboost, data=new\_data) 的输出](<../../.gitbook/assets/image (328).png>)

### 按类别的概率

{% hint style="info" %}
**注意：** 仅适用于[分类](../modules.md)用例。
{% endhint %}

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 在新数据上预测
new_data = diabetes.copy()
new_data.drop('Class variable', axis = 1, inplace = True)
predict_model(xgboost, raw_score = True, data = new_data)
```

![predict\_model(xgboost, raw\_score = True, data=new\_data) 的输出](<../../.gitbook/assets/image (525).png>)

### 设置概率阈值

{% hint style="info" %}
**注意：** 仅适用于[分类](../modules.md)用例（仅限二分类）。
{% endhint %}

将预测概率转换为类标签的阈值。除非设置了此参数，否则默认为模型创建过程中设置的值。如果未设置，则对于所有分类器，默认值为 0.5。仅适用于二分类。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 概率阈值为 0.3
predict_model(xgboost, probability_threshold = 0.3)
```

![predict\_model(xgboost, probability\_threshold = 0.3) 的输出](<../../.gitbook/assets/image (533).png>)

#### 在留置数据上比较不同阈值

![概率阈值 = 0.5 vs. 概率阈值 = 0.3](<../../.gitbook/assets/image (311).png>)

### 监控数据漂移

可以使用 `drift_report` 参数生成交互式漂移报告。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
xgboost = create_model('xgboost')

# 在新数据上预测
predict_model(xgboost, drift_report = True)
```

![predict\_model(xgboost, drift\_report = True) 的输出](<../../.gitbook/assets/image (133).png>)

![漂移报告 (1/N)](<../../.gitbook/assets/image (447).png>)

![漂移报告 (2/N)](<../../.gitbook/assets/image (368).png>)

![漂移报告 (3/N)](<../../.gitbook/assets/image (411).png>)

## finalize\_model

此函数在整个数据集（包括留置集）上训练给定模型。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
rf = create_model('rf')

# 完成模型
finalize_model(rf)
```

![finalize\_model(rf) 的输出](<../../.gitbook/assets/image (511).png>)

此函数不会更改模型的任何参数，只会在整个数据集（包括留置集）上重新拟合。

## deploy\_model

此函数将整个 ML 管道部署到云端。

```
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
lr = create_model('lr')

# 完成模型
final_lr = finalize_model(lr)

# 部署模型
deploy_model(final_lr, model_name = 'lr_aws', platform = 'aws', authentication = { 'bucket'  : 'pycaret-test' })
```

![deploy\_model(...) 的输出](<../../.gitbook/assets/image (274).png>)

### AWS
在将模型部署到 AWS S3（'aws'）之前，必须使用命令行界面配置环境变量。要配置 AWS 环境变量，请在 Python 命令行中键入 **aws configure**。需要以下信息，可以通过您的 Amazon 控制台帐户的身份和访问管理（IAM）门户生成：

- AWS 访问密钥 ID
- AWS 秘密密钥访问
- 默认区域名称（可以在 AWS 控制台的全局设置下看到）
- 默认输出格式（必须留空）

### GCP

要在 Google Cloud Platform（'gcp'）上部署模型，必须使用命令行或 GCP 控制台创建项目。创建项目后，必须创建服务帐户并下载服务帐户密钥作为 JSON 文件，以在本地环境中设置环境变量。

了解更多信息：[https://cloud.google.com/docs/authentication/production](https://cloud.google.com/docs/authentication/production)

### Azure

要在 Microsoft Azure（'azure'）上部署模型，必须在本地环境中设置用于连接字符串的环境变量。转到 Azure 门户上的存储帐户设置以访问所需的连接字符串。

- AZURE\_STORAGE\_CONNECTION\_STRING（作为环境变量必需）

了解更多信息：[https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-python?toc=%2Fpython%2Fazure%2FTOC.json](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-quickstart-blobs-python?toc=%2Fpython%2Fazure%2FTOC.json)

## save\_model

此函数将转换管道和经过训练的模型对象保存为 pickle 文件，以供以后使用。

```python
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
dt = create_model('dt')

# 保存管道
save_model(dt, 'dt_pipeline')
```

![save\_model(dt, 'dt\_pipeline') 的输出](<../../.gitbook/assets/image (497).png>)

## load\_model

此函数加载先前保存的管道。

```python
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 创建模型
dt = create_model('dt')

# 保存管道
save_model(dt, 'dt_pipeline')

# 加载管道
load_model('dt_pipeline')
```

![load\_model('dt\_pipeline') 的输出](<../../.gitbook/assets/image (457).png>)

## save\_config

此函数将所有全局变量保存到 pickle 文件中，允许以后恢复而无需重新运行设置函数。

```python
# 加载数据集
from pycaret.datasets import get_data
diabetes = get_data('diabetes')

# 初始化设置
from pycaret.classification import *
clf1 = setup(data = diabetes, target = 'Class variable')

# 保存配置
save_config('my_config')
```

## load\_config

此函数将全局变量从 pickle 文件加载到 Python 环境中。

```python
from pycaret.classification import load_config
load_config('my_config')
```

## convert\_model

此函数将经过训练的机器学习模型的决策函数转换为不同编程语言，如 Python、C、Java、Go、C# 等。如果要将模型部署到无法安装正常 Python 栈以支持模型推断的环境中，这将非常有用。

```python
# 加载数据集
from pycaret.datasets import get_data
juice = get_data('juice')

# 初始化设置
from pycaret.classification import *
exp_name = setup(data = juice,  target = 'Purchase')

# 训练模型
lr = create_model('lr')

# 转换模型
convert_model(lr, 'java')
```

![convert\_model(lr, 'java') 的输出](<../../.gitbook/assets/image (391).png>)

#### 视频：

{% embed url="https://www.youtube.com/watch?t=1s&v=xwQgfNC7808" %}

## create\_api

此函数接受输入模型并为推断创建一个 POST API。它仅创建 API，不会自动运行。要运行 API，必须使用 `!python` 运行 Python 文件。

```python
# 加载数据集
from pycaret.datasets import get_data
juice = get_data('juice')

# 初始化设置
from pycaret.classification import *
exp_name = setup(data = juice,  target = 'Purchase')

# 训练模型
lr = create_model('lr')

# 创建 API
create_api(lr, 'lr_api')

# 运行 API
!python lr_api.py
```

![create\_api(lr, 'lr\_api') 的输出](<../../.gitbook/assets/image (310) (1).png>)

初始化 API 后，使用 `!python` 命令可以在 localhost:8000/docs 上看到服务器。

![托管在 localhost 上的 FastAPI 服务器](<../../.gitbook/assets/image (74).png>)

#### 视频：

{% embed url="https://www.youtube.com/watch?t=3s&v=88M9c5Hc-k0" %}

## create\_docker

此函数为生产化 API 端点创建一个 `Dockerfile` 和 `requirements.txt`。
```
# 载入数据集
from pycaret.datasets import get_data
juice = get_data('juice')

# 初始化设置
from pycaret.classification import *
exp_name = setup(data=juice, target='Purchase')

# 训练模型
lr = create_model('lr')

# 创建 API
create_api(lr, 'lr_api')

# 创建 Docker
create_docker('lr_api')
```
![create_docker('lr_api') 的输出](<../../.gitbook/assets/image (154).png>)

你可以看到为你创建了两个文件。

![%load requirements.txt](<../../.gitbook/assets/image (68).png>)

![%load DockerFile](<../../.gitbook/assets/image (309).png>)

#### 视频：

{% embed url="https://www.youtube.com/watch?t=1s&v=xMgwEJ57uxs" %}

## create_app

这个函数用于创建一个基本的 `gradio` 应用程序进行推理。稍后还会扩展为其他应用程序类型，比如 `Streamlit`。

```
# 加载数据集
from pycaret.datasets import get_data
juice = get_data('juice')

# 初始化设置
from pycaret.classification import *
exp_name = setup(data = juice,  target = 'Purchase')

# 训练模型
lr = create_model('lr')

# 创建应用程序
create_app(lr)
```

![create_app(lr) 的输出](<../../.gitbook/assets/image (518) (1).png>)

#### 视频：

{% embed url="https://www.youtube.com/watch?v=4JyYhbW6eCA" %}