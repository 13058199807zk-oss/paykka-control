# 路由定时切换配置  
  
# 功能开关  
ENABLED = True  
NOTIFICATION = True  # 切换后是否发送通知  
  
# 时间表配置  
```json
SCHEDULES = 
[
    {
        "id": "morning_switch_USD",
        "description": "USD工作日早上切换到OCBC",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_OCBC",
        "time": "08:05",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "morning_switch_AUD",
        "description": "AUD工作日早上切换到OCBC",
        "currency_pair": {"sell": "AUD", "buy": "CNH"},
        "target_rule": "RULE_OCBC",
        "time": "08:05",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "afternoon_switch_USD",
        "description": "USD工作日下午切换到HCE_TOM",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_HCE_TOM",
        "time": "15:25",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "afternoon_switch_AUD",
        "description": "AUD工作日下午切换到YB",
        "currency_pair": {"sell": "AUD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "15:25",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "midnight_switch_USD",
        "description": "USD工作日凌晨切换到YB",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "23:50",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": False
    },
    {
        "id": "friday_night_switch_USD",
        "description": "USD周五晚上切换到YB",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_HKD",
        "description": "HKD周五晚上切换到YB",
        "currency_pair": {"sell": "HKD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "monday_morning_switch_HKD",
        "description": "HKD周一早上切换到HCE",
        "currency_pair": {"sell": "HKD", "buy": "CNH"},
        "target_rule": "RULE_HCE_TOM",
        "time": "09:05",
        "days": ["MON"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_AUD",
        "description": "AUD周五晚上切换到YB",
        "currency_pair": {"sell": "AUD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "monday_morning_switch_AUD",
        "description": "AUD周一早上切换到CC",
        "currency_pair": {"sell": "AUD", "buy": "CNH"},
        "target_rule": "RULE_CC",
        "time": "09:05",
        "days": ["MON"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "friday_night_switch_EUR",
        "description": "EUR周五晚上切换到YB",
        "currency_pair": {"sell": "EUR", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "monday_morning_switch_EUR",
        "description": "EUR周一早上切换到MDAQ",
        "currency_pair": {"sell": "EUR", "buy": "CNH"},
        "target_rule": "RULE_FX_TOM",
        "time": "09:05",
        "days": ["MON"],
        "enabled": True,
        "send_notification": True
    }
]
```
# 支持的规则列表
SUPPORTED_RULES = [
    "RULE_OCBC",
    "RULE_HCE_TOM", 
    "RULE_HCE_SPOT", 
    "RULE_YB",
    "RULE_CC",
    "RULE_FX_TOM",
    "RULE_FX_SPOT"
]
