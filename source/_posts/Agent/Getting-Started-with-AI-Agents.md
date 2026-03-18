---
title: Getting Started with AI Agents
date: 2026-01-15 18:11:00
tags:
archive: true 
---

### Note

- **Prompt Chaining**：像写管道（**Pipeline**）⼀样写，上⼀步的输出（**Output**） **=** 下⼀步的输⼊（**Input**）
- 不止写顺序执行的脚本，那样会很冗长，增加条件判断
- 并行化

### 提示词工程

- Zero-shot（零样本学习）：模型在**完全没有见过**相关示例的情况下处理新任务（依靠预训练知识和任务描述）。

```text
直接要求模型分类：
“将‘今天的天气真糟糕’分类为正面或负面情感。”
模型仅凭对语言的理解直接输出：负面
```

- Few-shot（少样本学习）：在模型训练时见过少量的相关示例进行参考，引导模型对齐示例输出结果。

```text
文本分类：给定几个例子
输入：“这部电影太精彩了” → 标签：正面
输入：“服务非常差” → 标签：负面
输入：“产品一般般” → 标签：中性

新输入：“这本书令人失望” → 模型预测：负面
```

文本分类实战：

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_OPENAI_API_KEY",
    base_url="https://api.deepseek.com/v1"
)

examples_types = ['编程语言', '操作系统', '数据库', '人工智能']

examples_data = {
    '编程语言': 'Python 是一种解释型、面向对象的高级编程语言，支持函数式编程和动态类型，广泛用于数据分析和Web开发。',
    '操作系统': 'Linux 是一种开源的类Unix操作系统内核，具有高稳定性和可定制性，被广泛应用于服务器和嵌入式设备。',
    '数据库': 'MySQL 是一款关系型数据库管理系统，采用SQL语言进行数据操作，支持事务和索引，适合中小型应用。',
    '人工智能': '深度学习是机器学习的一个分支，基于神经网络模拟人脑结构，在图像识别、自然语言处理领域有广泛应用。'
}

messages = [
    {
        "role": "system",
        "content": f"""你是一个文本分类专家，需要将输入的文本严格归类到以下类别之一：{examples_types}。
        要求：
        1. 只返回分类结果的标签（如：人工智能），不要返回任何额外解释；
        2. 必须从指定的4个类别中选择，不能自创类别。"""
    }
]
questions = [
    "Java 是一种面向对象的编程语言，拥有跨平台的特性，常用于企业级应用开发。",
    "Redis 是一款高性能的键值对非关系型数据库，支持多种数据结构，常用于缓存场景。",
    "Windows 11 是微软发布的新一代桌面操作系统，优化了界面交互和多任务处理能力。",
    "大语言模型是人工智能领域的重要技术，能够理解和生成人类语言，如GPT系列模型。",
    "Docker 是一款容器化技术，可将应用及其依赖打包成容器，简化部署流程。"
]

for label, example_text in examples_data.items():
    messages.append({"role": "user", "content": example_text})
    messages.append({"role": "assistant", "content": label})

for q in questions:
    response=client.chat.completions.create(
        model="deepseek-chat",
        messages=messages+[{"role": "user", "content": f"按照示例，回答这段文本：{q}"}]
    )
    print(response.choices[0].message .content)
```

### Langchain

- 向量库中余弦相似度的计算

```python
import numpy as np

def get_dot(vec_a,vec_b):
    if len(vec_a)!=len(vec_b):
        raise Exception("Vectors do not have the same dimension")

    dot_sum=0
    for a,b in zip(vec_a,vec_b):
        dot_sum+=a*b
    return dot_sum

def get_norm(vec):
    sum_squares=0
    for i in vec:
        sum_squares+=i*i
    return np.sqrt(sum_squares)


def cosine_similarity(vec_a,vec_b):
    result=get_dot(vec_a,vec_b)/(get_norm(vec_a)*get_norm(vec_b))
    print(result)

if __name__=="__main__":
    vec_a=[1,2,3]
    vec_b=[4,5,6]
    cosine_similarity(vec_a,vec_b)
```

- 使用`langchain`调用大模型

  - `invoke`一次性返回结果

  ```
  import os
  from langchain_community.llms import Tongyi
  
  os.environ["DASHSCOPE_API_KEY"] = "YOUR_API_KEY"
  
  model = Tongyi(model="qwen-max")
  
  response = model.invoke("你是什么大语言模型？能做什么？")
  print(response)
  ```

  - `stream`流式输出

```python
import os
from langchain_community.llms import Tongyi

os.environ["DASHSCOPE_API_KEY"] = "YOUR_API_KEY"

model = Tongyi(model="qwen-max")

response = model.stream("你是什么大语言模型？能做什么？")

for chunk in response:
    print(chunk,end="",flush=True)
```

- 聊天模型的应用

```python
from langchain_community.chat_models import ChatTongyi
from langchain_core.messages import HumanMessage,SystemMessage,AIMessage
import os

os.environ["DASHSCOPE_API_KEY"] = "YOUR_API_KEY"

chat = ChatTongyi(model="qwen3-max")

messages=[
    SystemMessage(content="你是一个计算机专家"), # 相当于OpenAI中的system
    HumanMessage(content="什么是LLM"), # 相当于OpenAI中的user
    AIMessage(content="LLM 是 Large Language Model（大语言模型）的缩写"), # 相当于OpenAI中的assistant
    HumanMessage(content="LLM 的核心特点有哪些？"),
]

response = chat.stream(input=messages)

for chunk in response:
    print(chunk.content,end="",flush=True)
```

- `langchain`调用嵌入模型

```python
from langchain_community.embeddings import DashScopeEmbeddings
import os

os.environ["DashScope_API_KEY"]="YOUR_API_KEY"

model=DashScopeEmbeddings()

print (model.embed_query("Hello World!"))
print (model.embed_documents(["你是谁","LLM","123"]))
```

