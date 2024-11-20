

## 开始使用

首先，让我们安装 CLI

```
>>> pip install -U "huggingface_hub[cli]"
```

在上面的代码段中，我们还安装了 `[cli]` 额外依赖项以改善用户体验，尤其是在使用 `delete-cache` 命令时。

安装完成后，您可以检查 CLI 是否已正确设置

```cmd
>>> huggingface-cli --help
usage: huggingface-cli <command> [<args>]

positional arguments:
  {env,login,whoami,logout,repo,upload,download,lfs-enable-largefiles,lfs-multipart-upload,scan-cache,delete-cache,tag}
                        huggingface-cli command helpers
    env                 Print information about the environment.
    login               Log in using a token from huggingface.co/settings/tokens
    whoami              Find out which huggingface.co account you are logged in as.
    logout              Log out
    repo                {create} Commands to interact with your huggingface.co repos.
    upload              Upload a file or a folder to a repo on the Hub
    download            Download files from the Hub
    lfs-enable-largefiles
                        Configure your repository to enable upload of files > 5GB.
    scan-cache          Scan cache directory.
    delete-cache        Delete revisions from the cache directory.
    tag                 (create, list, delete) tags for a repo in the hub

options:
  -h, --help            show this help message and exit
```

如果 CLI 正确安装，您应该会看到 CLI 中所有可用选项的列表。如果您收到错误消息，例如 `command not found: huggingface-cli`，请参阅[安装](https://hugging-face.cn/docs/huggingface_hub/installation)指南。

## huggingface-cli 登录

在许多情况下，您必须登录到 Hugging Face 帐户才能与 Hub 交互（下载私有存储库、上传文件、创建 PR 等）。为此，您需要从您的 [设置页面](https://hugging-face.cn/settings/tokens) 获取 [用户访问令牌](https://hugging-face.cn/docs/hub/security-tokens)。用户访问令牌用于向 Hub 验证您的身份。如果您想上传或修改内容，请确保设置具有写入权限的令牌。

获得令牌后，在您的终端中运行以下命令

```
>>> huggingface-cli login
```

此命令将提示您输入令牌。复制粘贴您的令牌并按*Enter*键。然后，系统会询问您是否也应将令牌保存为 git 凭据。如果您计划在本地使用`git`，请再次按*Enter*键（默认为是）。最后，它将调用 Hub 以检查您的令牌是否有效并将其保存在本地。

```cmd
_|    _|  _|    _|    _|_|_|    _|_|_|  _|_|_|  _|      _|    _|_|_|      _|_|_|_|    _|_|      _|_|_|  _|_|_|_|
_|    _|  _|    _|  _|        _|          _|    _|_|    _|  _|            _|        _|    _|  _|        _|
_|_|_|_|  _|    _|  _|  _|_|  _|  _|_|    _|    _|  _|  _|  _|  _|_|      _|_|_|    _|_|_|_|  _|        _|_|_|
_|    _|  _|    _|  _|    _|  _|    _|    _|    _|    _|_|  _|    _|      _|        _|    _|  _|        _|
_|    _|    _|_|      _|_|_|    _|_|_|  _|_|_|  _|      _|    _|_|_|      _|        _|    _|    _|_|_|  _|_|_|_|

To log in, `huggingface_hub` requires a token generated from https://hugging-face.cn/settings/tokens .
Enter your token (input will not be visible):
Add token as git credential? (Y/n)
Token is valid (permission: write).
Your token has been saved in your configured git credential helpers (store).
Your token has been saved to /home/wauplin/.cache/huggingface/token
Login successful
```

## huggingface-cli whoami

如果您想知道是否已登录，可以使用`huggingface-cli whoami`。此命令没有任何选项，只需打印您的用户名以及您在 Hub 中所属的组织

```cmd
huggingface-cli whoami
Wauplin
orgs:  huggingface,eu-test,OAuthTesters,hf-accelerate,HFSmolCluster
```

如果您未登录，将打印错误消息。

## 下载单个文件

一旦您完成了认证，就可以使用以下命令下载单个文件：

```bash
huggingface-cli download <repo_id> <filename>
```

其中，`<repo_id>` 是仓库的 ID，`<filename>` 是您要下载的文件名。例如，如果您想下载 `bert-base-uncased` 模型中的 `config.json` 文件，可以使用以下命令：

```cmd
huggingface-cli download bert-base-uncased config.json
```

## 指定下载路径

您还可以指定下载路径。例如，如果您想将文件下载到 `/path/to/download` 目录，可以使用以下命令：

```cmd
huggingface-cli download bert-base-uncased config.json --revision 1.0.0 --local-dir /path/to/download
```

```
huggingface-cli download bert-base-uncased  --local-dir /path/to/download
```



# huggingface 镜像使用

## 1. 安装依赖

```
pip install -U huggingface_hub
```

## 2.写脚本换源

```python
# 设置环境变量
import os
os.environ["HF_ENDPOINT"] = "https://hf-mirror.com"

# 代码下载
from huggingface_hub import hf_hub_download
hf_hub_download(repo_id="amritgupta/qafacteval",filename="README.md",local_dir="./qafacteval")
```

## 3.终端下载

```python
import os

# 设置环境变量
os.environ['HF_ENDPOINT'] = 'https://hf-mirror.com'

# 下载模型
os.system('huggingface-cli download bert-base-uncased  --local-dir /path/to/download')
```



## 4.指定模型存放路径

```py
from transformers import pipeline, AutoModelForSequenceClassification, AutoTokenizer

# 自定义模型存放路径
cache_dir = "/path/to/your/cache_dir"


# 使用 from_pretrained 加载模型和分词器，并指定缓存路径
model = AutoModelForSequenceClassification.from_pretrained("bert-base-uncased", cache_dir=cache_dir)
tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased", cache_dir=cache_dir)
```

