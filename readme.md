# 文章分析项目

本项目使用大语言模型(LLM) API对文章进行分类和情感分析。

## 概述

项目包含一个Jupyter Notebook（[ArticalAnalysis.ipynb](ArticalAnalysis.ipynb)），用于从Excel文件读取文章数据，使用LLM API确定每篇文章的类别和情感，然后将结果保存到另一个Excel文件中。

## 文件说明

* [ArticalAnalysis.ipynb](ArticalAnalysis.ipynb): 包含文章分析代码的Jupyter Notebook
* [TEXT.xlsx](TEXT.xlsx): 包含文章数据（id、标题、正文）的输入Excel文件
* [output_file.xlsx](output_file.xlsx): 包含分析结果（id、标题、正文、分类、情感）的输出Excel文件
* [readme.md](readme.md): 本文件，提供项目概述

## 依赖项

* `requests`: 用于向LLM API发送HTTP请求
* `pandas`: 用于读写Excel文件
* `tqdm`: 用于显示进度条
* `re`: 用于正则表达式处理

## 设置步骤

1. **安装依赖项：**
    ```sh
    pip install requests pandas tqdm
    ```

2. **API密钥：**
    * 从 [https://api.siliconflow.cn/](https://api.siliconflow.cn/) 获取API密钥
    * 将ArticalAnalysis.ipynb中的`"sk-"`替换为你的实际API密钥

3. **输入数据：**
    * 准备名为`TEXT.xlsx`的Excel文件，包含`id`、`title`和`text`列，存放待分析的文章

4. **配置：**
    * 如需使用不同的文件名，可修改ArticalAnalysis.ipynb中的`INPUT_EXCEL`和`OUTPUT_EXCEL`变量

## 使用方法

1. 在Jupyter Notebook或JupyterLab中打开ArticalAnalysis.ipynb
2. 运行所有单元格
3. 分析结果将保存到`output_file.xlsx`文件中

## 代码说明

* **`strip_code_block(content: str) -> str`**: 移除内容中的Markdown代码块标记（```）
* **`normalize_category(raw_category: str) -> str`**: 将LLM提取的类别标准化为预定义的有效类别
* **`normalize_attitude(raw_attitude: str) -> str`**: 将LLM提取的情感标准化为预定义的有效情感
* **`get_classification_and_sentiment(title: str, text: str) -> (str, str)`**: 向LLM API发送请求以获取文章的分类和情感。包含重试逻辑以处理可能的API错误

## LLM API使用

* 项目通过Siliconflow API使用Qwen/Qwen2.5-32B-Instruct模型
* `BASE_PAYLOAD`字典包含API请求的基本配置
* prompt指示LLM将文章分类为预定义类别之一并确定情感
* 响应预期为JSON格式

## 注意事项

* 代码包含错误处理和重试机制，以处理API速率限制和其他潜在问题
* 使用`time.sleep()`调用来避免超过API速率限制
* `VALID_CATEGORIES`和`VALID_ATTITUDES`集合定义了分类和情感的有效值