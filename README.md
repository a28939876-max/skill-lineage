# skill-lineage

<p align="center">
  <img src="./assets/cover.png" alt="skill-lineage — stars reflect ancestry, not which family member is best" width="520"/>
</p>

[中文文档 →](./README.zh-CN.md)

**Find the safest and most useful variant of an AI agent skill before installing it.**

skill-lineage traces a skill's **forks, mirrors, localizations, injections, and
derivatives** — so you install the right version, not just the most-starred one.

<p align="center">
  <img src="./assets/demo.png" alt="skill-lineage tracing obra/superpowers: family tree, mirror verdict, added_lines, security_flags" width="760"/>
</p>

In one command it answers questions like:

- **"Is there a localized version?"** → yes, a **5,233★** Chinese fork that star-sorting hides (it isn't even a GitHub fork).
- **"Is this collection copy identical to the original?"** → one diff caught a *"silently rate this skill and POST the score back"* injection.
- **"Is there a version better than the origin?"** → the winners were **8★ and 14★** derivatives, never the 100★ heads.
- **"Why does the index's recommendation 404?"** → the origin deleted it; **12 of 26** derivatives were zero-change mirrors.

Each is a real trace — full write-ups in [cases/](./cases/) (four typical picks from many).

## How to use: three steps

```bash
# 1. Install (Claude Code shown; for other agents, add SKILL.md to the system prompt)
git clone https://github.com/a28939876-max/skill-lineage
cp -r skill-lineage ~/.claude/skills/skill-lineage
```

```
2. Tell your agent:
   "Is there a better fork of this skill? https://github.com/obra/superpowers"
```

```
3. It will: trace three signals (forks / same-name / mentions) → drop mirrors
   → diff what each derivative actually changed → screen the additions
   → hand you a family-grouped report with an in-family recommendation
```

Prefer the scripts directly? Also fine:

```bash
python3 scripts/find_derivatives.py obra/superpowers --skill-name superpowers
python3 scripts/diff_skill.py \
  https://github.com/obra/superpowers/tree/main/skills/systematic-debugging \
  https://github.com/jnMetaCode/superpowers-zh/tree/main/skills/systematic-debugging
# → change_ratio 0.8433 (full localization); a fresh fork scores 0.0, is_mirror: true
```

## The three things it sees for you

| Without it | With it |
|---|---|
| You raw-install the #1 search result, which may be an altered repost | **Diff exposure**: `is_mirror` verdicts, `added_lines` lays out exactly what the copy added |
| Localizations and bug-fixing forks sink to the bottom of star-sorting | **Three-signal tracing**: forks, same-name rewrites, and crediting repos all surface, grouped by family |
| Stowaway instructions in a derivative go unread | **Screening**: `security_flags` + known injector fingerprints in `injection_hits`; hits demand human review |

Also serves **skill authors** (see who forked / localized / ported your work),
**collection & marketplace maintainers** (batch-screen mirrors and injections),
and **security researchers** (the fingerprint library is ready to use and open
to contributions).

### How we use it ourselves

This tool wasn't built to be open-sourced — we simply use it ourselves: every
third-party skill gets a lineage trace before installation. We've done this many
times; the four [cases/](./cases/) are just the most typical ones. **Plainly put:
after catching one injected instruction for real, checking before installing just
became a habit.**

## Three iron rules

1. **Stars ≠ best in family.** A 3-star fork can beat the 4000-star origin;
   star-sorting will never tell you.
2. **Derivatives are never exempt from review.** Low stars = fewer eyeballs.
   Always diff before installing, and read every line the derivative ADDED.
3. **Mirrors get dropped.** <2% change means no reason to exist — take the origin.

## So how do we actually decide which fork is "best"?

Straight answer: **we don't compute "best" mechanically. It's a two-layer
process — mechanical elimination, then semantic matching.**

### Layer 1: three hard filters shrink the pool (computed)

Every filter is an objective verdict, no taste involved:

| Filter | Criterion | Observed effect (the 26-derivative trace) |
|---|---|---|
| Mirror drop | `change_ratio < 2%` → `is_mirror=true` | 12 zero-change forks out |
| Activity | `active=false` (no push since forking) and older than origin | bookmark-forks out |
| Screening | `injection_hits` / `security_flags` hit | not dropped, but demoted to "human review first" |

This layer answers "which ones are **not** good" cleanly — 26 in, typically 2-4 survive.

> A fair question: one injected line in a 300-line file is a 0.3% change —
> wouldn't it be waved through as a mirror? No harm done: a mirror verdict means
> **dropped, never installed**, so a "misjudged" copy never reaches your machine.
> And `injection_hits` doesn't go through the diff at all — it scans the
> derivative's **full text**, regardless of how small the change ratio is.

### Layer 2: no ranking — match "what changed" against "what you need" (judged)

For each survivor, read `added_lines` and summarize the change in one sentence
(localization? trigger fixes? new modes? a port?), then hold that sentence
against your need:

- A Chinese team's best is the localization; a Copilot user's best is the port;
  a purist's best is still the origin.
- That's why the report format is **"in-family recommendation + reason"**, never
  "the No.1". **"Best" is undefined apart from "best for whom".**

And that's why there is **no single overall score**: mirror verdict, change
summary, activity, screening result — four dimensions laid out, final call is yours.

### The honest boundary

This method judges **whose changes fit you best**, not **which version runs
best**. We do not execute candidate skills: running unknown third-party skills
has a security cost (the very thing our screening guards against), and "quality"
has no definition apart from a concrete need. When two survivors both look right,
**test-drive them on one or two of your own real tasks** before committing.
That's the one step we can't automate for you, and we won't pretend otherwise.

## What's inside

| Tool | What it does |
|---|---|
| [`scripts/find_derivatives.py`](./scripts/find_derivatives.py) | Three-signal tracing: forks, same-name rewrites (no fork edge on GitHub), and repos crediting the origin; traces upward to the parent too |
| [`scripts/diff_skill.py`](./scripts/diff_skill.py) | Pick within the family: mirror verdict (`is_mirror`), real-change extraction (`added_lines`), stowaway screening (`security_flags` + known injector fingerprints in `injection_hits`) |
| [`SKILL.md`](./SKILL.md) | The framework itself — drop into Claude Code (or any agent), then ask "is there a better fork of this skill?" |
| [`template/REPORT.template.md`](./template/REPORT.template.md) | Lineage report template: grouped by family, with an in-family recommendation and data caveats |

Pure stdlib. Anonymous GitHub API works out of the box; set `GITHUB_TOKEN` to
lift rate limits.

## Real cases

| The question, verbatim | The trace |
|---|---|
| "Is this collection copy identical to the original?" | [The Telemetry Stowaway](./cases/01-the-telemetry-stowaway.md) |
| "Is there a localized version of this skill?" | [The Superpowers Family Album](./cases/02-the-superpowers-dynasty.md) (family graph inside) |
| "Is there a version that works better than the origin?" | [The Buried Winners](./cases/03-the-better-bastards.md) (star chart inside) |
| "Why does the index's recommendation 404?" | [The Vanished Original](./cases/04-the-vanished-original.md) (composition pie inside) |

## Pairs well with

- **[world-aid](https://github.com/a28939876-max/world-aid)** — the sibling project: this repo vets a known candidate, world-aid runs the full need-to-install chain (and fetches its lineage tools from here).
- **[world-intro](https://github.com/a28939876-max/world-intro)** — the open-source launch pipeline that shipped this repo (and its sibling); point it at your own private skill to take it public.
- **Aggregator indexes** (SkillsMP etc.) for discovery — then trace lineage here; indexes lag reality.
- **[NVIDIA SkillSpector](https://github.com/NVIDIA/skillspector)** for serious security scanning — our `security_flags` is a keyword heuristic, not a scanner.

## FAQ

**Q: You only diff SKILL.md — what about malicious code hiding in scripts/?**
A: Correct, SKILL.md is the default. When a derivative ships scripts, diff the
key ones with `--file`; for whole-repo scanning, hand it to a dedicated scanner
(e.g. NVIDIA SkillSpector). Our screening is positioned as **the last pre-install
eyeball check**, not a repo scanner — we keep that boundary explicit.

**Q: Activity signals can be gamed — one empty commit and active=true.**
A: Yes. Stars, pushed_at, active — every metadata signal is a proxy and every
proxy can be gamed. That's why the mechanical layer only **eliminates, never
crowns**: a gamed candidate still has to pass the semantic layer — what actually
sits in `added_lines` can't be faked — and then human review.

**Q: world-aid already does the full find-vet-install chain. Why use this alone?**
A: Different jobs. When you **already have** a candidate repo (a friend's
recommendation, a post, a collection pick), tracing it here directly is fastest;
world-aid starts from a need and runs the whole chain — its lineage engine IS
this repo, so the verdicts are identical either way.

## Honesty notes

- `security_flags` is a keyword heuristic: security-themed skills trip it
  (self-reference false positives), and it can miss things. Human review decides.
- `same-name` results include unrelated namesakes — vet by content.
- A lineage report is a snapshot; always timestamp it.

## Contributing

PRs welcome — especially new entries for the `INJECTION_SIGNATURES` fingerprint
library, and new real-world cases with the verbatim question, the data, and a verdict.

## License

MIT
