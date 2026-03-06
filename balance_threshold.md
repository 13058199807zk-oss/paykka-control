# PayKKa余额预警阈值配置
# 格式参考fx_threshold.md的成功实现

BALANCE_THRESHOLDS = {
    # 机构名必须完全匹配：易宝, Currencycloud, 海云汇
    ("易宝", "USD"): 9990000.00,
    ("易宝", "CNH"): 10000000.00,
    ("易宝", "HKD"): 10000.00,
    ("Currencycloud", "USD"): 10000.00,
    ("Currencycloud", "EUR"): 1000.00,
    ("海云汇", "USD"): 0.00,
    ("海云汇", "CNH"): 0.00,
}

# 注意：键是元组 (机构名, 币种)，值是阈值金额
