---
description: 学习如何安装PyCaret
---

# 💻 安装

{% hint style="info" %}
**PyCaret 3.0-rc 现已可用**。使用 `pip install --pre pycaret` 来尝试。查看这个示例 [Notebook](https://colab.research.google.com/drive/1\_H0sHYhzKGZDmgzrQLosuZAR3nOaL6CN?usp=sharing)。
{% endhint %}

## 安装

PyCaret 在以下 64 位系统上经过测试和支持：

* Python 3.6 – 3.8
* Python 3.9 仅适用于 Ubuntu
* Ubuntu 16.04 或更高版本
* Windows 7 或更高版本

使用 Python 的 pip 包管理器安装 PyCaret。

```
pip install pycaret
```

要安装完整版本（参见下面的依赖项）：

```
pip install pycaret[full]
```

{% hint style="info" %}
如果您想尝试我们的夜间构建（不稳定版本），可以从 pip 安装 **pycaret-nightly**。`pip install pycaret-nightly`
{% endhint %}

## 环境

为了避免与其他软件包可能发生的冲突，强烈建议使用虚拟环境，例如 python3 的虚拟环境（参见 [python3 虚拟环境文档](https://docs.python.org/3/tutorial/venv.html)）或者 [conda 环境](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html)。使用隔离的环境可以独立安装特定版本的 pycaret 及其依赖项，而不会受到先前安装的 Python 软件包的影响。&#x20;

```
# 创建 conda 环境
conda create --name yourenvname python=3.8

# 激活 conda 环境
conda activate yourenvname

# 安装 pycaret
pip install pycaret

# 创建 notebook 内核
python -m ipykernel install --user --name yourenvname --display-name "display-name"
```

{% hint style="warning" %}
PyCaret **尚不**与 sklearn>=0.23.2 兼容。
{% endhint %}

## GPU

使用 PyCaret，您可以在 GPU 上训练模型，加快工作流程速度。只需在 setup 函数中传递 `use_gpu = True` 即可在 GPU 上训练模型。API 的使用方式没有变化，但在某些情况下，可能需要安装其他库，因为它们不会随默认版本或完整版本一起安装。截至最新版本，以下模型可以在 GPU 上训练：

* 极端梯度提升（无需进一步安装）
* Catboost（无需进一步安装）
* Light Gradient Boosting Machine 需要 [GPU 安装](https://lightgbm.readthedocs.io/en/latest/GPU-Tutorial.html)
* 逻辑回归、岭分类器、随机森林、K 近邻分类器、K 近邻回归器、支持向量机、线性回归、岭回归、Lasso 回归 需要 [cuML >= 0.15](https://github.com/rapidsai/cuml)

## 依赖项

* 使用 `pip install pycaret` 安装的默认依赖项在[这里列出](https://github.com/pycaret/pycaret/blob/master/requirements.txt)。
* 使用 `pycaret[full]` 安装的可选依赖项在[这里列出](installation.md#install-from-pip)。
* 测试要求在[这里列出](https://github.com/pycaret/pycaret/blob/master/requirements-test.txt)。

#### 选择选项卡

{% tabs %}
{% tab title="requirements" %}
pandas&#x20;

scipy<=1.5.4&#x20;

seaborn&#x20;

matplotlib&#x20;

IPython&#x20;

joblib&#x20;

scikit-learn==0.23.2&#x20;

ipywidgets&#x20;

yellowbrick>=1.0.1&#x20;

lightgbm>=2.3.1&#x20;

plotly>=4.4.1&#x20;

wordcloud&#x20;

textblob&#x20;

cufflinks>=0.17.0&#x20;

umap-learn&#x20;

pyLDAvis&#x20;

gensim<4.0.0&#x20;

spacy<2.4.0&#x20;

nltk&#x20;

mlxtend>=0.17.0&#x20;

pyod&#x20;

pandas-profiling>=2.8.0&#x20;

kmodes>=0.10.1&#x20;

mlflow&#x20;

imbalanced-learn==0.7.0&#x20;

scikit-plot&#x20;

Boruta&#x20;

pyyaml<6.0.0&#x20;

numba<0.55
{% endtab %}

{% tab title="requirements-optional" %}
shap&#x20;

interpret<=0.2.4&#x20;

tune-sklearn>=0.2.1&#x20;

ray\[tune]>=1.0.0&#x20;

hyperopt&#x20;

optuna>=2.2.0&#x20;

scikit-optimize>=0.8.1&#x20;

psutil&#x20;

catboost>=0.23.2&#x20;

xgboost>=1.1.0&#x20;

explainerdashboard&#x20;

m2cgen&#x20;

evidently&#x20;

autoviz&#x20;

fairlearn&#x20;

fastapi&#x20;

uvicorn&#x20;

gradio&#x20;

fugue>=0.6.5&#x20;

boto3&#x20;

azure-storage-blob&#x20;

google-cloud-storage
{% endtab %}

{% tab title="requirements-test" %}
pytest&#x20;

moto&#x20;

codecov&#x20;
{% endtab %}
{% endtabs %}

{% hint style="info" %}
**注意：**我们正在积极努力减少下一个主要版本中的默认依赖项。我们打算在将来支持功能级别和模块特定的安装。例如：`pip install pycaret[nlp]`。
{% endhint %}

## 从源代码构建

要直接从 GitHub 安装软件包（最新源代码），请使用以下命令：

```
pip install git+https://github.com/pycaret/pycaret.git#egg=pycaret
```

不要忘记包含 `#egg=pycaret` 部分，以明确命名项目，这样 pip 可以在没有运行 `setup.py` 脚本的情况下跟踪其元数据。

#### 运行测试：

要启动测试套件，请从源代码目录外运行以下命令：

```
pytest pycaret
```

## Docker
Docker 使用容器创建虚拟环境，将 PyCaret 安装与系统的其余部分隔离开来。PyCaret Docker 预装了一个 Notebook 环境，可以与主机共享资源（访问目录、使用 GPU、连接互联网等）。PyCaret Docker 镜像在每个发布版本中都经过测试。

```
docker run -p 8888:8888 pycaret/slim
```

要使用完整版本的 Docker 镜像，请运行以下命令：

```
docker run -p 8888:8888 pycaret/full
```

要了解更多信息，请查看[此文档](https://hub.docker.com/r/pycaret/full)。