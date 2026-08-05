# Governance, Risk, and Compliance

> 📅 2026-08-05 · Core Concepts
> The business side of security: qualitative and quantitative risk assessment, the four ways to treat risk, the policy hierarchy, business continuity and disaster recovery, GDPR, NIST and ISO 27001 at a conceptual level, and why people are the last line of defense.

---

“We got hacked — but we have a firewall!” By now you know a firewall is not the whole of security. But there is a bigger misunderstanding waiting: **security is not only technical — there is an entire “business side.”** Who decides how much to spend on defense? Who decides which data may be collected and how long it may be kept? Who decides that a breach must be reported within hours? Those answers are the subject of this article: **governance, risk, and compliance.**

The technical side answers “how do we defend.” The governance side answers “what do we defend, why, and how far.” An organization can have brilliant engineers and still spend money in the wrong places, because no framework guided the decisions — and that is usually not the engineers' fault. It is a governance failure. This article turns security from “engineer intuition” into a system that can be managed, decided, and reviewed.

## Security is a decision problem, not just a technical one

**Governance** is the process of deciding who decides, and watching that they decide well. It lives at the top of an organization:

| Role | What it owns |
|---|---|
| **CISO** | the strategic lead: policy, budget, risk acceptance |
| **Risk committee** | cross-department agreement on which risks are acceptable |
| **Board / executives** | final oversight — security as a business risk, not an IT chore |

The most important governance move is **risk acceptance** — writing down “we are carrying this risk for now.” That sounds like slacking off, but it is actually healthy: no system can be perfectly safe, and putting on paper what you accept, and why, beats pretending you are fully secure. The real failure is not taking a risk; it is taking a risk **without knowing you did.**

## Risk management: qualitative and quantitative

How do you actually “manage” risk? Security splits it into two languages:

| | Qualitative | Quantitative |
|---|---|---|
| **Expressed as** | high / medium / low, in words | numbers and money |
| **Strengths** | fast, no precise data needed | comparable, sortable, budgetable |
| **Weaknesses** | subjective — people disagree | needs data, can feel falsely precise |

Most organizations start qualitative: list every risk, sort it high / medium / low, then go quantitative only for the few high-priority ones to figure out what they are actually worth. The two complement each other; they are not either/or.

Quantitative assessment comes with its own alphabet soup, and the exam loves it:

* **AV** (asset value) — what the asset is worth.
* **EF** (exposure factor) — how much of it one incident destroys.
* **SLE** (single loss expectancy) = AV × EF.
* **ARO** (annualized rate of occurrence) — how many times per year it happens.
* **ALE** (annualized loss expectancy) = SLE × ARO.

Worked example: a server is worth $100,000, a fire would destroy half of it (EF = 0.5), and fires happen once every two years (ARO = 0.5). SLE = 100,000 × 0.5 = 50,000; ALE = 50,000 × 0.5 = 25,000. **That $25,000 is your starting point for how much defense per year is worth paying for this asset.**

> Qualitative and quantitative are not either/or. You want “good enough decisions,” not “numbers that look precise.” False precision is more dangerous than an honest rough estimate — it makes everyone trust a number nobody validated.

## The four ways to treat a risk

Once a risk is measured, the question is what to do with it. There are exactly four answers:

| Treatment | Meaning | Example |
|---|---|---|
| **Avoid** | stop doing it, make it disappear | retire an old service nobody uses |
| **Transfer** | move the cost to someone else | cyber insurance, a third-party contract |
| **Mitigate** | add controls to shrink likelihood or impact | patching, MFA, network segmentation |
| **Accept** | carry the residual risk, on paper | a tiny known risk, explicitly accepted |

Notice that “accept” is an **active, documented** decision — not “we never thought about it.” An auditor is not scared by the existence of risk; they are scared by risk that nobody admits is there.

## Policies, standards, procedures, and baselines

Governance lands on the ground as documents. Security documents have a clear hierarchy, and each level answers a different question:

| Document | Level | Question it answers | Example |
|---|---|---|---|
| **Policy** | high-level statement | Why do we do this? | “All remote access must verify identity” |
| **Standard** | mandatory, specific | What counts as compliant? | “Remote access must use FIDO2 MFA” |
| **Procedure** | step-by-step | How, exactly? | “How to enroll a security key” |
| **Baseline** | minimum bar | At least this much? | “All servers patched within 30 days” |

Memory trick: a policy says **why**, a standard says **what you must do**, a procedure is the **recipe**, and a baseline is the **floor**. A policy without a standard says “be secure” but never defines what secure means — and then nobody knows how to comply even when they want to.

A policy is also not finished when it is written. It has a lifecycle: **draft → review and approval → awareness and training → enforcement and audit → periodic revision.** Publishing is not the point; people actually following it, being checked, and updating it is. A policy written years ago and never looked at again is worse than none — it gives everyone the comfortable illusion that someone is in charge.

## Business continuity and disaster recovery (BCDR)

No defense is perfect, and eventually something will go wrong. **BCDR** is the chapter that plans for “after it happens”: **business continuity (BCP)** keeps operations running; **disaster recovery (DRP)** brings you back after a disaster. Both orbit two numbers:

| Acronym | Full name | In one phrase | Example |
|---|---|---|---|
| **RTO** | Recovery Time Objective | how fast you recover | service back within 4 hours |
| **RPO** | Recovery Point Objective | how much data you may lose | at most 15 minutes of data |

Smaller RTO means faster recovery (usually pricier); smaller RPO means less data loss (usually pricier). Both are numbers you decide **in advance** — not numbers you shout in the middle of a fire.

The backup rule “3-2-1”: **3** copies, on **2** different kinds of media, **1** of them offline or offsite. And a plan must be exercised, at different levels of intensity:

| Exercise | What it looks like | Cost |
|---|---|---|
| **Tabletop** | a meeting that talks through a scenario, asking each person “what do you do?” | low |
| **Simulation** | some people really act, some play along | medium |
| **Full exercise** | run the whole failover and recovery for real | high |

> Backup is not disaster recovery. A backup only proves you have the data; DR requires an environment you can restore into. A restore procedure that was never rehearsed is self-comfort on fire day. Restoring into a test environment regularly is the most skipped — and most necessary — step in the plan.

## Compliance frameworks: GDPR, NIST, ISO 27001

**Compliance** means meeting legal, regulatory, and contractual obligations. You do not need to memorize clauses, but you should recognize three names conceptually:

| Framework | Origin | One-line positioning | Signature idea |
|---|---|---|---|
| **GDPR** | European Union | personal data protection law | consent, data subject rights, 72-hour breach notice |
| **NIST CSF** | NIST (US) | practical, risk-based framework | Identify / Protect / Detect / Respond / Recover |
| **ISO 27001** | international standard | an information security management system | PDCA loop, audits, continual improvement |

One-line memory: GDPR governs **data**, NIST governs **practice**, ISO 27001 governs **management systems**.

And compliance is not the same thing as security:

| | Compliance | Security |
|---|---|---|
| **Asks** | did we do it? | is it still working against what is changing? |
| **Rhythm** | periodic audit | continuous contest |
| **Outcome** | pass / fail | never finished |

> Compliance is the floor, not the ceiling. Every box can be ticked and the organization can still be breached next week. Confusing “did we do it” with “is it still effective” is the classic governance mistake.

## Security awareness: the last line of defense

Technology can be excellent and a well-crafted phishing email still gets through. That is why governance must include **security awareness** — not rules taped to a wall, but everyone understanding the **why**:

* **Teach reasons, not just rules**: explaining why reusing passwords is dangerous beats a weekly password-change policy.
* **Phishing simulations**: test with your own simulated phish to find who needs more help — measurement, not punishment.
* **Mistakes are a process problem**: when someone clicks, it means the process has a gap (say, no clear verification step). Blaming the frontline just teaches people to hide the problem.

We go deep on phishing defense in `blue-06-phishing-defense`. The thread to keep here: **people are not the weakest link — they are the last line of defense**, provided the organization bothers to train them as one.

## The edge of governance: law and ethics

Governance eventually has to answer “what is allowed.” The line running through this whole book: **unauthorized testing is illegal.** That is not only a technical judgment — it is a legal and ethical one. Personal data, notification duties, testing scope: each changes whether you may do something, and what happens if you do. `career-03-ethics-law` opens this up fully. Until then, keep one sentence: **knowing how does not grant permission to use.**

## Next

Governance turns security into something that can be managed, decided, and reviewed. With this, all five Security+ domains are in place. Next is the full-map review: `secplus-07-practice-questions` walks back through every key idea and uses practice questions to confirm that you understand — rather than merely recognize — them.

#### Q: A server is worth $100,000 (asset value). A fire would destroy half of its value (exposure factor 0.5), and fires are expected once every two years (annualized rate of occurrence 0.5). What is the annualized loss expectancy (ALE)?

* 25,000

* 50,000

* 100,000

* 10,000

> 💡 SLE = asset value x exposure factor = 100,000 x 0.5 = 50,000. ALE = SLE x annualized rate of occurrence = 50,000 x 0.5 = 25,000 — your starting point for how much defense per year is worth paying.
