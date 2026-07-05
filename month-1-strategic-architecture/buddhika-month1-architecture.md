# Enterprise Architecture Plan

> **Author:** Buddhika Amarasinghe
> **Role:** Tech Lead
> **Company:** GlobalCart (regional e-commerce, expanding internationally + B2B)
> **Horizon:** 2025–2028

---

## Executive Summary

GlobalCart is a five-year-old, single-country e-commerce business serving 2M
monthly active users on a monolithic platform and generating $50M in annual
revenue. Over the next three years the business intends to enter five
international markets, launch a B2B wholesale line, grow to 10M MAU, and reach
$200M in revenue. The current monolith cannot absorb that growth: it has no
concept of region, tenancy, or data residency, and its coupling means every
change is a full-system risk. Left unaddressed, the architecture — not the
market — becomes the ceiling on the business.

This plan proposes an **evolutionary transition to a modular, region-aware
platform** rather than a big-bang rewrite. We keep the monolith running and
revenue-generating while carving out the capabilities that expansion actually
requires — an API gateway, an externalized identity/auth service, a
region-aware data layer, and a dedicated B2B commerce capability — using the
strangler-fig pattern. This sequencing lets us deliver the first international
market in 2025, launch B2B in early 2026, and reach 10M-MAU scale by end of
2026, all inside a $2M annual technology budget and a hiring cap of 10
engineers per year.

The strategy is deliberately **buy-where-undifferentiated, build-where-we-win**:
we buy payments, identity infrastructure, and managed data platforms so scarce
engineering capacity is spent on the B2B commerce engine and the customer
experience that actually differentiate GlobalCart. Governance is lightweight but
real — a small Architecture Review process, a set of published principles, and
Architecture Decision Records — designed to enable teams, not to gate them.

---

## Current State Assessment

### System Landscape

GlobalCart today is a single deployable monolith backed by one primary
relational database, fronted by a load balancer and CDN, running in one region.

```
                         ┌─────────────┐
        Customers ─────► │     CDN     │
        (single          └──────┬──────┘
         country)               │
                         ┌──────▼──────┐
                         │Load Balancer│
                         └──────┬──────┘
                                │
                   ┌────────────▼─────────────┐
                   │   MONOLITH (5 yrs old)    │
                   │  ┌──────┐ ┌──────┐        │
                   │  │Catalog│ │Cart  │        │
                   │  ├──────┤ ├──────┤        │
                   │  │Orders │ │Auth  │        │
                   │  ├──────┤ ├──────┤        │
                   │  │Payment│ │Search│        │
                   │  ├──────┤ ├──────┤        │
                   │  │Users  │ │Promo │        │
                   │  └──────┘ └──────┘        │
                   └────┬──────────────┬───────┘
                        │              │
                 ┌──────▼─────┐  ┌─────▼──────┐
                 │ Primary DB │  │ 3rd-party  │
                 │ (single    │  │ payment    │
                 │  region)   │  │ provider   │
                 └────────────┘  └────────────┘
```

**Characteristics**
- One codebase, one deploy pipeline, one release cadence (all-or-nothing).
- All modules share one schema; foreign keys cross domain boundaries freely.
- Authentication is embedded in the monolith (custom session logic).
- Single region; no data-residency controls; no tenancy model.
- Batch/reporting queries run against the primary DB, competing with live traffic.

### Capability Analysis

| Capability | Current State | Gap | Priority |
|--------------------------|---------------------------|-----------------------------|----------|
| Multi-region deployment | None (single region) | Critical | High |
| Data residency (GDPR) | None | Critical (legal blocker) | High |
| B2B support (accounts, pricing, quotes) | None | Required for new revenue | High |
| Externalized identity/auth | Embedded in monolith | Blocks SSO, B2B, scale | High |
| API gateway / edge routing | None | Needed for decoupling | High |
| Horizontal scalability | Limited (monolith + one DB)| Needed for 10M MAU | Medium |
| Independent deployability | None (single artifact) | Needed for velocity | Medium |
| Observability (traces/SLOs)| Basic logs/metrics only | Needed to operate at scale | Medium |
| Search & catalog scale | In-DB, degrades under load | Needed for 5x growth | Medium |
| Internationalization (i18n, currency, tax) | Single locale/currency | Required for expansion | High |

### Technical Debt Inventory

*(Summarized here; full strategy in `buddhika-month1-techdebt.md`.)*

| Category | Debt | Business Impact |
|----------|------|-----------------|
| Structural | Tight coupling across domains in one schema | Every change is high-risk; blocks independent scaling and expansion |
| Security | Custom, embedded auth; no MFA, weak session mgmt | Security/compliance risk; blocks B2B SSO and market entry |
| Data | Single-region DB, no residency controls | **Legal blocker** for GDPR markets |
| Operational | Single pipeline, manual release steps | Low deployment frequency; slow, risky releases |
| Scalability | Reporting on primary DB; no caching tier | Performance degrades under peak load |

---

## Target Architecture

### Vision

By 2028, GlobalCart operates a **region-aware, modular commerce platform** where
independently deployable services share a common gateway and identity layer, data
is stored in the region it belongs to (satisfying GDPR/residency by design), and
a dedicated B2B capability runs alongside the consumer storefront on shared
foundations. Teams own services end-to-end, ship independently, and are guided —
not gated — by published principles and a lightweight review process. The
platform scales to 10M+ MAU horizontally, and undifferentiated concerns
(payments, identity infra, managed data) are bought so engineering focuses on
what makes GlobalCart win.

### Architecture Principles

1. **Business capability first.** We organize systems and teams around business
   capabilities (catalog, ordering, identity, B2B), not technical layers.
   *Rationale:* aligns ownership with value, reduces cross-team coupling, makes
   the org chart and the architecture reinforce each other.

2. **Evolve, don't rewrite.** We extract capabilities from the monolith
   incrementally (strangler-fig) and keep it revenue-generating throughout.
   *Rationale:* a big-bang rewrite at our size, budget, and hiring cap is the
   single highest risk to the business; incremental delivery de-risks value.

3. **Buy the undifferentiated, build the differentiating.** Default to buying
   commodity capabilities (payments, identity infra, managed DB); build only
   where GlobalCart competes (B2B commerce, customer experience).
   *Rationale:* scarce capacity (10 hires/yr, $2M budget) must go to
   revenue-differentiating work.

4. **Region-aware and compliant by design.** Data residency, locale, currency,
   and tax are first-class platform concerns, not per-feature retrofits.
   *Rationale:* GDPR/residency are legal blockers; retrofitting compliance is
   far more expensive and risky than designing for it.

5. **Independently deployable, loosely coupled.** Services communicate over
   well-defined APIs/events and own their data; no shared database across
   capabilities. *Rationale:* enables independent scaling, faster/safer
   releases, and team autonomy.

6. **Observable and operable by default.** Every service ships with health,
   metrics, tracing, and an SLO. *Rationale:* you cannot operate at 10M MAU or
   run five regions on logs alone.

### System Context Diagram (C4 Level 1)

```
        ┌──────────────┐        ┌──────────────┐        ┌──────────────┐
        │  Consumer    │        │  B2B Buyer   │        │  Ops / Admin │
        │  Shopper     │        │  (wholesale) │        │  Staff       │
        └──────┬───────┘        └──────┬───────┘        └──────┬───────┘
               │                       │                       │
               └───────────┬──────────┴───────────┬───────────┘
                           │                       │
                   ┌───────▼───────────────────────▼────────┐
                   │        GlobalCart Platform              │
                   │  (consumer storefront + B2B portal +    │
                   │   commerce core, region-aware)          │
                   └───┬──────────┬──────────┬──────────┬────┘
                       │          │          │          │
              ┌────────▼──┐ ┌─────▼────┐ ┌───▼─────┐ ┌──▼────────┐
              │ Payment   │ │ Identity │ │ Tax /   │ │ Shipping/ │
              │ Provider  │ │ Provider │ │ VAT svc │ │ Logistics │
              │ (Stripe/  │ │ (Auth0/  │ │         │ │ carriers  │
              │  Adyen)   │ │  Cognito)│ │         │ │           │
              └───────────┘ └──────────┘ └─────────┘ └───────────┘
```

### Container Diagram (C4 Level 2)

```
  Consumers / B2B Buyers / Admin
                │
        ┌───────▼────────┐
        │  CDN / Edge     │
        └───────┬────────┘
                │
        ┌───────▼────────────────┐
        │      API Gateway        │  auth, routing, rate-limit, region-route
        └──┬────┬────┬────┬───┬───┘
           │    │    │    │   │
   ┌───────▼─┐ ┌▼───────┐ ┌▼──────────┐ ┌▼────────────┐ ┌▼──────────────┐
   │ Auth /  │ │Catalog │ │  Ordering  │ │  B2B         │ │  Strangler     │
   │Identity │ │Service │ │  Service   │ │  Commerce    │ │  Facade →      │
   │Service  │ │        │ │            │ │  Engine      │ │  MONOLITH      │
   │(buy)    │ │        │ │            │ │  (build)     │ │  (remaining    │
   └────┬────┘ └───┬────┘ └─────┬──────┘ └──────┬───────┘ │   modules)     │
        │          │            │               │         └───────┬────────┘
   ┌────▼───┐ ┌────▼────┐ ┌─────▼─────┐  ┌──────▼──────┐   ┌──────▼───────┐
   │Identity│ │Catalog  │ │Orders DB  │  │ B2B DB      │   │ Legacy DB    │
   │store   │ │DB+Search│ │(region-   │  │(accounts,   │   │(shrinking)   │
   │        │ │index    │ │ partition)│  │ pricing)    │   │              │
   └────────┘ └─────────┘ └───────────┘  └─────────────┘   └──────────────┘

           ┌───────────────── Event Bus (async) ─────────────────┐
           │  order.placed, inventory.updated, price.changed ...  │
           └──────────────────────────────────────────────────────┘

  Cross-cutting: Observability (traces/metrics/logs), CI/CD, IaC, Secrets
```

### Key Architecture Decisions

| Decision | Options Considered | Choice | Rationale |
|----------|--------------------|--------|-----------|
| Migration approach | (a) Big-bang rewrite, (b) Strangler-fig incremental, (c) Lift-and-shift only | **Strangler-fig incremental** | Keeps revenue flowing; de-risks; fits budget/hiring limits |
| Decoupling entry point | (a) Split DB first, (b) API gateway + facade first, (c) UI micro-frontends | **API gateway + facade first** | Enables routing/extraction without touching core data yet |
| Identity/auth | (a) Keep custom, (b) Build new service, (c) Buy managed IdP | **Buy managed IdP** | Security + SSO + B2B org support out of the box; frees capacity |
| Data residency | (a) One global DB, (b) Region-partitioned data, (c) Full per-region stacks | **Region-partitioned data** with regional stores for PII | Meets GDPR/residency without 5x infra cost |
| B2B platform | (a) Buy SaaS B2B, (b) Build engine, (c) Extend monolith | **Build B2B commerce engine** | Core differentiator; SaaS can't model our pricing/accounts |
| Service communication | (a) Sync REST only, (b) Async events only, (c) Hybrid REST + events | **Hybrid**: REST for queries, events for state changes | Balances simplicity, decoupling, and consistency needs |
| Infrastructure | (a) Self-managed, (b) Managed cloud services | **Managed cloud services** | Small ops team; managed reduces toil and time-to-market |

---

## Governance

### Architecture Review Process

- **Architecture Review Board (ARB):** Tech Lead + 2–3 rotating senior engineers.
  Meets weekly (30–45 min); asynchronous review is the default.
- **What requires review:** new services, cross-team API/contract changes,
  datastore choices, anything touching auth, payments, or personal data
  (GDPR-relevant), and any decision hard/expensive to reverse ("one-way doors").
- **What does not:** changes fully inside a team's own service boundary
  ("two-way doors"). Teams stay autonomous by default.
- **Mechanism:** proposals arrive as a short **ADR** (context, decision, options,
  consequences). ARB reviews async; only contested items go to the meeting.
  Target turnaround: 3 business days.

### Standards and Guidelines

- **Golden paths, not mandates:** a recommended stack, service template, CI/CD
  pipeline, and observability defaults that make the right way the easy way.
- **API standards:** versioned, backward-compatible, documented (OpenAPI);
  breaking changes require deprecation windows.
- **Security & compliance baseline:** SSO via the managed IdP, secrets in a
  vault, PII classified and region-scoped, no PII in logs.
- **Every service ships with:** health checks, metrics, tracing, an SLO, an owner,
  and a runbook.

### Exception Handling

- Any team may request an exception to a standard via a **time-boxed ADR** stating
  the reason, the risk accepted, and an **expiry/review date**.
- Exceptions are logged in a public register (an ADR *is* the record) so debt is
  visible, not hidden. The ARB reviews expiring exceptions to decide renew,
  remediate, or promote-to-standard.
- Principle: **exceptions are cheap to request and always visible** — this keeps
  governance from becoming a bottleneck while preventing silent divergence.
