要做的不是简单的“工业聊天机器人”，而是一个大型 AI 工业应用平台，建议从一开始就按“平台化”设计。

把整体架构拆成 12 大模块。

---

# 一、整体架构

可以先把整个系统理解成：

```text
┌─────────────────────────────────────────────────────────────┐
│                     ① 应用层 Application                    │
│                                                             │
│  AI Chat │ 设备中心 │ CAD/3D │ 知识库 │ 工单 │ 报表 │ BI    │
└──────────────────────────────┬──────────────────────────────┘
                               │
┌──────────────────────────────▼──────────────────────────────┐
│                     ② AI Interaction                       │
│                                                             │
│  AI SDK │ Streaming │ Multimodal │ Voice │ Vision │ Canvas │
└──────────────────────────────┬──────────────────────────────┘
                               │
┌──────────────────────────────▼──────────────────────────────┐
│                     ③ Agent Platform                        │
│                                                             │
│ Agent │ Workflow │ Planner │ Memory │ Tool Calling │ MCP    │
└──────────────────────────────┬──────────────────────────────┘
                               │
          ┌────────────────────┼────────────────────┐
          ▼                    ▼                    ▼
┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐
│ ④ Knowledge     │  │ ⑤ Tool Platform │  │ ⑥ Model Platform│
│                 │  │                 │  │                 │
│ RAG             │  │ MCP             │  │ Model Router    │
│ Retriever       │  │ MES             │  │ LLM             │
│ Reranker        │  │ ERP             │  │ VLM             │
│ Embedding       │  │ PLC             │  │ Embedding       │
│ Vector DB       │  │ CAD             │  │ Reranker        │
└─────────────────┘  └─────────────────┘  └─────────────────┘
          │                    │                    │
          └────────────────────┼────────────────────┘
                               ▼
┌─────────────────────────────────────────────────────────────┐
│                 ⑦ Industrial Data Platform                  │
│                                                             │
│ MES │ ERP │ PLM │ SCADA │ IoT │ PLC │ OPC-UA │ MQTT │ CAD   │
└──────────────────────────────┬──────────────────────────────┘
                               │
┌──────────────────────────────▼──────────────────────────────┐
│                    ⑧ Data Infrastructure                    │
│                                                             │
│ PostgreSQL │ Redis │ Object Storage │ Vector DB │ TSDB       │
└──────────────────────────────┬──────────────────────────────┘
                               │
┌──────────────────────────────▼──────────────────────────────┐
│                ⑨ Platform Infrastructure                    │
│                                                             │
│ Auth │ RBAC │ Tenant │ API Gateway │ Queue │ Scheduler       │
└──────────────────────────────┬──────────────────────────────┘
                               │
┌──────────────────────────────▼──────────────────────────────┐
│             ⑩ Observability / Security / Governance         │
│                                                             │
│ Logs │ Trace │ Evaluation │ Audit │ Security │ Cost │ ACL    │
└─────────────────────────────────────────────────────────────┘
```

---

# 二、① AI 应用层

这是用户真正看到的东西。

建议至少做：

### 1. AI Chat

```text
AI 对话
├── 普通问答
├── 多轮对话
├── 文件上传
├── 图片
├── CAD
├── 视频
├── 语音
├── Tool Call
├── RAG
├── Deep Research
└── Agent
```

---

### 2. 工业 Copilot

例如：

> “帮我分析 3 号机床今天的异常。”

AI 自动：

```text
查询设备
    ↓
查询报警
    ↓
查询历史维修记录
    ↓
查询设备手册
    ↓
查询实时参数
    ↓
分析
    ↓
给出故障原因
    ↓
给出处理建议
```

---

### 3. 设备中心

```text
设备列表
设备详情
设备状态
实时参数
历史数据
报警
维修记录
运行日志
设备寿命
预测维护
```

---

### 4. 工单系统

```text
故障
 ↓
AI 自动诊断
 ↓
创建工单
 ↓
分配工程师
 ↓
维修
 ↓
上传维修结果
 ↓
AI 总结
 ↓
形成知识
```

这一块最终会和 RAG 形成闭环。

---

# 三、② AI Interaction Layer

这是 AI SDK 最适合干的事情。

例如：

```text
AI SDK
├── Streaming
├── Chat
├── Tool UI
├── Structured Output
├── Multimodal
├── File
├── Voice
└── Model Provider
```

同时建议增加：

### Voice

```text
Speech To Text
       ↓
     Agent
       ↓
Text To Speech
```

工业 Pad 上尤其有价值。

工程师可以：

> “查询一下 3 号设备现在的主轴温度。”

---

# 四、③ Agent Platform

这是整个系统的“大脑”。

建议拆成：

```text
Agent Platform
│
├── Agent
├── Workflow
├── Planner
├── Memory
├── Tool Calling
├── MCP
├── Human-in-the-loop
├── State
├── Task
└── Scheduler
```

---

## Agent

例如：

```text
Supervisor Agent
│
├── Knowledge Agent
├── Equipment Agent
├── Maintenance Agent
├── CAD Agent
├── SQL Agent
└── Report Agent
```

不要让一个 Agent 什么都做。

大型系统一定要Agent 专业化。

---

# 五、④ RAG / Knowledge Platform

这是工业 AI 的核心之一。

不要只做：

```text
PDF → Embedding → Vector DB
```

大型工业系统建议：

```text
                 Knowledge Platform
                        │
       ┌────────────────┼────────────────┐
       ▼                ▼                ▼
   Document          Structured       Graph
       │                │                │
       ▼                ▼                ▼
 PDF/Word            MES/ERP          Equipment
 CAD                 SQL              Relationship
 Excel               API              Fault
 Images                                Parts
       │
       ▼
     Parser
       │
       ▼
     Chunking
       │
       ▼
   Embedding
       │
       ▼
    Retriever
       │
       ▼
    Reranker
       │
       ▼
     Context
       │
       ▼
      LLM
```

---

## 知识库至少支持

```text
PDF
Word
Excel
TXT
Markdown
CAD
图片
视频
设备手册
维修记录
工单
MES数据
ERP数据
SQL
API
```

---

# 六、⑤ Tool / MCP Platform

这是工业 AI 和普通 ChatGPT 最大的区别。

AI 不能只“回答”。

它必须能够：

```text
查询
读取
分析
控制
执行
```

例如：

```text
MCP
│
├── MES
│   ├── queryOrder
│   ├── queryProduction
│   └── queryMaterial
│
├── Equipment
│   ├── getStatus
│   ├── getTemperature
│   └── getAlarm
│
├── PLC
│   ├── read
│   └── write
│
├── CAD
│   ├── parse
│   ├── measure
│   └── generate3D
│
└── ERP
    ├── queryInventory
    └── queryPurchase
```

---

# 七、⑥ Model Platform

不要让业务代码直接：

```text
openai(...)
qwen(...)
deepseek(...)
```

应该统一成：

```text
                Model Gateway
                     │
       ┌─────────────┼─────────────┐
       ▼             ▼             ▼
     Cloud          Cloud         Local
       │             │             │
      GPT           Qwen          Ollama
      Claude        Kimi          vLLM
      Gemini        DeepSeek      SGLang
```

然后做：

### Model Router

例如：

```text
简单问题
 ↓
便宜模型

复杂推理
 ↓
Reasoning Model

图片
 ↓
Vision Model

Embedding
 ↓
Embedding Model

Rerank
 ↓
Reranker
```

以后甚至可以：

```text
成本
速度
质量
上下文
任务类型
```

综合决定模型。

---

# 八、⑦ Industrial Data Platform

这一层非常重要。

不要让 Agent 直接连接各种工业系统。

应该：

```text
Agent
 ↓
Tool / MCP
 ↓
Industrial API Layer
 ↓
数据系统
```

支持：

```text
MES
ERP
PLM
SCADA
WMS
CRM
IoT
PLC
OPC-UA
MQTT
Modbus
REST API
WebSocket
```

---

# 九、⑧ 数据基础设施

我建议你初期：

```text
PostgreSQL
├── 用户
├── 租户
├── Agent
├── Conversation
├── Message
├── Knowledge
├── Document
├── Device
├── WorkOrder
└── Audit
```

然后：

```text
pgvector
```

做向量。

再加：

```text
Redis
```

负责：

```text
Cache
Session
Queue
Rate Limit
Agent State
```

对象存储：

```text
S3 / MinIO
```

保存：

```text
PDF
CAD
图片
视频
附件
模型文件
```

如果工业实时数据量非常大，再增加：

```text
TimescaleDB
InfluxDB
ClickHouse
```

---

# 十、⑨ 平台基础能力

大型 AI 系统一定要提前考虑：

### 用户

```text
User
Organization
Tenant
Department
Role
Permission
```

### RBAC

例如：

```text
管理员
工程师
维修人员
操作员
访客
```

不同角色：

```text
能看什么
能问什么
能调用什么 Tool
能执行什么操作
```

尤其是：

> AI Tool 的权限

非常重要。

例如：

```text
AI 可以：

查询 PLC      ✅
读取设备      ✅
修改 PLC      ❌

工程师：

查询 PLC      ✅
读取设备      ✅
修改 PLC      ⚠️
```

---

# 十一、⑩ AI 安全与治理

工业 AI 千万不要忽略这一层。

建议：

```text
AI Governance
│
├── Prompt Security
├── Tool Permission
├── Data Permission
├── PII
├── Audit
├── Model Policy
├── Cost Control
├── Rate Limit
├── Prompt Injection
├── Tool Injection
└── Human Approval
```

特别是：

```text
AI
 ↓
PLC
 ↓
生产设备
```

绝对不能：

```text
LLM → PLC
```

直接执行。

必须：

```text
LLM
 ↓
Tool
 ↓
Permission
 ↓
Validation
 ↓
Human Approval
 ↓
PLC
```

---

# 十二、⑪ Observability

大型 AI 应用必须知道：

> AI 到底为什么这么回答？

所以需要：

```text
Trace
│
├── User Request
├── Agent
├── LLM
├── Prompt
├── RAG
├── Retriever
├── Reranker
├── Tool
├── SQL
└── Final Answer
```

例如：

```text
用户问题
  ↓ 120ms

Intent Agent
  ↓ 800ms

Retriever
  ↓ 200ms

Reranker
  ↓ 400ms

Equipment API
  ↓ 120ms

GPT
  ↓ 3.2s

Final
```

这样才能排查：

```text
为什么慢？
为什么回答错？
哪个 Tool 出错？
哪个模型最贵？
RAG 为什么没找到？
```

---

# 十三、⑫ AI Evaluation

这是很多 AI 项目最容易忽略的。

大型系统一定要有：

```text
Evaluation
│
├── RAG Evaluation
├── Agent Evaluation
├── Tool Evaluation
├── Model Evaluation
├── Prompt Evaluation
└── Regression Test
```

例如建立 1000 个工业问题：

```text
问题
标准答案
参考文档
应该调用的 Tool
正确结果
```

每次修改：

```text
Prompt
Agent
RAG
Model
Retriever
Reranker
```

自动测试。

---

# 十四、最后形成一个完整平台

我建议你的最终目录甚至可以设计成：

```text
industrial-ai/
│
├── apps/
│   ├── web/
│   ├── admin/
│   ├── pad/
│   └── mobile/
│
├── ai/
│   ├── agents/
│   ├── workflows/
│   ├── memory/
│   ├── rag/
│   ├── prompts/
│   ├── models/
│   └── evaluation/
│
├── tools/
│   ├── mcp/
│   ├── mes/
│   ├── erp/
│   ├── plc/
│   ├── cad/
│   └── equipment/
│
├── knowledge/
│   ├── parser/
│   ├── chunking/
│   ├── embedding/
│   ├── retrieval/
│   └── reranking/
│
├── data/
│   ├── postgres/
│   ├── vector/
│   ├── redis/
│   ├── object-storage/
│   └── timeseries/
│
├── platform/
│   ├── auth/
│   ├── tenant/
│   ├── rbac/
│   ├── api/
│   ├── queue/
│   └── scheduler/
│
└── observability/
    ├── tracing/
    ├── logging/
    ├── evaluation/
    └── audit/
```

---
