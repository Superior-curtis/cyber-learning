# General Security Concepts (AAA, MFA, Least Privilege)

> 📅 2026-08-05 · Core Concepts
> Domain 1 is the shared vocabulary of security. This article walks through AAA, MFA factors, least privilege, defense in depth, zero trust, and control types — the concepts every later article assumes you already know.

---

On the map from the last article, Domain 1 is only 12% of the exam — but it is the **shared vocabulary of the whole field**. Every article that follows, whether about architecture, operations, or governance, will lean on the terms introduced here. Skip this one and every later stop gets awkward.

The good news: Domain 1 has no hard theory. It is a set of words the industry agreed to use in a certain way: AAA, MFA, least privilege, defense in depth, zero trust, and control types. Explain those six families clearly and this article is done.

## AAA: authentication, authorization, accounting

The most common alphabet soup in security is **AAA** — three A's standing for three stages of the identity flow:

* **Authentication**: prove you are who you say you are. Entering a password, scanning your face, receiving an SMS code — that is this step.
* **Authorization**: once verified, decide what you *may do*. Which files you can read, whether you can delete, whether you can transfer money — that is a permission question.
* **Accounting**: *record* what you did. Who signed in at what time, who changed which file — a trail you can review later.

Most people think of "logging in" as one thing, but it is two independent questions: "who are you?" and "what can you do?" Seen separately, a lot of security design clicks — you can be authenticated but not authorized to read a document, and that is normal, because passing authentication does not mean passing authorization.

```text
Authentication (who you are) → Authorization (what you can do) → Accounting (what you did)
```

Accounting is the most neglected of the three. Without an audit trail, when something goes wrong you can only guess. Later, `blue-02-logging-siem` covers logs and monitoring in depth — for now remember: **the third A is what makes the first two trustworthy.**

## The three factor categories: something you know, have, and are

You have heard the term multi-factor authentication (MFA). The idea behind it: verification methods fall into **factors**, and multi-factor means combining two or more *different categories*. The factor categories SY0-701 likes to test:

| Factor | Meaning | Examples | Main weakness |
|---|---|---|---|
| Something you know | knowledge | password, PIN, security answers | gets phished, guessed, reused |
| Something you have | possession | phone, OTP, smart card, key | can be stolen, lost |
| Something you are | inherence | fingerprint, face, iris | biometrics cannot be "reset" |
| Somewhere you are | location | corporate Wi-Fi, geo-location | spoofable, usually an aid |
| Something you do | behavior | typing rhythm, mouse patterns | rare, often continuous verification |

The trap the exam loves: **"password + PIN" is not MFA.** Both are knowledge factors — you know two things, but they come from the same category. True MFA must cross categories, like "password (know) + phone code (have)" or "password (know) + fingerprint (are)."

## MFA in practice: what threat are you answering?

MFA is not better the fancier it gets; it should **answer the threat you are actually worried about**. Holding that mapping in mind makes many exam questions easy:

| Threat | Why MFA helps |
|---|---|
| Password phished or stolen | the attacker still needs a second factor, like your phone |
| Password reuse | if one site is breached, other accounts still have a second lock |
| Credential brute force | without the second factor, guessing the password is not enough |

One practical note: the common "SMS code" is not the strongest option — texts can be intercepted and SIM cards swapped. Stronger options are usually an **authenticator app or a hardware key**. For the exam, remember the categories and the trade-offs; in practice, starting with an app is a perfectly good first step.

There is also a related trend you will hear called **passwordless login** — replacing the password itself with a biometric or a hardware key. The idea is the same as MFA: move the dependence away from what a person can remember. The exam will not go into implementation detail, but knowing the term and why it is stronger will help you read questions.

## Least privilege: hand out fewer keys

**Least privilege** in one sentence: **every person or program gets only the minimum permissions needed to do the job — nothing more.** It sounds like common sense, yet real systems are full of counterexamples: shared admin accounts, folders everyone can read, every service running as root.

Why does least privilege matter so much? Because **permissions equal the damage an attacker can do once they seize an account.** Give an employee read access to every customer record, and when their machine gets infected, you have effectively handed that data to the malware. Fewer permissions means a smaller blast radius for any single compromised account.

A few classic practices:

* normal user accounts run with **standard rights**, and installing software or changing system settings requires a separate elevation;
* admin work happens in **dedicated admin accounts** that are not logged in when unused;
* service accounts get only the permissions they actually use;
* when someone leaves or a project ends, **revoke access immediately.**

Least privilege is one of the foundations of zero trust (next), and a theme that runs through this entire book — `blue-07-iam-zero-trust` goes deep on identity and access management in practice.

## Defense in depth: an onion is not one layer

**Defense in depth** means: do not count on any single control to stop an attack, so stack layers of defense — like peeling an onion, an attacker who breaks one layer still meets the next.

Concretely, protecting a database server can involve all of these at once:

| Layer | Example |
|---|---|
| Physical | data-center access control, locked racks |
| Network | firewalls, network segmentation, intrusion detection |
| Host | OS patching, antivirus, least privilege |
| Application | access control, input validation |
| Data | database encryption, backup and recovery |
| Process | backup policy, incident-response plan, staff training |

The mindset at the core of defense in depth is **assume breach**: not "this layer is strong, so we are safe," but "if this layer is broken, the next one is still standing." Each layer has its own cost; the point is never to put all eggs in one basket.

## Zero trust: never trust, always verify

**Zero trust** pushes defense in depth to its logical extreme. The traditional model is a *castle and moat*: inside is trusted, outside needs defending. Zero trust flips that: **assume no request is trustworthy — whether it comes from the intranet or the internet — and verify it anyway.**

The slogan at its core is **"never trust, always verify."** Zero trust is not a product; it is a set of principles:

* every request is verified for identity and permission, never waved through because it is "on the intranet";
* least privilege is the default, not the exception;
* sensitive resources get finer-grained access control instead of a wide-open network;
* assume a breach has happened or will happen, so keep monitoring and auditing.

For the exam, remembering "zero trust treats inside and outside the same — every request gets verified" captures the point. The implementation layer (SASE, microsegmentation, IAM) unfolds further in `blue-07-iam-zero-trust`.

## Security control types: sorting your defenses

SY0-701 enjoys testing **control type** classification. The most common split is by *when the control acts*:

| Control type | What it does | Examples |
|---|---|---|
| Preventive | stops the event from happening | firewalls, permissions, encryption |
| Detective | notices the event happening | logs, alerts, intrusion detection |
| Corrective | repairs after the event | restore from backup, patching, quarantine |
| Deterrent | discourages would-be attackers | warning signs, cameras, warning pages |
| Compensating | substitutes an alternative mechanism | badge reader locked, so a temporary pass |

The other split you will see: what *kind* of thing the control is — **technical** (firewall, encryption, antivirus), **managerial** (policy, training, audit process), and **physical** (access control, locks, guards). One defense can belong to several categories at once — a password policy is both managerial and preventive.

A shortcut for exam questions: ask "does it act before, during, or after?" Before → preventive; while watching → detective; repair → corrective. Get that timeline down and classification questions almost never miss.

## Common exam traps

Here are the places candidates trip most often on this article's material:

| Trap | The fix |
|---|---|
| Assuming "password + PIN" is MFA | both are knowledge factors — it is single-factor |
| Mixing up authentication and authorization | authentication asks *who you are*; authorization asks *what you may do* |
| Believing "on the intranet" means safe | zero trust treats inside and outside alike; every request is verified |
| Building prevention but no detection | use preventive + detective + corrective controls together |

These traps are really one mistake: **remembering the names of concepts without remembering the boundaries between them.** The exam loves boundaries — MFA's boundary is factor category, AAA's boundary is the split between authentication and authorization. Nail the boundaries and it is worth more than ten extra definitions.

> The Domain 1 vocabulary is a shared language, not a stack of test points. Every article you read next — architecture, operations, governance — comes back to words like AAA, least privilege, and control types. Read this one well and you have laid the foundation for all of them.

## Next

Domain 1 gave you the words to talk with. The next article moves into Domain 2: **what the bad guys are up to** — malware, social engineering, vulnerability classes, and the countermeasure for each: `secplus-03-threats-mitigations`.

To review the basics first, go back to `found-02-cia` (the CIA triad — confidentiality, integrity, and availability are exactly what all those control types exist to protect); to go deeper on access control, jump ahead to `blue-07-iam-zero-trust`.

#### Q: Why is using a password plus a PIN for login NOT considered multi-factor authentication (MFA)?

* Because a PIN is too short and easy to guess

* Because both fall under the knowledge factor

* Because both are set by the user themselves

* Because it has only two layers and MFA needs three

> 💡 Multi-factor means crossing factor categories. A password and a PIN both belong to something you know, so they are one factor — not MFA.
