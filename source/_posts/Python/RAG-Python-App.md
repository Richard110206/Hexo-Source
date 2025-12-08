---
title: RAG Python App
date: 2025-11-16 12:57:21
tags: [updating, Python, RAG]
index_img: https://github.com/Richard110206/Blog-image/blob/main/cover/RAG%20Python%20App.png?raw=true
category: Python
category_bar: true
description: This Production-Ready RAG Python app lets you deploy locally with ease. Try out RAG functionalities firsthand and enjoy a smooth experience!
---


项目代码参考了[Tech with Tim ProductionRAGPythonApp](https://www.youtube.com/watch?v=AUQJ9eeP-Ls&t=4425s)，并对其源代码进行了一定修改，如更换 OpenAI 的嵌入模型和大语言模型等，并开源在 github 上： [ProductionRAGPythonApp](https://github.com/Richard110206/ProductionRAGPythonApp)，但对我来说更重要的是能够通过代码学习整个 RAG 的思想及 API 调用的方法等，欢迎 PR!

项目的环境配置和运行方法请参考项目的 README 文件，这里我主要对 RAG 学习过程中遇到的问题、库进行梳理工作，让我们开始吧！

首先，我们使用 uv 创建一个项目
```bash
pip install uv
```
对项目进行初始化：
```bash
(base) richard@RicharddeMacBook-Air RAGProductionApp % uv init .
Initialized project `ragproductionapp` at `/Users/richard/CUMT-ChatBot/RAGProductionApp`
```
给项目添加相关依赖：
```bash
(base) richard@RicharddeMacBook-Air RAGProductionApp % uv add fastapi llama-index-core llama-index-readers-file python-dotenv qdrant-client unicorn streamlit openai
```
{%note info%} 
**什么是 uv？**
uv 是一个高性能的 Python 项目管理工具，主要用于依赖管理、虚拟环境管理以及项目构建。

- 项目初始化后自动生成相关文件，结构如下
```bash
(base) richard@RicharddeMacBook-Air uv-learning % tree
.
├── main.py
├── .gitignore
├── pyproject.toml
├── python-version
└── README.md
```
我们逐个拆解一下每个文件的作用：
- `pyproject.toml`：
  - 声明项目核心信息，包括名称、版本、描述等
  - 规定项目依赖的 Python 版本和第三方包，用于包管理工具（如 uv、pip）识别依赖
  - 关联项目说明文档（README.md），方便用户和工具查看项目详情
```
[project]
name = "python-uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = []
```
当使用 `uv add <denpendencies>` 命令添加依赖时，uv 会自动更新 `pyproject.toml` 的 `dependencies` 列表；复现环境时，执行 `uv pip install . `即可根据 `pyproject.toml` 安装所有依赖，就像是这样：
```
[project]
name = "ragproductionapp"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13"
dependencies = [
    "fastapi>=0.116.1",
    "inngest>=0.5.6",
    "llama-index-core>=0.14.0",
    "llama-index-readers-file>=0.5.4",
    "openai>=1.107.0",
    "python-dotenv>=1.1.1",
    "qdrant-client>=1.15.1",
    "streamlit>=1.49.1",
    "uvicorn>=0.35.0",
]
```
uv 也提供创建虚拟环境的方式：
```bash
uv venv       # 创建 .venv 虚拟环境
source .venv/bin/activate  # 激活 Linux/Mac
.venv\Scripts\activate  # 激活 Windows
uv pip install .           # 在虚拟环境中安装依赖
```
{%endnote%}

```bash
(hexo-rag) richard@RicharddeMacBook-Air RAGProductionApp % uv run uvicorn main:app
INFO:     Started server process [20305]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

```bash
(hexo-rag) richard@RicharddeMacBook-Air RAGProductionApp % npx inngest-cli@latest dev -u  http://127.0.0.1:8000/api/inngest --no-discovery 
Need to install the following packages:
inngest-cli@1.13.4
Ok to proceed? (y) y

[09:03:30.568] INF starting executor grpc server svc=executor addr=:50053
[09:03:30.569] INF service starting caller=executor service=executor
[09:03:30.569] INF service starting caller=devserver service=devserver
[09:03:30.569] INF starting event stream backend=redis
[09:03:30.569] INF service starting caller=runner service=runner
[09:03:30.569] INF subscribing to function queue
[09:03:30.569] INF service starting caller=api service=api
[09:03:30.569] INF starting server caller=api addr=0.0.0.0:8288
[09:03:30.569] INF service starting caller=connect-gateway service=connect-gateway
[09:03:30.570] INF starting connect gateway grpc server caller=connect-gateway addr=:50052
[09:03:30.571] INF subscribing to events topic=events


        Inngest Dev Server online at 0.0.0.0:8288, visible at the following URLs:

         - http://127.0.0.1:8288 (http://localhost:8288)
         - http://10.6.227.126:8288
```

在浏览器中输入`http://localhost:8288/runs`进入控制台界面

```bash
(hexo-rag) richard@RicharddeMacBook-Air RAGProductionApp % docker run -d --name qdrantRagDb -p 6333:6333 -v "./qdrant_storage:/qdrant/storage" qdrant/qdrant
Unable to find image 'qdrant/qdrant:latest' locally
latest: Pulling from qdrant/qdrant
4f4fb700ef54: Pull complete 
e363695fcb93: Pull complete 
8867fa9eb8c4: Pull complete 
93617c057df8: Pull complete 
3638f8fe1dda: Pull complete 
f907b10db73d: Pull complete 
efc2301f4369: Pull complete 
c3a47fd98373: Pull complete 
Digest: sha256:0fb8897412abc81d1c0430a899b9a81eb8328aa634e7242d1bc804c1fe8fe863
Status: Downloaded newer image for qdrant/qdrant:latest
d0b630ffc2e6845ab104c2545da801a1b44af02d8d810f162ff57d2fecc50ee2
```

如果是`windows`用户路径需要修改使用`"$(pwd)/qdrant_storage:/qdrant/storage" `

注意要先启动`docker desktop`
```bash
uv run streamlit run .\streamlit_app.py
```

## RAG
在学习代码之前，我们先了解一下 **RAG** 的基本原理，全称是 **Retrieval-Augmented Generation**，即“**检索增强生成**”，是构建个人知识库的关键技术。当前风靡全球的大模型如chatgpt、Gemini、Deepseek多为通用型，**无法对高度个性化的问题给出精确解答**。例如，若我想知道“学校南门每晚几点关门”或“矿大宿舍的报修电话是多少”，通用模型显然无法提供准确信息。

这正是RAG的价值所在：通过事先为模型提供特定的知识库，使模型能够基于这些定制化信息进行回答，从而实现大模型的个性化应用。
{%fold into@RAG相较于直接使用大模型上传文件的优势？%}
1. 身份的转变：从“使用者”变为“创造者”，根据需求定制自己的AI应用
2. 解决“幻觉”问题：通过提供精准的上下文并要求引用来源，能构建出值得信赖的AI应用，这对于企业级场景是刚需。
3. 应对复杂场景：当您的知识库包含成千上万个文档时，简单的文件上传功能会显得力不从心，而RAG架构是为处理这种海量、复杂的检索任务而生的。
4. 技术集成的核心：RAG不仅仅是一个独立技术，它更是将向量数据库、嵌入模型、大语言模型等现代AI核心组件串联起来的“胶水”。通过学习RAG，可以系统地学习了当前AI应用开发的核心技术栈。
{%endfold%}
### (i) 索引
在用户提问之前我们需要将文档做成一个方便快速查找的索引：
1. **加载数据**：收集所有可能用到的文档，格式可以是 PDF、Word、TXT、网页 HTML、数据库记录等。
2. **分割**文本：由于长文本会包含很多无关信息，不利于精准检索，因此需要将长文档切分成更小的、语义完整的文本块（Chunks）。
3. **向量化**：使用一个“嵌入模型”将每个文本块转换成一串数字，即向量。这个向量可以理解为文本在数学空间中的“坐标”，语义相近的文本，其向量在空间中的距离也更近。
4. **存储向量**：将这些向量及其对应的原始文本存储到一个专门的数据库中，这个数据库被称为**向量数据库**。这个过程就像是给所有复习资料做了一个详细的目录索引。

### (ii) 检索
当用户提出一个问题（即查询）时，RAG 系统开始在存储的数据库中查找:
1. **查询向量化**：使用与第一阶段相同的嵌入模型，将用户的问题也转换成一个查询向量。
2. **相似性搜索**：在向量数据库中，寻找与“查询向量”最相似的几个文本块向量。相似性通常通过计算向量间的距离（如余弦相似度）来衡量，距离越近，代表语义越相关。
3. **获取参考文本**：系统检索出最相关的 K 个文本块（例如，Top-3 或 Top-5），作为回答问题的参考依据。

### (iii) 生成
现在通过前面的检索过程，我们已经获取到了与用户问题最相关的文本块，接下来我们需要将这些文本块与用户的问题一起输入到大模型中，让模型根据这些信息生成一个准确的回答。
1. **组合提示**：系统会构建一个精心设计的提示
2. **增强生成**：将组合好的提示发送给大语言模型。LLM 会基于这个“被增强了的”上下文信息来生成回答。
3. **输出最终答案**：LLM 产生的最终答案会返回给用户。这个答案不仅利用了模型自身的通用知识，更关键的是融入了从私有数据中检索到的最新、最相关的信息。
### 主函数
### 文本嵌入生成`data_loader.py`
### 向量存储`vector_db.py`
### 前端交互`streamlit_app.py`