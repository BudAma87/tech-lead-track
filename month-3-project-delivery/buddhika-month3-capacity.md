# Capacity Planning: Microservices Migration

> **Author:** Buddhika Amarasinghe · **Role:** Tech Lead

## Team Composition

| Name | Role | Availability | Skills |
|------|------|--------------|--------|
| Tech Lead | Lead | 80% (20% other duties) | Full stack, K8s |
| Senior 1 | Engineer | 100% | Backend, Java |
| Senior 2 | Engineer | 50% (split project) | Backend, Node |
| Senior 3 | Engineer | 100% | Full stack |
| Mid 1 | Engineer | 100% | Frontend, learning K8s |
| Mid 2 | Engineer | 50% (split project) | Backend |

**Total Capacity:** 0.8 + 1.0 + 0.5 + 1.0 + 1.0 + 0.5 = **4.8 FTE**

## Velocity Analysis

### Historical Velocity
- Team average (full team, steady state): 32 story points/sprint
- With current allocation (4.8 FTE + ramp): ~25 points/sprint expected

### Project Estimation
| Phase | Story Points | Sprints | Duration |
|-------|--------------|---------|----------|
| Foundation | 80 | 3-4 | 6-8 weeks |
| Core Services | 200 | 8-10 | 16-20 weeks |
| Supporting | 120 | 5-6 | 10-12 weeks |
| Cutover | 50 | 2-3 | 4-6 weeks |
| **Total** | **450** | **18-23** | **36-46 weeks** |

**Analysis:** At the optimistic end (18 sprints) the plan fits the 36-week
timeline; at the realistic end (with risk drag) it slips past it. Current capacity
is **borderline insufficient** for the 36-week target once risks are applied.

## Capacity Gap Analysis

**Required Velocity:** 450 points / 18 sprints = **25 points/sprint** ✓ (matches adjusted velocity)

**Risk Factors:**
- Learning curve: -20% for first 8 weeks (~-20 points total)
- Integration delays: -10% estimated (~-45 points equivalent effort)
- Holiday season / summer leave: -2 sprints

**Adjusted Timeline:** ~42 weeks (with risks) — a **~6-week gap** vs. the 36-week
plan, which lands the project uncomfortably close to the Black Friday freeze.

## Mitigation Options

### Option 1: Add Contractor (Recommended)
**Cost:** $50K for 6 months
**Impact:** +0.8 FTE, +8 points/sprint
**Timeline Impact:** Back to ~36 weeks
**Why recommended:** Closes the gap without cutting scope or risking the freeze;
contractor can absorb well-scoped supporting-service work while the core team
handles high-coupling extractions.

### Option 2: Reduce Scope
**Approach:** Defer 2 non-critical services (e.g., Search, Cart enhancements)
**Impact:** -80 points
**Timeline Impact:** ~32 weeks
**Trade-off:** Faster, cheaper, but leaves part of the monolith alive longer.

### Option 3: Extend Timeline
**New End Date:** November 2025
**Impact:** Black Friday risk returns (cutovers near/into the freeze)
**Not Recommended**

## Sprint-by-Sprint Plan

### Sprint 1-2: Foundation
| Engineer | Focus | Points |
|----------|-------|--------|
| Tech Lead | K8s architecture | 8 |
| Senior 1 | CI/CD setup | 8 |
| Senior 3 | Auth service | 8 |
| Mid 1 | Learning/support | 2 |
| **Total** | | **26** |

### Sprint 3-4: User + Product (early Core)
| Engineer | Focus | Points |
|----------|-------|--------|
| Senior 2 (50%) | User service | 5 |
| Senior 3 | Product service | 8 |
| Senior 1 | Dual-write + reconciliation | 8 |
| Tech Lead (80%) | Design review + unblock | 4 |
| Mid 1 | Product service support | 3 |
| **Total** | | **28** |

### Sprint 7-8: Order Service (peak coupling)
| Engineer | Focus | Points |
|----------|-------|--------|
| Senior 1 | Order service (lead) | 9 |
| Senior 3 | Order integration + tests | 7 |
| Mid 2 (50%) | Data migration scripts | 4 |
| Tech Lead (80%) | Cutover planning + review | 4 |
| **Total** | | **24** |

### Sprint 11-12: Supporting Services + Integrations
| Engineer | Focus | Points |
|----------|-------|--------|
| Senior 1 | Payment service | 8 |
| Senior 3 | Notification + contract tests | 7 |
| Senior 2 (50%) | Inventory service | 5 |
| Mid 1 | Cart service | 4 |
| **Total** | | **24** |

*(Contractor, if approved, adds ~8 pts/sprint from Sprint 5 onward.)*

## Leave and Availability

| Month | Planned Leave | Impact |
|-------|---------------|--------|
| July | 3 weeks total | -1 sprint capacity |
| August | 2 weeks total | Minimal |
| December | 4 weeks total | -2 sprint capacity |

## Capacity Monitoring

**Weekly Tracking:**
- Actual vs planned velocity
- Availability changes (half-time engineers pulled further?)
- Scope changes

**Triggers for Re-planning:**
- Velocity < 80% target for 2 sprints
- Unplanned absence > 1 week
- Scope increase > 50 points
