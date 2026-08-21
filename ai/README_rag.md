# 一、主流 RAG 框架

```text
                用户问题
                   │
                   ▼
        ┌─────────────────────┐
        │   RAG Orchestrator  │  ← LangChain / LlamaIndex / Haystack
        └──────────┬──────────┘
                   │
         ┌─────────▼─────────┐
         │ Retrieval / Search │ ← Milvus / Qdrant / Weaviate
         │ 向量 + BM25 + 混合检索 │
         └─────────┬─────────┘
                   │
             Reranker
                   │
         ┌─────────▼─────────┐
         │       LLM         │ ← Qwen / GPT / DeepSeek / GPT-OSS
         └────────────────────┘
```

## 1. LlamaIndex ⭐⭐⭐⭐⭐

[LlamaIndex 官方文档](https://docs.llamaindex.ai/?utm_source=chatgpt.com)

这是我认为**最值得你研究的 RAG 框架之一**。

它的核心思想非常简单：

> **把各种数据连接到 LLM。**

例如：

```text
PDF
Word
Excel
数据库
网页
API
Notion
Google Drive
      │
      ▼
 LlamaIndex
      │
 ├── Document
 ├── Chunk
 ├── Embedding
 ├── Index
 ├── Retriever
 ├── Reranker
 └── Query Engine
      │
      ▼
     LLM
```

它特别强调：

* 数据 ingestion
* Document
* Index
* Retriever
* Query Engine
* RAG Pipeline
* Agent
* Workflow

所以如果你准备**自己写工业 AI 的 RAG 层**，LlamaIndex 很值得学。

---

# 二、LangChain

[LangChain 官方 Retrieval 文档](https://docs.langchain.com/oss/python/langchain/retrieval?utm_source=chatgpt.com)

LangChain 更像一个：

> **LLM 应用开发基础设施 / 编排框架**

它当然可以做 RAG，而且现在官方文档直接把 RAG 分成：

* 2-step RAG
* Agentic RAG
* Hybrid RAG

([Docs by LangChain][1])

典型结构：

```text
LangChain
    │
    ├── Prompt
    ├── Retriever
    ├── Tool
    ├── Agent
    ├── Memory
    ├── RAG
    └── Model
```

所以：

**LlamaIndex 更偏“数据/RAG”**

**LangChain 更偏“Agent/应用编排”**

不过现在两者功能已经大量重叠。

---

# 三、Haystack

[Haystack 官方文档](https://docs.haystack.deepset.ai/?utm_source=chatgpt.com)

Haystack 是另一个非常成熟的 RAG/AI Pipeline 框架。

它比较强调：

```text
Document
   ↓
Preprocessor
   ↓
Retriever
   ↓
Ranker
   ↓
Prompt
   ↓
Generator
```

特点是**Pipeline 思维非常强**。

例如：

```text
用户问题
   ↓
BM25 Retriever
   +
Vector Retriever
   ↓
Document Join
   ↓
Reranker
   ↓
Prompt Builder
   ↓
LLM
```

如果你喜欢比较工程化、模块化的 RAG Pipeline，Haystack 很不错。

---

# 四、RAGFlow ⭐⭐⭐⭐⭐

这个我特别建议你研究。

[RAGFlow GitHub](https://github.com/infiniflow/ragflow?utm_source=chatgpt.com)

RAGFlow 和上面的区别很大。

它不是单纯的 Python RAG SDK，而是：

> **完整的开源 RAG 平台 / RAG Engine。**

目前官方定位已经从单纯 RAG 扩展到 **RAG + Agent + Context Engine**。([GitHub][2])

它可以直接处理：

* PDF
* Word
* Excel
* PPT
* Markdown
* 图片
* 扫描文档
* 网页
* 结构化数据

而且重点是它有比较强的**文档理解能力**。

例如工业场景：

```text
                RAGFlow
                   │
       ┌───────────┼───────────┐
       ▼           ▼           ▼
    PDF图纸      维修手册      Excel
       │           │           │
       └───────────┼───────────┘
                   ▼
              DeepDoc
                   │
             Chunk / Index
                   │
             Retrieval
                   │
              Reranking
                   │
                   ▼
                  LLM
```

它还支持 Agent、MCP、Memory 等能力。([GitHub][2])

### 对你尤其重要

你之前一直在考虑：

> **工业 AI + CAD/设计图纸 + 设备维修手册 + RAG + Agent**

那么 RAGFlow 的方向其实非常贴近你的需求。

---

# 五、GraphRAG

[Microsoft GraphRAG GitHub](https://github.com/microsoft/graphrag?utm_source=chatgpt.com)

这个和传统 RAG 有一个很大的区别。

传统 RAG：

```text
问题
 ↓
Embedding
 ↓
向量搜索
 ↓
Top K 文档
 ↓
LLM
```

GraphRAG：

```text
文档
 ↓
实体抽取
 ↓
关系抽取
 ↓
Knowledge Graph
 ↓
社区/关系分析
 ↓
LLM
```

例如你的工业设备：

```text
设备A
 │
 ├── 使用 → 电机M
 │
 ├── 使用 → 轴承B
 │
 ├── 出现 → E102报警
 │
 └── 维修 → 张师傅
```

用户问：

> “E102 经常出现是什么原因？”

GraphRAG 可以利用：

```text
E102
 ↓
关联设备
 ↓
关联部件
 ↓
历史维修记录
 ↓
故障原因
```

因此它特别适合：

**复杂关系查询、企业知识图谱、跨文档推理。**

---

# 六、LightRAG

LightRAG 是近几年比较受关注的轻量级 Graph RAG 方向。

它的核心目标是：

> **让 Graph-based RAG 不要像传统 GraphRAG 那么重。**

大概：

```text
普通 RAG

Document
 ↓
Chunk
 ↓
Vector
 ↓
Search


LightRAG

Document
 ↓
Entity / Relation
 ↓
Graph + Vector
 ↓
Hybrid Retrieval
 ↓
LLM
```

它比较适合研究：

**Graph + Vector + RAG**

---

# 七、向量数据库不是 RAG Engine

这一点你一定要分清。

下面这些经常被一起讨论：

### Milvus

[Milvus 官方文档](https://milvus.io/docs?utm_source=chatgpt.com)

### Qdrant

[Qdrant 官方文档](https://qdrant.tech/documentation/?utm_source=chatgpt.com)

### Weaviate

[Weaviate 官方文档](https://docs.weaviate.io/?utm_source=chatgpt.com)

### pgvector

[pgvector GitHub](https://github.com/pgvector/pgvector?utm_source=chatgpt.com)

它们主要负责：

```text
Embedding
    ↓
Vector Database
    ↓
Similarity Search
    ↓
Top K
```

不是完整 RAG。

---

# 八、RAG 真正核心其实是 Retriever

很多人以为：

> RAG = 向量数据库

其实不是。

一个比较成熟的 RAG：

```text
                Query
                  │
          ┌───────┴───────┐
          ▼               ▼
      Vector Search     BM25
          │               │
          └───────┬───────┘
                  ▼
             Hybrid Search
                  │
                  ▼
               Reranker
                  │
                  ▼
              Top 5 Docs
                  │
                  ▼
                 LLM
```

其中 **Reranker 非常重要**。

例如：

```text
用户：

“3号机床主轴温度过高怎么办？”
```

Vector Search 找出来：

```text
1. 主轴温度异常处理
2. 机床日常维护
3. 冷却液更换规范
4. 3号机床维修记录
5. 主轴轴承更换
```

然后 Reranker 再判断：

```text
1 → 0.96
4 → 0.94
5 → 0.88
3 → 0.71
2 → 0.52
```

最终交给 LLM：

```text
1
4
5
```

这通常比单纯 Vector Search 好很多。Reranking 的作用就是对第一阶段检索结果重新排序，以提高相关性。([Weaviate][3])

---

# 九、我给你一个实际的技术选型表

| 技术             | 定位                  |   RAG | Agent | Graph |  文档解析 |   适合你 |
| -------------- | ------------------- | ----: | ----: | ----: | ----: | ----: |
| **LlamaIndex** | RAG Framework       | ⭐⭐⭐⭐⭐ |  ⭐⭐⭐⭐ |  ⭐⭐⭐⭐ |  ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **LangChain**  | AI 编排               |  ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |   ⭐⭐⭐ |   ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Haystack**   | RAG Pipeline        | ⭐⭐⭐⭐⭐ |  ⭐⭐⭐⭐ |   ⭐⭐⭐ |  ⭐⭐⭐⭐ |  ⭐⭐⭐⭐ |
| **RAGFlow**    | RAG Engine/Platform | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |  ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **GraphRAG**   | Graph RAG           |  ⭐⭐⭐⭐ |   ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |   ⭐⭐⭐ |  ⭐⭐⭐⭐ |
| **LightRAG**   | Graph RAG           |  ⭐⭐⭐⭐ |   ⭐⭐⭐ | ⭐⭐⭐⭐⭐ |   ⭐⭐⭐ |  ⭐⭐⭐⭐ |
| **Milvus**     | Vector DB           |  ⭐⭐⭐⭐ |     ❌ |     ⭐ |     ❌ |  ⭐⭐⭐⭐ |
| **Qdrant**     | Vector DB           |  ⭐⭐⭐⭐ |     ❌ |     ⭐ |     ❌ |  ⭐⭐⭐⭐ |
| **Weaviate**   | Vector DB           |  ⭐⭐⭐⭐ |     ❌ |    ⭐⭐ |     ❌ |  ⭐⭐⭐⭐ |
| **pgvector**   | PostgreSQL Vector   |   ⭐⭐⭐ |     ❌ |     ⭐ |     ❌ | ⭐⭐⭐⭐⭐ |

---

# 十、如果是你这个工业 AI 项目，我不建议“全家桶”

结合你之前一直在做的：

**Next.js + Mastra + AI Gateway + Supabase + Agent + Tool Calling + 工业设备 + CAD/维修资料**

我反而建议你把架构拆开：

```text
                    用户
                     │
                     ▼
              Next.js / Flutter
                     │
                     ▼
                Mastra Agent
                     │
          ┌──────────┼──────────┐
          │          │          │
          ▼          ▼          ▼
        LLM        Tools       RAG
                     │          │
                 MES / ERP      │
                 IoT / API      │
                                ▼
                          LlamaIndex
                                │
                    ┌───────────┼───────────┐
                    ▼           ▼           ▼
                 Vector       BM25       Reranker
                   DB                       │
                    │                      │
                    └──────────┬───────────┘
                               ▼
                              LLM
```

### 我个人更推荐两种路线

**路线 A：自己掌控，适合长期产品化**

```text
Mastra
 +
LlamaIndex
 +
Qdrant / Milvus / pgvector
 +
BGE / Qwen Embedding
 +
BGE Reranker
 +
GPT / Qwen / DeepSeek / GPT-OSS
```

优点：

**灵活、可控、方便和你的 Agent 架构结合。**

---

**路线 B：快速做工业知识库**

```text
RAGFlow
   +
Mastra
   +
你的 Tool / MCP
   +
LLM
```

RAGFlow 专门负责：

```text
PDF
Word
Excel
CAD相关文档
维修手册
设备说明书
知识库
检索
Rerank
引用
```

Mastra 负责：

```text
Agent
Tool Calling
MCP
业务流程
设备 API
MES
ERP
工单
```

这个组合我认为**非常适合你现在的工业 AI 项目**。

尤其是 RAGFlow 当前已经具备 Agent、MCP、Memory 等能力，因此不一定非要把所有东西自己重新实现。([GitHub][2])

**如果你准备真正落地，我建议下一步重点研究的不是 10 个 RAG 框架，而是把这 6 个东西搞明白：**

```text
1. Chunking
2. Embedding
3. Vector Search
4. Hybrid Search
5. Reranker
6. Graph RAG
```

把这 6 个搞懂，你基本就真正理解 **RAG 推理链路**了。

[1]: https://docs.langchain.com/oss/python/langchain/retrieval?utm_source=chatgpt.com "Retrieval - Docs by LangChain"
[2]: https://github.com/infiniflow/ragflow/blob/main/README.md?utm_source=chatgpt.com "ragflow/README.md at main · infiniflow/ragflow · GitHub"
[3]: https://weaviate.io/developers/weaviate/concepts/reranking?utm_source=chatgpt.com "Reranking | Weaviate"
