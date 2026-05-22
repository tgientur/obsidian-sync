# OpenClaw 飞书集成

> 飞书 Bot 配置、配对、测试命令汇总

---

## 飞书 App 凭证

```
App ID:     cli_a97896c38ef8dcc3
App Secret: shZFBiQ7ugzFtG0Wms9BThgkkocGzinV
```

## 配置命令

```bash
# 填入飞书 App ID 和 Secret
openclaw config set channels.feishu.appId "cli_a97896c38ef8dcc3"
openclaw config set channels.feishu.appSecret "shZFBiQ7ugzFtG0Wms9BThgkkocGzinV"
openclaw config set channels.feishu.enabled true

# 设为 WebSocket 长连接模式（推荐）
openclaw config set channels.feishu.connectionMode websocket

# 私聊策略：首次需配对确认
openclaw config set channels.feishu.dmPolicy pairing

# 群聊策略：仅白名单群可用
openclaw config set channels.feishu.groupPolicy allowlist

# 群聊中必须 @ 机器人才回复
openclaw config set channels.feishu.requireMention true

# 重启网关使配置生效
openclaw gateway restart
```

## 测试命令

```bash
# 配对飞书用户
openclaw pairing approve feishu P3YEQYM3

# 发送测试消息
openclaw message send --channel feishu --to "用户995146" --message "测试消息"

# 发送测试 Webhook
curl -X POST -H "Content-Type: application/json" \
  -d '{"msg_type":"text","content":{"text":"Hello! TrendRadar 测试消息"}}' \
  "https://open.feishu.cn/open-apis/bot/v2/hook/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
```

## DeepSeek API 测试

```bash
curl -s -o /dev/null -w "HTTP状态码: %{http_code}\n" \
  https://api.deepseek.com/v1/models \
  -H "Authorization: Bearer sk-5e658196d6024561aa5f4f7f6a737516"

# 列出模型
curl https://api.deepseek.com/v1/models \
  -H "Authorization: Bearer sk-5e658196d6024561aa5f4f7f6a737516"
```
