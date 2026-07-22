### wsl --install

以**管理员 PowerShell**运行：

```powershell
wsl --install
```

###  安装

```
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
source ~/.bashrc   # or: source ~/.zshrc
hermes             # Start chatting!
```

### 常用命令

hermes-agent 使用
```
wsl -d Ubuntu
export http_proxy="http://192.168.15.88:7897";export https_proxy="http://192.168.15.88:7897";export ALL_PROXY="http://192.168.15.88:7890"
hermes # 开始聊天 交互式 CLI — 发起对话
hermes model # 选择您的 LLM 提供商和模型
hermes tools # 配置要启用的工具
hermes config set # 设置各个配置值
hermes gateway # 启动消息网关（Telegram、Discord 等）
hermes setup # 运行完整的设置向导（一次性配置所有内容）
hermes claw migrate # 从 OpenClaw 迁移（如果来自 OpenClaw）
hermes update # 更新到最新版本
hermes doctor # 诊断任何问题
```

### 设置代理脚本

```
# 1. 定义配置块，增加唯一标识符防止重复写入
PROXY_BLOCK_START="# >>> WSL PROXY CONFIG >>>"
PROXY_BLOCK_END="# <<< WSL PROXY CONFIG <<<"

# 2. 清理旧配置，防止多次执行导致 .bashrc 变得臃肿
sed -i "/$PROXY_BLOCK_START/,/$PROXY_BLOCK_END/d" ~/.bashrc

# 3. 写入新配置（使用更加健壮的 IP 获取方式）
cat << EOF >> ~/.bashrc
$PROXY_BLOCK_START
# 动态获取宿主机 IP：先尝试路由表，如果失败则尝试 resolv.conf
export hostip=\$(ip route show | grep default | head -n 1 | awk '{print \$3}')
if [ -z "\$hostip" ]; then
export hostip=\$(grep nameserver /etc/resolv.conf | awk '{print \$2}')
fi

export PROXY_PORT=7897
export http_proxy="http://\$hostip:\$PROXY_PORT"
export https_proxy="http://\$hostip:\$PROXY_PORT"
export all_proxy="socks5://\$hostip:\$PROXY_PORT"

# 必须配置 NO_PROXY，防止本地开发流量走代理
export NO_PROXY="localhost,127.0.0.1,::1,*.local,192.168.*,10.*,172.16.*,172.17.*,172.18.*,172.19.*,172.20.*"
$PROXY_BLOCK_END
EOF

echo "本地开发白名单（NO_PROXY）已自动配置。"
echo "WSL2 网络代理已永久配置完成。"
echo "当前 Windows 宿主机 IP: 172.23.48.1"
echo "本地开发白名单（NO_PROXY）已自动配置。"
```

```
sed -i '/WSL PROXY CONFIG/,+50d' ~/.bashrc

cat << 'EOF' >> ~/.bashrc

# >>> WSL PROXY CONFIG (STATIC) >>>

export PROXY_HOST=192.168.15.88
export PROXY_PORT=7897

export http_proxy="http://$PROXY_HOST:$PROXY_PORT"
export https_proxy="http://$PROXY_HOST:$PROXY_PORT"
export all_proxy="socks5://$PROXY_HOST:$PROXY_PORT"

export NO_PROXY="localhost,127.0.0.1,::1,*.local,192.168.*,10.*,172.*"

# <<< WSL PROXY CONFIG <<<

EOF
```
source ~/.bashrc
echo $http_proxy

curl ipinfo.io
curl -I https://www.google.com