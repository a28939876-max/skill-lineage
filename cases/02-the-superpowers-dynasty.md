# 案例二：一个爆款 skill 的全家福 / The Superpowers Family Album

> 需求原话：「这个 skill 有没有中文版？」
> 答案：有——一个原版，养出一个 5229⭐ 的汉化版、一个全员改名的增强版、一个 Copilot 移植版，和一群一字未改的镜像。

## 族树（实测快照）

对 `obra/superpowers`（Claude Code 最著名的 skill 合集之一）跑 `find_derivatives.py`：

```mermaid
graph TD
    O["obra/superpowers<br/>原版 · 14 个 skill"]
    O --> ZH["jnMetaCode/superpowers-zh<br/>5229⭐ · 完整汉化 + 6 个原创"]
    O --> ST["GadaaLabs/claude-code-on-steroids<br/>62⭐ · 14→24 skills · 全部改名"]
    O --> CP["DwainTR/superpowers-copilot<br/>65⭐ · Copilot CLI 移植"]
    O --> TR["gotocx/superpowers-trae<br/>41⭐ · TRAE 迁移"]
    O --> BR["superspec 33⭐ / openspec-schemas 132⭐<br/>桥接件：接进 spec 生态"]
    O --> MI["mesotron7x/mesopowers<br/>0⭐ · 纯镜像"]
    style ZH fill:#dfd,stroke:#080
    style MI fill:#eee,stroke:#999,stroke-dasharray: 5 5
```

两次 `diff_skill.py` 实测（同一个 skill 文件 `systematic-debugging`）：

| 对比对象 | change_ratio | is_mirror | 改动概括 |
|---|---|---|---|
| 原版 vs superpowers-zh | **0.8433** | false | 逐行翻译，结构未动 → 中文用户的族内推荐 |
| 原版 vs mesopowers | **0.0** | **true** | 一字未改 → 淘汰 |

## 这个族证明了什么

1. **哪个版本最好，取决于你是谁。** 中文团队选 superpowers-zh 最合适（它自己就拿了 5229⭐——衍生版也能做得很大）；用 Copilot 的选 DwainTR 的移植版；想要原汁原味的还是选原版。**离开"给谁用"，谈不上"最好版本"。**
2. **same-name 路线不可省。** superpowers-zh 不是 fork——GitHub 的 fork 图谱里根本没有它。只看 forks 你会以为这个族只有镜像。
3. **衍生有三种**：改良（steroids）、移植（copilot/trae）、桥接（superspec）。修谱时别只盯着"改了原文件的"。
4. **衍生版一改文件名，diff 就对不上了。** steroids 把 24 个 skill 全部重命名，和原版没有一个路径相同——这时 `diff_skill.py` 会 404，正确做法是退回 trees API 把新旧名字对上，或者退一步读它 README 概括改动。
5. **镜像一判一个准。** 刚建不久的 fork 一字未改：change_ratio 精确为 0.0。这种衍生版不用再花时间讨论。

## 数据来源

- 本篇是众多实测修谱记录里挑出的典型之一。
- `find_derivatives.py obra/superpowers --skill-name superpowers`
- `diff_skill.py` 实测两对：origin vs superpowers-zh（0.8433）、origin vs mesopowers（0.0）
- GitHub trees API 交集验证：origin 14 个 SKILL.md vs steroids 24 个，交集 0。
