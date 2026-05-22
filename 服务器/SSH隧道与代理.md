# SSH 隧道与代理

> 通过本地 Clash 代理为云服务器提供网络访问能力（反向隧道方案）

---

## 前提条件

- 本地电脑已安装 OpenSSH 客户端
- 本地 Clash 已开启，且 **Allow LAN** 已打开
- 服务器的 SSH 端口（22 或自定义端口）可正常连接

---

## 第一步：确认本地 Clash 端口

Clash HTTP 代理端口通常为 `7890`，确认 Allow LAN 已开启。

## 第二步：建立反向隧道（本地 PowerShell）

```powershell
# 基本命令
ssh -NfR 7890:localhost:7890 -p 22 root@服务器IP

# 推荐：加心跳包防止断连
ssh -o ServerAliveInterval=60 -NfR 7890:localhost:7890 -p 22 root@服务器IP
```

| 参数 | 作用 |
|------|------|
| `-N` | 不执行远程命令 |
| `-f` | 后台运行 |
| `-R 7890:localhost:7890` | 服务器 7890 → 本地 7890 |
| `-p 22` | 服务器 SSH 端口 |
| `-o ServerAliveInterval=60` | 每 60 秒发心跳包 |

> ⚠️ 保持 PowerShell 窗口不关闭

## 第三步：服务器上验证隧道

```bash
# 查看端口监听
netstat -tlnp | grep 7890
# 期望输出: tcp  0  0 127.0.0.1:7890  0.0.0.0:*  LISTEN

# 测试代理
curl -x http://127.0.0.1:7890 -I https://www.google.com
# 期望输出: HTTP/1.1 200 Connection established
```

## 第四步：设置服务器环境变量

```bash
# 临时设置
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890

# 永久生效
echo 'export http_proxy=http://127.0.0.1:7890' >> ~/.bashrc
echo 'export https_proxy=https_proxy=http://127.0.0.1:7890' >> ~/.bashrc
source ~/.bashrc
```

## 第五步：Docker 配置代理

```bash
sudo mkdir -p /etc/systemd/system/docker.service.d

sudo tee /etc/systemd/system/docker.service.d/proxy.conf <<-'EOF'
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:7890"
Environment="HTTPS_PROXY=http://127.0.0.1:7890"
Environment="NO_PROXY=localhost,127.0.0.1,.local"
EOF

sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 各服务器快捷隧道

```bash
# 阿里云
ssh -NfR 7890:localhost:7890 -p 22 root@47.80.12.193

# 腾讯云
ssh -NfR 7890:localhost:7890 -p 22 root@150.158.3.58
```

## 常见问题

| 问题 | 原因 | 解决 |
|------|------|------|
| `Connection refused` | Clash 未开或端口不对 | 检查 Clash 端口和 Allow LAN |
| `Connection timed out` | 安全组未放行 | 检查云服务器安全组 22 端口 |
| `curl: (35) SSL_ERROR_SYSCALL` | Clash 节点问题 | 切换 Clash 节点重试 |
| `Connection reset by peer` | 隧道通但 Clash 侧拒绝 | Allow LAN 没开，或 SSH 转发被视为外部连接。**Windows 本机**打开 Clash 面板 → Settings → Allow LAN（允许局域网连接），然后重试 `curl -x http://127.0.0.1:7890 -I https://www.baidu.com` |

## 注意：Windows SSH 隧道的 Connection reset

现象：`ssh -R` 隧道建成功了（服务器 7890 已监听），但 `curl -x` 报 `Recv failure: Connection reset by peer`。

根因：Windows 上的 OpenSSH 把从远程转发回来的连接视为外部连接，Clash 的 Allow LAN 未开启时会直接 reset。

解决：**本机 Clash** 打开 Allow LAN 开关，重启 Clash 即可。

```bash
# 隧道建好后的验证命令（服务器上执行）
curl -x http://127.0.0.1:7890 -I https://www.baidu.com
# 正常返回 HTTP/1.1 200 才算通
```

## OpenHanako 服务器部署备忘

**腾讯云 150.158.3.58**，用户 ubuntu，密码登录。

### 关键修复：launch.js 覆盖 HANA_HOME

`scripts/launch.js` 里写死了 `process.env.HANA_HOME = join(homedir(), ".hanako-dev")`，导致 server 启动时读的是 `.hanako-dev` 而非 `.hanako`。

**修复：创建软链**
```bash
rm -rf ~/.hanako-dev && ln -s ~/.hanako ~/.hanako-dev
```
这样 `npm run server` 和 `pm2 start` 都能读到正确的配置。

### 代码修改

- `core/agent-manager.js`：`log.error` → `log`（日志函数不存在的问题）
- `core/model-manager.js`：`syncAndRefresh()` 中去掉 `if (changed)` 条件，强制刷新 registry

### PM2 保活

```bash
cd ~/openhanako
HANA_HOME=/home/ubuntu/.hanako pm2 start npm --name hanako -- run server
pm2 save
pm2 startup systemd -u ubuntu
```

### 微信 bridge
配置文件 `~/.hanako/agents/hanako/config.yaml` 中已有 WeChat iLink 配置，启动后自动连接。

### 验证
- Server 端口：`14500`
- 微信发消息给 bot 测试回复
- 日志：`pm2 logs hanako`
