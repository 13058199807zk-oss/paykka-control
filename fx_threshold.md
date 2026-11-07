# ========== 超限提醒配置（核心） ==========
# 1. 企业微信机器人Webhook
#    ⚠️ 已移出公开仓库：本文件为 public，写入密钥等于公开。
#    改由服务器 .env 的 WECHAT_WEBHOOK_URL 提供；此处留空即自动回落 .env。
#    BOT 解析逻辑：_parse_fx_threshold_md() 取不到该行时返回 ""，
#    发送侧 `webhook or WECHAT_WEBHOOK_URL` 会回落到 .env 的值，行为不变。
# 2. “已交割”交易超限阈值配置
ALERT_THRESHOLDS = {
    "USD/CNH": 1450000.0,
    "EUR/CNH": 150000.0,
    "GBP/CNH": 100000.0
}
# 3. “处理中”交易超限阈值配置
PROCESSING_ALERT_THRESHOLDS = {
    "USD/CNH": 1250000.0,
    "EUR/CNH": 250000.0,
    "GBP/CNH": 120000.0,
    "HKD/CNH": 1550000.0
}
# 4. T+0交易时段配置
TRADING_HOURS = {
    "OCBC": {"start": "09:00", "end": "14:00"},
    "YB": {"start": "00:00", "end": "23:59"},
    "MDAQ": {"start": "00:00", "end": "23:59"}
}
未配置的机构默认24小时交易
# 5. 提醒开关
ENABLE_WECHAT_ALERT = True
