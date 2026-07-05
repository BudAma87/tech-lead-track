# Build vs Buy Analysis

> **Author:** Buddhika Amarasinghe · **Role:** Tech Lead · **Company:** GlobalCart
> **Guiding principle:** buy the undifferentiated, build the differentiating —
> scarce capacity (10 hires/yr, $2M budget) goes to what makes us win.

Scoring: each criterion rated 1–5 (5 = best). Weighted score = Σ(weight × rating).

---

## Component 1: Payment Processing

### Options
1. **Build:** Custom payment gateway (direct acquirer/PSP integrations, PCI scope in-house).
2. **Buy:** Stripe, Adyen, or similar PSP.
3. **Hybrid:** Buy PSP(s) + a thin custom orchestration layer (routing, retries, reconciliation, multi-PSP).

### Evaluation Matrix
| Criterion | Weight | Build | Buy | Hybrid |
|-----------|--------|-------|-----|--------|
| Time to market | 20% | 2/5 | 5/5 | 4/5 |
| Total cost (3yr) | 25% | 2/5 | 4/5 | 3/5 |
| Flexibility (multi-market, multi-PSP) | 20% | 5/5 | 3/5 | 5/5 |
| Maintenance burden (incl. PCI) | 15% | 1/5 | 5/5 | 3/5 |
| Strategic value | 20% | 3/5 | 3/5 | 4/5 |
| **Weighted Score** | | **2.60** | **3.95** | **3.85** |

*Calc: Build = .20·2+.25·2+.20·5+.15·1+.20·3 = 2.60; Buy = .20·5+.25·4+.20·3+.15·5+.20·3 = 3.95; Hybrid = .20·4+.25·3+.20·5+.15·3+.20·4 = 3.85.*

### Recommendation
**Buy (Stripe/Adyen)** now, with a **thin orchestration seam** designed in so we
can evolve toward Hybrid as multi-market payment needs mature.

### Rationale
Payments are undifferentiated for GlobalCart — customers don't choose us for our
gateway — and building means owning PCI scope, fraud, and per-market acquirer
integrations we're not staffed for. Buy scores highest and gets us live in Market
#1 fastest. We keep a small orchestration abstraction (our own payment-intent
API in front of the PSP) so that if a specific market needs a local PSP or we
want multi-PSP routing later, we add it without a rewrite. That preserves the
Hybrid upside at Buy's speed and cost.

### Implementation Plan
- Phase 1–2: integrate PSP behind an internal `payments` façade; move off the
  legacy embedded flow. PSP-hosted checkout to minimize PCI scope.
- Per market: enable local payment methods/currencies via PSP config.
- Later (as needed): add orchestration for retries, reconciliation, and a
  second PSP for redundancy/local coverage.

---

## Component 2: Multi-Region Database

### Options
1. **Build:** Self-managed database clusters per region with custom replication and residency logic.
2. **Buy:** Fully managed global/distributed DB service (e.g., managed cloud DB with region pinning).
3. **Hybrid:** Managed regional databases + our own data-residency routing and partitioning layer.

### Evaluation Matrix
| Criterion | Weight | Build | Buy | Hybrid |
|-----------|--------|-------|-----|--------|
| Time to market | 20% | 2/5 | 4/5 | 4/5 |
| Total cost (3yr) | 25% | 2/5 | 3/5 | 4/5 |
| Flexibility (residency control) | 20% | 5/5 | 3/5 | 5/5 |
| Maintenance burden | 15% | 1/5 | 5/5 | 3/5 |
| Compliance fit (GDPR/residency) | 20% | 4/5 | 3/5 | 5/5 |
| **Weighted Score** | | **2.65** | **3.50** | **4.25** |

*Calc: Build = .20·2+.25·2+.20·5+.15·1+.20·4 = 2.65; Buy = .20·4+.25·3+.20·3+.15·5+.20·3 = 3.50; Hybrid = .20·4+.25·4+.20·5+.15·3+.20·5 = 4.25.*

### Recommendation
**Hybrid** — managed regional databases with a GlobalCart-owned residency/
routing layer that pins each customer's PII to the correct region.

### Rationale
Pure Buy (a single global managed DB) simplifies ops but gives us weaker control
over *where specific data physically lives* — the exact thing GDPR/residency
demands. Pure Build gives control but at unacceptable ops burden for a small SRE
team. Hybrid gets managed reliability *and* precise residency: we let the cloud
run the engines, and we own only the thin routing/partitioning logic that
compliance actually requires. Highest weighted score and best compliance fit.

### Implementation Plan
- Phase 2: stand up managed DBs in target regions; build the residency-aware
  data-access layer (region resolved from tenant/customer).
- Classify data (PII vs non-PII); pin PII in-region, allow non-PII global
  read replicas for performance.
- Legal sign-off on data flows as a launch gate per market.

---

## Component 3: B2B Commerce Engine

### Options
1. **Build:** Custom B2B engine (org accounts, buyer roles, contract/tiered pricing, quotes, approvals).
2. **Buy:** B2B commerce SaaS/platform.
3. **Hybrid:** Buy base commerce SaaS + build custom pricing/quoting on top.

### Evaluation Matrix
| Criterion | Weight | Build | Buy | Hybrid |
|-----------|--------|-------|-----|--------|
| Time to market | 15% | 2/5 | 5/5 | 4/5 |
| Total cost (3yr) | 20% | 3/5 | 3/5 | 3/5 |
| Flexibility / fit to our model | 25% | 5/5 | 2/5 | 4/5 |
| Maintenance burden | 15% | 2/5 | 5/5 | 3/5 |
| Strategic value (differentiation) | 25% | 5/5 | 2/5 | 4/5 |
| **Weighted Score** | | **3.65** | **3.05** | **3.65** |

*Calc: Build = .15·2+.20·3+.25·5+.15·2+.25·5 = 3.65; Buy = .15·5+.20·3+.25·2+.15·5+.25·2 = 3.05; Hybrid = .15·4+.20·3+.25·4+.15·3+.25·4 = 3.65.*

### Recommendation
**Build.** Build and Hybrid tie numerically (3.65), but Build wins on the two
highest-weighted criteria — **flexibility and strategic value (25% each)** —
which is where the tie should break for a differentiating capability.

### Rationale
The B2B platform *is* the new business line; contract pricing, org hierarchies,
approvals, and quoting are exactly where GlobalCart differentiates and where SaaS
products force us into their model. Buy scores lowest despite fast time-to-market
because low fit and low strategic value dominate. We build the engine on our own
extracted commerce foundations (catalog, ordering, identity) so it reuses shared
platform rather than duplicating it. Scope is controlled via an MVP-first
approach to protect budget and hiring capacity.

### Implementation Plan
- Phase 3: build B2B engine — org accounts, RBAC buyers, contract/tiered
  pricing, quote-to-order — on top of extracted catalog + ordering services.
- MVP first (accounts + contract pricing + quotes); defer advanced approvals/
  punch-out/EDI to the 2027 runway.
- Reuse the managed IdP for B2B SSO and the region layer for compliant B2B data.

---

## Summary Matrix

| Component | Decision | Investment | Timeline |
|-----------|----------|------------|----------|
| Payments | **Buy** (Stripe/Adyen) + thin orchestration seam | ~$50K/yr | Q1–Q2 2025 |
| Multi-Region Database | **Hybrid** (managed DBs + residency layer) | ~$300K | Q3–Q4 2025 |
| B2B Commerce Engine | **Build** on shared platform | ~$500K | Q1–Q2 2026 |

**Portfolio logic:** buy/borrow speed for commodity (payments), take managed
reliability while keeping compliance control (data), and invest build capacity
only where it creates new revenue and differentiation (B2B) — all within the
$2M/yr budget and 10-hire/yr constraint.
