# What Security+ Covers: The Domain Map

> 📅 2026-08-05 · Core Concepts
> Security+ is one of the most recognized entry-level certifications in security. This article maps the SY0-701 exam structure and its five weighted domains, so you know the route before you start the climb.

---

Before you take Security+, know exactly what map you are buying. Security+ is CompTIA's entry-level security certification, and it is one of the most commonly suggested first credentials in the field. It does not test whether you can write exploits or build networks — it tests **breadth of security concepts**: whether you can talk to engineers, admins, and bosses in one shared language about every brick of a defense system.

The current version (at the time of writing) is **SY0-701**. It splits the whole exam into five **domains**, each with a weight. The weight is not decoration: **the number of questions roughly follows the weight.** A domain worth 28% will produce more than twice as many questions as one worth 12%. Knowing the weights is knowing where your study time should go.

This article is the map of those five domains. Read it and you will know what the next six articles cover, and how to order your learning.

## What the exam looks like

Here are the raw numbers:

| Item | Number |
|---|---|
| Questions | up to 90 |
| Time | 90 minutes |
| Question types | multiple choice + performance-based (PBQ) |
| Passing score | 750 (out of 900) |
| Validity | 3 years, renewable with continuing education |

One number gets overlooked: **the passing score is 750, not "answer 70%."** Out of 900, that works out to a bit above seven in ten, but the point is that it is not a fixed proportion — treat every question as serious, because there is no cushion for sloppy early sections.

One more thing worth knowing: **exam versions get retired on a schedule.** SY0-701 is neither the first version nor the last — CompTIA revises the exam every few years and adjusts domains and weights. Every number in this article is for SY0-701; if you meet a newer version, read the latest official exam objectives instead of drilling old question banks.

## Performance-based questions: the other half of the exam

Security+ has two question types. **Multiple choice** you already know. The other is the **Performance-Based Question (PBQ)**, which does not hand you four options — it **drops you into a small scenario**:

* drag firewall rules into the right positions;
* read a log and decide whether it shows an attack or a false positive;
* put an incident-response process back in the correct order;
* choose the right backup and recovery strategy for a situation.

People who have taken the exam will tell you the PBQs are the real test — they check *application*, not *recall*. Handed a messy little scene, can you apply what you know? Do not only drill flashcards; practice scenario questions.

## The five domains at a glance

Here is the whole map. Memorize "domain name + weight" first; the detail comes in later articles:

| Domain | Weight | One-line summary |
|---|---|---|
| 1. General Security Concepts | 12% | cryptography, AAA, zero trust, control types — the shared vocabulary |
| 2. Threats, Vulnerabilities, and Mitigations | 22% | malware, social engineering, vulnerability classes, and their defenses |
| 3. Security Architecture | 18% | network and cloud architecture, secure design principles, data protection |
| 4. Security Operations | 28% | incident response, monitoring, security tools, operational processes |
| 5. Security Program Management and Oversight | 20% | risk management, policy, compliance, resilience |

Notice that **Domain 4 is the only one above a quarter of the exam** — operations is the heart of this certification. If your time is short, spend most of it on Domains 2 and 4; that is the best return on effort.

## Domain 1: General Security Concepts (12%)

This is the *vocabulary* domain: a clean pass through the terms everyone in security uses casually. AAA (authentication, authorization, accounting), the categories of MFA, least privilege, defense in depth, zero trust's "never trust, always verify," and the security control types (preventive, detective, corrective).

It is the most theoretical of the five, and also the easiest to score on — its content is definitions you can simply memorize. The good news: it is exactly the material of `secplus-02-general-concepts`, the very next article.

## Domain 2: Threats, Vulnerabilities, and Mitigations (22%)

This is the *adversary* domain: types of attackers, families of malware, how social engineering works, and the big classes of vulnerabilities. **The point is not to memorize names — it is to pair every threat with a mitigation.** Most questions go "an attack happens; pick the correct defense."

It is the closest domain on the map to a red-team mindset, but this book frames it from the defender's side: you study threats in order to set up defenses, not to strike. The full threat map lives in `secplus-03-threats-mitigations`.

## Domain 3: Security Architecture (18%)

This is the *building* domain: where network devices sit, where firewalls belong, the security boundary in cloud and virtualized environments, secure design principles (build security in rather than bolt it on), and how to protect data across its lifecycle.

Domain 1 gives you vocabulary; Domain 3 gives you the blueprint. Many candidates find architecture questions strange because they are not "recite a definition" but "think about how a system should be shaped" — which makes this one of the most realistic domains in the exam.

## Domain 4: Security Operations (28%)

This is the *shift work* domain, and the single highest weight: logging and monitoring, security tools (SIEM, SOAR), vulnerability management, the full incident-response lifecycle (detect, report, contain, root-cause, recover, review), and forensic fundamentals.

If you have ever taken part in a real incident at a company, this domain will feel familiar. No hands-on experience yet? Do not worry — later in this book, `blue-02-logging-siem` and `blue-04-incident-response` go deeper into exactly this territory.

## Domain 5: Security Program Management and Oversight (20%)

This is the *office* domain: how risk assessments work, how policies and standards get written, how compliance regimes (GDPR, HIPAA, and friends) map to practice, business continuity and disaster recovery, supply-chain risk, and how security is governed and overseen inside an organization.

It is not glamorous, but it is why security *survives* — without governance, even good defenses are a flash in the pan. The plain-language version of this domain is `secplus-06-governance-risk`.

## Who should take this certification

Security+ is most valuable to a fairly specific set of people:

| Who | Why it helps |
|---|---|
| Someone moving into security | It is the most recognized entry ticket, and many job postings list it as a plus or a requirement |
| An IT person pivoting roles | You know systems; this fills in the security lens you are missing |
| A non-technical manager | You need to talk to security teams; this hands you the vocabulary |
| A student | Almost no barrier to entry, and the learning curve is beginner-friendly |

The flip side: if you already have years of security practice, this exam will feel easy. Its job is *breadth*, not depth — depth comes from the domains that follow. If that is you, the later parts of this book — incident response, hardening, forensics — will be a better use of your time.

## Common misconceptions

Three misconceptions show up again and again from first-time candidates. Let us retire them now:

| Misconception | Reality |
|---|---|
| "It is a hacker certification that teaches me to attack" | No. It is a defender's roadmap; it tests *defense*, not offense |
| "Memorize the five domains and you are done" | Multiple choice rewards recall, but PBQs reward application; train both |
| "Passing it means I know security" | It is the start line, not the finish line; depth comes from later domains and real practice |

## How to study the map

Seasoned advice works better than "read the textbook front to back":

#### Start with Domains 2 and 4

They carry the most weight and best build the threat-to-defense mental model.\n\n→ go straight to secplus-03-threats-mitigations and secplus-05-security-operations

#### Treat Domain 1 as a dictionary

No need to memorize every AAA and MFA detail at once; look things up as you go and it will stick.

#### Practice a few PBQs daily

Multiple choice tests recall; PBQs test applying knowledge to a scene. Drill both.

#### Finish with Domain 5

Learn governance and compliance by understanding, not by rote, alongside secplus-06-governance-risk.

> Weights are not decoration. CompTIA publishes the question split for every exam, so allocating study time by weight is the most efficient strategy — the 28% Domain 4 deserves well over a quarter of your effort.

## From this map forward

With the map in hand, follow the series in order — one article, one domain:

* `secplus-02-general-concepts` — Domain 1: AAA, MFA, least privilege, zero trust, control types.
* `secplus-03-threats-mitigations` — Domain 2: the threat and vulnerability map, each paired with a defense.
* `secplus-04-security-architecture` — Domain 3: architecture and secure design principles.
* `secplus-05-security-operations` — Domain 4: operations, monitoring, and incident response.
* `secplus-06-governance-risk` — Domain 5: risk, policy, and compliance.
* `secplus-07-practice-questions` — a final set of mixed practice questions to wrap up.

Next stop is the most "theoretical" yet most scoreable domain, general security concepts: `secplus-02-general-concepts`.

#### Q: On SY0-701, which single domain carries the highest weight?

* General Security Concepts (Domain 1)

* Security Architecture (Domain 3)

* Security Operations (Domain 4)

* Security Program Management and Oversight (Domain 5)

> 💡 Security Operations (Domain 4) is 28% — the only domain above a quarter of the exam. Question counts roughly follow the weights, so it deserves the most study time.
