下面给你一套**Windows 环境下，使用 Docker 安装部署 Dify（官方推荐方式）**的**完整且可靠步骤**，按这个走基本不会踩坑。
（适合 Windows 10 / 11）

---

## 一、环境准备（必做）

### 1️⃣ 系统要求

* Windows 10 22H2 / Windows 11（**必须支持 WSL2**）
* 至少 **8GB 内存**（推荐 16GB）
* 开启 CPU 虚拟化（BIOS 里开启 VT-x / SVM）

---

### 2️⃣ 安装 WSL2

以**管理员 PowerShell**运行：

```powershell
wsl --install
```

完成后**重启电脑**

检查版本：

```powershell
wsl -l -v
```

确保是：

```
VERSION 2
```

如果不是：

```powershell
wsl --set-default-version 2
```

---

### 3️⃣ 安装 Docker Desktop（Windows）

下载：
👉 [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)

安装时注意勾选：

* ✅ Use WSL 2 instead of Hyper-V

安装完成后：

* 启动 Docker Desktop
* Settings → Resources → WSL Integration
* 勾选你的 Linux 发行版（一般是 Ubuntu）

验证：

```powershell
docker --version
docker compose version
```

---

## 二、获取 Dify 源码（官方方式）

### 1️⃣ 打开 PowerShell 或 Windows Terminal

```powershell
cd D:\
git clone https://github.com/langgenius/dify.git
cd dify
```

> ⚠️ 路径建议 **不要放在 C:\Users 下**，避免 IO 性能问题

---

## 三、配置 Dify（关键步骤）

### 1️⃣ 进入 docker 目录

```powershell
cd docker
```

### 2️⃣ 复制环境配置文件

```powershell
copy .env.example .env
```

### 3️⃣ 编辑 `.env`（最小可用配置）

用 VS Code / Notepad++ 打开 `.env`，重点检查👇

```env
# 对外访问地址（本地部署这样写）
DIFY_WEB_URL=http://localhost
DIFY_API_URL=http://localhost

# 日志
LOG_LEVEL=INFO

# 数据存储
VECTOR_STORE=weaviate
```

👉 **不需要现在就配 OpenAI Key，可以部署完再配**

---

## 四、启动 Dify（核心）

### 1️⃣ 启动服务

在 `dify/docker` 目录执行：

```powershell
docker compose up -d
```

⏳ 第一次会拉镜像，耐心等 5–10 分钟

---

### 2️⃣ 确认所有容器正常

```powershell
docker ps
```

你应该看到类似：

* dify-api
* dify-web
* dify-worker
* postgres
* redis
* weaviate
* nginx

---

## 五、访问 Dify

### 1️⃣ 浏览器打开

```
http://localhost
```

### 2️⃣ 初始化管理员账号

第一次访问会提示：

* 创建管理员账号
* 设置邮箱 + 密码

完成后即可进入 Dify 控制台 🎉

---

## 六、配置模型（以 OpenAI 为例）

进入：

> Settings → Model Provider

填写：

* OpenAI API Key
* 模型：`gpt-4o-mini` / `gpt-4.1` 等

保存即可使用

---

## 七、常见问题（非常重要）

### ❌ 1. 端口冲突（80 被占用）

修改 `.env`：

```env
NGINX_PORT=8080
```

然后：

```powershell
docker compose down
docker compose up -d
```

访问：

```
http://localhost:8080
```

---

### ❌ 2. Docker 拉不下来镜像（国内网络）

建议：

* Docker Desktop → Settings → Resources → Proxies
* 配置代理（如 127.0.0.1:7890）

或使用镜像加速器（阿里云 / DaoCloud）

---

### ❌ 3. WSL 磁盘占用过大

```powershell
wsl --shutdown
```

---

## 八、目录结构说明（你作为程序员建议看）

```
dify/
 ├─ api        后端服务（FastAPI）
 ├─ web        前端（Next.js）
 ├─ worker     异步任务
 ├─ docker     Docker 编排核心
 └─ volumes    数据持久化
```

---

## 九、生产部署建议（给你一句重点）

> **Windows 仅适合开发 / 测试**
>
> 生产环境建议：

* Ubuntu 22.04
* Docker + Nginx
* 外置 PostgreSQL + Redis
* 对象存储（MinIO / OSS）

---

如果你需要👇

* **Windows + 内网访问**
* **Dify 接国产大模型（通义 / 智谱 / DeepSeek）**
* **私有化生产部署架构图**
* **把 Dify 接入你自己的系统（Java / React）**

你直接说一句：
👉「我现在是【开发 / 生产】环境，想接【什么模型】」
我给你一步到位方案。
