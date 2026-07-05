# Stakeholder Analysis: Real-Time Analytics Platform

> **Author:** Buddhika Amarasinghe · **Role:** Tech Lead

## Stakeholder Register

### Key Stakeholders

#### Sarah — VP Product
**Role:** Executive sponsor on Product side
**Interest Level:** High
**Influence Level:** High
**Current Stance:** Supportive but concerned about timeline
**Motivations:**
- Wants market differentiation
- Needs to hit Q3 revenue targets
- Cares about customer feedback
**Concerns:**
- Engineering velocity
- Feature scope creep
- Competitive pressure
**Preferred Communication:** Weekly 1:1, data-driven
**Engagement Strategy:**
- Share weekly progress with metrics
- Involve in key architecture decisions
- Align on MVP scope upfront

---

#### Mike — Design Director
**Role:** Controls design resource allocation
**Interest Level:** Medium
**Influence Level:** Medium-High
**Current Stance:** Skeptical — team overloaded
**Motivations:**
- Predictable timelines and clear scope
- Team wellbeing (avoiding burnout)
- Growth opportunities for his designers
**Concerns:**
- 3 other high-priority projects competing for the same people
- A previous project ran over timeline
- Being committed to open-ended scope
**Preferred Communication:** Informal 1:1s, visual mock-driven, low-ceremony
**Engagement Strategy:**
- Create value first (help with their UX tech debt)
- Offer a phased, low-risk commitment (4-week trial)
- Frame as visibility/growth for a specific designer

---

#### Jennifer — Data Team Lead
**Role:** Owns the data pipeline team
**Interest Level:** Low (past conflict)
**Influence Level:** Medium
**Current Stance:** Resistant — bad past experience with Engineering
**Motivations:**
- Being treated as a partner, not a service desk
- Fair credit for her team's work
- Realistic integration timelines
**Concerns:**
- Repeat of the recommendation-engine project where her team was publicly blamed
- Being brought in late, after decisions are made
- Under-resourced against yet another Eng ask
**Preferred Communication:** Direct, private 1:1s first; written follow-ups
**Engagement Strategy:**
- Acknowledge and apologize for past harm
- Include Data in early architecture discussions
- Formalize collaboration (see partnership proposal)

---

#### Tom — Operations Manager
**Role:** Owns infrastructure/on-call support
**Interest Level:** Low
**Influence Level:** Medium
**Current Stance:** Neutral — doesn't see this as his priority
**Motivations:**
- System stability and low operational toil
- No surprise on-call load
- Clear ownership boundaries
**Concerns:**
- Real-time platform = new operational burden
- Being handed something to run without input
- Reliability impact on existing services
**Preferred Communication:** Concise, risk/ops-focused; include in planning early
**Engagement Strategy:**
- Bring into planning in Week 2 (co-design operability)
- Show SLOs, runbooks, and on-call plan up front
- Position reliability as a shared goal

---

#### David — Finance Director
**Role:** Approves and tracks budget
**Interest Level:** Low
**Influence Level:** Medium
**Current Stance:** Questioning — unclear ROI
**Motivations:**
- Defensible ROI and spend discipline
- Predictable, staged spend
- Alignment with company financial targets
**Concerns:**
- Budget size vs. uncertain return
- Ongoing run costs of a real-time platform
- Scope/cost creep
**Preferred Communication:** Formal, numbers-first, written business case
**Engagement Strategy:**
- Present a phased budget with clear ROI and contingency
- Tie spend to revenue/differentiation Sarah is targeting
- Offer staged approval gates

---

#### CTO & VP Engineering
**Role:** Executive technical leadership (keep satisfied / sponsors)
**Interest Level:** Medium (CTO), High (VP Eng)
**Influence Level:** High
**Current Stance:** Supportive of strategic direction
**Motivations:** Strategic alignment, engineering health, delivery confidence
**Concerns:** Cross-team friction, delivery risk, precedent set by the initiative
**Preferred Communication:** Monthly exec summary; escalation path for blockers
**Engagement Strategy:** Enlist VP Eng to help unblock Design/Data; keep CTO informed strategically

---

## Stakeholder Map

### Power/Interest Grid

```
High Power │ Keep Satisfied            │ Manage Closely
           │ - CTO                     │ - VP Product (Sarah)
           │ - Finance Director (David)│ - VP Engineering
           │ - Design Director (Mike)  │
───────────┼───────────────────────────┼──────────────────────
Low Power  │ Monitor                   │ Keep Informed
           │ - Other teams             │ - Data Lead (Jennifer)
           │ - Ops Manager (Tom)       │ - Product Managers
           │                           │
           └───────────────────────────┴──────────────────────
             Low Interest                High Interest
```

*Note: Mike sits on the boundary (Medium-High influence) and is managed as a
"keep satisfied / influence" stakeholder; Jennifer is lower formal power but high
strategic importance, hence "keep informed + repair."*

## Relationship Map

```
                       ┌─────────┐
                       │   You   │
                       └────┬────┘
              ┌─────────────┼─────────────┐
              │             │             │
          ┌───▼───┐     ┌───▼───┐     ┌───▼───┐
          │Product│     │Design │     │  Ops  │
          │(ally) │     │(skept)│     │(neutr)│
          └───┬───┘     └───────┘     └───────┘
              │            ▲              ▲
              │  past      │              │
              │  conflict  │              │
              │            │              │
          ┌───┴───┐        │              │
          │ Data  │◄───────┴──────────────┘
          │(resist)│
          └───────┘
   (Sarah/Product can act as a bridge to help repair Data relationship)
```

## Engagement Plan

| Stakeholder | Priority | Next Action | Timeline |
|-------------|----------|-------------|----------|
| Sarah (VP Product) | Critical | Weekly sync + MVP scope alignment | Ongoing |
| Mike (Design) | High | Coffee chat + offer UX-debt help | This week |
| Jennifer (Data) | High | 1:1 to repair relationship | This week |
| Tom (Ops) | Medium | Include in planning, co-design operability | Week 2 |
| David (Finance) | Medium | Present phased ROI business case | Week 3 |
| VP Eng / CTO | Medium | Monthly exec update; enlist to unblock | Ongoing |
