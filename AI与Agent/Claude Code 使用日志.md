# Claude Code 使用日志

> 记录每次 Claude Code 会话的需求、操作、改动及结果。

---

## 2026-05-11 — A股银行股量化回测系统三项增强功能实现

### 需求

在 `D:\cc project` 的回测程序中实现三个增强功能：
1. **ATR动态阈值**：根据市场波动率（ATR）动态调整买入/卖出/止损阈值
2. **银行板块择时**：在银行板块明显高估/低估时调整仓位比例
3. **多因子轮动**：在银行股内根据 PB/ROE/股息率等因子动态轮动

该项目之前已有部分实现（在别的对话中做了一部分），但因上下文不够丢弃，需继续完成。

### 调研结果

通读全部源码（9个文件，约2000行）后发现：

| 功能 | 状态 | 说明 |
|------|------|------|
| ATR动态阈值 | ✅ 已实现 | `config.py` + `strategy.py:simulate_daily_t0` 中完整实现 |
| BK0475市场择时 | ⚠️ 代码已有但未接线 | `_get_timing_cash_ratio()` 逻辑正确，但 `bk0475_data` 参数始终为 `None` |
| PB/ROE/股息率因子轮动 | ❌ 仅价格代理因子 | `_compute_factor_weights` 只用动量、波动率、1/price 三个价格派生因子 |

### 改动详情

#### 1. `bank_backtest/data_loader.py` — 新增基本面数据获取 (~200行)

- `fetch_bank_fundamentals()` → 从东方财富 `datacenter.eastmoney.com` API 拉取季度财报（ROE、BPS）和分红历史
- `_fetch_financial_reports()` → 调 `RPT_LICO_FN_CPD` 报表接口
- `_fetch_dividend_history()` → 调 `RPT_SHAREBONUS_DET` 分红接口
- `_build_fallback_fundamentals()` → API 失败时用银行板块均值（ROE 11%, BPS年增8%, 股息率4.5%）构建回退数据
- `merge_fundamentals_to_prices()` → `pd.merge_asof` 按交易日向前填充基本面数据

#### 2. `bank_backtest/strategy.py` — 因子模型重构

- `set_fundamentals()` → 注入基本面数据
- `_compute_factor_weights()` 从 3 因子改为 **5 因子**：
  - PB估值（真实 BPS/price，低PB得分高）
  - ROE质量（季度财报加权平均ROE TTM）
  - 股息率（DPS/price）
  - 动量（126日累计收益）
  - 低波动（-252日波动率）
- z-score 改用 **MAD（绝对中位差）稳健缩放**，替代 std

#### 3. `bank_backtest/backtest_engine.py` — 数据管道

- `__init__` 增加 `fundamentals` 参数，自动注入策略
- `run_backtest()` 便捷函数同步更新

#### 4. `main.py` — 主入口接线

- `load_data()` 返回值从单个 DataFrame 扩展为 `(stock_data, bk0475_data, fundamentals)` 三元组
- 回测和敏感性分析均传入 BK0475 + 基本面数据
- `--quick` / `--sensitivity` / `--report` 三种模式均适配

#### 5. `bank_backtest/sensitivity.py` — 敏感性分析全链路

- 6 个函数全部增加 `bk0475_data` / `fundamentals` 参数
- 新增 `_filter_bk0475()` / `_filter_fundamentals()` 按子窗口裁剪

#### 6. `bank_backtest/config.py` — 参数微调

- `stop_atr_multiplier` 从 0.3 → 0.5（止损阈值更合理）

#### 7. `bank_backtest/report.py` — 报告增强

- 6.3 节从简单 checkbox 改为三个详细子章节（6.3.1 / 6.3.2 / 6.3.3），每个功能 4-7 行公式+说明
- 6.4 保留为真正的未来优化建议

### 执行验证

- 语法编译：8/8 文件通过
- 快速回测 (`python main.py --quick`)：跑通，约2分钟
- 报告输出：`research_report.md` 时间戳更新为 22:43
- 图表输出：5张图全部刷新
- 日志零 error/traceback
- 回测结果：累计 98.91%，年化 10.74%，夏普 0.44，最大回撤 20.25%

### 用户反馈与修正

1. **"报告时间怎么是旧的"** → 只改源码未重新跑，重跑 `python main.py --report`
2. **"怎么报告只打勾没有说明"** → 修改 `report.py`，把 checkbox 替换为三个详细章节
3. **"`[x]` 是没实现吗"** → 解释 `[x]` 是 Markdown 勾选/已完成，`[ ]` 才是未实现
4. **记忆点1**：以后改完 Code 必须跑一遍 → 自检 → 审计 → 纠错
5. **记忆点2**：收到"收工"才写日志到 Obsidian，同时更新索引

### 涉及文件清单

| 文件 | 操作 |
|------|------|
| `bank_backtest/data_loader.py` | 追加 ~200 行基本面数据模块 |
| `bank_backtest/strategy.py` | 重写 `_compute_factor_weights` + 新增 `set_fundamentals` |
| `bank_backtest/backtest_engine.py` | `__init__` + `run_backtest` 参数扩展 |
| `bank_backtest/config.py` | `stop_atr_multiplier` 微调 |
| `bank_backtest/sensitivity.py` | 全链路 6 函数参数扩展 + 2 个过滤辅助函数 |
| `bank_backtest/report.py` | 6.3 节改为三个详细子章节 |
| `main.py` | `load_data` 返回值扩展 + 引擎调用透传 |

---

*此日志由 Claude Code 在"收工"时自动生成*
