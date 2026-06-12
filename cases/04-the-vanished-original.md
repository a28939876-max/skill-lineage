# 案例四：消失的原版 / The Vanished Original

> 索引站说这个 skill 有 497⭐ 和一窝衍生版。等你顺着链接去装——它已经不在原仓库里了。

## 事情经过

2026-06-12 上午，某聚合索引站显示 `aaronjmars/aeon` 仓库里有个 `skill-security-scan`（497⭐，描述完整），且已形成 5+ 成员的衍生族。

当天下午修谱验证：

- 原仓库还在，星数已涨到 507⭐，**但 `skills/skill-security-scan/` 这个路径已经不存在了**——raw 直链 404，trees API 全量列举确认：该 skill 已被改名或移除，索引站的数据是滞后快照。
- `find_derivatives.py` 拉到 26 个衍生：其中 **十几个 fork 是一字未改的镜像**（描述还是营销文案原文），真正有信息量的反而是 `same-name` 路线——

```
same-name 捞出的独立同名生态（与 aeon 无 fork 关系）：
 ├─ huifer/skill-security-scan（149⭐）：中文 CLI，装前扫描工具
 ├─ senaykt/iac-security-scan-skills（42⭐）：IaC 方向特化
 ├─ caidongyun/agent-security-skill-scanner（26⭐）
 └─ …另有 8 个同名小项目
```

## 教训

1. **索引站是快照，GitHub 是现实。** 聚合站（SkillsMP 等）可能滞后数天到数周；装之前永远以仓库当前状态为准，404 ≠ 你输错了路径，可能是它真没了。
2. **fork 数量是最廉价的虚假繁荣。** 26 个衍生里 20 个是零改动镜像——fork 按钮一秒就能点，is_mirror 判定让它们现出原形。
3. **same-name 路线再次立功**：当原版自己都不稳定时，独立同名生态（149⭐ 的中文实现）可能才是真正可装的选择。
4. 修谱报告必须带**查询时间戳**（见 template 的 Data caveats 节）——族谱是某一时刻的快照，不是永久事实。

## 数据来源

- `find_derivatives.py aaronjmars/aeon --skill-name skill-security-scan`（2026-06-12）
- raw 直链 main/master 双 404 + trees API 全量路径核对。
