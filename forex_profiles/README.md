# forex_profiles —— 汇差方案库

BOT 指令「汇差方案列表」/「汇差方案切换 <方案名>」读取本目录下的 `*.json`。
（`README.md` 不是方案，不会被列表收录。）

## 文件格式（结构 = 线上 detail 的 items，切换时零转换）

```jsonc
{
  "name": "weekend",                    // 显示名
  "aliases": ["周末", "weekend"],        // 可匹配名（文件名 stem / name / aliases 任一命中即可）
  "note": "周末人民币不报价，USD/CNH 归零",
  "updated": "2026-09-30T11:12:20+08:00",
  "config": {
    "USD": {
      "items": [                          // ★ 必须是该 sell_cur 的【全部】buy_cur
        {"buy_cur": "CNH",
         "configs": [{"provider": "BOC", "type": "BY_DIFF", "value": 0}],
         "cost":    {"provider": "YB",  "type": "BY_DIFF", "value": -0.002}}
      ]
    }
  }
}
```

* `type`：`BY_RATIO` = 百分比 / `BY_DIFF` = 固定汇差
* `configs` = 数据源（可多条）→ 决定「平台标准报价」；`cost` = 成本底价（单条）

## ★ 两条铁律

1. **一个 sell_cur 下必须写全所有 buy_cur**。线上 `edit` 接口是**整表替换**，
   只写几条 = 提交后把其余币对**删掉**。
   BOT 在切换前会做硬校验：方案与线上的 buy_cur 集合不一致 → **直接拒绝**，不会提交。
2. 切换 = **发起审核**（李冰洁 / 林毅放行后才生效），且默认先出 diff 等【确认】。

## 怎么用

| 你要做的 | 怎么做 |
|---|---|
| 新建方案 | 飞书发「汇差查询 快照」→ 复制生成的 JSON → 在本目录新建 `xxx.json` 粘贴 |
| 改方案 | 直接编辑本目录的 JSON（有 Git 历史，可回滚） |
| 看方案列表 | 飞书发「汇差方案列表」 |
| 切方案 | 飞书发「汇差方案切换 <方案名>」→ 看 diff → 回【确认】 |
| 跳过确认 | 「汇差方案切换 <方案名> 直接」（⚠️ 谨慎） |

> 预览「方案 vs 线上」的差异由 BOT 的「汇差方案切换」内置 diff 承担（GitHub 上没有线上数据）。
