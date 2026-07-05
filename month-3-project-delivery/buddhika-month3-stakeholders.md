# Stakeholder Communication Plan

> **Author:** Buddhika Amarasinghe · **Role:** Tech Lead

## Stakeholder Map

| Stakeholder | Interest | Influence | Needs |
|-------------|----------|-----------|-------|
| VP Engineering | High | High | Confidence, risks |
| Product Director | High | Medium | Timeline, features |
| CTO | Medium | High | Strategic alignment |
| Support Manager | Medium | Low | Training, impact |
| Finance | Low | Medium | Budget tracking |

**Engagement strategy (interest × influence):**
- **High/High (VP Eng):** manage closely — frequent, candid, risk-forward.
- **High influence, lower interest (CTO):** keep satisfied — concise exec summaries.
- **High interest, lower influence (Product Director, Support Mgr):** keep informed.
- **Low/low-ish (Finance):** monitor — periodic budget updates.

## Communication Matrix

| Audience | Format | Frequency | Content | Owner |
|----------|--------|-----------|---------|-------|
| VP Engineering | 1:1 | Weekly | Risks, decisions | Tech Lead |
| Product | Status email | Weekly | Progress, blockers | Tech Lead |
| CTO | Exec summary | Monthly | Strategy, milestones | Tech Lead |
| All stakeholders | Newsletter | Bi-weekly | Progress update | PM |
| Team | Stand-up | Daily | Tasks, blockers | Tech Lead |
| Finance | Budget update | Monthly | Spend vs. plan | PM |
| Support Manager | Training brief | Per service go-live | Impact, changes | Tech Lead |

## Status Report Template

### Weekly Status: Microservices Migration
**Week of:** [Date]
**Overall Status:** 🟢 On Track / 🟡 At Risk / 🔴 Off Track

#### Summary
[2-3 sentence executive summary]

#### Progress This Week
- ✅ Completed: [Items]
- 🔄 In Progress: [Items]
- ⏳ Planned: [Items]

#### Key Metrics
| Metric | Target | Actual | Trend |
|--------|--------|--------|-------|
| Velocity | 25 pts | 27 pts | ↑ |
| Services migrated | 3 | 3 | → |
| Test coverage | 80% | 78% | ↓ |
| Uptime | 99.9% | 99.95% | → |

#### Risks & Issues
| Item | Status | Action |
|------|--------|--------|
| [Risk/Issue] | [Status] | [Next step] |

#### Decisions Needed
- [ ] [Decision with deadline]

#### Next Week Focus
- [Priority 1]
- [Priority 2]

---

## Escalation Framework

### Level 1: Team Resolution
**Triggers:** Minor blockers, routine issues
**Timeline:** 1-2 days
**Owner:** Tech Lead

### Level 2: Management
**Triggers:** Resource conflicts, timeline risk
**Timeline:** 1 week
**Owner:** Engineering Manager

### Level 3: Executive
**Triggers:** Significant budget/timeline impact
**Timeline:** Immediate
**Owner:** VP Engineering

## Communication Scenarios

### Scenario: Milestone at Risk
**Audience:** VP Engineering, Product Director
**Format:** Urgent meeting (30 min)
**Content:**
1. Current situation (facts only)
2. Root cause analysis
3. Options with trade-offs
4. Recommendation
5. Ask for decision

**Template:**
> "We've identified that M2.2 (Product service) is at risk of a 2-week delay due to
> integration contract mismatches. We see three options:
> 1. Add resources (+$X, on time)
> 2. Reduce scope (defer Search service)
> 3. Accept delay (impacts Phase 3 start)
> We recommend option 1 because it protects the Black Friday freeze buffer."

### Scenario: Major Success
**Audience:** All stakeholders
**Format:** Email + team celebration
**Content:**
1. Achievement and significance
2. Team recognition
3. What this enables
4. Next milestone

**Template:**
> "🎉 Order service is live and serving 100% of traffic with p95 latency 15% better
> than the monolith. Huge credit to Senior 1 and the team. This unblocks Payment
> and Inventory extraction — next milestone M3.1 (Payment) in 3 weeks."

### Scenario: Production Incident During Migration
**Audience:** VP Engineering, Support Manager (+ CTO if prolonged)
**Format:** Incident channel + short written update every 30 min
**Content:**
1. Impact (who/what is affected) and current status
2. Action taken (e.g., rolled back to monolith via feature flag)
3. ETA to resolution
4. Follow-up: post-incident review scheduled

**Template:**
> "Incident: elevated errors on checkout after Order cutover at 14:05. Action:
> rolled traffic back to monolith at 14:12; checkout stable. Root cause under
> investigation. No data loss (reconciliation clean). PIR scheduled tomorrow."
