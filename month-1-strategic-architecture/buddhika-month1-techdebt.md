# Technical Debt Strategy

> **Author:** Buddhika Amarasinghe · **Role:** Tech Lead · **Company:** GlobalCart
> **Context:** 5-year-old monolith must support 5 markets, B2B, and 5x growth
> inside a $2M/yr budget and 10-hire/yr cap.

---

## Current Debt Inventory

### Critical (Must Address)
| Item | Impact | Effort | Risk if Ignored |
|------|--------|--------|-----------------|
| Monolith coupling (shared schema, cross-domain FKs) | High | Large | Blocks expansion & independent scaling |
| Legacy embedded auth (custom sessions, no MFA/SSO) | High | Medium | Security breach; blocks B2B SSO & market entry |
| No data-residency controls (single-region DB) | High | Large | **Legal blocker** — cannot launch GDPR markets |
| No i18n / single currency & locale | High | Medium | Cannot serve international markets |

### High Priority (Should Address)
| Item | Impact | Effort | Risk if Ignored |
|------|--------|--------|-----------------|
| Single release pipeline / manual steps | Med-High | Medium | Slow, risky releases; low velocity caps throughput |
| Reporting/analytics on primary DB | Med-High | Medium | Peak-load contention degrades customer experience |
| No caching / CDN tuning for scale | Med | Medium | Latency and cost problems at 10M MAU |
| Thin observability (logs only, no traces/SLOs) | Med | Medium | Blind operation across services/regions |

### Medium Priority (Could Address)
| Item | Impact | Effort | Risk if Ignored |
|------|--------|--------|-----------------|
| In-DB search (no dedicated index) | Med | Medium | Search degrades as catalog grows |
| Inconsistent API versioning/contracts | Med | Small | Integration friction as services multiply |
| Sparse automated test coverage in core modules | Med | Large | Slows safe refactoring/extraction |

### Low Priority (Won't Address Now)
| Item | Impact | Effort | Justification for deferring |
|------|--------|--------|-----------------------------|
| Legacy admin UI styling/UX debt | Low | Medium | Internal-only; no revenue/compliance impact |
| Minor code-style / lint inconsistencies | Low | Small | Cosmetic; address opportunistically |
| Old feature flags not cleaned up | Low | Small | Low risk; batch cleanup later |

---

## Debt Categories

### Intentional Debt
Debt taken deliberately, with a payback plan, to hit expansion deadlines:
- **Strangler facade / temporary dual-run:** the monolith and new services
  coexist during extraction. *Payback:* removed at monolith decommission (Phase 4).
- **B2B MVP shortcuts:** advanced approvals/EDI deferred to the 2027 runway.
  *Payback:* tracked as backlog items with owners and target quarters.
- **Read replicas of non-PII data globally:** a pragmatic performance choice;
  *payback:* revisited if residency rules tighten.

### Accidental Debt
Debt accumulated over five years without deliberate decision:
- Shared schema and cross-domain foreign keys.
- Embedded custom authentication.
- Business logic duplicated across modules; reporting queries on the primary DB.
*Approach:* surfaced during decomposition; retired as services are extracted.

### Environmental Debt
Debt from external/time factors:
- Framework/runtime versions approaching end-of-life.
- Single-region infrastructure assumptions baked into config.
- Third-party libraries with known CVEs needing upgrades.
*Approach:* tracked with EOL dates; upgrades scheduled ahead of support windows.

---

## Payback Strategy

### Approach
1. **20% Time Rule:** each squad dedicates ~20% of every sprint to debt reduction,
   protected and not traded away for features.
2. **Strategic Refactoring:** large structural work (service extraction, auth
   migration, region layer) is funded as roadmap initiatives, not squeezed into
   the 20% — debt paydown *is* the expansion work here.
3. **Opportunistic:** apply the "campsite rule" — improve modules whenever you
   touch them (add tests, clean seams) during feature work.

### Prioritization Framework
```
Score = (Impact × Risk) / (Effort × Cost)
```
- **Impact:** business/customer harm if unaddressed (1–5).
- **Risk:** likelihood × severity of failure (1–5).
- **Effort:** engineering time (1–5, higher = more).
- **Cost:** spend/opportunity cost (1–5, higher = more).
- Higher score = pay back sooner. Compliance/security blockers get a floor
  priority regardless of score (e.g., data residency, legacy auth).

### Annual Debt Budget
- **Q1: $150K** — Critical items (auth externalization, decomposition seams)
- **Q2: $100K** — High priority (pipeline, observability) + region-layer prep
- **Q3: $100K** — Ongoing (search index, caching, test coverage)
- **Q4: $150K** — Pre-expansion hardening (residency, load readiness)

*Total ≈ $500K/yr, ~25% of the $2M budget — consistent with the goal of driving
the debt ratio from 35% toward <20%.*

---

## Governance

### Debt Review Process
- **New debt is logged, not hidden.** Any deliberate shortcut is recorded as an
  ADR/backlog item with: reason, risk accepted, owner, and a review/expiry date
  (ties into the Architecture Review exception process).
- **Monthly debt review:** Tech Lead + squad leads re-score the register, confirm
  the 20% allocation was honored, and re-prioritize.
- **Definition of Done includes debt:** no feature "done" if it silently adds
  untracked debt.

### Metrics
| Metric | Current | Target |
|--------|---------|--------|
| Known debt items | 47 | < 30 |
| Critical items | 8 | 0 |
| Debt ratio (est. remediation vs codebase) | 35% | < 20% |
| Sprint capacity spent on debt | ~5% (ad hoc) | 20% (protected) |
| Services with SLO + tracing | ~0% | 100% |
| Change failure rate | ~25% | < 15% |

### Reporting
- **To engineering:** live debt register + dashboard (item counts, critical
  count, ratio trend, 20%-adherence) reviewed monthly.
- **To executives:** quarterly one-pager framing debt in business terms —
  "critical items block market launch / create security exposure," progress
  against targets, and budget spent vs the ~$500K/yr allocation.
- **Framing principle:** report debt as **risk to business goals** (expansion,
  B2B, scale, compliance), not as an engineering-only concern — this is how the
  20% and the $500K stay funded.
