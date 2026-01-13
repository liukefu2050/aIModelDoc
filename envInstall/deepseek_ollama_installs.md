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

### 运行模型（命令行）

```
ollama run deepseek-r1:7b
```
