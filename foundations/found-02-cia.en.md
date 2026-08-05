# The CIA Triad: Confidentiality, Integrity, Availability

> 📅 2026-08-05 · Core Concepts
> Three properties every good defense is really protecting: only the right people can read it, it has not been secretly changed, and it is there when you need it. Meet the CIA triad.

---

No, not that CIA. The **CIA triad** is the oldest organizing idea in computer security — three properties that every defense, from a password to a firewall to a backup plan, is quietly protecting:

* **C**onfidentiality — the right people can read it, others cannot.
* **I**ntegrity — it is what it is supposed to be, unchanged by accident or by an attacker.
* **A**vailability — it is there when you need it.

Almost every security failure you have ever read about is a failure of one (or more) of these three. Once you can name which one failed, the news stops being noise.

Why these three? Because they cover nearly the whole promise a system makes to its users: what you hand over will not be seen by others, will not be swapped for something else, and will be usable when you need it. Some textbooks add more properties (authenticity, non-repudiation), but most can be derived from these three. **Get the three pillars right and the rest is refinement.**

## Confidentiality: the right people can read it

Confidentiality means **secrecy where it matters**: data is only readable by people (or programs) allowed to see it. It is the most intuitive idea of “security” — keeping others from looking.

The everyday example is a **password leak**. Your password is a secret; it should stay between you and the site you use. If a database gets stolen and passwords are leaked, confidentiality is broken — strangers can now read what only you and the site should know. Why does it hurt so much? Because passwords get **reused** — whoever has this one may try it against your email and bank next.

Other everyday examples:

* A group chat message only the members should see, and someone screenshots it.
* A company’s salary list sitting on a shared drive everyone can open.
* Your email, if your account gets taken over.

How defenders protect confidentiality: **encryption** (scrambling data so only the key-holder can read it), **access control** (who is allowed to open what), and **authentication** (proving you are who you say you are). All three are needed — strong encryption with a weak password is a lock with an open door.

## Integrity: it is what it is supposed to be

Integrity means **nothing important changed without permission** — whether by accident or by an attacker. The data you see is the data that was written. Confidentiality is about *being seen*; integrity is about *being true*.

The everyday example is a **tampered page**. Imagine a news site where an attacker edits the article to say the opposite of what the journalist wrote, or an online store where someone quietly changes the price of an item. Readers see a page that *looks* official — but it is not what the site published. That is broken integrity.

Other everyday examples:

* A bank statement where one number was changed.
* Software that was modified to include a backdoor.
* A form where the total was altered to $1.

How defenders protect integrity: **hashing** (a fingerprint of data that changes if even one character changes), **digital signatures** (proof of who wrote something), and **change logs** (recording who changed what, when). You will not think about integrity daily, but it is often sneakier than a leak — tampered data *looks perfectly normal*.

## Availability: it is there when you need it

Availability means **the service and data are reachable when legitimately needed**. A system that is up and correct is useless if no one can reach it.

The everyday example is a **DDoS attack** — a distributed denial of service. Attackers point thousands of machines at a website and flood it with traffic until it can no longer answer real visitors. The site still exists, the data is still intact — but no one can get in. Availability is broken.

Other everyday examples:

* A hard drive that died, taking your only copy of a report with it.
* A power cut at the data center.
* A bug that crashes the app every afternoon.

How defenders protect availability: **backups**, **redundancy** (multiple copies on multiple machines), **load balancing** (spreading traffic), and **DDoS protection**. Notice that availability defense is rarely glamorous — no conspiracy, just an extra copy and an extra route. Yet it is often the most business-critical pillar: three hours of downtime is three hours of lost sales.

## The three, side by side

| Pillar | The question it answers | Broken example | Main defenses |
|---|---|---|---|
| **Confidentiality** | Who may read it? | leaked passwords | encryption, access control |
| **Integrity** | Is it still what it should be? | tampered page | hashing, signatures |
| **Availability** | Can I reach it when I need it? | DDoS attack | backups, redundancy |

A useful trick: **when you hear about a breach, name the pillar.** A stolen password list is confidentiality. A defaced website is integrity. A site that keeps crashing is availability. Naming the pillar tells you what the defense has to rebuild.

## One product, all three pillars

Take a single product — a banking app — and watch all three pillars appear at once:

| Feature | Pillar it protects | How |
|---|---|---|
| Face unlock to open the app | Confidentiality | only you can reach the balance screen |
| Transaction confirmations and receipts | Integrity | you can verify nothing was silently changed |
| Servers in two data centers | Availability | if one site fails, the other still answers |

The app is not three separate systems. It is one system, and each feature quietly answers one of the three questions. Look at any product this way and its design choices stop being mysterious.

## A news-headline workout

Try a few realistic headlines; every one lines up with a pillar:

| Headline | Pillar broken |
|---|---|
| A company’s password database is published for download | Confidentiality |
| Attackers replace a city website’s homepage with a prank image | Integrity |
| A ticketing site collapses on the day tickets go on sale | Availability |
| A bank app is replaced with a malicious version that records logins | Confidentiality + Integrity |
| A hospital’s patient portal goes down during an emergency | Availability |
| An attacker intercepts and alters a payment confirmation email | Integrity |

Some incidents break two at once — a compromised bank app steals credentials (confidentiality) and shows fake balances (integrity). Get comfortable naming “how many, and which,” and your read on the news sharpens fast.

## Real products trade them off

Here is the part most textbooks skip: the three pillars **fight each other.** You cannot maximize all three at once, and real products are a constant act of trading.

| Scenario | What gets traded | Why it is worth it |
|---|---|---|
| An ATM times out after 30 seconds | **availability** for confidentiality | better to log you out than let a stranger in |
| A game lets you play offline | **integrity** (no live check) for availability | fun is better than perfect freshness |
| A bank requires two-factor on login | **availability** (slower login) for confidentiality | annoyance beats account theft |
| A hospital keeps paper backups | **confidentiality** (paper is leaky) for availability | a system that never crashes can save lives |

The job of a security engineer is not “maximize everything.” It is **decide, deliberately, what to protect most in each situation** — and tell the users what was traded.

## Beyond the triad

Textbooks occasionally extend the model — adding **authenticity** and **non-repudiation**, or splitting out **accountability**. For getting started, do not memorize them: **most situations are covered by the original three**, and the extensions just sharpen the detail. When this book later discusses digital signatures and log auditing, you will meet them naturally.

## Use it tonight

The triad becomes second nature only with practice. Tonight, pick one app you use every day — email, a bank app, a game — and fill in four blanks:

| Question | Your answer |
|---|---|
| What would a confidentiality breach look like here? | e.g., someone reads your private messages |
| What would an integrity breach look like here? | e.g., a message is silently altered |
| What would an availability breach look like here? | e.g., the app is down right when you need it |
| Which pillar does this product protect the most? | this is the interesting one |

The last question is the payoff: the answer tells you what the company decided you value most. Naming it — even roughly — is the whole skill, and it takes about five minutes.

> The triad is a thinking tool, not a checklist. Its power is that any security question, however messy, reduces to “who may read it / has it changed / can I reach it?” Answer those three and you have already found where the weakness lives.

## Next

The triad names *what* we protect. The next article hands you the *vocabulary* for talking about it — a plain-language glossary of the terms you will meet everywhere: CVE, 0-day, phishing, botnet, SIEM, and more. That is `found-03-glossary`.

#### Q: An attacker floods a charity donation site with traffic so no one can donate during a big fundraiser. Which pillar is broken?

* Confidentiality

* Integrity

* Availability

* None — the data is still intact

> 💡 The data and the site are unchanged and unread — the problem is that real donors cannot reach them. That is availability, the third pillar: being there when legitimately needed.
