# ? 大模型（LLM）学习路线图

## ? 概述

本路线图适合具有编程基础（Java / 前端 / 全栈）的开发者，目标是掌握 **大模型推理、微调、RAG、应用开发** 等核心技能，从零到能独立完成 AI 项目落地。

---

# ? 阶段 0：环境准备（1–3 天）

### ? 目标

能够正确运行 Python、PyTorch、vLLM、大模型推理环境。

### ? 必备工具

* WSL2（Ubuntu 22.04）
* Python 3.10
* PyTorch + CUDA（需 NVIDIA GPU）
* venv 或 Conda
* VSCode / JupyterLab

### ? 要会做

* 创建 Python 虚拟环境
* 安装 PyTorch GPU 版
* 验证 `nvidia-smi`
* 安装 vLLM 并成功加载模型

---

# ? 阶段 1：LLM 基础概念（3–7 天）

### ? 目标

理解大模型的核心原理，无需数学推导。

### ? 需要掌握的概念

* Transformer 架构
* Attention（注意力机制）
* Token / Tokenizer
* 位置编码
* 推理（Inference） vs 训练（Training）
* Prompt 格式、Prompt 工程

### ? 推荐资料

* Stanford CS25（中文笔记）
* 《动手学深度学习》 Transformer 章节
* B 站：李沐《Attention》

---

# ? 阶段 2：大模型推理与部署（5–10 天）

### ? 目标

学会在本地或服务器加载大模型，并提供 API 服务。

### ? 推荐框架

| 框架       | 说明             |
| -------- | -------------- |
| vLLM     | ? 最强推理框架，企业主流  |
| FastChat | 老牌，但已被 vLLM 替代 |
| Ollama   | 轻量部署，适合个人      |
| LMDeploy | 腾讯推出，速度极快      |

### ? 需要掌握

* 加载 HuggingFace 模型
* 通过 vLLM 提供 OpenAI 兼容 API
* 配置推理参数（max_tokens、temperature、top_p）
* 使用 LoRA 权重进行推理
* Node.js/Java 调用本地模型

---

# ? 阶段 3：大模型微调（Fine-tuning）（10–20 天）

### ? 目标

能独立完成一个大模型的微调并部署。

### ? 微调方式（必学）

* **SFT（监督微调）**
* **LoRA / QLoRA（推荐，显存 8G–24G 可用）**
* 指令微调（Instruction Tuning）
* DPO / PPO（进阶，可选）

### ? 主流微调框架

* HuggingFace Transformers
* PEFT（LoRA）
* TRL（最常用的 SFT 工具）
* DeepSpeed / ColossalAI（高级）

### ? 你要能做到：

* 准备自定义训练数据
* 做一次 QLoRA 微调
* 使用 vLLM + LoRA-path 推理
* 合并权重（可选）

---

# ? 阶段 4：RAG（检索增强生成）（5–15 天）

### ? 目标

构建企业最常用的知识库问答系统。

### ? 必学技术

* Embedding（向量化）
* 向量数据库（Milvus / pgvector / Chroma / FAISS）
* 文档分段、召回、重写
* LangChain / LlamaIndex

### ? 要会做：

1. 使用 Embedding 模型转换向量
2. 搭建向量数据库
3. 构建 RAG 流程：

    * 文档 → 分段 → 向量化 → 检索 → 填充 → 生成
4. 构建搜索增强问答系统

---

# ? 阶段 5：大模型应用工程（10–20 天）

### ? 目标

能够开发完整的 AI 应用系统（企业落地能力）。

### ? 必须掌握

* Prompt engineering
* Agent（自主任务链）
* Function Calling
* 长文本处理
* 多模态（图像/语音）
* React + Chat UI 构建前端
* 后端（Java / Node）对接模型

### ? 典型项目

* 文档问答系统
* AI 客服
* 大模型代码助手
* AI 报表生成
* 企业知识库

---

# ? 阶段 6：深入原理（可选，1–3 个月）

### ? 目标

更深入理解底层机制，适合有兴趣或要研发底层系统的人。

### ? 需要学习内容

* Transformer 训练细节
* KV Cache 与加速
* FlashAttention
* 模型并行（DP / TP / PP / FSDP）
* vLLM 结构与 PagedAttention
* 优化器（AdamW / Lion）

### ? 推荐论文

* LLaMA 系列论文
* FlashAttention
* vLLM
* DeepSeek 技术报告

---

# ? 全路线图图示

```
阶段 0 环境准备  
→ 阶段 1 基础概念  
→ 阶段 2 推理部署  
→ 阶段 3 微调  
→ 阶段 4 RAG  
→ 阶段 5 应用开发  
→ 阶段 6 原理深入（可选）
```

---

# ? 最终你能掌握的能力

* ? 本地大模型部署
* ? 企业级 AI 应用开发
* ? 模型微调能力（QLoRA/SFT）
* ? RAG 知识库
* ? Agent 自动化
* ? 前后端 + 模型一体化工程能力

---

