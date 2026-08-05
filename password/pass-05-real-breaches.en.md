# Lessons from Real Password Breaches

> 📅 2026-08-05 · Deep Dive
> RockYou, LinkedIn, Adobe, 23andMe — four famous password breaches unpacked: what went wrong, how attackers got in, and how each lesson becomes a defense habit.

---

## Password breaches are a design problem, not bad luck

Over the past two decades, the internet has seen several "hundred-million-scale" password leaks. It is easy to assume the hackers were simply brilliant. But look closely at the cases, and you will find the same root cause again and again: **the company stored passwords in the wrong place**.

This article treats four of the most-cited breaches as teaching material. For each one we look at what happened, what went wrong at the root, and the defense lesson you should keep. None of the lessons require elite hacking skill to understand — but each one needs to be remembered by every engineer, including you.

> This article is defense teaching, not an attack manual. Do not use leaked credential data for anything unauthorized — accessing other people's accounts and data without permission is illegal in most countries.

## Four cases at a glance

| Breach | Year | Scale | Root cause | One-line lesson |
|--------|------|-------|-----------|-----------------|
| RockYou | 2009 | ~32 million passwords | Passwords stored in plaintext | Never store passwords in plaintext |
| LinkedIn | 2012 | ~6.5M hashes first, ~117M records later | Unsalted SHA-1 | Use slow, salted hashes |
| Adobe | 2013 | 150M+ user records | Encryption instead of hashing | Encryption is not hashing |
| 23andMe | 2023 | ~6.9M ancestry records | Credential stuffing + password reuse | Reused passwords are the real risk |

Keep this table in mind; now we go case by case.

***

## Case 1: RockYou (2009) — the cost of plaintext

RockYou built Facebook games. In 2009, attackers used SQL injection to break into its database and took roughly 32 million user passwords. The most striking part: **the passwords were stored as plaintext**. The attackers did not need to crack anything — they simply copied the table and went home.

### What went wrong

* Passwords sat in the database in readable form, like a house key taped to the doorframe.
* No hashing, no encryption, no masking — not even one layer.
* The intrusion itself was the entry point, but the scale of the damage was decided entirely by how the passwords were stored.

### Defense lessons

1. **Never store passwords in plaintext.** A user's password should never exist in readable form anywhere.
2. Store them with **slow, salted** adaptive hashes such as bcrypt, argon2, or scrypt.
3. Even when a breach happens, make sure there is a wall between "data taken" and "data usable."

> For a refresher on the difference between hashing and encryption, revisit `crypto-01-hash-vs-encryption` and `crypto-02-password-hashing`.

***

## Case 2: LinkedIn (2012) — salt is not optional

In 2012, LinkedIn was breached. Around 6.5 million SHA-1 password hashes surfaced first; in 2016, a larger set of roughly 117 million account records was offered for sale online.

LinkedIn did not store plaintext — it used SHA-1. The problem is that this SHA-1 had **no salt**.

### Why "no salt" is fatal

* SHA-1 is extremely fast. With GPUs, attackers can compute billions of hashes per second, so brute force is cheap.
* Without a salt, **identical passwords produce identical hashes**. If two users choose the same password, their hashes are exactly the same — crack one and you have cracked both.
* Unsalted hashes can also be checked against precomputed tables (rainbow tables), which turn common passwords into instant lookups.

### Defense lessons

1. Use deliberately **slow** adaptive algorithms like bcrypt, argon2, or scrypt to push the cost of brute force through the roof.
2. Give **every password its own unique salt**, so the same password never produces the same hash twice.
3. Choosing the right hashing scheme directly decides how many leaked passwords actually get recovered.

***

## Case 3: Adobe (2013) — encryption is not hashing

In 2013, Adobe was breached and more than 150 million user records leaked, including passwords and the answers to password-hint questions. Adobe protected the passwords with 3DES encryption and shared **one key across all of them**.

### What went wrong

* **Encryption is reversible.** With the key — or a guessable one — passwords can be decrypted back to plaintext. Adobe's key was recoverable from material taken in the same breach.
* **Password hints** ("What is your pet's name?") made cracking easier; the hint was often half the answer.
* Adobe also reused passwords across its own internal systems, letting the intrusion spread laterally.

### Defense lessons

1. **Hashing is not encryption.** Hashing is one-way; encryption is reversible. Password verification needs one-way hashing, not reversible encryption.
2. Manage keys **separately from the data** they protect, rotate them, and never share one key across everything.
3. Drop password hints, or at least stop them from leaking so much.
4. Do not reuse credentials internally — the same password should not protect development, staging, and production at once.

***

## Case 4: 23andMe (2023) — the chain reaction of reused passwords

In 2023, the genetics company 23andMe was breached, and roughly 6.9 million ancestry records were scraped. The attack was not a clever exploit — it was **credential stuffing**: taking old "email plus password" pairs from earlier breaches and trying them on other sites. It only works when users reuse passwords.

### Why this one matters

* Attackers already hold huge lists of leaked pairs; **they do not need to crack anything new**.
* 23andMe's "DNA Relatives" feature shared data by default, so once one account was in, relatives' data came along with it.
* The message: your password may never have been "stolen" directly — someone is still using it to knock on your door.

### Defense lessons

1. **Do not reuse passwords.** A password manager that generates a unique password per site is one of the cheapest, most effective defenses (see `pass-04-defenses`).
2. **Turn on MFA.** Even when a password falls, the second factor can still hold the door.
3. Use breach-monitoring services to learn early when your credentials show up in someone else's dump, and reset right away.
4. For service providers: sensitive data-sharing features should be **opt-in**, not on by default.

***

## Put the lessons into practice: three steps for engineers

#### Audit how you store passwords today

Open one user table in your database and check the column: plaintext, reversible encryption, or a salted slow hash? This is your first mirror.

#### Migrate to slow, salted hashing

Move to bcrypt, argon2, or scrypt. Give every password a unique salt and migrate old records gradually.\n\n→ revisit: crypto-02-password-hashing

#### Add MFA and breach monitoring

Give accounts a second lock so a lost password is not a lost account, and watch monitoring feeds to spot your credentials in public dumps early.

## Five lessons that keep repeating

Put all four cases together and the real lesson repeats five times:

1. **Storage design decides everything** — plaintext, fast hashing without salt, and reversible encryption are three flavors of the same disaster.
2. **Salt must be unique** — every password deserves its own.
3. **Hashing must be slow** — adaptive algorithms push brute force to an impossible price.
4. **Never reuse passwords** — attackers love knocking on new doors with old keys.
5. **Add another layer** — MFA and monitoring mean "password lost" no longer equals "account lost."

## A defense checklist you can use tomorrow

| Check | Pass criteria |
|-------|---------------|
| Password storage | Only bcrypt / argon2 / scrypt, and salted |
| Salt | Unique and random per password |
| Plaintext or reversible encryption | No field in the database can be turned back into a password |
| MFA | Enabled on every important account |
| Password reuse | A password manager, one unique password per site |
| Breach monitoring | Subscribed, and reset immediately on alert |

> If you take away one sentence: your defense is not about how memorable your password is — it is about how worthless it becomes once stolen. Hashing plus salt plus MFA is the combination that makes it worthless.

These breaches are history, but their lessons replay every day. Next, we move from passwords to applications and look at how OWASP ranks the top web security risks.

→ next: web-01-owasp-top10

***

## Quiz

#### Q: What is the most striking lesson of the RockYou breach?

* Storing passwords with SHA-1 is secure enough

* Passwords were stored in plaintext, so attackers did not even need to crack them

* Security questions alone keep accounts safe

* Salted hashes can never be cracked

> 💡 RockYou stored roughly 32 million passwords as plaintext in its database. The attackers took the whole table and did not need to crack a single one.

***
