# 使用 WSL2 (Ubuntu)

Windows 原生 Python 环境不完全支持 vLLM 的 CUDA 架构。
启用 WSL2

## 安装

```
wsl --install -d Ubuntu
```

### 使用本机网络代理

ipconfig 查看本机IP：192.168.15.113
在 WSL 内使用这个 IP 访问代理，例如：

export http_proxy=http://192.168.15.53:7890;export https_proxy=http://192.168.15.53:7890
$Env:http_proxy="http://192.168.15.53:7890";$Env:https_proxy="http://192.168.15.53:7890"

### 更新 Ubuntu 并安装依赖

进入 Ubuntu 终端（可以在 Windows 搜索 “Ubuntu” 打开）
```
sudo apt update && sudo apt upgrade -y
sudo apt install -y python3.10 python3.10-venv python3-pip git wget curl build-essential
```

python 安装问题
添加 Python 3.10 的官方 PPA 源
```
sudo apt update
sudo apt install -y software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa -y
sudo apt update

sudo apt install -y python3.10 python3.10-venv python3-pip

```

### 创建虚拟环境（推荐）

Python3.10 环境缺少 ensurepip（安装 pip 的标准库模块）
```
sudo apt install -y python3.10-venv python3.10-distutils python3.10-lib2to3 python3.10-dev
sudo apt install -y python3-pip
```
激活虚拟环境
```
python3.10 -m venv vllm-env
source vllm-env/bin/activate
验证：
python --version
应显示 Python 3.10.x
```
虚拟环境创建有时候会报错：比如在当前目录在 /mnt/d/...，也就是 Windows 的 NTFS 分区挂载路径
切换到 WSL 内部目录
```
cd ~
pwd
输出应类似 /home/liukefu
```
### 安装 CUDA 支持（如果你有 NVIDIA GPU）

在 Windows 确保安装了 NVIDIA 驱动
打开 NVIDIA 控制面板 → 查看驱动版本
确保版本 ≥ 525.xx（支持 CUDA 12）

2️⃣ 在 WSL2 中安装 CUDA 工具包（轻量版）
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo apt update
sudo apt install -y cuda-toolkit
```
验证：
nvidia-smi
应能看到你的 GPU 型号

### 安装 PyTorch + vLLM

安装匹配 CUDA 的 PyTorch（官方推荐 CUDA 12.1）：
```
pip install torch==2.3.0 torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```
安装 vLLM：
pip install vllm
验证：
python -m vllm.version
python -c "import vllm; print(vllm.__version__)"

### 下载并运行 DeepSeek 模型

示例（以 deepseek-coder-6.7b-base 为例）：
```
python -m vllm.entrypoints.openai.api_server --model deepseek-ai/deepseek-coder-6.7b-base

设置 HF 镜像
export HF_ENDPOINT=https://hf-mirror.com
python -m vllm.entrypoints.openai.api_server --model deepseek-ai/deepseek-coder-6.7b-base

```


📍 运行后输出类似：
```
INFO  Loaded model deepseek-ai/deepseek-coder-6.7b-base
INFO  Starting OpenAI-compatible API server at http://127.0.0.1:8000/v1
```

现在你可以通过浏览器访问：
👉 http://localhost:8000/docs

看到 Swagger API 页面 🎉

### 测试模型是否正常工作

在 WSL 里新开一个终端窗口，执行：
```
curl http://localhost:8000/v1/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "deepseek-ai/deepseek-coder-6.7b-base",
    "prompt": "Write a Python function that reverses a string",
    "max_tokens": 100
  }'
```
如果一切正常，会返回 JSON 格式的回复

###  WebUI 可视化界面

安装开源 WebUI（例如 Open WebUI）
```
pip install open-webui
open-webui serve
```

然后访问：
👉 http://localhost:3000

在设置里把 API 地址改为：
http://localhost:8000/v1
选择模型：deepseek-ai/deepseek-coder-6.7b-base

已经完成了：
Windows + WSL2 + Ubuntu 环境配置
GPU 加速的 vLLM 部署
本地 DeepSeek 模型运行 + OpenAI 接口 + WebUI 前端