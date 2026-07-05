# Conflict Resolution Playbook

> **Author:** Buddhika Amarasinghe · **Role:** Tech Lead

## Conflict Type 1: Priority Disagreement

### Scenario
Product wants Feature A; Engineering believes Feature B is a prerequisite.

### Resolution Framework

**Step 1: Understand Both Positions**
- Product perspective: Customer X needs A for renewal
- Engineering perspective: Without B, A will be unstable

**Step 2: Identify Common Ground**
- Both want customer success
- Both want a quality product
- Both are constrained by time

**Step 3: Generate Options**
1. Do B first, then A (Engineering preferred)
2. Do A first, accept tech debt (Product preferred)
3. Parallel track with reduced scope for both
4. Negotiate timeline extension for proper sequence

**Step 4: Evaluate Together**
| Option | Customer Impact | Tech Impact | Timeline |
|--------|-----------------|-------------|----------|
| B→A | Delayed value | Stable | +4 weeks |
| A→B | On time | Risky | On time |
| Parallel | Partial value | Moderate | +2 weeks |

**Step 5: Propose Solution**
> "What if we do a minimal version of B (2 weeks instead of 4) that addresses the
> critical stability concerns, then proceed with A? Customer gets value 2 weeks
> late but with confidence."

---

## Conflict Type 2: Resource Competition

### Scenario
Another Tech Lead needs the same engineer you need.

### Resolution Framework

**Step 1: Avoid Zero-Sum Framing**
- Not: "My project vs. your project"
- Instead: "How do we solve both problems?"

**Step 2: Understand Their Situation**
- Why do they need this person?
- What's their deadline pressure?
- Are there alternatives for them?

**Step 3: Explore Creative Solutions**
1. Split time (50/50 or 70/30)
2. Sequential (you first, then them)
3. A different person who could do the work
4. Extend one timeline to avoid the conflict
5. Bring in a contractor for one project

**Step 4: Escalate Constructively (if needed)**
- Present both needs objectively
- Come with options, not just the problem
- Let the manager decide priorities

**Script:**
> "I know we both need Alex. Rather than fighting over it, can we figure out what
> would work for both of us? If you need Alex for the auth work, could Sarah handle
> the API piece for me? Or if your deadline is more flexible, I could release Alex
> to you in 3 weeks."

---

## Conflict Type 3: Past Grudge Affecting Current Work

### Scenario
A team member (e.g., Jennifer/Data) holds resentment from a previous project conflict.

### Resolution Framework

**Step 1: Address Directly**
- Don't ignore the elephant
- Name the dynamic
- Take responsibility for your part

**Step 2: Listen Fully**
- Let them express frustration
- Don't defend or explain
- Acknowledge their experience

**Step 3: Commit to a Different Future**
- Specific changed behaviors
- A check-in mechanism
- Permission to call it out if the pattern repeats

**Script:**
> "I sense some tension from the last project. I'd rather address it than pretend
> it's not there. What happened? [Listen] I hear that you felt [summary]. I'm sorry
> that was your experience. Here's what I'll do differently..."

---

## Conflict Type 4: Priority/Ownership Gap (Ops "Not My Priority")

### Scenario
Operations doesn't see the analytics platform as their priority and is slow to commit support.

### Resolution Framework

**Step 1: Connect to Their Goals**
- Frame reliability/operability as a shared outcome, not an Eng ask
- Show how early involvement reduces their future on-call pain

**Step 2: Reduce Their Uncertainty**
- Bring SLOs, runbooks, and an on-call plan to the table proactively
- Offer to co-design operability rather than "hand it over"

**Step 3: Make Commitment Small and Concrete**
- Ask for a defined, bounded involvement (e.g., 2 planning sessions + review)
- Escalate to VP Eng only if a genuine priority conflict remains

**Script:**
> "Tom, I don't want to drop a new system on your team later. I'd rather design the
> operability with you now — SLOs, runbooks, on-call — so it's boring to run. Could
> we get you into two planning sessions so your concerns shape it from the start?"

---

## De-escalation Techniques

### When Emotions Run High
1. **Pause:** "Let's take a 10-minute break"
2. **Reframe:** "We're both trying to do what's best for the customer/product..."
3. **Move locations:** "Let's grab coffee and continue"
4. **Bring in a neutral party:** "Would it help to have [name] join?"

### Warning Signs to Watch
- Raised voices
- Defensive body language
- Repeating the same points
- Personal attacks
- Silent withdrawal
