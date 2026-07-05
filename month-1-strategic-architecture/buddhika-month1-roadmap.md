# Technology Roadmap: 2025–2028

> **Author:** Buddhika Amarasinghe · **Role:** Tech Lead · **Company:** GlobalCart
> **Budget:** $2M/yr technology · **Hiring cap:** 10 engineers/yr · Start team: 25

---

## Strategic Objectives

1. **Enable international expansion** — launch 5 markets with GDPR/data-residency compliance.
2. **Launch B2B platform** — new wholesale revenue line.
3. **Scale to 10M MAU** — from 2M today (5x) without service degradation.
4. **Improve development velocity** — from monolithic all-or-nothing releases to independent, frequent deploys.

Business targets these enable: **$50M → $200M revenue** and **2M → 10M MAU** by end of 2028.

---

## Roadmap Overview

Four delivery phases over 2025–2026, then a scale-and-optimize runway through
2028. Each phase keeps the monolith live and revenue-generating (strangler-fig).

### Phase 1: Foundation (Q1–Q2 2025)
**Theme:** Decouple and Prepare

| Initiative | Dependencies | Team Size | Investment |
|-----------|--------------|-----------|------------|
| API Gateway + strangler facade | None | 3 | $200K |
| Managed Identity/Auth service (buy + integrate) | API Gateway | 2 | $150K |
| CI/CD, IaC, observability baseline | None | 2 | $150K |
| Domain decomposition & seams (catalog/orders boundaries) | API Gateway | 3 | $150K |

**Exit Criteria:**
- [ ] API Gateway handling 100% of traffic (all routes proxied)
- [ ] Auth externalized to managed IdP; monolith delegates authentication
- [ ] Automated CI/CD pipeline; deploys go from weekly → 2x/week
- [ ] Tracing + SLOs live for gateway and first extracted service

### Phase 2: Expansion Ready (Q3–Q4 2025)
**Theme:** Multi-region Foundation

| Initiative | Dependencies | Team Size | Investment |
|-----------|--------------|-----------|------------|
| Region-aware data layer + residency controls | Auth service, decomposition | 4 | $300K |
| Catalog service extraction + search index | API Gateway, seams | 3 | $200K |
| i18n / currency / tax integration | Catalog service | 2 | $150K |
| First international market launch | All above | 3 | $150K |

**Exit Criteria:**
- [ ] Data residency enforced; PII stored in-region (GDPR-compliant)
- [ ] Catalog served from extracted service with dedicated search index
- [ ] Localization (language, currency, tax) live end-to-end
- [ ] **Market #1 launched** and taking live orders

### Phase 3: B2B Launch (Q1–Q2 2026)
**Theme:** New Business Line

| Initiative | Dependencies | Team Size | Investment |
|-----------|--------------|-----------|------------|
| B2B Commerce Engine (accounts, roles, contract pricing, quotes) | Catalog, Auth, region layer | 5 | $500K |
| Ordering service extraction (shared consumer + B2B) | Catalog, region layer | 3 | $250K |
| B2B portal / buyer experience | B2B engine | 3 | $200K |
| Markets #2–#3 launch | Region layer, catalog | 2 | $100K |

**Exit Criteria:**
- [ ] **B2B platform live** with org accounts, role-based buyers, contract pricing, quotes
- [ ] Ordering service handles both consumer and B2B flows
- [ ] Markets #2 and #3 launched
- [ ] First B2B wholesale revenue booked

### Phase 4: Scale (Q3–Q4 2026)
**Theme:** Performance and Growth

| Initiative | Dependencies | Team Size | Investment |
|-----------|--------------|-----------|------------|
| Horizontal scaling + caching/CDN tuning for 10M MAU | Extracted services | 3 | $250K |
| Event-driven backbone (async order/inventory/pricing) | Ordering, catalog | 3 | $200K |
| Monolith decommission (remaining modules) | All extractions | 4 | $250K |
| Markets #4–#5 launch | Region layer | 2 | $100K |

**Exit Criteria:**
- [ ] Load-tested and serving **10M MAU** at target p95 latency
- [ ] Event bus carries core state changes; no cross-service DB coupling
- [ ] Monolith retired or reduced to a thin, non-critical remnant
- [ ] All **5 markets** live

> **2027–2028 runway:** optimize unit economics, deepen B2B features, mature
> platform/self-service tooling, and harden reliability toward $200M revenue.

---

## Dependencies and Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| Decomposition takes longer than planned, delaying expansion | Med | High | Strangler-fig delivers value incrementally; gate each market on Phase-2 exit criteria, not on full decomposition |
| Hiring cap (10/yr) leaves teams under-resourced | High | High | Buy undifferentiated capabilities; prioritize ruthlessly; use contractors for one-off migration spikes |
| GDPR/data-residency non-compliance blocks a market | Med | Critical | Region-aware data layer in Phase 2 *before* any launch; legal sign-off as a launch gate |
| Budget overrun beyond $2M/yr | Med | High | Buy-vs-build discipline; managed services over self-hosting; quarterly re-forecast |
| Migration causes production incidents / revenue loss | Med | High | Parallel-run, feature flags, canary releases, robust observability, rollback runbooks |
| B2B engine scope creep | High | Med | MVP first (accounts + contract pricing + quotes); defer advanced features to 2027 runway |
| Key-person / knowledge concentration on monolith | Med | Med | Pair extraction work, document seams in ADRs, spread ownership |

---

## Resource Plan

### Team Structure Evolution

- **2025 start (25 engineers):** re-form from monolith-centric groups into
  capability squads — Platform/Gateway, Identity, Catalog/Search, Data/Region,
  plus a Migration squad.
- **2026 (~35–45):** add Ordering, B2B Commerce, and B2B Experience squads;
  Platform team matures into an enabling/internal-platform team.
- **2028 target (~50–55):** stable capability squads (consumer commerce, B2B,
  platform/SRE, data), each owning services end-to-end with clear SLOs.

Hiring: **10/yr** → 25 → 35 → 45 → ~55 across 2025–2028.

### Skills Requirements

| Skill area | Why needed | Source |
|-----------|-----------|--------|
| Cloud/managed infra & IaC | Multi-region, managed services | Hire + upskill |
| Distributed systems / event-driven | Service extraction, event bus | Hire (senior) + upskill |
| Identity & security | Managed IdP, GDPR | Hire specialist + vendor support |
| Data engineering / residency | Region-partitioned data | Hire + partner |
| B2B commerce domain | Build differentiator | Hire domain-experienced + product partnership |
| SRE / observability | Operate at 10M MAU / 5 regions | Grow Platform team |

### Hiring Plan

| Year | New hires | Priority roles |
|------|-----------|----------------|
| 2025 | 10 | Platform/infra (3), distributed-systems seniors (3), identity/security (2), data (2) |
| 2026 | 10 | B2B commerce (4), ordering/distributed (3), SRE (2), data (1) |
| 2027 | 10 | SRE/reliability (3), B2B depth (3), performance (2), data/ML (2) |
| 2028 | 10 | Fill gaps, redundancy on key capabilities, platform tooling |

---

## Success Metrics

| Milestone | Metric | Target |
|-----------|--------|--------|
| Phase 1 Complete | Deployment frequency | 2x current (weekly → 2x/week) |
| Phase 1 Complete | Traffic through gateway | 100% |
| Phase 2 Complete | International markets live | 1 |
| Phase 2 Complete | GDPR/residency compliance | 100% of markets certified |
| Phase 3 Complete | B2B platform | Live, first revenue booked |
| Phase 3 Complete | Markets live | 3 |
| Phase 4 Complete | MAU served at target latency | 10M @ p95 SLO |
| Phase 4 Complete | Markets live | 5 |
| 2028 (business) | Annual revenue | $200M |
| 2028 (business) | Monthly active users | 10M |
| Ongoing | Change failure rate | < 15% |
| Ongoing | Lead time for change | days → hours |
| Ongoing | Technical debt ratio | 35% → < 20% |
