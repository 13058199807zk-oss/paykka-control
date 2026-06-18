# Paykka Bot V17 - 完整状态快照
# 生成时间: 2026-06-07 23:10 GMT+8
# 沙箱 ID: 27d4a8a8-0c29-4352-993e-ca15c2da56d3

## 一、核心架构

### 主控制器
- V17 最新: paykka_controllerV17.py (352KB)
- V16 备份: paykka_controllerV16.py (352KB)

### V17 新增模块
- paykka_comparison.py — 渠道与友商报价对比引擎 QuoteComparisonEngine

### 14个核心模块（V17模块化架构）
paykka_controllerV17.py  主控制器（命令系统 + 飞书Bot）
paykka_comparison.py      报价对比引擎（V17新增）
paykka_competitor.py      友商汇率查询（XT/WF/LL/AW/KS/SK）
paykka_config.py          全局配置
paykka_core.py            核心工具（认证/HTTP/日志）
paykka_history_quote.py   路由历史报价（OPS API，采样版）
paykka_xt_rate.py          XT汇率管理（Cookie认证）
paykka_balance.py          余额查询
paykka_commands.py         命令处理
paykka_dedup.py            消息去重
paykka_exchange_rate.py    汇率监控
paykka_forex_config.py     汇差配置
paykka_fx_monitor.py       换汇交易监控
paykka_lark.py             飞书接口
paykka_rate_monitor.py     汇率监控
paykka_routing_config.py   路由配置
paykka_routing_sched.py    路由调度

### 守护进程
lark_bot_daemon.py         飞书Bot守护进程
routing_scheduler_daemon.py 路由调度守护进程
balance_monitor.py          余额监控

### 外部通知
wecom_notify.py             企业微信Webhook通知

## 二、报价对比功能（V17核心新增）

### 命令格式
报价对比 [货币对] [天数] [渠道...] [友商...]

### 示例
报价对比                           → USD/CNH, 3天, YB+MDAQ+BOC+HCE + XT
报价对比 USDCNH 7D                 → USD/CNH, 7天
报价对比 NGNUSD 3D YB MDAQ         → NGN/USD, 3天, 仅YB+MDAQ
报价对比 USDCNH 7D XT WF           → 全部渠道 + XT+WF
报价对比 USD CNY 1D                → USD/CNY, 1天

### 核心类: QuoteComparisonEngine (paykka_comparison.py)
- run_comparison(base, quote, days, channels, competitors)
- _query_channels() → PaykkaHistoryQuoteQuery (OPS API)
- _query_competitors() → CompetitorHistoryQuery (XT/WF/LL API)
- _normalize_channel_record() → UTC→北京时间转换
- _generate_chart() → matplotlib多源叠加图
- _export_excel() → pandas 3-sheet Excel
- _format_summary() → 文本汇总 + [CHART:path]标记

### 输出
/workspace/logs/charts/compare_*_*_*d_*.png
/workspace/logs/compare_*_*d_*.xlsx
/workspace/logs/compare_raw_*.json

### 已知限制
- XT getWithSource API 不支持 CNH，仅返回 CNY（在岸人民币）
- CNH/CNY 基差约 100-200 pips，属正常市场现象
- 渠道 OPS API 单次最大3天，自动分段查询

## 三、XT 认证机制

### 登录方式
使用 Playwright + xvfb-run + headless=False 绕过 CAPTCHA
xt_stealth_login.py — 隐身登录脚本

### 认证缓存: xt_auth_cache.json
- cookie_str: 完整 Cookie 字符串（含 token/refreshToken/XSRF-TOKEN）
- xsrf_token: XSRF 防护令牌
- server_grant_id: 服务端授权ID
- jwt_token: JWT（实际存于 Cookie 的 token 字段中）

### API 端点
getWithSource: /api/v1/spotfx/graph/present-rate/v2/getWithSource
  参数: fromCcy, toCcy, type=WEEK/MONTH, siteCode=CN
  返回: sourceRatePointList.SPOT[{rateTime, rate, fromCcy, toCcy, source}]

### 关键发现
- XT 使用 Cookie 认证（非 JWT Bearer）
- JWT 存在 Cookie 的 'token' 字段（eyJ...格式）
- xvfb-run 显示英文登录页（"Login"按钮），有 DISPLAY 显示中文（"登录"）
- headless=True 会触发 geetest CAPTCHA，必须 headless=False

## 四、竞品汇率集成状态

✅ XT — 已适配，支持16货币对（实时+历史），Cookie认证
✅ AW — 已适配，clientRate取倒数
⚠️ WF — 已部分适配，历史数据可用
⚠️ LL — 已部分适配
⚠️ KS — 待优化
⚠️ SK — 待优化

## 五、渠道 vs XT 对比（上一轮测试结果）

| 渠道 | 数据量 | 报价区间 | 类型 |
|------|--------|----------|------|
| YB | 145条 | 6.7561~6.7899 | USD/CNH |
| MDAQ_FX_TOM | 144条 | 6.7643~6.7903 | USD/CNH |
| BOC | 54条 | 6.7535~6.7791 | USD/CNH |
| HCE_FX_TOM | 145条 | 6.7648~6.7908 | USD/CNH |
| XT | 69条 | 6.7186~6.7671 | USD/CNY |

## 六、命令系统架构

### 命令注册: IntegratedCommandProcessor.commands (dict)
### 别名注册: IntegratedCommandProcessor.command_aliases (list of tuple)
### 命令流程:
用户消息 → LarkBotHandler.handle_message()
  → MessageDeduplicator.is_processed() 去重
  → processor.process(text)
    → parse_command(text) 解析
      → 精确匹配 self.commands
      → _fuzzy_match_command(text) 模糊匹配
    → handler(args) 执行
      → safe_execute(impl_func, args) 安全包装（重试2次）
  → 检测 [CHART:path] 标记 → reply_image() 发送图片
  → MessageDeduplicator.mark_as_processed() 标记

### V17 新增命令
"报价对比": self.handle_quote_comparison
别名: 报价对比, 汇率对比, 多方对比, 渠道对比, 渠道友商对比, 友商渠道对比
参数提取: _extract_comparison_args(text) → (pair_str, days, channels, competitors)

## 七、配置文件速查

### 飞书Bot
LARK_APP_ID = "<见 paykka_config.py>"
LARK_APP_SECRET = "<见 paykka_config.py>"

### GitHub
GITHUB_TOKEN = "<见 paykka_config.py>"
GITHUB_REPO_NAME = "13058199807zk-oss/paykka-control"

### 企业微信Webhook
WECHAT_WEBHOOK_URL = "<见 paykka_config.py>"

### OPS API
BALANCE_BASE_URL = "https://ops-bk.cb.paykka.com"

## 八、待办事项

1. ✅ 四路渠道 vs XT 报价对比（已完成）
2. ✅ 报价对比功能集成到V17命令系统（已完成）
3. ⬜ 完善 WF/LL/KS/SK 历史汇率查询
4. ⬜ 新增报价对比命令的更多货币对测试
5. ⬜ OCBC 退款/争议邮件 AI 自动化
6. ⬜ M-DAQ 汇率调整通知自动化
7. ⬜ 跨境支付资金流预测系统 V1.0
8. ⬜ 试用期转正汇报材料

## 九、恢复指南

### 从备份恢复
tar -xzf paykka_backup_v17_20260607.tar.gz
cd paykka_backup_v17 && cp *.py *.json /workspace/

### 启动 Bot
python3.11 lark_bot_daemon.py

### XT 重新登录
xvfb-run --auto-servernum python3.11 xt_stealth_login.py

### 测试报价对比
python3.11 -c "
from paykka_comparison import QuoteComparisonEngine
from paykka_controllerV16 import PaykkaSystemManager
# ... 获取 token 后调用 run_comparison()
"

### 关键依赖
pip3 install lark-oapi pyotp playwright pandas matplotlib openpyxl requests schedule pytz PyGithub
playwright install chromium
