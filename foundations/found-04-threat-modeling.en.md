# Threat Modeling for Beginners

> 📅 2026-08-05 · Core Concepts
> Before spending any budget on defenses, figure out what you are protecting. Break a system into assets, attackers, attack vectors, and protection scope, then use STRIDE to build a mental map you can keep checking.

---

You saved up a whole year of salary, installed a door advertised as bulletproof, and forgot to check the windows. An intruder never needs to touch that door — they climb through a window and they are inside in ten minutes. The door was fine. The problem is you never asked, before spending the money: where would the enemy actually come in?

That is exactly what a threat model tries to answer. It is not a tool, and not a document you file away. It is a thinking habit: **before spending anything on defenses, be clear about what you own that is valuable, who wants it, how they could reach it, and how far you plan to defend.** Get those clear, and your defense budget lands where it matters.

If you are new to this series, `found-01-what-is-security` covers what security even means, and `found-02-cia` introduces confidentiality, integrity, and availability. This article teaches the first lesson of security: model the threat first, then defend.

## Why model before defending

Most people's instinct is to install defenses fast: a firewall here, encryption there, two-factor everywhere. But defenses are only good against threats you actually identified. If you never asked who is coming or how, the most expensive lock in the world is just guarding the door that was never the entrance.

Flip the order, and security becomes two steps:

1. **Understand the risk.** Who might attack you? With what method? What is the worst case if it works?
2. **Then choose defenses.** The threats that are most likely and most damaging get the budget, and the rest get less.

A threat model is step one, made formal. It forces you to answer questions instead of buying gadgets by intuition. Defenders have a saying: **if you do not know what you are protecting, you cannot know where to protect it.**

| Approach | Intuition | Threat model thinking |
|---|---|---|
| Starting point | What tools do I have | What is worth protecting |
| First question | Is this feature secure | How could an enemy hurt me |
| Budget | Spread evenly everywhere | Focused on the likely threats |
| Outcome | Bought a lot, unclear what it stops | Fewer, sharper defenses |

> A threat model is not a prophecy of every possible attack. Its job is to make assumptions explicit: who you think the enemy is, what the assets are, where the boundary sits. Clear assumptions give everyone a shared language — and make it possible to judge whether a defense is enough.

## Four things: assets, attackers, vectors, scope

A complete threat model answers four groups of questions. Remember them as one sentence: **who (attacker) will use what (vector) to take what (asset), and where does my scope (boundary) end.**

**Assets** — what an enemy wants to take or break. Accounts, passwords, credit card data, source code, trade secrets, even "the server keeps working" can all be assets.

**Attackers** — the people who want those assets. Motivation changes everything. A random internet opportunist, an organized crime group hunting card numbers, a competitor, a disgruntled ex-employee — they do not attack in the same way.

**Attack vectors** — the path from attacker to asset. Phishing emails, weak passwords, unpatched software, an insider walking out with data, all count.

**Protection scope** — the boundary you plan to defend. Is it the whole company or just one server? Who owns the cloud, and who does not? If the boundary is fuzzy, responsibility is fuzzy too.

## Make it concrete with one example

Let us run the process on a small case: a coffee shop's ordering website.

* **Assets**: customers' email addresses and card numbers, order records, the manager's admin account.
* **Attackers**: most likely a cybercrime group after card numbers; next, a mischievous high-schooler; very unlikely, a nation-state actor.
* **Vectors**: the checkout page where customers type card details, the admin panel the manager signs into with a password, the delivery API.
* **Scope**: the shop cannot control the physical security of the cloud servers — that is the cloud provider's job. But the website's code, configuration, and database permissions are on them.

> This is a defense design exercise, not an attack tutorial. Threat modeling is everyday blue-team work: security teams run the same reasoning to find weaknesses in their own systems and patch them before an enemy does. Evaluating your own systems and practicing in CTF labs is the direction this book encourages.

Once the list is on the table, the priorities are obvious: encryption and login protection on the checkout page matter far more than keeping a prankster out. The model does not fix anything by itself, but it tells you which vulnerability would hurt the most.

## STRIDE: turn threats into a checklist

"Think about who might attack you" is vague. Microsoft's STRIDE classifies the common threats so you can check them one by one:

| Letter | Threat | Plain meaning | One-line example |
|---|---|---|---|
| S | Spoofing | Pretending to be someone else | Signing in as another user |
| T | Tampering | Changing what you should not | Altering a transaction amount in transit |
| R | Repudiation | Doing it then denying it | Placing an order, then claiming you did not |
| I | Information disclosure | Reading what you should not | Card numbers leaking |
| D | Denial of service | Making the system unusable | Overloading the server so nobody can order |
| E | Elevation of privilege | Small power, big actions | An employee gaining admin rights |

STRIDE is not the threats themselves — it is a classification sheet. Map "what an enemy might do" onto the letters and you stop missing whole categories.

## Run STRIDE back over the coffee shop

Let us take the shop and walk it through STRIDE one letter at a time. The result is a concrete risk map:

| STRIDE | Concrete example at the shop | Severity |
|---|---|---|
| S Spoofing | Someone steals the manager's session and logs into the admin panel as them | High — effectively admin rights |
| T Tampering | A delivery order's amount or address gets changed | Medium — hurts operations and trust |
| R Repudiation | A customer claims they never placed an order, and there is no record | Low, but leads to disputes |
| I Disclosure | The checkout page stores card numbers as plaintext in the database | Highest — regulations and liability |
| D Denial of service | A bot swarm brings down the order API | Medium — revenue hit |
| E Elevation | A normal employee finds a way to reach manager rights | High — same as S |

Once the table is filled, the priorities jump out on their own: **fix I (disclosure) and S (spoofing) first, then argue about the rest.** That is the value of STRIDE — not book theory, but a checklist that shows you exactly where the holes are.

## How to write a model down

A threat model does not need to be a thick document, but it should be drawable. Common practices:

* **Data flow diagram**: draw every hop data takes from the user to the database. Every hop is an attack surface, and a place to put a control.
* **Trust boundary**: mark where trust starts and ends. Between the browser and the server is the untrusted zone; inside the server is the trusted zone.
* **Assumption list**: write down things like "we assume the cloud provider handles physical security" and "we assume staff do not write passwords on sticky notes." The moment an assumption stops holding, the model must be revisited.

The diagram does not need to be beautiful — just readable by you and your team. Modeling is not about producing paperwork; it is about making risk visible, because a risk you cannot see is a risk you cannot defend.

## Common myths, taken apart

Threat modeling carries its share of misunderstandings. The three most common:

* **Myth one: a threat model is a pile of documents.** Quite the opposite — it is a way of thinking. Documents just preserve the thinking; the point is that the thinking happens.
* **Myth two: only big companies need it.** Personal websites, side projects, even your home network all have assets, attackers, and vectors. The model can be smaller, but that does not mean it is unnecessary.
* **Myth three: do it once and you are done.** Threats change as the system changes. A model is alive, and only meaningful when it stays updated alongside the system.

Flip those three myths around and you have the essence of threat modeling: **think, at a small and ongoing scale, about who might attack your system and how.**

## Modeling is a loop, not a one-time task

A threat model is not homework you hand in once. Systems get new features, move to the cloud, grow APIs — and every change can open a new vector:

* **When the system changes**: new API, third-party login, server move — run the model again.
* **When new intelligence appears**: a technique is surging in the wild — feed it into the model and re-evaluate.
* **On a schedule**: review at least every six months and check whether the assumptions still hold.

The uncomfortable truth of security is that defense is an ongoing contest. `blue-01-hardening` will later turn these judgments into real system settings. This article's job is to give you the framework to make those judgments.

## Five questions to ask yourself

Even when you are not writing anything down, run these five questions whenever you evaluate a system:

1. What is the most valuable asset in this system?
2. Who is most likely to come after it?
3. What path would they most likely take?
4. Would my current defenses stop that path?
5. If it got through anyway, how would I detect it and respond?

Answer those five and you have just produced a minimum viable threat model. Write the answers down and it is documented; change the system and ask them again.

## Where this series goes next

This article gives you the habit of modeling before defending. Next, you need to understand the machinery underneath so you can evaluate the vectors themselves — `found-05-how-the-web-works` walks through DNS, IP, ports, and HTTP in one pass. After that, every attack and defense article you read will stand on firmer ground.

## The one-liner to remember

A threat model is not a prophecy — it is making assumptions explicit. Who (attacker) will use what (vector) to take what (asset), and where your scope ends. Sort out those four before you buy any defense, because the most valuable moment in security is the one before you act.

#### Q: What should come first in a threat model?

* Install a firewall and antivirus right away

* Identify assets, attackers, vectors, and protection scope

* Change every password to the most complex one

* Wait until you are actually attacked

> 💡 Threat modeling says understand the risk before choosing the defense. You need to know what you protect and how the enemy reaches it, or the budget lands in the wrong place.
