# 🔄 Paykka Bot V17 — 恢复与记忆同步文档

> **用途**：新对话开始时，让 AI 读取本文件即可恢复工作环境与上下文记忆。
> **生成时间**：2026-09-26
> **项目**：Paykka 跨境支付清算系统 Bot（V17）

---

## 一、给新对话 AI 的指令（复制这段）

```
这是 Paykka Bot 项目的延续。请先读取本恢复文档了解全部上下文，
然后告诉我当前进展和下一步建议。完整代码备份在 GitHub 仓库
13058199807zk-oss/paykka-control 的 v17_backup/ 目录，
但沙箱可能连不上 GitHub，若连不上请让我手动提供代码包。
```

---

## 二、恢复步骤

### 方式 A：从 GitHub 恢复（网络通时）

```bash
git clone https://13058199807zk-oss:<TOKEN>@github.com/13058199807zk-oss/paykka-control.git
cd paykka-control/v17_backup
tar -xzf paykka_backup_v17_20260607.tar.gz
cp paykka_backup_v17/* /workspace/
```

### 方式 B：手动提供（网络不通时）

用户手动上传 `paykka_backup_v17_20260607.tar.gz` 到新对话，AI 解压到 `/workspace`。

### 环境依赖

```bash
pip3 install lark-oapi pyotp playwright pandas matplotlib openpyxl \
             requests schedule pytz PyGithub beautifulsoup4
playwright install chromium
```

---

## 三、项目架构

### 主控制器
- `paykka_controllerV17.py`（352KB）— V17 主控制器，含命令系统 + 飞书 Bot
- `paykka_controllerV16.py`（352KB）— V16 备份

### 核心模块（14个）
| 模块 | 作用 |
|------|------|
| `paykka_comparison.py` | **V17新增** 报价对比引擎 QuoteComparisonEngine |
| `paykka_competitor.py` | 友商汇率查询（XT/WF/LL/AW/KS/SK） |
| `paykka_config.py` | 全局配置（含所有密钥） |
| `paykka_core.py` | 核心工具（认证/HTTP/日志） |
| `paykka_history_quote.py` | 路由历史报价查询（OPS API，采样版） |
| `paykka_xt_rate.py` | XT 汇率管理（Cookie 认证） |
| `paykka_balance.py` | 余额查询 |
| `paykka_commands.py` | 命令处理 |
| `paykka_dedup.py` | 消息去重 |
| `paykka_exchange_rate.py` | 汇率监控 |
| `paykka_forex_config.py` | 汇差配置 |
| `paykka_fx_monitor.py` | 换汇交易监控 |
| `paykka_lark.py` | 飞书接口 |
| `paykka_rate_monitor.py` | 汇率监控 |
| `paykka_routing_config.py` | 路由配置 |
| `paykka_routing_sched.py` | 路由调度 |

### 守护进程
- `lark_bot_daemon.py` — 飞书 Bot 守护进程
- `routing_scheduler_daemon.py` — 路由调度守护进程
- `balance_monitor.py` — 余额监控

---

## 四、V17 核心新增：报价对比功能

### 命令格式
```
报价对比 [货币对] [天数] [渠道...] [友商...]
```

### 示例
```
报价对比                           → USD/CNH, 3天, YB+MDAQ+BOC+HCE + XT
报价对比 USDCNH 7D                 → USD/CNH, 7天
报价对比 NGNUSD 3D YB MDAQ         → NGN/USD, 3天, 仅YB+MDAQ
报价对比 USDCNH 7D XT WF           → 全部渠道 + XT+WF
报价对比 USD CNY 1D                → USD/CNY, 1天
```

### 实现要点（`paykka_comparison.py`）
- `QuoteComparisonEngine.run_comparison(base, quote, days, channels, competitors)`
- 渠道查询：`PaykkaHistoryQuoteQuery`（OPS API，采样整点/半点）
- 友商查询：`CompetitorHistoryQuery`（XT/WF/LL API）
- 字段标准化：`release_time`→`time`（UTC→北京时间+8），`bid_rate`→`rate`
- 输出：matplotlib 多源叠加图 + pandas 3-sheet Excel + JSON 原始数据
- 文本汇总末尾附 `[CHART:path]` 标记，由 `handle_message` 自动发图

---

## 五、XT 认证机制

| 项目 | 说明 |
|------|------|
| 登录方式 | Playwright + xvfb-run + headless=False 绕过 CAPTCHA |
| 认证类型 | **Cookie 认证**（非 JWT Bearer） |
| JWT 位置 | Cookie 的 `token` 字段（`eyJ...`格式） |
| 缓存文件 | `xt_auth_cache.json`（cookie_str/xsrf_token/server_grant_id） |
| 历史API | `/api/v1/spotfx/graph/present-rate/v2/getWithSource` |
| 参数 | `fromCcy, toCcy, type=WEEK/MONTH, siteCode=CN` |
| 返回结构 | `sourceRatePointList.SPOT[{rateTime, rate, fromCcy, toCcy, source}]` |
| 重新登录 | `xvfb-run --auto-servernum python3.11 xt_stealth_login.py` |

### ⚠️ 关键发现
- **XT 的 getWithSource API 不支持 CNH，仅返回 CNY（在岸人民币）**
- USD/CNH 和 CNH/USD 均返回空 `sourceRatePointList`
- CNH/CNY 基差约 100-200 pips，属正常市场现象
- headless=True 会触发 geetest CAPTCHA，必须 headless=False

---

## 六、竞品集成状态

| 友商 | 状态 | 备注 |
|------|------|------|
| XT | ✅ 已适配 | 16货币对，实时+历史，Cookie认证 |
| AW | ✅ 已适配 | clientRate 取倒数 |
| WF | ⚠️ 部分 | 历史数据可用 |
| LL | ⚠️ 部分 | — |
| KS | ⚠️ 待优化 | — |
| SK | ⚠️ 待优化 | — |

---

## 七、命令系统架构

### 流程
```
用户消息 → LarkBotHandler.handle_message()
  → MessageDeduplicator.is_processed() 去重
  → processor.process(text)
    → parse_command(text) 解析
      → 精确匹配 self.commands
      → _fuzzy_match_command(text) 模糊匹配
    → handler(args) 执行
      → safe_execute(impl, args) 安全包装（重试2次）
  → 检测 [CHART:path] → reply_image() 发图
```

### V17 新增命令
```python
"报价对比": self.handle_rate_comparison
# 别名: 报价对比, 汇率对比, 多方对比, 渠道对比, 渠道友商对比, 友商渠道对比
# 参数提取: _extract_comparison_args(text) → (pair, days, channels, competitors)
```

---

## 八、配置速查（密钥见 paykka_config.py）

| 配置 | 值 |
|------|-----|
| 飞书 APP_ID | `cli_a92121fea1f85bc9` |
| 飞书 APP_SECRET | 见 paykka_config.py |
| GitHub 仓库 | `13058199807zk-oss/paykka-control` |
| GitHub Token | 见 paykka_config.py |
| 企业微信 Webhook | 见 paykka_config.py |
| OPS API | `https://ops-bk.cb.paykka.com` |

---

## 九、待办清单

- [x] 四路渠道 vs XT 报价对比
- [x] 报价对比功能集成到 V17 命令系统
- [ ] 完善 WF/LL/KS/SK 历史汇率查询
- [ ] 报价对比命令的更多货币对测试
- [ ] OCBC 退款/争议邮件 AI 自动化
- [ ] M-DAQ 汇率调整通知自动化
- [ ] 跨境支付资金流预测系统 V1.0（10类画像/8币种/4模型）
- [ ] 试用期转正汇报材料

---

## 十、上一轮对比测试结果（USD/CNH, 3天）

| 来源 | 数据量 | 报价区间 | 类型 |
|------|--------|----------|------|
| YB | 145条 | 6.7561~6.7899 | USD/CNH |
| MDAQ_FX_TOM | 144条 | 6.7643~6.7903 | USD/CNH |
| BOC | 54条 | 6.7535~6.7791 | USD/CNH |
| HCE_FX_TOM | 145条 | 6.7648~6.7908 | USD/CNH |
| XT | 69条 | 6.7186~6.7671 | USD/CNY |

---

## 十一、项目背景

- **用户**：郑凯（xgd.com 清算部，资金运营岗）
- **职责**：商户跨境资金管理、头寸管理、清算保障
- **当前阶段**：试用期转正汇报准备
- **偏好**：中文、简洁直接、结构化输出、V版本迭代、增量验证、安全意识强（避免敏感材料上传公网）

---

*本文件由 Paykka Bot V17 环境自动生成，用于跨对话记忆同步。*
