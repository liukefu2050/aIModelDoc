# 使用 ollama 运行deepseek

Windows环境下常见问题
休眠 / 锁屏 → Ollama 掉线
显卡驱动自动更新 → 模型崩
后台被系统调度限制

## 下载 Ollama

https://ollama.com

安装完成后，自动启动后台服务
默认监听：
http://localhost:11434

验证是否安装成功（PowerShell / CMD）：
ollama -v

### Ollama 中运行 DeepSeek


拉取 DeepSeek 模型
ollama pull deepseek-r1:7b
ollama pull deepseek-coder:6.7b
ollama pull qwen3:30b
ollama pull 在任何目录执行都可以下载模型，目录完全无关
Ollama 模型实际存放在哪里（Windows）
默认路径是：
C:\Users\<你的用户名>\.ollama\models


### 运行模型（命令行）

```
ollama run deepseek-r1:7b
ollama stop deepseek-r1:8b
```

###  常用方法
1️⃣ 使用 ollama list
ollama list


### Windows 下如何【关闭 Ollama 服务】
✅ 方法 1：系统托盘（最推荐）

右下角任务栏 → 显示隐藏图标

找到 Ollama 🦙

右键 → Quit Ollama

👉 这是最干净、安全的关闭方式
👉 会释放显存 / 端口 / 模型进程


###  如何【再次启动 Ollama】
✅ 方法 1：开始菜单（推荐）

开始菜单 → 搜索 Ollama

点击启动

✅ 方法 2：命令行启动
ollama serve
启动后监听：

http://localhost:11434
