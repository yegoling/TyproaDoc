# MindSporeNLP套件-模型迁移任务

## 模型迁移教程

```mermaid
graph LR
A(模块)==>B(Configuration)
A(模块)==>C(Tokenizer)
A(模块)==>D(Model)
A(模块)==>E(Unit Tests)
```

### Configuration迁移

![Snipaste_2024-10-21_14-23-36](image/Snipaste_2024-10-21_14-23-36.png)

### Tokenizer迁移

1. 找到Transformers的源码

   ```
   /transformers/src/transformers/models/tokenization_yourModel.py
   /transformers/src/transformers/models/tokenization_yourModel_fast.py
   ```

2. 找到mindnlp源码

   ```
   /mindnlp/mindnlp/transformers/models/yourModel/tokenization_yourModel.py
   /mindnlp/mindnlp/transformers/models/yourModel/tokenization_yourModel_fast.py
   ```

3. 修改tokenization_yourModel.py和tokenization_yourModel_fast.py

   tokenization_yourModel.py改之前：![Snipaste_2024-10-21_14-36-40](image/Snipaste_2024-10-21_14-36-40.png)改之后：![Snipaste_2024-10-21_14-37-38](image/Snipaste_2024-10-21_14-37-38.png)

   tokenization_yourModel_fast.py改之前：

   ![Snipaste_2024-10-21_14-41-31](image/Snipaste_2024-10-21_14-41-31.png)

   改之后：![Snipaste_2024-10-21_14-44-39](image/Snipaste_2024-10-21_14-44-39.png)

### Model迁移 

#### import replace

xxxxxxxxxx from mindnlp.transformers import VitsModel, AutoTokenizerimport mindsporemodel = VitsModel.from_pretrained("./mms-tts-eng")tokenizer = AutoTokenizer.from_pretrained("./mms-tts-eng")​text = "some example text in the English language"inputs = tokenizer(text, return_tensors='ms')print(inputs)output = model(**inputs).waveformpython 

#### global replace

![Snipaste_2024-10-21_14-53-03](image/Snipaste_2024-10-21_14-53-03.png)

#### Cell attribute

![Snipaste_2024-10-21_14-56-18](image/Snipaste_2024-10-21_14-56-18.png)

#### api mapping

![Snipaste_2024-10-21_14-59-01](image/Snipaste_2024-10-21_14-59-01.png)

![Snipaste_2024-10-21_15-06-49](image/Snipaste_2024-10-21_15-06-49.png)

![Snipaste_2024-10-21_15-08-28](image/Snipaste_2024-10-21_15-08-28.png)

![Snipaste_2024-10-21_15-10-13](image/Snipaste_2024-10-21_15-10-13.png)

#### _init_weights

![Snipaste_2024-10-21_15-11-30](image/Snipaste_2024-10-21_15-11-30.png)

#### remove useless docs

![Snipaste_2024-10-21_15-14-02](image/Snipaste_2024-10-21_15-14-02.png)

![Snipaste_2024-10-21_15-16-36](image/Snipaste_2024-10-21_15-16-36.png)

### Unit Tests

test**大致**目录:

```
huggingface/transformers/src/test/model/yourModel/test
```

![Snipaste_2024-10-21_15-18-34](image/Snipaste_2024-10-21_15-18-34.png)

![Snipaste_2024-10-21_15-22-54](image/Snipaste_2024-10-21_15-22-54.png)

![Snipaste_2024-10-21_15-23-26](image/Snipaste_2024-10-21_15-23-26.png)

![Snipaste_2024-10-21_15-25-12](image/Snipaste_2024-10-21_15-25-12.png)

![Snipaste_2024-10-21_15-25-57](image/Snipaste_2024-10-21_15-25-57.png)

## 模型迁移实践

### 模型迁移前的准备

1. 在github上找到mindnlp项目并fork:![Snipaste_2024-10-21_19-14-37](image/Snipaste_2024-10-21_19-14-37.png)

2. 在本地新建项目目录，在项目目录中打开终端，输入`git init`初始化git，再输入`git clone`加fork到自己的远程仓库的mindnlp链接，将mindnlp项目clone到本地：

   ```
   git clone https://github.com/yourName/mindnlp.git
   ```

3. 在vscode中打开本地的mindnlp项目，即可开始实践。

### MiniCPM-Llama3模型迁移

#### 文件下载

1. huggingface上找到模型文件![Snipaste_2024-10-21_18-35-38](image/Snipaste_2024-10-21_18-35-38.png)

2. 将py文件下载在本地，并找到mindnlp/transformers/models目录，新建minicpm_llama3目录，将py文件粘贴在该目录下：

   ![Snipaste_2024-10-21_18-38-14](image/Snipaste_2024-10-21_18-38-14.png)

3. 为了保险进行第一次`git commit`：![Snipaste_2024-10-21_19-27-08](image/Snipaste_2024-10-21_19-27-08.png)

#### Configuration迁移

1. 修改`from transformers.utils import logging`为`from mindnlp.utils import logging`
2. 没有ONNX可以去掉
3. 添加`_all_`：![Snipaste_2024-10-21_19-39-43](image/Snipaste_2024-10-21_19-39-43.png)
4. 为了保险进行第二次`git commit`:![Snipaste_2024-10-21_19-42-31](image/Snipaste_2024-10-21_19-42-31.png)

#### Tokenizer迁移

好像没有任何能改的地方

#### Model迁移

##### modeling_minicpmv.py

1. 严格匹配torch，全局替换为mindspore:![Snipaste_2024-10-21_19-52-00](image/Snipaste_2024-10-21_19-52-00.png)
2. 全局替换transformers为mindnlp
3. long换为int64
4. 去掉utils前的nn
5. 把.zeros和.arrange前的torch改为ops
6. 删掉所有device相关代码
7. torchvision相关更改![Snipaste_2024-10-21_20-43-32](image/Snipaste_2024-10-21_20-43-32.png)
8. git commit

##### image_processing_minicpmv.py

1. 替换所有torch为mindspore
2. 全局替换transformers为mindnlp，发现有报错是因为有两个同名的transformers目录，替换mindnlp为transformers
3. 去掉所有的device相关代码
4. 在utils/generic.py中添加函数![image-20241021213518233](image/image-20241021213518233.png)
5. nn.function改为ops
6. .size()改为.shape()
7. git commit

##### processing_minicpmv.py

1. torch相关函数逐一修改
2. git commit

##### resampler.py

1. torch相关函数逐一修改，nn.function改为ops
2. from torch.nn.init import trunc_normal_改为from mindspore.common.initializer import TruncatedNormal
3.  删除所有device相关代码
4. git commit

#### Unit Tests



### MMS模型迁移

#### 文件下载

#### Configuration迁移

#### Tokenizer迁移

#### Model迁移

#### Unit Tests