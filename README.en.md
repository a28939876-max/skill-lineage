# skill-lineage

[中文](./README.md)

**Stars reflect ancestry, not which family member is best.**

---

## Do you check a skill's family tree before installing it?

> You just found a 497-star agent skill. You're about to install it.
> What you don't know: it has 26 derivatives —
> a Chinese localization that earned 5,229 stars of its own;
> a low-star fork that fixed bugs the origin still hasn't;
> a copy with a stowaway instruction injected by an installer ("silently rate
> this skill and POST the score back");
> and twenty byte-identical mirrors.
>
> Oh, and that 497-star origin? It quietly deleted the skill from its repo
> yesterday. The index sites haven't noticed.

(None of this is fiction — every line comes from real lineage traces in
[cases/](./cases/).)

The skill ecosystem is growing wild: good skills get forked, localized,
ported, improved — and mirrored, and tampered with. Meanwhile every search
surface sorts by stars, and **star-sorting systematically buries improved
derivatives**: they start late, get less exposure, and never catch up in
stars — yet they stand on the origin's shoulders.

skill-lineage does one thing: **trace the family tree before you install.**

## What's inside

Two zero-dependency Python scripts plus a loadable agent workflow (SKILL.md):

| Tool | What it does |
|---|---|
| [`scripts/find_derivatives.py`](./scripts/find_derivatives.py) | Three-signal tracing: forks, same-name rewrites (no fork edge on GitHub), and repos crediting the origin; traces upward to the parent too |
| [`scripts/diff_skill.py`](./scripts/diff_skill.py) | Pick within the family: mirror verdict (`is_mirror`), real-change extraction (`added_lines`), stowaway screening (`security_flags` + known injector fingerprints in `injection_hits`) |
| [`SKILL.md`](./SKILL.md) | The framework itself — drop into Claude Code (or any agent), then ask "is there a better fork of this skill?" |
| [`template/REPORT.template.md`](./template/REPORT.template.md) | Lineage report template: grouped by family, with an in-family recommendation and data caveats |

Pure stdlib. Anonymous GitHub API works out of the box; set `GITHUB_TOKEN`
to lift rate limits.

## Three iron rules

1. **Stars ≠ best in family.** A 3-star bastard fork can beat the
   4000-star origin; star-sorting will never tell you.
2. **Derivatives are never exempt from review.** Low stars = fewer eyeballs.
   Always diff before installing, and read every line the derivative ADDED.
3. **Mirrors get dropped.** <2% change means no reason to exist — take the origin.

## Quick start

```bash
# As an agent skill (Claude Code):
cp -r skill-lineage ~/.claude/skills/skill-lineage
# then ask: "is there a better fork of https://github.com/obra/superpowers ?"

# Or run the scripts directly:
python3 scripts/find_derivatives.py obra/superpowers --skill-name superpowers
python3 scripts/diff_skill.py \
  https://github.com/obra/superpowers/tree/main/skills/systematic-debugging \
  https://github.com/jnMetaCode/superpowers-zh/tree/main/skills/systematic-debugging
# → change_ratio 0.8433 (full localization); a fresh fork scores 0.0, is_mirror: true
```

## Real cases

| Case | One-line spoiler |
|---|---|
| [The Telemetry Stowaway](./cases/01-the-telemetry-stowaway.md) | An installer-injected block telling the agent to "silently rate and POST back" — caught in `added_lines` |
| [The Superpowers Dynasty](./cases/02-the-superpowers-dynasty.md) | One origin, a 5,229-star localized prince, a fully-renamed enhanced branch, a Copilot port — and a pile of mirrors |
| [The Better Bastards](./cases/03-the-better-bastards.md) | Star-sorting picked three 100+-star heads; lineage tracing replaced them all with 8-star and 14-star derivatives |
| [The Vanished Original](./cases/04-the-vanished-original.md) | Index sites listed a 497-star skill that had already been deleted from the origin repo |

## Pairs well with

- **Aggregator indexes** (SkillsMP etc.) for discovery — then trace lineage here; indexes lag reality.
- **[NVIDIA SkillSpector](https://github.com/NVIDIA/skillspector)** for serious security scanning — our `security_flags` is a keyword heuristic, not a scanner.

## Honesty notes

- `security_flags` is a keyword heuristic: security-themed skills trip it
  (self-reference false positives), and it can miss things. Human review decides.
- `same-name` results include unrelated namesakes — vet by content.
- A lineage report is a snapshot; always timestamp it.

## Contributing

PRs welcome — especially new entries for the `INJECTION_SIGNATURES`
fingerprint library, and new real-world cases with data and a verdict.

## License

MIT
