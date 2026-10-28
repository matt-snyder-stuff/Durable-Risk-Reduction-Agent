# Durable Risk Reduction (DRR) Agent

Security teams plan in 12 to 18 month blocks, and the plan gets interrupted constantly. Meanwhile attackers turn a new patch into a working exploit in hours. DRR is a way to look at the work already in front of you and decide what to do first, based on two questions:

1. **How much reach does this work remove?** How much of the environment one compromise can touch.
2. **How long does the result hold?** Before an attacker routes around it.

This repo contains a scoring agent that drafts a DRR score for every task on a plan, plus a spreadsheet if you would rather score by hand. It comes from Matt Snyder's Security Field Day 16 talk, *We Scored Our Own Security Plan. Here's What Changed.*

## The score

```
DRR = (Reduction × Durability) ÷ (Effort + H × (Debt + Cost))
```

| Input | What it measures | Scored by |
|---|---|---|
| Reduction | How much reach the work removes (1 to 5) | Security architecture |
| Durability | How long the result holds (1 to 5) | Security architecture |
| Effort | Build time, paid once (1 to 5) | The team doing the work |
| Debt | Recurring labor: tuning, triage, upkeep (1 to 5) | The team doing the work |
| Cost | Recurring licensing and infrastructure (1 to 5) | The team doing the work |
| H | Your tooling refresh cycle, in years (default 3) | Set once per organization |

| Band | Score |
|---|---|
| Seawall | above 1.50 |
| Durable | 0.50 to 1.50 |
| Weak | 0.15 to 0.50 |
| Sandcastle | below 0.15 |

Scores are ordinal. Read them in bands, and do not treat small decimal differences as meaningful.

## What is in this repo

| File | What it is |
|---|---|
| [`drr-scorer.md`](drr-scorer.md) | The scoring agent, as a Claude Code subagent. Also readable on its own as the full rubric. |
| [`drr-score-sheet.xlsx`](drr-score-sheet.xlsx) | A spreadsheet that does the math, with a calibration tab. No agent needed. |
| [`sample-report.md`](sample-report.md) | What the agent's output looks like, using the five reference projects. |

## Quick start with Claude Code

1. Download [`drr-scorer.md`](drr-scorer.md) and put it in your project's `.claude/agents/` folder, or in `~/.claude/agents/` to use it in every project.
2. Open the file and fill in the **Org profile** section. Use categories, not secrets: no hostnames, account IDs, or names.
3. Ask Claude Code something like: *"Use the drr-scorer agent to score our Q3 security plan."* The agent reads whatever your session can reach, such as an issue tracker or wiki connected over MCP, or a list you paste in.
4. Review the report. It is marked **draft** until security architecture signs off on Reduction and Durability.

**Keep it read-only.** The agent file denies the Write and Edit tools. Claude Code subagents inherit all of the session's MCP tools when no `tools` list is set, so if your session can write to your tracker, consider adding an explicit `tools` allowlist of read-only tools. See the [Claude Code subagents documentation](https://code.claude.com/docs/en/sub-agents).

## Without an agent

Open `drr-score-sheet.xlsx`, set H to your refresh cycle, and type your inputs into the blue cells. Run the **Calibration** tab first. If the five reference projects do not land in their expected bands, check your inputs before scoring anything real.

## Calibrate first

Before scoring a real plan, score these five reference projects. They anchor the scale so two people mean the same thing by a 4.

| Reference project | R | D | E | Debt | Cost | DRR | Band |
|---|---|---|---|---|---|---|---|
| Standing admin access to just-in-time elevation | 4 | 5 | 2 | 1 | 1 | 2.50 | Seawall |
| Microsegment the crown-jewel production estate | 4 | 5 | 5 | 3 | 2 | 1.00 | Durable |
| Close one standing open firewall rule | 1 | 5 | 1 | 1 | 1 | 0.71 | Durable |
| East-west visibility platform, detection only | 1 | 2 | 4 | 4 | 5 | 0.06 | Sandcastle |
| One SIEM correlation rule, one technique | 1 | 1 | 2 | 4 | 2 | 0.05 | Sandcastle |

These are illustrative reference points, not measured results.

## Where this can fool you

- **Two inputs get inflated.** Reduction on work that touches many systems without removing a path, and Durability on detection with an unvalidated baseline. That is why architecture signs off both.
- **It scores structural durability, not business value.** Compliance work and program work (content, research, support) stay on the plan. Track them separately rather than treating a low score as a verdict.
- **The agent drafts. People decide.** Treat every report as a first pass.

## Version history

- **v2 (DRR).** Adds Reduction, makes H a setting, and adds bands, a calibration check, per-perspective disagreement, and planning flags.
- **v1 (Half-Life Index).** Scored Durability against cost alone, which ranked tiny permanent fixes above large structural ones. Retired.
