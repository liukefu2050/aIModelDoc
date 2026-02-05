# Windows（WSL2）+ Docker 跑 Dify 

# 一、前置条件（先确认）

### 1️⃣ Docker Desktop

* 已安装
* **使用 WSL 2**
* **Linux Containers 模式**

验证：

```bat
docker info | findstr OSType

docker --version
docker-compose --version

```

必须是：

```
OSType: linux
```

---

### 2️⃣ 建议目录（很重要）

放在 **纯英文路径**，不要中文：

```
D:\dev\dify
```

---

# 二、获取 Dify 源码（官方仓库）

```bat
cd D:\dev
git clone https://github.com/langgenius/dify.git
cd dify
```

---

# 三、进入 Docker Compose 目录

Dify 已经帮你写好了 compose 文件：

```bat
cd docker
```

你现在目录结构应该是：

```
dify/
 └─ docker/
     ├─ docker-compose.yml
     ├─ .env.example
```

---

# 四、配置环境变量（必须一步）

### 1️⃣ 复制环境配置

```bat
copy .env.example .env
cp .env.example .env  
```
windows 本地运行需要改
```
CONSOLE_API_URL=http://localhost
```
# 五、一条命令启动 Dify 🚀

```bat
docker compose up -d

关闭
docker compose down
```

首次启动会：

* 拉 **PostgreSQL / Redis / Weaviate**
* 构建 dify-api / dify-web
* 启动 7～9 个容器（正常）

⏳ 第一次 **5–10 分钟**，耐心等

---

# 六、确认是否启动成功

### 1️⃣ 看容器状态

```bat
docker compose ps
```

所有服务状态应为：

```
Up
```

---

### 2️⃣ 看日志（推荐）

```bat
docker compose logs -f api
```

看到类似：

```
Application startup complete
```

说明 API 启动成功。

---

# 七、打开 Dify 控制台

浏览器访问：

```
http://localhost:3000
```
访问不了的。需要docker-compose.yaml 文件web 服务下加上端口映射

```
version: "3.9"

services:
  web:
    image: langgenius/dify-web:1.12.0
    restart: always
    environment:
      NODE_ENV: production
    ports:
      - "3000:3000"

  api:
    image: langgenius/dify-api:1.12.0
    restart: always
    environment:
      NODE_ENV: production
    ports:
      - "5001:5001"

  redis:
    image: redis:6-alpine
    restart: always
    ports:
      - "6379:6379"

  db_postgres:
    image: postgres:15-alpine
    restart: always
    environment:
      POSTGRES_PASSWORD: example
    ports:
      - "5432:5432"

```

# 你的环境变量
ports:
- "3000:3000"   # ← 这一行必须加

首次会进入：

👉 **管理员账号初始化页面**

设置：

* 管理员邮箱
* 密码

---

# 八、配置模型（第一次必做）

登录后：

1. Settings → **Model Provider**
2. 选择：

    * OpenAI / Azure / 通义 / 智谱 / Ollama 等
3. 填 API Key
4. 保存

---

# 九、常用 Dify Docker 命令（记住这几个）

```bat
docker compose up -d        # 启动
docker compose down         # 停止
docker compose restart      # 重启
docker compose logs -f api  # 看 API 日志
```

---

# 十、常见问题速解（很重要）

### ❌ 端口打不开

* 检查端口占用
* 或改 `.env` 里的端口

---

### ❌ 启动卡很久

正常，第一次要拉很多镜像
用下面看进度：

```bat
docker ps
```

---

### ❌ 国内拉镜像慢

建议：

* Docker Desktop 配 **镜像加速**
* 或用代理（你前面已经在用）

---

## 十一、你现在这套 Dify 架构是这样的

```
浏览器
  ↓
dify-web (3000)
  ↓
dify-api (5001)
  ↓
PostgreSQL / Redis / Weaviate
```

---

## 十二、下一步我可以直接帮你做的

结合你前面 **OpenClaw + Docker** 的情况，我可以继续给你：

1️⃣ **Dify + Ollama（本地模型）完整方案**
2️⃣ **Dify 接入企业微信 / 飞书 / 钉钉**
3️⃣ **Dify + OpenClaw 联动架构**
4️⃣ **Dify 生产环境（Nginx + 域名 + HTTPS）**

你只要说一句：
👉 **“下一步做 X”**
