# Defending: MFA, Password Managers, and Lockout

> 📅 2026-08-05 · Core Concepts
> The payoff: slow hashing, password managers, passkeys, phishing-resistant MFA, lockout and throttling, and breach monitoring. Why long random plus a manager plus MFA beats complexity rules.

---

## The defender has a weapon rack too

The last two chapters laid every attacker card on the table: dictionaries, brute force, rules, GPUs, rainbow tables. By now it can feel like a losing game — but the defender has a weapon rack too, and every tool on it maps to a specific weakness. This chapter systematically closes each gap exposed in `pass-02-cracking-101` and `pass-03-cracking-tools`.

Hold on to the master thread: **cracking time = keyspace ÷ guesses per second.** Everything in defense either grows the numerator or shrinks the denominator. Every defense below fits that equation.

## Defense one: switch to deliberately slow hashing

Remember the GPU trying billions of MD5s per second? That is the worst possible starting point. The first thing a defender does is make sure the system **does not store passwords with a fast hash at all** — period. Choose a modern password hash (argon2id, bcrypt, scrypt) and salt every account independently; the `crypto-02-password-hashing` article covers the reasoning in depth.

> The algorithm is a speed gate, not a detail. On the same GPU, argon2id can cut guesses-per-second to a millionth of what MD5 allows. Upgrading the algorithm is equivalent to making every password several characters longer out of thin air.

Why can you verify this yourself? Run `hashcat -b -m 25600` from `pass-03-cracking-tools` and watch how slow it really is. The numbers speak — which is why choosing the algorithm always sits at the top of the defensive checklist.

## Defense two: a password manager makes long random actually possible

This series keeps hammering "long random passwords" — but no human can memorize a hundred different 20-character passwords. Enter the most underrated defensive tool: the **password manager**. It generates, stores, and fills strong random passwords for you; you only need to remember one master password.

The manager fixes more than length. It fixes **reuse**. Password reuse feeds dictionary attacks: one leaked old password becomes a key that opens all your accounts. A manager gives every site a unique password, so even if one service gets breached, your other accounts stay untouched.

Choosing a manager: prefer locally encrypted storage, open source or independently audited products, and enable two-factor on the master password. It deserves a deliberate choice.

## Interlude: length floors and passphrases

Instead of forcing users to pack every symbol into an 8-character password, simply **raise the length floor**. A **passphrase** — a few random words strung together, like `celery orbit velvet tractor` — buys its keyspace through length, and humans find it surprisingly memorable.

This matches the conclusion of `pass-02-cracking-101` exactly: length beats complexity. A 20-character passphrase, even lowercase only, has a far larger keyspace than a 10-character mix of upper case, digits, and symbols. Password manager generators likewise lean toward long random strings rather than short complex ones — the latter only *look* secure.

## Defense three: passkeys, the successor to passwords

**Passkeys** aim to retire the password altogether. The idea comes from public-key cryptography (`crypto-03-asymmetric-crypto`): the server stores only your public key, the private key stays on your device, and login proves identity with a signature unlocked by biometrics.

Against every cracking technique in this book, passkeys have a structural advantage: there is no password, so there is no hash to guess; the private key never leaves the device, so a phishing site cannot steal it. It is the closest thing to a root-cause fix. But adoption requires ecosystem buy-in, so for most systems passkeys are "modern passwords on steroids," not an immediate replacement for everything.

## Defense four: MFA, so guessing the password is not enough

However strong a password is, it can leak. **Multi-factor authentication (MFA)** works on the logic that even if an attacker gets your password, they still need a second factor to sign in. The second factor belongs to a different category — not something you *know* but something you *have* (phone, security key) or *are* (fingerprint, face).

But MFA has levels, and they are not equal:

| MFA type | Example | Phishing resistance |
|----------|---------|---------------------|
| SMS codes | One-time code by text | Weak (SIM swapping) |
| TOTP authenticator | Google Authenticator, Authy | Medium |
| Push approval | Tap "Allow" on your phone | Medium |
| Passkey / FIDO2 key | YubiKey, platform passkeys | Strong |

**Phishing-resistant MFA** (FIDO2, passkeys) binds the credential to a specific website domain, so even a fake login page cannot replay it. If you are choosing MFA for a system, reach for the phishing-resistant variant — that battle is covered in detail in `blue-06-phishing-defense`.

## Interlude: do not leave a backdoor in security questions

"What was your first pet's name?" "Where did you grow up?" — **security questions** like these often appear in account recovery flows, and they are a perfect target for dictionaries and OSINT (`recon-01-recon-osint`): the answers are findable on social media, or are simply common names. The cleanest move is to skip security questions entirely and use backup codes, MFA recovery, or passkeys instead. If you must keep them, treat the answers as another secret — and protect them with the same throttling.

## Defense five: lockout and throttling make brute force pointless

Remember the equation? The denominator is guesses-per-second, and GPUs shrink it. But if you limit attempts **server-side**, the table turns: no matter how fast the GPU is, you only allow it one try per second.

* **Account lockout**: after N consecutive failures, lock the account for a while.
* **Throttling**: cap login attempts per IP address or per account.
* **Captchas and delays**: add a human-vs-machine cost.

Watch the side effect: attackers can weaponize lockout to lock *your users* out of their own accounts. In practice, throttling usually beats hard lockout — it slows the attacker without hurting innocent people.

## Defense six: breach monitoring tells you the password already leaked

The best password to change is one you know has leaked. **Breach monitoring** services (like Have I Been Pwned) aggregate publicly disclosed breach databases so you can check whether your email or passwords appear in known leaks.

Organizations can go further: compare internal password hashes against breach lists to find out whether **employees or users are currently using passwords that are public knowledge** — then force a reset. This is the `pass-03-cracking-tools` audit workflow extended into production. Attackers use the same data; defenders should too.

## Interlude: training that explains the why

Older password rules were often "12+ characters, caps, digits, symbols, rotate every 90 days" — telling people *what* to do without *why*. Modern password policy education has shifted focus: teach users to use a **password manager**, introduce them to **passkeys**, and explain **why reuse is the killer**. When users understand that "reuse means one leak opens everything," they tend to follow the rules more willingly than when forced to rotate on a schedule.

## Defense seven: combine the three

Individual defenses work, but real security comes from combination. Why does "long random + a manager + MFA" beat complexity rules?

| Strategy | Fights against | Weakness |
|----------|---------------|----------|
| Complexity rules (forced caps + digits + symbols) | Pure brute force | Hard to remember → reuse → weaker overall |
| Long random passwords | Dictionary, rules, brute force | Humans cannot remember → need a manager |
| Password manager | Reuse and weak passwords | Master password can be phished |
| MFA (phishing-resistant) | Leaked passwords, phishing | Needs ecosystem support |
| Slow hash + salt | GPU throughput | Protects the server side only |

Complexity rules look strict, but they push people toward sticky notes and the same password everywhere — shifting the attack surface from "brute force" to "leaked-then-reused," which happens far more often. "Long random + manager + MFA" kills one whole class of attack per tool, and each tool plugs the other's gaps. **It is the more pragmatic modern answer than "remember a more complex password."**

## A one-page defense checklist

Compress the whole chapter into a checklist you can pin to the wall:

* [ ] Passwords stored with argon2id, bcrypt, or scrypt, salted per account
* [ ] No fast hashes (MD5, SHA-1) anywhere for password storage
* [ ] A password manager is mandatory; reuse is banned
* [ ] Length floor of 12+, or passphrases in use
* [ ] MFA on every login, preferring phishing-resistant variants
* [ ] Throttling and rate limits on login and recovery flows
* [ ] Hashes compared against breach lists on a schedule; resets on a hit
* [ ] Training explains the why, not just the rules

Each item maps back to a whole section of this chapter. If one item is missing, start there — defense is not achieved in one shot, it is built item by item.

## Do not forget the recovery flow

No matter how good the lock, a user will eventually forget their key. Account recovery is one of the favorite shortcuts attackers hunt for — a badly designed "forgot password" flow lets them bypass all seven defenses above. A few principles: send recovery only to channels you have verified (not a mutable number or mailbox), layer MFA onto it, throttle recovery attempts just like logins, and notify the user after any recovery. Defense is a chain, not a single link.

## When the worst happens: detection and response

No matter how complete the defense, never assume it cannot be bypassed. So there is one more layer: detection and response.

* **Unusual login alerts**: a new device, a new country, a login at 3 a.m. — all signals worth telling the user about.
* **Login failure trends**: many failures on one account, or one or two failures on many accounts (the signature of password spraying) — both should trigger an alert.
* **Credential change notifications**: a password reset, an MFA change, a recovery flow run — these events should reach the owner immediately.

The details of detection belong to `blue-02-logging-siem` and `blue-04-incident-response`. The thread to hold here: password defense is not "set it and forget it" — it is a loop of prevent, detect, and respond.

## The enterprise layer: SSO and zero trust

Individuals can get by with a manager and MFA; enterprises need **identity infrastructure**. **Single sign-on (SSO)** lets employees move through every internal system with one set of enterprise credentials, centralizing both password management and auditing; **conditional access** decides whether to let someone in based on device, location, and risk signals. `blue-07-iam-zero-trust` develops these ideas fully — the thread to hold here is simple: collapsing "a password per system" into "one identity hub plus MFA" shrinks the attack surface dramatically, and removes the biggest feeding ground for the "one password opens everything" tragedies you will meet in `pass-05-real-breaches`.

## One paragraph of summary

Seen through the lens of attack mechanics: dictionaries fear long random; rules fear passwords that follow no pattern; GPUs fear slow hashes; rainbow tables fear salt; reuse fears the manager; phishing fears phishing-resistant MFA; brute force fears throttling. Defense is not one impenetrable wall — it is a set of interlocking gears, each making guesses-per-second smaller or the keyspace larger.

## What is next

Theory and practice are both in place; now look at the real world. `pass-05-real-breaches` walks through several major real-world breach events and shows how each defense above — by *not* being implemented — turned into catastrophe. History is the best defense textbook there is.

#### Q: Why does a long random password plus a manager plus MFA beat complexity rules?

* Each piece eliminates a whole class of attack, and their gaps complement each other

* Because complexity rules have been proven technically invalid

* Because MFA can fully replace passwords

* Because short passwords are no longer accepted by any system

> 💡 Complexity rules push people toward reusing passwords, shifting the attack surface; long random kills dictionary and brute force, the manager kills reuse, and MFA kills the impact of leaks — each plugs the gaps of the rest.
