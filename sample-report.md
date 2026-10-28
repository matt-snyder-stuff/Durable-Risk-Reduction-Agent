# Plan Scores: Example Plan

*Scored: example · DRR v2 · H = 3 · four review perspectives per task*
*status: draft. Reduction and Durability are not final until security architecture signs off.*

> This is an illustrative sample using the five reference projects from the calibration set. The spreads and flags are examples of the format, not measured results.

## Calibration
Pass. All five reference projects landed in their expected bands.

## Org profile used
Medium-sized, high-churn, cloud-native environment. SIEM, CNAPP, and EDR already owned. Three-year tooling refresh cycle.

## Summary
- Tasks scored: 5
- By band: Seawall 1 · Durable 2 · Weak 0 · Sandcastle 2
- Flagged: compliance-required 0 · program-work 0 · disagreement 1 · insufficient-detail 0
- Review first: EX-02 (disagreement), EX-01 (top of the ranking)

## Seawall

| Rank | ID | Task | Workstream | R | D | E | Db | C | DRR | Spread | Flags |
|------|----|------|------------|---|---|---|----|---|-----|--------|-------|
| 1 | EX-01 | Standing admin access to just-in-time elevation | Identity | 4.0 | 5.0 | 2.0 | 1.0 | 1.0 | **2.50** | 1 | hardening |

**EX-01:** Replaces standing administrator access with time-limited elevation. Removes a credential path into most production systems (Reduction 4), and the standing privilege no longer exists (Durability 5, absence-based). Runs on the existing identity platform, so Cost is 1.

## Durable

| Rank | ID | Task | Workstream | R | D | E | Db | C | DRR | Spread | Flags |
|------|----|------|------------|---|---|---|----|---|-----|--------|-------|
| 2 | EX-02 | Microsegment the crown-jewel production estate | Network | 4.0 | 5.0 | 5.0 | 3.0 | 2.0 | **1.00** | 2 | hardening, disagreement |
| 3 | EX-03 | Close one standing open firewall rule | Network | 1.0 | 5.0 | 1.0 | 1.0 | 1.0 | **0.71** | 0 | hardening |

**EX-02:** Moves the crown-jewel estate to default-deny between workloads. Removes most lateral paths into the estate (Reduction 4) and holds regardless of attacker technique (Durability 5). Heavy lift (Effort 5) with real upkeep for policy exceptions (Debt 3). *Disagreement:* engineering scored Debt 4 and security architecture scored 2; the gap is how much exception handling to expect. Review before relying on the score.

**EX-03:** Closes one standing rule. Tiny reach (Reduction 1) but permanent and nearly free. Worth taking first, but not a substitute for the architecture work in EX-02.

## Sandcastle

| Rank | ID | Task | Workstream | R | D | E | Db | C | DRR | Spread | Flags |
|------|----|------|------------|---|---|---|----|---|-----|--------|-------|
| 4 | EX-04 | East-west visibility platform, detection only | Detection | 1.0 | 2.0 | 4.0 | 4.0 | 5.0 | **0.06** | 1 | new-tool-no-hardening-gap |
| 5 | EX-05 | One SIEM correlation rule, one technique | Detection | 1.0 | 1.0 | 2.0 | 4.0 | 2.0 | **0.05** | 0 | unlinked-detection |

**EX-04:** Adds visibility but removes no reach (Reduction 1). Behavioral detection without a validated baseline (Durability 2), plus a new platform subscription (Cost 5). No specific hardening problem named.

**EX-05:** One rule for one technique (Durability 1). Needs ongoing tuning in a high-churn environment (Debt 4). No linked automated response or preventive fix.

## Key takeaways
- The identity change tops the plan: it removes real reach, it lasts, and it is cheap to keep.
- Segmentation is durable despite the effort, because it removes the most reach of anything here. Resolve the Debt disagreement before scheduling.
- The two detection-only items land in the same band at the bottom. Their scores differ by 0.01, which is not a meaningful difference.
- Before starting new detection work, link it to an automated response or a preventive fix.

## Score reference
DRR = (R × D) ÷ (E + H × (Db + C)) · Seawall above 1.50 · Durable 0.50 to 1.50 · Weak 0.15 to 0.50 · Sandcastle below 0.15
