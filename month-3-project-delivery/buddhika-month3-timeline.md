# Project Timeline: Microservices Migration

> **Author:** Buddhika Amarasinghe · **Role:** Tech Lead

## Timeline Overview

**Start:** January 2025
**End:** September 2025
**Duration:** 9 months
**Team:** 6 engineers (4.8 FTE equivalent)

## Phases

### Phase 1: Foundation (Weeks 1-6)
**Theme:** Infrastructure and first service

**Milestones:**
| Week | Milestone | Deliverable | Owner |
|------|-----------|-------------|-------|
| 2 | M1.1 | K8s cluster deployed | DevOps |
| 4 | M1.2 | CI/CD pipeline for microservices | DevOps |
| 5 | M1.3 | Auth service deployed | Senior 1 |
| 6 | M1.4 | Phase 1 complete, demo | Tech Lead |

**Dependencies:**
- Cluster before CI/CD
- CI/CD before first service

**Risks:**
- K8s learning curve
- CI/CD complexity

**Exit Criteria:**
- [ ] Infrastructure operational
- [ ] Auth service handling 100% auth traffic
- [ ] Monitoring and alerting in place

---

### Phase 2: Core Services (Weeks 7-16)
**Theme:** Extract core business logic

**Services:**
1. User Service (Weeks 7-9)
2. Product Service (Weeks 9-12)
3. Order Service (Weeks 12-16)

**Milestones:**
| Week | Milestone | Deliverable | Owner |
|------|-----------|-------------|-------|
| 9 | M2.1 | User service live | Senior 2 |
| 12 | M2.2 | Product service live | Senior 3 |
| 16 | M2.3 | Order service live | Senior 1 |

**Dependencies:**
- Auth service (Phase 1) before User service
- User + Product services before Order service (orders reference both)
- Dual-write + reconciliation in place before first core cutover

**Risks:**
- Data consistency during dual-write (Critical risk)
- Order service is the largest/most coupled extraction
- Half-time availability of Senior 2 during User service

**Exit Criteria:**
- [ ] User, Product, Order services live and serving 100% of their traffic
- [ ] Reconciliation reports clean for 2 consecutive weeks per service
- [ ] Performance within budget (p95 ≤ target) for each service

---

### Phase 3: Supporting Services (Weeks 17-26)
**Theme:** Extract remaining supporting capabilities

**Services:**
4. Payment Service (Weeks 17-19)
5. Notification Service (Weeks 20-21)
6. Inventory Service (Weeks 22-24)
7. Cart Service (Weeks 24-25)
8. Search Service (Weeks 25-26)

**Milestones:**
| Week | Milestone | Deliverable | Owner |
|------|-----------|-------------|-------|
| 19 | M3.1 | Payment service live | Senior 1 |
| 21 | M3.2 | Notification service live | Senior 3 |
| 24 | M3.3 | Inventory service live | Senior 2 |
| 26 | M3.4 | Cart + Search services live | Mid 1 / Senior 3 |

**Dependencies:**
- Order service before Payment service
- Product + Order before Inventory service
- Anti-corruption layer + contract tests before each integration cutover

**Risks:**
- Third-party integration failures (Payment, Notification)
- Limited testing-environment capacity slows parallel cutovers
- Summer leave (July/August) reduces capacity

**Exit Criteria:**
- [ ] All 8 services live and independently deployable
- [ ] Integration contract tests green
- [ ] Monolith traffic reduced to residual/admin only

---

### Phase 4: Cutover & Decommission (Weeks 27-36)
**Theme:** Final cutover, stabilization, decommission

**Milestones:**
| Week | Milestone | Deliverable | Owner |
|------|-----------|-------------|-------|
| 30 | M4.1 | Final traffic cutover complete | Tech Lead |
| 33 | M4.2 | Stabilization + performance sign-off | Senior 1 |
| 35 | M4.3 | Monolith decommissioned | Tech Lead |
| 36 | M4.4 | Project close + retro | Tech Lead |

**Dependencies:**
- All services (Phase 3) live before final cutover
- Black Friday freeze respected (no cutovers in November)

**Risks:**
- Black Friday freeze compresses the schedule
- Late-surfacing data or performance issues during stabilization

**Exit Criteria:**
- [ ] 100% traffic on microservices; monolith off
- [ ] 99.9% uptime maintained throughout
- [ ] Runbooks, dashboards, and ownership handed over

---

## Critical Path

```
K8s Setup (2w) → CI/CD (2w) → Auth (2w) → User (3w)
   → Product (3w) → Order (4w)
   → Payment (3w) → Notification (2w)
   → Inventory (3w) → Final Cutover (3w)
```

**Critical Path Duration:** 27 weeks (with buffer: 36 weeks)

> Note: Cart and Search are **not** on the critical path (they run in parallel /
> have float behind Inventory), so they don't extend the 27-week core chain.

## Buffer Allocation

| Buffer Type | Allocation | Purpose |
|-------------|------------|---------|
| Phase buffer | 2 weeks/phase | Unknown unknowns |
| Integration buffer | 1 week/service | Integration issues |
| Final buffer | 4 weeks | Pre-cutover stabilization |
| **Total buffer** | 12 weeks | ~33% of timeline |

## Key Dates

| Date | Event | Implication |
|------|-------|-------------|
| Jan 6 | Project start | Phase 1 begins |
| Jun 30 | Core services done | Phase 2 exit |
| Nov 1 | Black Friday freeze | No deployments |
| Nov 30 | Freeze ends | Resume deployments |
| Sep 30 | Project deadline | All services migrated |

## Gantt Chart

```
Phase / Service        Wk: 1   4   8   12  16  20  24  28  32  36
Foundation                 [====]
  K8s/CI-CD                [==]
  Auth                       [==]
Core Services                  [============]
  User                         [===]
  Product                        [===]
  Order                            [====]
Supporting Services                    [==========]
  Payment                              [===]
  Notification                            [==]
  Inventory                                 [===]
  Cart / Search                               [===]
Cutover & Decommission                            [==========]
Black Friday FREEZE ......................................[Nov]
```

*(Attach the tool-generated Gantt visual for stakeholder distribution.)*
