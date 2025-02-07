# 文章分析项目

本项目使用大语言模型(LLM) API对文章进行分类和情感分析。

## 概述

项目包含一个Jupyter Notebook（[ArticalAnalysis.ipynb](ArticalAnalysis.ipynb)），用于从Excel文件读取文章数据，使用LLM API确定每篇文章的类别和情感，然后将结果保存到另一个Excel文件中。

## 文件说明

* [ArticalAnalysis.ipynb](ArticalAnalysis.ipynb): 包含文章分析代码的Jupyter Notebook
* [TEXT.xlsx](TEXT.xlsx): 包含文章数据（id、标题、正文）的输入Excel文件
* [output_file4.xlsx](output_file4.xlsx): 包含分析结果（id、标题、正文、分类、情感）的输出Excel文件
* [readme.md](readme.md): 本文件，提供项目概述

## 依赖项

* `requests`: 用于向LLM API发送HTTP请求
* `pandas`: 用于读写Excel文件
* `tqdm`: 用于显示进度条
* `re`: 用于正则表达式处理
* `json`: 用于JSON数据处理
* `IPython`: 用于交互式调试输出

## 设置步骤

1. **安装依赖项：**
    ```sh
    pip install requests pandas tqdm ipython
    ```

2. **API密钥：**
    * 获取阿里云DashScope API密钥
    * 将ArticalAnalysis.ipynb中的`"sk-"`替换为你的实际API密钥

3. **输入数据：**
    * 准备名为`TEXT.xlsx`的Excel文件，包含`id`、`title`和`text`列

4. **配置：**
    * 修改`INPUT_EXCEL`和`OUTPUT_EXCEL`变量以使用不同的文件名
    * 可调整`BASE_PAYLOAD`中的模型参数（温度、top_p等）

## 使用方法

1. 在Jupyter环境中打开ArticalAnalysis.ipynb
2. 运行所有单元格
3. 分析结果将保存到`output_file4.xlsx`文件中

## 代码说明

* **`strip_code_block(content: str) -> str`**: 处理API返回的代码块格式
* **`normalize_category(raw_category: str) -> str`**: 将类别标准化为预定义值
* **`normalize_attitude(raw_attitude: str) -> str`**: 将情感标准化为预定义值
* **`get_classification_and_sentiment(title: str, text: str) -> (str, str)`**: API请求核心函数
* **`update_debug(msg: str)`**: 用于实时显示调试信息

## LLM API使用

* 使用阿里云DashScope的qwen-max-latest模型
* API端点：`https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions`
* 包含完整的错误处理和重试机制
* 通过`time.sleep()`控制请求频率

## 预定义值

### 有效类别
```python
["奥运", "国内政治", "国际政治", "军事和防御", "国内秩序", "经济", "劳动关系", 
 "商务活动", "交通", "卫生福利", "人口", "教育", "传播", "住房", "环境", "能源", 
 "科技", "社会关系", "灾害", "体育", "文化活动", "时尚", "庆典", "人类爱好", "其他"]
```

### 有效情感
```python
["positive", "negative", "hostile", "pleasure", "pain"]
```

## 注意事项

* 包含进度条显示功能
* 实时显示API调用的调试信息
* 每次API请求后默认等待1秒以控制请求频率
* 遇到错误时会自动重试最多5次