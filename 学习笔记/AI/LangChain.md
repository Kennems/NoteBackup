---
title : 'LangChain'
date : 2026-03-22T20:43:04+08:00
lastmod: 2026-05-05T17:05:36+08:00
description : "LangChain 是一个用于构建 LLM 应用的开源框架，提供模型调用、Prompt 模板、输出解析等工具链。"
image : img/cat.jpg
draft : false
categories : ["AI"]
tags : ["学习笔记", "AI"]
---
# LangChain

LangChain 是一个用于构建 LLM 应用的开源框架，提供模型调用、Prompt 模板、输出解析等工具链。

## 核心组件

### Models（模型）

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-4", temperature=0.7)
response = llm.invoke("Hello, world!")
print(response.content)
```

### Prompts（提示模板）

```python
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_messages([
    ("system", "You are a helpful assistant."),
    ("user", "{input}")
])

# 生成完整提示
formatted = prompt.format(input="What is LangChain?")
```

### Parsers（输出解析器）

```python
from langchain_core.output_parsers import StrOutputParser

parser = StrOutputParser()
result = parser.invoke("This is a response")
```

## LCEL (LangChain Expression Language)

使用 `|` 管道符串联组件：

```python
chain = prompt | llm | parser
response = chain.invoke({"input": "Explain AI in 3 bullet points"})
print(response)
```

## 常用链

```python
# RAG (Retrieval-Augmented Generation)
from langchain_core.runnables import RunnablePassthrough
from langchain_core.vectorstores import InMemoryVectorStore

vectorstore = InMemoryVectorStore.from_documents(documents, embeddings)
retriever = vectorstore.as_retriever()

rag_chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | llm
    | parser
)
```

## 参考

- [LangChain 官方文档](https://python.langchain.com/)
- [LangChain Expression Language (LCEL)](https://python.langchain.com/docs/concepts/lcel/)
