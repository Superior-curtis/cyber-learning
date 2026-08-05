# Concept Review & Practice Questions

> 📅 2026-08-05 · Deep Dive
> Tying the whole Security+ series into one map: the five domains at a glance, common exam traps, and practice questions covering the key ideas from secplus-02 through 06, so you know you understand rather than memorize.

---

After reading the previous six articles, you may feel like you “kind of get it.” And “kind of get it” is exactly the most dangerous state before an exam — it lets you mistake familiarity for understanding. This article teaches nothing new. It lays the whole map flat and uses exam questions to force you to check yourself: do you understand, or have you just seen these words before?

There is a simple test for the difference between familiarity and understanding: close the book and try to explain the concept in your own words. If you can, that is understanding; if you can only recognize it, that is familiarity. Every question below is your chance to explain it.

## The five domains, one map

The SY0-701 exam is split into five domains, and this book's `secplus-` series follows exactly that map:

| Domain | Theme | This book |
|---|---|---|
| 1. General Security Concepts | CIA, AAA, MFA, least privilege | `secplus-02-general-concepts` |
| 2. Threats, Vulnerabilities, and Mitigations | attack types and their defenses | `secplus-03-threats-mitigations` |
| 3. Security Architecture | segmentation, zero trust, defense in depth | `secplus-04-security-architecture` |
| 4. Security Operations | monitoring, SIEM, hardening, response | `secplus-05-security-operations` |
| 5. Security Program Management and Oversight | governance, risk, compliance | `secplus-06-governance-risk` |

If you can close the book and give a one-minute summary of each chapter — the core idea, why it matters, and what breaks when it fails — you are already halfway there. The chapter you cannot summarize is the one you still need to study.

This table is not just for review. The real exam deliberately mixes concepts across domains — a scenario about governance can test an architectural control. The practice questions below will get you used to that jump.

## A self-check before you start

Five questions, one per domain. Answer honestly before you peek at the quizzes below:

* [ ] Can I name the CIA triad and give an example that protects only two of the three?
* [ ] Can I pair five common attacks with one mitigation each?
* [ ] Can I explain the difference between zero trust and defense in depth?
* [ ] Can I say what a SIEM does, and the order of steps in incident response?
* [ ] Can I compute an ALE, and tell a policy apart from a standard?

A box left empty is not a failure — it is a map of exactly where to study before the exam. This checklist has no score; its job is to turn “I do not know what I do not know” into “I know exactly which chapter I do not know.” Only the second state is actionable.

## Common exam traps

Multiple-choice questions change their surface constantly, but the traps are remarkably fixed:

| Trap | What it looks like | How to beat it |
|---|---|---|
| Best, not correct | “What is the **best / highest priority**?” | it tests priorities, not facts |
| Swapped terms | qualitative vs quantitative, RTO vs RPO | build your own contrast table |
| Answering attack with attack | “What is the best **mitigation**?” | remember every attack and its defense as a pair |
| Long stem, short answers | the keyword hides in the stem | extract asset, threat, and phase first |
| All options look right | “Which **best** describes…?” | pick the one closest to the scenario, not the vaguest |

> This exam tests the why, not the memorized answer. Run every concept through three questions — what it is, why it exists, what happens when it fails — and you will out-score someone who memorized the glossary. The question can change its clothes; the skeleton does not.

## Exam-room strategy

How you take the test matters as much as what you know:

* **Scan first**: answer the ones you know, mark the uncertain ones, do not get stuck on question one.
* **Narrow to two**: eliminate the obviously wrong options, then pick the one closest to the scenario.
* **For scenario questions, grab three things**: what is the asset, who is the threat, and which phase is this (prevent / detect / respond).
* **Do not second-guess**: unless you have a clear reason, your first instinct is usually right.

## How to use these practice questions

Answer each question yourself before you peek at the answer. Getting one right only proves you know that question — not that chapter — so read the explanation anyway; that is where the “why” lives. A wrong answer is not a loss; it pinpoints exactly where your next review session should go. Treat the quiz as a learning tool, not a verdict.

## Practice 1: General Security Concepts

Recall `secplus-02-general-concepts`: authentication factors come in three classes — something you know, something you have, something you are. Phishing-resistant variants bind the credential to a specific site domain. Try one:

#### Q: A user logs in with a password and an SMS one-time code. Which authentication factor does the SMS code represent?

* Something you are

* Something you have

* Something you know

* Somewhere you are

> 💡 A code delivered to a device in your hands is a possession factor. It is a weak version of one (SMS can be redirected), but conceptually it still counts as something you have.

## Practice 2: Threats, Vulnerabilities, and Mitigations

Recall `secplus-03-threats-mitigations`: phishing targets the person, not the system, so the best mitigations are almost always people and process — training, plus a second verification that leaves the message channel.

#### Q: An employee receives an urgent email that appears to come from the CEO, asking them to buy gift cards and reply with the codes. Which control does the most to stop this attack?

* Disabling email entirely

* Security awareness training plus verifying the request out of band

* Raising the password length to 20 characters

* Encrypting the mailbox

> 💡 Phishing targets the person, not the technology. Training teaches people to recognize pressure and authority tricks, and out-of-band verification (a phone call, not a reply) catches the fake before any money moves.

## Practice 3: Security Architecture and Operations

Recall `secplus-04-security-architecture` and `secplus-05-security-operations`: defense in depth means “one compromised host is not game over.” Segmentation caps the blast radius; SIEM and detection make the intrusion visible.

#### Q: An attacker has compromised one web server inside a company. Which control does the most to limit what they can reach next?

* Faster backups

* Network segmentation between tiers

* A longer password on that server

* Rotating the server certificate more often

> 💡 Segmentation contains the blast radius: even with one host taken over, the attacker cannot jump straight to the database tier or move laterally. That is defense in depth doing its job.

## One sentence per chapter

If you only have six lines to review the night before, keep these six. They are the load-bearing walls of the whole course; everything else is detail:

| Chapter | Takeaway |
|---|---|
| `secplus-01-overview` | Security+ is a test of five domains, not one skill |
| `secplus-02-general-concepts` | everything rests on the CIA triad and least privilege |
| `secplus-03-threats-mitigations` | every attack has a matching defense |
| `secplus-04-security-architecture` | defense in depth: one host down is not game over |
| `secplus-05-security-operations` | monitoring, hardening, and response are the daily work |
| `secplus-06-governance-risk` | risk, policy, and compliance turn security into decisions |

## Turning the map into skill

The practice questions prove understanding, but understanding still needs to become muscle memory. Next, take the knowledge somewhere real: `lab-03-ctf-101` brings you into a CTF practice environment, where small real challenges test everything from the previous chapters — there is no better review than doing. And if any chapter still feels shaky, walk it again from the start at `secplus-01-overview`: the whole map unfolds from there.

Before you go, one last question reviews the fifth domain — governance, risk, and compliance. Recall `secplus-06-governance-risk`: risk can be expressed qualitatively (high / medium / low) or quantitatively (money); quantitative work runs on AV, EF, SLE, ARO, and ALE. And remember that compliance is the floor, not the ceiling.

One honest reminder: practice questions are only a sample, and the real exam always contains variations you have not seen. The robust way to prepare is spaced repetition — read today, test yourself in two days, test again in a week — so the knowledge moves from short-term memory into long-term. Treat every quiz result as the map for your next review, not as a verdict on you.

#### Q: A server is worth $100,000 (asset value). A fire would destroy half of its value (exposure factor 0.5), and fires are expected once every two years (annualized rate of occurrence 0.5). What is the annualized loss expectancy (ALE)?

* 25,000

* 50,000

* 100,000

* 10,000

> 💡 SLE = asset value x exposure factor = 100,000 x 0.5 = 50,000. ALE = SLE x annualized rate of occurrence = 50,000 x 0.5 = 25,000 — the annual budget starting point for defending that asset.
