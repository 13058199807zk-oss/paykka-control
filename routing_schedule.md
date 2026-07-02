# 路由定时切换配置  
  
# 功能开关  
ENABLED = True  
NOTIFICATION = True  # 切换后是否发送通知  
  
# 时间表配置  
```json
SCHEDULES = 
[
    {
        "id": "morning_switch",
        "description": "工作日早上切换到OCBC",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_OCBC",
        "time": "08:05",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "afternoon_switch",
        "description": "工作日下午切换到HCE_TOM",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_HCE_TOM",
        "time": "14:55",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "midnight_switch",
        "description": "工作日凌晨切换到YB",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "23:50",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": False,
        "send_notification": True
    },
    {
        "id": "friday_night_switch",
        "description": "周五晚上切换到YB",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "22:30",
        "days": ["FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "sunday_night_switch",
        "description": "周日晚上切换到CC",
        "currency_pair": {"sell": "GBP", "buy": "CNH"},
        "target_rule": "RULE_CC",
        "time": "23:50",
        "days": ["SUN"],
        "enabled": False,
        "send_notification": True
    }
]
```
# 支持的规则列表
SUPPORTED_RULES = [
    "RULE_OCBC",
    "RULE_HCE_TOM", 
    "RULE_YB",
    "RULE_FX_TOM"
]
