# 族谱报告 / Lineage Report：<skill 名或需求>

> 日期：<YYYY-MM-DD> · 数据来源：GitHub API（find_derivatives.py）+ raw diff（diff_skill.py）
> 原版：<owner/repo>（N⭐，最后 push <日期>）

## 族树 / Family tree

```
族：<skill名>（原版 owner/repo，N⭐，最后更新 x 月）
 ├─ 衍生 A owner/repo（M⭐，x 天前更新；relation: fork|same-name|mentions-origin）
 │    改动概括：<一句话：汉化？加模式？修触发？适配某工具？>
 │    ← 族内推荐（理由：<为什么它比原版/其它衍生更对口>）
 ├─ 衍生 B owner/repo（纯镜像 is_mirror=true，淘汰）
 └─ 衍生 C owner/repo（security_flags/injection_hits 命中 <n> 条，人审后再议）
```

## 逐个评估 / Per-derivative assessment

### <owner/repo> — <推荐 | 淘汰 | 待人审>
- 关系：fork / same-name / mentions-origin / parent
- 活跃度：active=<true|false>，pushed_at=<日期>（原版 <日期>）
- diff：change_ratio=<x>，is_mirror=<bool>
- 改动概括：<读 added_lines 后的一句话>
- 安检：security_flags <n> 条（<逐条判断：误报/需关注>）；injection_hits <n> 条
- 判词：<一两句，为什么推荐/淘汰>

（每个衍生版重复上述小节）

## 结论 / Verdict

- **族内推荐**：<owner/repo>，理由：<...>
- 原版仍是最优？<是/否，为什么>
- 装前注意：<security_flags/injection_hits 的人审结论；额外 key/依赖>

## 数据可信度声明 / Data caveats

- 查询时间：<时间戳>；GitHub 搜索与索引站可能滞后。
- same-name 结果含无血缘的同名项目，已按描述/内容人工甄别：<甄别说明>
- security_flags 为关键词启发式，结论以人审为准。
