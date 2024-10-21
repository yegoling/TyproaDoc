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

![Snipaste_2024-10-21_14-51-21](image/Snipaste_2024-10-21_14-51-21.png)

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

## MiniCPM-Llama3模型迁移



## MMS模型迁移