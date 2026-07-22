
# OpenCode 本地安装与快速启动指南

OpenCode 是一个开源 AI 编程智能体 (AI coding agent)，类似 Claude Code，但更加开放和灵活。:contentReference[oaicite:0]{index=0}

---

##  一、安装依赖

### 1 Node.js（可选）
- 部分安装方式需要 Node.js（Windows/macOS/Linux）
- 推荐安装 Node.js 最新 LTS

---

##  二、安装 OpenCode

OpenCode 提供多种安装方式，可以按系统和习惯选择。

---

###  官方一键安装脚本（推荐）

```bash
curl -fsSL https://opencode.ai/install | bash
````

这个脚本自动检测操作系统/架构，下载对应版本并添加到 PATH。([Opencode][1])

---

###  通过包管理器安装

####  macOS / Linux

```bash
brew install anomalyco/tap/opencode
```

> 官方推荐的 Homebrew 官方源比普通 `brew install` 更及时。([OpenCode][2])

---

####  Windows

可通过多种方式：

还可以使用 npm：

```bash
npm install -g opencode-ai@latest
```

 Node 方式需要先安装 Node.js。([Opencode][1])

---

####  Linux 发行版包

部分发行版可用：

```bash
paru -S opencode-bin
```

也可下载 `.deb`, `.rpm`, AppImage 等安装包。([OpenCodex][3])

---

####  Docker 启动（沙盒环境）

```bash
docker run -it --rm ghcr.io/anomalyco/opencode
```

适合实验或独立环境。([Opencode][1])

---

## 用配置文件 配置 apiKey

```bash
%USERPROFILE%\.config\opencode\opencode.json
~/.config/opencode/opencode.json
```

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "openai": {
      "npm": "@ai-sdk/openai",
      "name": "OpenAI",
      "options": {
        "apiKey": "sk-xxxx"
      }
    }
  }
}
```

##  三、启动 OpenCode

安装完成后，在终端中运行：

```bash
opencode
```

---

##  四、首次启动与配置

首次运行时，你可能会看到一个欢迎向导或一些提示：

1. 选择登录方式
   使用 GitHub 或 OpenCode 账号登录（可选）
2. 选择或添加 AI 模型
   OpenCode 支持多种模型，可用内置试用模型或配置自己的 API Key
3. 导入现有配置（可选）
   如果之前使用过 VS Code 插件等，可导入设置。([OpenCodex][4])

---

##  五、基本使用

在 OpenCode 终端中输入命令：

```
/help
```

可以查看帮助菜单。

使用类似 CLI 的交互方式执行任务，例如：

```
/run "创建一个简单的 Python Web 服务器"
```

---

##  六、高级用法

###  切换 Agent

OpenCode 内置两个 Agent：

* `build`：默认模式，可对代码执行写入、修改等操作
* `plan`：只读模式，用于分析和规划工作（通过 Tab 键切换）([GitHub][5])

---

###  配置模型 Provider

创建或编辑配置文件：

```bash
~/.config/opencode/opencode.json
```

示例：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "openai": {
      "npm": "@ai-sdk/openai",
      "name": "OpenAI",
      "options": {
        "apiKey": "你的APIKEY"
      }
    }
  }
}
```

---

##  七、桌面与 Web 版本（可选）

OpenCode 支持：

* 桌面应用（Beta）
* Web 界面（TUI 以外的可视化体验）

可在官网或 Releases 页面下载对应平台版本（macOS, Windows, Linux）。([Opencode][1])

---

##  八、常见问题与排错

如果无法启动：

* 删除旧配置文件夹后重新安装
* 使用命令查看日志：

```bash
opencode --log-level DEBUG
```

日志通常位于：

* macOS/Linux: `~/.local/share/opencode/log/`
* Windows: `%USERPROFILE%\.local\share\opencode\log/` ([Open Code][6])

---

##  九、小贴士

* OpenCode 是 MIT 协议开源项目，不依赖特定提供商。([GitHub][5])
* 支持多种模型和本地模型（如 Ollama、LM Studio）配置。
* 可将项目中配置文件纳入版本管理，与团队共享。([CN-SEC][7])

---

##  参考资源

* GitHub 仓库： [https://github.com/anomalyco/opencode](https://github.com/anomalyco/opencode) ([GitHub][5])
* 官方文档： [https://opencode.ai/docs/](https://opencode.ai/docs/) ([OpenCode][2])
* 中文快速入门指南： 多篇社区文章与教程（参见搜索结果）


##  本地代码启动open code 

window bun 环境不兼容
```bash

Remove-Item -Recurse -Force node_modules
Remove-Item bun.lock
bun -v
bun install
bun dev serve --hostname=127.0.0.1 --port=4096
```

##  wsl本地代码启动open code
