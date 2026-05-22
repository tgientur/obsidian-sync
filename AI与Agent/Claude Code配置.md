# Claude Code 配置

> 使用 DeepSeek 作为 Claude Code 后端的配置

---

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_AUTH_TOKEN": "sk-f15b65b64ae84f39b160efeb19e49874",
    "ANTHROPIC_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-v4-pro[1m]",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-v4-flash",
    "CLAUDE_CODE_EFFORT_LEVEL": "max"
  },
  "theme": "dark-daltonized"
}
```

### 关键点

- **Base URL** 指向 DeepSeek 的 Anthropic 兼容接口
- 主模型：`deepseek-v4-pro`（Opus/Sonnet 级别）
- 子 Agent 模型：`deepseek-v4-flash`（轻量任务）
- 思考深度（Effort Level）：`max`
- 主题：暗色高对比度（daltonized）
