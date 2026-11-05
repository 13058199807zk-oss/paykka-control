# 路由定时切换配置  
  
# 功能开关  
ENABLED = True  
NOTIFICATION = True  # 切换后是否发送通知  
  
# 时间表配置  
```json
SCHEDULES = 
[
    {
        "id": "morning_switch_USDCNH",
        "description": "USDCNH工作日早上切换",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_OCBC_TOD",
        "time": "08:05",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "morning_switch_HKDCNH",
        "description": "HKDCNH工作日早上切换",
        "currency_pair": {"sell": "HKD", "buy": "CNH"},
        "target_rule": "RULE_OCBC_TOD",
        "time": "08:05",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "morning_switch_HKDUSD",
        "description": "HKDUSD工作日早上切换",
        "currency_pair": {"sell": "HKD", "buy": "USD"},
        "target_rule": "RULE_OCBC_TOD",
        "time": "08:05",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "morning_switch_AUDCNH",
        "description": "AUDCNH工作日早上切换",
        "currency_pair": {"sell": "AUD", "buy": "CNH"},
        "target_rule": "RULE_OCBC_TOD",
        "time": "08:05",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "afternoon_switch_USDCNH",
        "description": "USDCNH工作日下午切换",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_OCBC_TOM",
        "time": "15:25",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "afternoon_switch_HKDCNH",
        "description": "HKDCNH工作日下午切换",
        "currency_pair": {"sell": "HKD", "buy": "CNH"},
        "target_rule": "RULE_OCBC_TOM",
        "time": "15:25",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "afternoon_switch_HKDUSD",
        "description": "HKDUSD工作日下午切换",
        "currency_pair": {"sell": "HKD", "buy": "USD"},
        "target_rule": "RULE_OCBC_TOM",
        "time": "15:25",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "afternoon_switch_AUDCNH",
        "description": "AUDCNH工作日下午切换",
        "currency_pair": {"sell": "AUD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "15:25",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "midnight_switch_USDCNH",
        "description": "USDCNH工作日凌晨切换",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "23:50",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": False
    },
    {
        "id": "midnight_switch_HKDCNH",
        "description": "HKDCNH工作日凌晨切换",
        "currency_pair": {"sell": "HKD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "23:50",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": False
    },
    {
        "id": "midnight_switch_HKDUSD",
        "description": "HKDUSD工作日凌晨切换",
        "currency_pair": {"sell": "HKD", "buy": "USD"},
        "target_rule": "RULE_YB",
        "time": "23:50",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": False
    },
    {
        "id": "friday_night_switch_USDCNH",
        "description": "USDCNH周五晚上切换",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_HKDCNH",
        "description": "HKDCNH周五晚上切换",
        "currency_pair": {"sell": "HKD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_HKDUSD",
        "description": "HKDUSD周五晚上切换",
        "currency_pair": {"sell": "HKD", "buy": "USD"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "monday_morning_switch_HKDCNH",
        "description": "HKDCNH周一早上切换",
        "currency_pair": {"sell": "HKD", "buy": "CNH"},
        "target_rule": "RULE_HCE_TOM",
        "time": "09:05",
        "days": ["MON"],
        "enabled": False,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_AUDCNH",
        "description": "AUDCNH周五晚上切换",
        "currency_pair": {"sell": "AUD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "monday_morning_switch_AUDCNH",
        "description": "AUDCNH周一早上切换",
        "currency_pair": {"sell": "AUD", "buy": "CNH"},
        "target_rule": "RULE_CC",
        "time": "09:05",
        "days": ["MON"],
        "enabled": False,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_EURCNH",
        "description": "EURCNH周五晚上切换",
        "currency_pair": {"sell": "EUR", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "monday_morning_switch_EURCNH",
        "description": "EURCNH周一早上切换",
        "currency_pair": {"sell": "EUR", "buy": "CNH"},
        "target_rule": "RULE_CC",
        "time": "09:05",
        "days": ["MON"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_GBPCNH",
        "description": "GBPCNH周五晚上切换",
        "currency_pair": {"sell": "GBP", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "monday_morning_switch_GBPCNH",
        "description": "GBPCNH周一早上切换",
        "currency_pair": {"sell": "GBP", "buy": "CNH"},
        "target_rule": "RULE_CC",
        "time": "09:05",
        "days": ["MON"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_CADCNH",
        "description": "CADCNH周五晚上切换",
        "currency_pair": {"sell": "CAD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "monday_morning_switch_CADCNH",
        "description": "CADCNH周一早上切换",
        "currency_pair": {"sell": "CAD", "buy": "CNH"},
        "target_rule": "RULE_CC",
        "time": "09:05",
        "days": ["MON"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_USDHKD",
        "description": "USDHKD周五晚上切换",
        "currency_pair": {"sell": "USD", "buy": "HKD"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "monday_morning_switch_USDHKD",
        "description": "USDHKD周一早上切换",
        "currency_pair": {"sell": "USD", "buy": "HKD"},
        "target_rule": "RULE_CC",
        "time": "09:05",
        "days": ["MON"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_EURHKD",
        "description": "EURHKD周五晚上切换",
        "currency_pair": {"sell": "EUR", "buy": "HKD"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "monday_morning_switch_EURHKD",
        "description": "EURHKD周一早上切换",
        "currency_pair": {"sell": "EUR", "buy": "HKD"},
        "target_rule": "RULE_CC",
        "time": "09:05",
        "days": ["MON"],
        "enabled": True,
        "send_notification": True
    }
]
```
# 支持的规则列表
SUPPORTED_RULES = [
    "RULE_OCBC_TOD",
    "RULE_OCBC_TOM",
    "RULE_OCBC_SPOT",
    "RULE_HCE_TOD",
    "RULE_FX_TOD",
    "RULE_OCBC",
    "RULE_HCE_TOM", 
    "RULE_HCE_SPOT", 
    "RULE_YB",
    "RULE_CC",
    "RULE_FX_TOM",
    "RULE_FX_SPOT"
]
