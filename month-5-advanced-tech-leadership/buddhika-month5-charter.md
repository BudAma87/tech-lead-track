# Team Charter: Platform Engineering

## Our Mission

**What we do:**
Build and operate the foundation that enables product teams to deliver value to
customers quickly and reliably.

**Why it matters:**
Every product feature depends on our platform. Our reliability is their
reliability. Our speed is their speed. We win when they win — invisibly.

---

## Our Values

> Values are only real when they show up in how we behave on a bad day. Each
> below names *what it means* and *how we actually live it* — if we can't point
> to a behavior, it's a slogan, not a value.

### 1. Ownership
**What it means:**
We own our systems end-to-end. If it's broken, we fix it. If it's not
documented, we document it. "Not my service" is not an answer when a customer
is affected.

**How we live it:**
- On-call rotation with the authority and access to actually fix things
- Documentation and runbooks kept current as part of "done"
- Proactive monitoring and improvement, not reactive firefighting

### 2. Collaboration
**What it means:**
We succeed together. We help before being asked. We share knowledge generously
instead of hoarding it as job security.

**How we live it:**
- Pairing and mobbing encouraged for hard or high-context problems
- Regular knowledge-sharing sessions; no single points of human failure
- Help first, then dig into the why

### 3. Growth
**What it means:**
Everyone here should be more capable in a year than they are today. We trade
some short-term efficiency for long-term capability — including letting people
struggle productively.

**How we live it:**
- Stretch assignments matched to growth goals from 1:1s
- Senior engineers mentor; juniors are given real ownership early
- Mistakes in service of learning are expected and supported

### 4. Craft
**What it means:**
We take pride in work that is correct, maintainable, and humane to operate —
not just work that ships. Quality is a feature, especially for a platform.

**How we live it:**
- Code review focuses on clarity and long-term cost, not just correctness
- We pay down technical debt deliberately, not only when it hurts
- We measure ourselves by how easy our systems are to change and run

---

## How We Work

### Decision Making

**Types of Decisions:**

| Type | Who Decides | Process |
|------------------|---------------------|---------------|
| Architectural | Tech Lead + team | RFC process |
| Daily technical | Individual engineer | Inform team |
| Team process | Team consensus | Team meeting |
| People/hiring | Tech Lead + EM | Collaboration |

**Disagreement Resolution:**
1. Discuss directly in a 1:1 or small group first
2. Bring data and 2+ options to the team (not just a complaint)
3. Tech Lead makes the call if the team can't converge in reasonable time
4. **Disagree and commit** — once decided, we all row in the same direction

> We optimize for *good decisions made at a reasonable speed*, not perfect
> decisions made too late. Most decisions are two-way doors — reversible — and
> should be made fast by whoever is closest to the work.

### Communication

**Async-First Principles:**
- Default to written communication; it scales, includes, and creates a record
- Meetings have an agenda and produce notes, or they don't happen
- Respect focus time — not everything is urgent

**Channels:**
| Channel | Use For |
|------------------|--------------------------------|
| Slack #platform | Quick questions, updates |
| Email | External stakeholders, formal |
| Docs | Decisions, designs, knowledge |
| Meeting | Discussion, alignment, relationships |

**Response Expectations:**
| Channel | Expected Response |
|---------------|-------------------|
| Urgent Slack | 1 hour |
| Normal Slack | Same day |
| Email | Next business day |
| Code review | 24 hours |

### Meetings

**Standing Meetings:**
| Meeting | When | Duration | Purpose |
|----------------|----------|----------|-----------|
| Daily standup | 9:30 AM | 15 min | Sync + unblock |
| Sprint planning | Monday | 2 hours | Plan |
| Retro | Friday | 1 hour | Improve |
| Tech discussion | Thursday | 1 hour | Deep dive / RFC review |

**Meeting Norms:**
- On time, or notify the group
- Cameras on when possible (for remote/hybrid connection)
- One speaker at a time; make space for quieter voices
- Notes documented and shared

---

## Team Norms

### Code and Quality
- All code reviewed before merge
- Tests required for new features and bug fixes
- Documentation updated *with* the change, not "later"
- On-call runbooks kept current

### Work-Life
- Core collaboration hours: 10 AM – 4 PM (local); flex around that
- Respect out-of-office and off-hours; no expectation of after-hours replies
- Sustainable pace is a team commitment, not a perk
- Take your vacation — and we cover for each other so you can disconnect

### Feedback
- Give feedback regularly and close to the event, not saved for review season
- Assume good intent
- Focus on behavior and impact, not character
- Praise in public, critique in private

---

## Success Metrics

### Team Health
| Metric | Target | How Measured |
|-------------------|--------|----------------|
| Team satisfaction | 4.5/5 | Quarterly survey |
| Attrition | <10%/year | HR data |
| Growth feedback | Positive | 1:1s |
| Onboarding time | <2 weeks to first ship | New-hire tracking |

### Delivery
| Metric | Target | How Measured |
|------------------|--------|--------------|
| Sprint completion | 80%+ | Jira |
| Incidents | <2/month | PagerDuty |
| Deploy success | 99%+ | CI/CD |
| Change lead time | <1 day | CI/CD analytics |

---

## Growth Paths

### Individual Growth
- Regular 1:1s with explicit career discussions, not just status
- Learning budget: $2,000/year/person
- Conference attendance encouraged (and shared back with the team)
- Internal mobility supported, even when it costs us a good engineer

### Team Growth
- Tech talks and knowledge sharing on a regular cadence
- Cross-training to remove single points of human failure
- Stretch assignments and rotating responsibilities (incident commander, etc.)
- Structured mentorship pairing seniors with juniors

---

## Charter Maintenance

**Review Cadence:** Quarterly
**Owner:** Tech Lead
**Update Process:** Team discussion and consensus; changes logged
**Last Updated:** 2026-06-18

> This charter is a living document. If a norm here no longer matches how we
> actually (and rightly) work, we change the charter — we don't quietly ignore
> it. A charter that collects dust is worse than none, because it teaches the
> team that written commitments don't matter.
