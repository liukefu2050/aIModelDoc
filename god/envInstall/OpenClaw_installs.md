
# 本地运行 OpenClaw 快速指南

本文档整理了 **OpenClaw 本地安装、配置、启动、关闭和再次启动** 的完整流程，适合第一次部署的用户。

---

# 运行引导向导（openclaw onboard）

Install (recommended)
Runtime: Node ≥22.
```
npm install -g openclaw@latest
# or: pnpm add -g openclaw@latest

openclaw onboard --install-daemon
```


## 1?? 模型选择

启动 OpenClaw 时会提示选择模型：


Default model
|  > Keep current (qwen-portal/coder-model)
|    Enter model manually
|    qwen-portal/coder-model
|    qwen-portal/vision-model

```

- **推荐选择**：`Keep current (qwen-portal/coder-model)`  
- 原因：
  - 默认代码模型
  - 兼容性最好
  - 适合本地调试、Chatflow、Agent
- **注意**：
  - `vision-model` 只用于图像识别，多数场景不需要
  - 手动输入模型名只在你有自定义模型时使用

---

## 2?? 选择 Channel

```

Select channel (QuickStart)
|    Telegram (Bot API)
|    WhatsApp (QR link)
|    Discord (Bot API)
|    Feishu (Lark Open Platform) (configured)
|    ...
|    Skip for now

```

- **推荐选择**：`Skip for now`
- 原因：
  - 本地运行无需对接外部 IM
  - 可以先测试 Chatflow / Agent 功能
  - 避免不必要的报错和配置麻烦

---

## 3?? Skills 配置

### ① Skills 状态

```

Skills status
|  Eligible: 3
|  Missing requirements: 47
|  Blocked by allowlist: 0

```

- **推荐操作**：`Yes`（配置技能）
- 原因：
  - 注册可用的 3 个基础技能
  - 不影响后续使用
  - Missing requirements 可以后续按需安装

### ② Node Manager

```

Preferred node manager for skill installs
|  > npm
|    pnpm
|    bun

```

- **推荐选择**：`npm`
- 原因：
  - 兼容性最好
  - 文档和安装脚本默认使用 npm
  - pnpm、bun 可能报错或不兼容

### ③ 安装缺失依赖

```

Install missing skill dependencies
|  [?] Skip for now
|  [ ] 1password
|  [ ] github
|  ...

```

- **推荐操作**：保持 `Skip for now`
- 原因：
  - 大部分技能需要外部 API Key 或系统依赖
  - 初次本地运行不必安装
  - 后续按需单独安装即可

### ④ API Key 配置

对于提示 `Set XXX_API_KEY for YYY?`：

- **GOOGLE_PLACES_API_KEY / GEMINI_API_KEY / LOCAL-PLACES**  
- **推荐选择**：`No`
- 原因：
  - 本地运行不需要
  - 避免配置失败或中断
  - 只有需要使用对应 skill 时才设置

---

## 4?? 启动 Bot（Hatch）

```

How do you want to hatch your bot?
|  > Hatch in TUI (recommended)
|    Open the Web UI
|    Do this later

```

- **推荐选择**：`Hatch in TUI`
- 原因：
  - 直接在终端运行
  - 不依赖浏览器
  - 最稳、最易调试
- 后续可选择：
  - `openclaw web` 启动浏览器 Web UI

---

## 5?? 启动成功验证

成功启动后，你会看到：

```

session agent:main:main
Wake up, my friend!
Hey. I just came online. Who am I? Who are you?
gateway connected | idle
agent main | session main (openclaw-tui) | qwen-portal/coder-model | tokens 0/128k (0%)

````

- 表示 OpenClaw 已成功运行
- Agent 已上线，可输入指令测试：
  ```text
  你是谁？
  用 3 步解释什么是 OpenClaw
````

---

## 6?? 关闭 OpenClaw

* 在 TUI 或 Web UI 下，**按 `Ctrl + C`**
* 安全停止，不会破坏配置
* 配置文件通常在 `~/.openclaw`，保持不变

---

## 7?? 再次启动 OpenClaw

### 使用 TUI（推荐）

```bash
启动
openclaw tui
重启gateway
openclaw gateway restart

```

* 重新加载上一次的 Agent 和模型
* 直接进入交互终端

### 使用 Web UI（可选）

```bash
openclaw web
```

* 打开浏览器可视化界面
* 默认地址示例：`http://localhost:3000`

---

## ? 小贴士

1. **API Key**：不需要立刻设置，按需再加
2. **技能**：初次运行可全跳过，后续需要再单独安装
3. **模型**：默认 `qwen-portal/coder-model` 足够使用
4. **Channel**：Skip for now → 本地跑最稳
5. **Node manager**：npm 最稳

---


