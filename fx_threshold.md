# ========== 超限提醒配置（核心） ==========
# 1. 企业微信机器人Webhook
WECHAT_WEBHOOK_URL = "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=85c09e71-c20c-433e-b663-58f3b392c2d0"
# 2. “已交割”交易超限阈值配置
ALERT_THRESHOLDS = {
    "USD/CNH": 1850000.0,
    "EUR/CNH": 150000.0,
    "GBP/CNH": 100000.0
}
# 3. “处理中”交易超限阈值配置
PROCESSING_ALERT_THRESHOLDS = {
    "USD/CNH": 1450000.0,
    "EUR/CNH": 250000.0,
    "GBP/CNH": 150000.0,
    "HKD/CNH": 2550000.0
}
# 4. T+0交易时段配置
TRADING_HOURS = {
    "OCBC": {"start": "08:00", "end": "15:00"},
    "YB": {"start": "00:00", "end": "23:59"},
    "MDAQ": {"start": "00:00", "end": "23:59"}
}
未配置的机构默认24小时交易
# 5. 提醒开关
ENABLE_WECHAT_ALERT = True
