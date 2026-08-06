# Smart Contract Vulnerabilities (Conceptual, Educational)

> 📅 2026-08-05 · Deep Dive
> A smart contract is code that 'can never be changed once written' — so its bugs are permanent scars. This article conceptually breaks down the classic contract vulnerability classes (reentrancy, overflow, access control) and the defense for each. Defense-first; no exploitable code.

---

`chain-01` and `chain-02` gave you the ledger and the keys. Now the most-watched corner of on-chain security: **smart contracts.**

A smart contract is **a program that runs on the chain** — and the on-chain rule is "once written, never changed." So: **once a contract has a bug, it is a permanent scar.** This article conceptually breaks down the classic vulnerability classes and defenses — defense-first, no exploitable code.

> This is a defense lesson, not an attack lesson. We only describe how the flaws happen and how to defend. Real contract-flaw details are public research, but applying exploitation to real assets is a crime; learn only on controlled testnets and practice grounds.

## Why contract bugs are especially "expensive"

With traditional programs, a bug is fixed by **shipping an update.** Smart contracts cannot — once deployed, the logic is fixed forever:

* Money caught in a flaw can only be moved via a "fixed contract," but the assets under attack are often already lost.
* Each contract-hacking incident loses millions.
* So the first principle of contract security is: **before launch, treat bugs as "certain to exist" and audit accordingly.**

## Classic vulnerability classes (conceptual)

| Class | One-line concept | Defense direction |
|---|---|---|
| Reentrancy | Between "send money" and "update state," the other side can call you again | Update state first, then transfer; use a mutex |
| Integer overflow | Values overflow/underflow, bypassing checks | Safe-math libraries, bounds checks |
| Access control | Only the owner should act, but anyone can | Strict permission modifiers, audits |
| Precision/rounding | Decimal rounding silently loses funds | Fixed-precision representation |
| Oracle dependency | Price data gets manipulated | Reliable sources, fault tolerance |

The point is not memorizing names but the shared root: **contracts bind "money logic" and "program logic" together, so any logic flaw can be a direct financial loss.**

## Reentrancy: the classic example

Reentrancy deserves a closer look — it is the most famous contract attack (the DAO incident).

The concept: a contract **sends funds out**, then **updates its own accounting.** "Sending out" calls the recipient's program. If the recipient is an attacker-controlled contract, it can, in the instant before you update your ledger, **call you again** — so you send again, still before updating, and loop until the funds run dry.

The defense is clear:

1. **Change state first, transfer after** (Checks-Effects-Interactions).
2. **Use a mutex**: block re-entry while an operation is in progress.

> Reentrancy's lesson is not blockchain-only. The window between "commit" and "fulfill" appears everywhere in traditional systems too — CSRF in web-04-ssrf-csrf-upload also exploits a state-update window. Understand the pattern and it serves you anywhere.

## Why it is especially hard here

Contract security is hard for structural reasons:

* **Immutable**: mistakes cannot be fixed.
* **Public**: everyone's funds sit on the same inspectable program; attackers have strong motive and time to study it.
* **Money is code**: a bug is directly money.

That is why the industry standard is "**multi-audit**": independent teams audit before launch, thorough rehearsal on testnets, and keeping the riskiest logic as simple as possible.

## Next

You have met the vulnerability classes. Final stop — fold on-chain security into practice: `chain-04-blockchain-security-practice` looks at how the whole blockchain ecosystem gets attacked (phishing, fake tokens, exchanges) — and how to learn and defend safely.

#### Q: Why are smart-contract bugs far more severe than ordinary program bugs?

* Because contracts are more complex

* Because a contract is immutable once deployed, so bugs are permanent, and a bug is directly a financial loss

* Because a contract can only be written once

* Because there is no testnet

> 💡 Immutable + public + money-is-code: cannot fix, everyone can see, and a bug is money — three factors stacked together.
