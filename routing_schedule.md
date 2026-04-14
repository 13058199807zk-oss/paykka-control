# 路由定时切换配置

## 功能开关
ENABLED = True
NOTIFICATION = True  # 切换后是否发送通知

## 时间表配置
 |配置格式说明:|
   |id: 唯一标识符|
   |description: 描述|
   |currency_pair: 货币对 {sell: 卖出币种, buy: 买入币种}|
   |target_rule: 目标路由规则|
   |time: 切换时间 (24小时制 HH:MM)|
   |days: 生效日期 [MON,TUE,WED,THU,FRI,SAT,SUN]|
   |enabled: 是否启用|

SCHEDULES = [
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
        "time": "14:50",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "saturday_switch",
        "description": "周六早上切换到YB",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_YB",
        "time": "04:55",
        "days": ["SAT"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "sunday_night_switch",
        "description": "周日晚上切换到CC",
        "currency_pair": {"sell": "USD", "buy": "CNH"},
        "target_rule": "RULE_CC",
        "time": "22:00",
        "days": ["SUN"],
        "enabled": True,
        "send_notification": True
    },
    {
        "id": "eur_switch",
        "description": "欧元工作日切换到MDAQ",
        "currency_pair": {"sell": "EUR", "buy": "CNH"},
        "target_rule": "RULE_MDAQ_WALLEX",
        "time": "09:00",
        "days": ["MON", "TUE", "WED", "THU", "FRI"],
        "enabled": True,
        "send_notification": True
    }
]

## 支持的规则列表
# 从FX_STRATEGY_RULES中获取支持的路由规则
