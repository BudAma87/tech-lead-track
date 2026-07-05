# Project Risk Assessment: Microservices Migration

> **Author:** Buddhika Amarasinghe · **Role:** Tech Lead · **Project:** Monolith → 8 Microservices

## Risk Register

### Risk 1: Data Consistency During Migration
**Category:** Technical
**Probability:** High (4/5)
**Impact:** Critical (5/5)
**Risk Score:** 20 (Critical)

**Description:**
During the cutover period, data could become inconsistent between the monolith and
new microservices, leading to business data loss or corruption.

**Indicators:**
- Data sync errors in logs
- Customer complaints about missing data
- Reconciliation report discrepancies

**Mitigation Strategies:**
1. **Primary:** Implement dual-write with reconciliation
   - Owner: Tech Lead
   - Timeline: Before Phase 2
   - Cost: 2 weeks engineering effort
2. **Secondary:** Feature flag for rollback capability
   - Owner: Senior Engineer
   - Timeline: Before each cutover
   - Cost: 1 week per service

**Contingency Plan:**
If data inconsistency detected:
1. Immediately halt migration
2. Route all traffic to monolith
3. Run reconciliation process
4. Fix issue before resuming

---

### Risk 2: Team Knowledge Gaps
**Category:** Resource
**Probability:** Medium (3/5)
**Impact:** High (4/5)
**Risk Score:** 12 (High)

**Description:**
The team has limited Kubernetes and distributed-systems experience (Mid 1 is
actively learning K8s). Knowledge gaps slow delivery and increase the chance of
design mistakes in service boundaries and inter-service communication.

**Indicators:**
- Estimates repeatedly exceeded on infra tasks
- Rework on service boundaries / API contracts
- Bottlenecking on the one or two people who "know K8s"

**Mitigation Strategies:**
1. **Primary:** Targeted upskilling + pairing
   - Owner: Tech Lead
   - Timeline: Weeks 1–8 (front-loaded)
   - Cost: ~20% velocity in first 8 weeks (already budgeted)
2. **Secondary:** External K8s/architecture consultant for design reviews
   - Owner: Engineering Manager
   - Timeline: Phase 1–2 checkpoints
   - Cost: ~$15K

**Contingency Plan:**
If knowledge gap blocks a milestone:
1. Reassign the task to the strongest available engineer
2. Bring in consultant for a focused design session
3. Simplify the first service to reduce novelty, defer advanced patterns

---

### Risk 3: Third-Party Integration Failures
**Category:** Technical / External
**Probability:** Medium (3/5)
**Impact:** High (4/5)
**Risk Score:** 12 (High)

**Description:**
Extracted services must re-establish integrations (payment provider, email/SMS,
analytics, shipping). Contract mismatches, auth changes, or rate limits can break
integrations that "just worked" inside the monolith.

**Indicators:**
- API error-rate spikes on integration endpoints
- Timeout / rate-limit responses from providers
- Failed webhook deliveries

**Mitigation Strategies:**
1. **Primary:** Anti-corruption layer + contract tests per integration
   - Owner: Tech Lead
   - Timeline: Per integration, before its cutover
   - Cost: 1 week per integration
2. **Secondary:** Provider sandbox testing + staged rollout with monitoring
   - Owner: Senior 3
   - Timeline: Before each go-live
   - Cost: 2 days per integration

**Contingency Plan:**
If an integration fails in production:
1. Fall back to the monolith path for that integration (feature flag)
2. Engage provider support with captured request/response evidence
3. Fix, re-run contract tests, then re-cut over

---

### Risk 4: Performance Degradation
**Category:** Technical
**Probability:** Medium (3/5)
**Impact:** High (4/5)
**Risk Score:** 12 (High)

**Description:**
Splitting a monolith introduces network hops, serialization, and chatty calls.
Poorly designed boundaries can degrade latency/throughput below the 99.9% SLA and
customer expectations.

**Indicators:**
- p95/p99 latency rising after a service goes live
- Increased error budget burn
- Database connection saturation / N+1 cross-service calls

**Mitigation Strategies:**
1. **Primary:** Performance budget + load testing before each cutover
   - Owner: Senior 1
   - Timeline: Before each service go-live
   - Cost: 3 days per service
2. **Secondary:** Caching, async messaging, and coarse-grained APIs by design
   - Owner: Tech Lead
   - Timeline: Design phase of each service
   - Cost: Built into estimates

**Contingency Plan:**
If SLA breached after cutover:
1. Roll back to monolith via feature flag
2. Profile the hot path; add caching / batch calls
3. Re-test against performance budget before retry

---

### Risk 5: Black Friday Timeline Pressure
**Category:** Schedule / Business
**Probability:** Low (2/5)
**Impact:** Medium (3/5)
**Risk Score:** 6 (Medium)

**Description:**
The November Black Friday freeze removes ~4 weeks of deployment capacity at a
critical point. Compressing work around the freeze risks rushed, lower-quality
cutovers, while the two half-time engineers reduce absorptive capacity.

**Indicators:**
- Milestones bunching up right before the freeze
- Pressure to deploy risky changes late October
- Schedule float trending toward zero before November

**Mitigation Strategies:**
1. **Primary:** Sequence risky cutovers well before/after the freeze; freeze = stabilize-only
   - Owner: Tech Lead
   - Timeline: Reflected in the timeline plan
   - Cost: Planning only
2. **Secondary:** Use the freeze for hardening, docs, and reconciliation catch-up
   - Owner: Engineering Manager
   - Timeline: November
   - Cost: None (repurposed time)

**Contingency Plan:**
If work slips into the freeze window:
1. Hold non-critical cutovers until December
2. Protect the freeze — no exceptions without VP Eng sign-off
3. Re-baseline the timeline in early December

---

## Risk Matrix

|                 | Low Impact | Medium | High | Critical |
|-----------------|-----------|--------|------|----------|
| **Very Likely** | | | | |
| **Likely** | | | | Risk 1 |
| **Possible** | | | Risk 2, 3, 4 | |
| **Unlikely** | | Risk 5 | | |

## Risk Monitoring Plan

| Risk | Review Frequency | Owner | Escalation Trigger |
|------|------------------|-------|--------------------|
| Data consistency | Daily during migration | Tech Lead | Any sync errors |
| Knowledge gaps | Weekly | Engineering Manager | Milestone slip |
| Integrations | Per integration | Tech Lead | API failures |
| Performance | Per cutover + weekly | Senior 1 | p95 > budget / SLA breach |
| Black Friday pressure | Weekly (Sep–Oct) | Tech Lead | Float < 1 week before Nov |

## Summary
- **Critical Risks:** 1 (Data consistency)
- **High Risks:** 3 (Knowledge gaps, Integrations, Performance)
- **Medium Risks:** 1 (Black Friday pressure)
- **Total Mitigation Budget:** ~8 weeks engineering effort + ~$15K consultant
