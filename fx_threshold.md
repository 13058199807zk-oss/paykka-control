# ========== 超限提醒配置（核心） ==========
# 1. 企业微信机器人Webhook（替换为你创建的机器人地址）
WECHAT_WEBHOOK_URL = "https://qyapi.weixin.qq.com/cgi-bin/webhook/send?key=4c15eb09-0b7d-4ca7-b378-ae169ddc4917"
# 2. 超限阈值配置（可根据需求调整）
ALERT_THRESHOLDS = {
    "USD/CNH": 100000.0,
    "EUR/CNH": 50000.0,
    "GBP/CNH": 30000.0
}
# 3. 提醒开关（可按需关闭）
ENABLE_WECHAT_ALERT = True
