---
title: Ollama API For Python
date: 2025-10-18 09:01:10
tags: [updating, Python, API, Ollama]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/Ollama%20API.png?raw=true
categories: Python
category_bar: true
description: A lightweight Python API service built with FastAPI to connect local Ollama LLMs, featuring API key verification, credit management, and easy integration for secure, controlled access to local model capabilities.
---


本文将会记录我本地部署大模型并进行 API 接口开发的全流程！

[项目Repo](https://github.com/Richard110206/API-For-LLM)

### Reference


[Learn Ollama in 15 Minutes - Run LLM Models Locally for FREE](https://www.youtube.com/watch?v=UtSSMs6ObqY&t=600s)

1. 从官方网站下载安装 [Ollama](https://ollama.com/)，mac用户可以使用命令行进行安装：
```bash
brew install ollama
```
2. 安装完成后可以在本地拉取所需的大语言模型：（其余模型可以在官网上找到）
```bash
ollama run mistral
```
看到`<<`的标志说明部署成功，可以与本地大模型进行对话了！

我们也可以根据自己的需求对大模型进行自定义，编写`Modelfile`文件，定义大模型的参数：
```Modelfile
FROM mistral

PARAMETER temperature 1

SYSTEM """
You are a student from Nanjing No.13 senior high school,and is fully prepared for Gaokao.
"""
```
{%note info%}
- `FROM <模型名>`：指定基础模型（**预训练模型**或**微调后的模型**）（`.gguf`格式）
  
  - `FROM llama2-7b`（基于 Llama 2 的 70 亿参数模型）
  - `FROM ./my_finetuned_model.gguf`（本地微调后的模型文件）

{%note info%}
`.gguf`是一种开源的大语言模型的文件格式
{%endnote%}

- `PARAMETER <参数名> <参数值>`：指定模型参数

| 参数名     | 作用   | 常见取值范围 / 示例 |
|:--------------:|:------------:|:----------:|
| `temperature` | 控制输出随机性（值越高越随机，越低越确定）     | 0~2（例如：0.3（严谨）、1.5（发散））  |
| `top_k`          | 生成时仅从概率最高的k个词中选择（限制候选词范围） | 1~100（例如：50、10（更集中））  |
| `top_p`（核采样） | 从累计概率超过p的词中随机选择（动态调整候选词数量） | 0~1（例如：0.9（常用）、0.5（更严格）） |
| `max_tokens`     | 限制生成的最大 token 数（避免输出过长） | 整数（例如：512、1024）  |
| `stop`           | 定义终止符（遇到该字符时停止生成）    | 字符串（例如：stop: "###"、stop: "\n"） |
| `repeat_penalty`  | 惩罚重复内容（值越高，越避免重复） | 1~2（例如：1.2（轻微惩罚）、1.8（严格））   |
| `presence_penalty` | 惩罚已出现过的主题（鼓励新内容） | -2~2（例如：0.5、-0.5（允许重复）） |
| `frequency_penalty` | 惩罚高频出现的词（减少冗余词汇） | -2~2（例如：0.3） |

- `SYSTEM "<提示词>"`：定义模型的角色、行为规则或背景信息（引导模型生成符合设定的回复）。
{%endnote%}

根据自己需求修改好`Modelfile`文件后，就可以创建自定义模型了：
```bash
ollama create student -f ./Modelfile
```
`ollama create <模型名> -f <配置文件名>`：根据模型名运行


#### Reference
[How To Build an API with Python (LLM Integration, FastAPI, Ollama & More)](https://www.youtube.com/watch?v=cy6EAp4iNN4)

完成项目依赖文档`requirements.txt`，并完成必要库的安装：
```requirements.txt
fastapi
uvicorn # 用于运行fastapi应用
ollama # 使用本地LLM
python-dotenv # 用于加载环境变量
requests # 用于测试API
```
```bash
pip install -r requirements.txt
```

{%note info%}
### About  requirements
`requirements.txt` 是一个包含 Python 包依赖关系的文件，避免因依赖版本不兼容而出现 **“It works on my machine”** 的问题。

#### 什么时候使用？
- **项目协作**：其他开发者拿到项目后，通过该文件可快速安装所需依赖，无需手动逐个查找库名和版本。
- **部署项目**：在服务器、容器（如 Docker）或云平台上部署时，通过该文件一键安装依赖，保证环境一致性。

#### 如何编写、运行？
包含库名和版本号`基础格式：库名==版本号`：
```txt
requests==2.31.0
numpy==1.26.0
pandas>=2.0.0  # 也支持>=、<=等模糊版本约束（但推荐精确版本）
flask==2.3.3
```

在终端中进入 `requirements.txt` 所在目录，运行：

```bash
pip install -r requirements.txt
```

{%note danger%}
非 Python 生态的依赖无法直接通过它管理
{%endnote%}
{%endnote%}


```python
# main.py
from fastapi import FastAPI, Depends, HTTPException, Header
import ollama
import os
from dotenv import load_dotenv 

load_dotenv()

API_KEY_CREDITS = {os.getenv("API_KEY"): 10}

app = FastAPI()

def verify_api_key(x_api_key: str = Header(None)):  
    credits = API_KEY_CREDITS.get(x_api_key, 0)  
    if credits <= 0:
        raise HTTPException(status_code=401, detail="Invalid API Key, or no credits")
    return x_api_key

@app.post("/generate")
def generate(prompt: str, x_api_key: str = Depends(verify_api_key)):
    API_KEY_CREDITS[x_api_key] -= 1
    response = ollama.chat(
        model="mistral",
        messages=[{"role": "user", "content": prompt}]
    )
    return {"response": response["message"]["content"]}
```
{%note info%}
- `load_dotenv()`从`.env`文件中加载环境变量
#### 为什么要有 .env 文件？
**1. 保护敏感信息**
项目中常涉及数据库密码、API 密钥、Token、密钥等敏感数据。如果直接写在代码里（比如 `API_KEY = "abc123"`），一旦代码上传到 GitHub 等公开仓库，这些信息会被泄露，导致安全风险。用 `.env` 存储这些信息，再通过 `.gitignore` 忽略该文件，可避免敏感信息泄露。
**2. 区分环境配置**
一个项目通常有开发环境（本地开发）、测试环境（测试服务器）、生产环境（线上服务器），不同环境的配置可能不同（比如数据库地址、API 域名）。用 .env 可以为不同环境创建不同的配置文件，切换环境时只需替换配置文件，无需修改代码。
**3. 简化配置管理**
当配置需要修改时，直接修改 `.env` 文件即可，无需在代码中逐个查找硬编码的配置，降低维护成本。
#### 如何使用`.env`文件？
**1. 为 `键=值`的形式（键值对）**
```.env
API_KEY=abc123456  # API 密钥（敏感信息）
DB_HOST=localhost  # 数据库地址（环境相关配置）
```

**2. 在代码中读取**
需要借助工具库（如 Python 的 `python-dotenv`、Node.js 的 `dotenv`）加载 `.env` 中的配置到系统环境变量，再在代码中读取。
```python
from dotenv import load_dotenv
import os
load_dotenv()
api_key=os.getenv("API_KEY")
print(api_key)
# 输出结果：abc123456
```
- `load_dotenv()`：若不带参数，它会自动在当前工作目录中寻找名为 `.env` 的文件，并加载其中的环境变量。
- 如果配置文件区分不同环境（如`.env.dev`），则需要通过 `dotenv_path` 参数指定文件路径：`load_dotenv(dotenv_path=".env.dev")`。

**3. 忽略 .env 文件**
在项目的 `.gitignore` 文件中添加 `.env`，防止其被提交到代码仓库：
```.gitignore
.env  # 忽略主配置文件
.env.*  # 可选：忽略所有环境相关的配置文件
```

{%endnote%}

{%note info%}
```python
def verify_api_key(x_api_key: str = Header(None)):  
    credits = API_KEY_CREDITS.get(x_api_key, 0)  
    if credits <= 0:
        raise HTTPException(status_code=401, detail="Invalid API Key, or no credits")
    return x_api_key
```
- `verify_api_key`：这是一个 FastAPI 的依赖项（dependency）函数，无需显式，框架就会帮你自动调用这个依赖函数。
- `x_api_key: str = Header(None)`:
   
   - `x_api_key: str`：这里是 Python 的类型注解，用来声明变量 `x_api_key` 的类型是 str   
   - `Header(None)`：这是 FastAPI 的参数获取方式，指定 `x_api_key` 的值需要从请求头（Header） 中获取
- `raise HTTPException(status_code, detail)`：这是 FastAPI 中用于主动抛出错误响应。
   
   - `status_code=401`：这是 HTTP 状态码，用于表示未授权（Unauthorized）错误。
   - `detail="Invalid API Key, or no credits"`：这是错误详情，当 API 密钥无效或无可用额度时，会返回这个错误信息。

{%endnote%}

{%note info%}
```python
@app.post("/generate")
def generate(prompt: str, x_api_key: str = Depends(verify_api_key)):
    API_KEY_CREDITS[x_api_key] -= 1
    response = ollama.chat(
        model="mistral",
        messages=[{"role": "user", "content": prompt}]
    )
    return {"response": response["message"]["content"]}
```
- `@app.post("/generate")`：这是 FastAPI 的路由装饰器
  - `.post("/generate")` ：当客户端以 POST 方法访问 `/generate` 路径时，会执行下面的 `generate` 函数。
  - `Depends(verify_api_key)`：表示在执行 `generate` 函数前，先执行 `verify_api_key` 函数（之前定义的密钥验证函数），并将其返回值传给 `x_api_key`，如果 `verify_api_key` 验证失败（抛出 401 错误），`generate` 函数不会执行，直接返回错误给客户端。
- `ollama.chat(model,messages)`：用于与 AI 模型进行对话交互
  - `model="mistral"`：指定使用的 AI 模型是 "mistral"。
  - `messages=[{"role": "user", "content": prompt}]`：这是一个消息列表，包含一个用户消息。
    - `"role": "user"`：表示这是一个用户消息。
    - `"content": prompt`：用户消息的内容是 `prompt` 变量的值。
```python
messages=[
    {"role": "system", "content": "who are you?"},
    {"role": "user", "content": "tell me what's LLM?"},
]
```
{%endnote%}