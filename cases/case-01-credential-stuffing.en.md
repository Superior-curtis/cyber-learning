# Credential Stuffing: One Password, Many Accounts

> 📅 2026-08-05 · Deep Dive
> Site A leaks your password; the attacker tries the same credentials on sites B, C, D — because you reuse passwords, one leak becomes many compromised accounts. This is a complete attack-chain case, and a live lesson in why not to reuse passwords.

---

**Case**: you registered at shopping site A with your email and password `sunshine2020`. One day site A is breached and its database leaks. A month later, password-reset emails start arriving — social network, bank, work email, one after another.

You were not "hacked" five times. You were "stuffed" once. This attack chain is called **credential stuffing**.

> Security note: this is a defense lesson — teaching you the chain and why password management matters. Real credential stuffing is a crime; what you should take away is "password manager + no reuse."

## The chain

#### Leak

Site A database leaks; your credentials appear in a public breach list.

#### Collect

The attacker organizes the leak into a "username:password" dictionary.

#### Automated attempts

The same credentials are automatically tried against sites B, C, D.

#### Compromise

Every site where you reused the password gets logged into, one after another.

**Key: the attacker "cracked" nothing.** They just took your "already leaked password" and tried it wherever you "reused it" — the mechanism of `pass-02-cracking-101` and the storage of `pass-01-how-passwords-are-stored` merge into one story.

## Why it works

Credential stuffing rests on **password reuse**. Statistically, most people use the same password on multiple sites — because "remembering" is tiring.

| Situation | Result |
|---|---|
| A unique password per site | One leak = one loss |
| The same password everywhere | One leak = loss of everything |

> Stuffing turns "one leak" into "total compromise" by exploiting reuse. Conversely, a password manager plus a unique password per site breaks the chain at the first link.

## Detection and defense

**As a user:**

* A password manager with long, random, unique passwords per site (`pass-04-defenses`).
* Enable MFA — even if credentials are stuffed, there is another lock.
* Use a breach-check service to see if your credentials appear in leaks.

**As a site:**

* Monitor abnormal logins (new devices, odd geography, failures-then-success bursts).
* Check against "known leaked passwords" and block logins with them.
* Require MFA, especially for sensitive accounts.

> The most important lesson: stuffing is not "high-tech," it is "statistics." It bets that you reuse passwords. Password manager + MFA + unique passwords make this chain nearly fail.

## Next

This chain threads `pass-01` through `pass-04` into one story. For the full defense chapter, `pass-04-defenses`; for historical breach cases, `pass-05-real-breaches`.

#### Q: What is the most fundamental reason credential stuffing succeeds?

* The attacker cracked a strong password

* The user reuses the same password across sites, so one leak applies to many accounts

* The site has no firewall

* The password is too short

> 💡 Stuffing is not cracking; it is reusing already-leaked credentials elsewhere. Password reuse turns one leak into total compromise.
