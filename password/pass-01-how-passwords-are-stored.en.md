# How Passwords Are Actually Stored

> 📅 2026-08-05 · Core Concepts
> Websites do not store your password text — they store its fingerprint. Learn how hash, salt, and pepper turn passwords into hard-to-reverse digests, and why this design protects you when a database leaks.

---

Imagine you work at a coffee shop and you write every customer's credit card number on a whiteboard on the wall. Anyone who walks in could photograph the whole set. Sadly, many early websites stored passwords exactly like that: as plaintext. It took a steady stream of database leaks before the industry learned the lesson — **passwords should not be stored; they should be transformed.**

This article explains what modern websites actually store, and how the hash / salt / pepper design still protects you in the worst case, when the entire database walks away. If you have not read `crypto-01-hash-vs-encryption` and `crypto-02-password-hashing` yet, start there — those two articles lay out the core properties of hashing.

## Three storage styles, three disaster levels

Looking at the three historical approaches shows why this question matters:

| Approach | Plain meaning | After the database leaks |
|---|---|---|
| Plaintext | Store the password as-is | Every password exposed immediately. Disaster |
| Encryption | Encrypt with a key, reversible | If the key leaks too, it is plaintext; and the key itself needs a vault |
| Hashing | One-way transform into a fixed-size fingerprint | No direct reversal, but still crackable in weak designs |

The decisive difference is **direction**. Encryption is two-way: with the key you can reverse it, so key custody becomes a second vault. Hashing is one-way: what goes in does not come back out. That is exactly what passwords need — **a system should have no ability to reverse your password at all.**

## Hash: the fingerprint of a password

A hash function turns an input of any length into a fixed-length string. The same input always yields the same output, and changing one character changes the output completely.

Using `sha256` as a demo (educational only — real password storage picks different algorithms):

```bash
# turn a password into a hash (concept only)
echo -n "correct horse" | sha256sum
# 6d2582b8... (always 64 hex characters)
```

That hash is the fingerprint: the website verifies the fingerprint, not the original. You type your password, the site hashes it, and compares the result against the stored hash. Since hashing is one-way, an attacker with the database still cannot directly read your password.

But there is a deadly catch. Human passwords are not that many: `password`, `123456`, `iloveyou`... An attacker can hash all the common passwords in advance to build a lookup table, then match it against the stolen database. That table is called a **rainbow table**. Same password, same hash algorithm, same hash — one lookup and it hits.

## Salt: make everyone look different

The cure for rainbow tables is to mix in a random "salt" before hashing.

* Every user gets a **different random salt**.
* The stored value is not `hash(password)` but `hash(password + salt)`, with the salt stored alongside.
* Because every salt differs, even identical passwords produce different hashes — the rainbow table collapses instantly.

| | No salt | With salt |
|---|---|---|
| Same password | Same hash, table lookup works | Different hash, table lookup fails |
| Precomputed table | Works for every user | Must be recomputed per user |
| Attacker cost | Compute once, hit everyone | Compute once, hit one user |

Salt does not need to be secret. Its job is not concealment; it is making every user's hash unique. Even with the salt public, the attacker is forced to crack each user individually.

## Pepper: one last lock on the server side

Salt lives in the database; pepper is different. It is a **secret value that exists only on the server side** and is never stored with the hashes.

* The stored value is `hash(password + salt + pepper)`.
* When the database leaks, the pepper **does not leak with it**. The attacker gets hashes and salts but is missing that one ingredient.
* Even with the entire database in hand, the attacker still lacks a critical input, and the cost of cracking climbs steeply.

| | Salt | Pepper |
|---|---|---|
| Where it lives | In the database with the hash | On the server, separate from the database |
| Secret? | No | Yes, must be protected |
| After a leak | Attacker can still crack user by user | Attacker is missing an ingredient |
| Per-user? | Yes, different for everyone | No, usually one shared system-wide value |

> Think of the three layers as one chain: hash makes the password unrecoverable, salt makes every result unique, and pepper leaves the attacker short of a critical ingredient even after a database leak. Together they are the modern standard posture for password storage.

## The moment of login, end to end

Now stitch the sections together and watch one login actually happen:

#### The user types a password

The password travels only between the user and the site, and it should go over HTTPS (see found-05-how-the-web-works).

#### The site looks up the salt for that user

Every user has their own random salt, stored in the database.

#### The site computes hash(password + salt + pepper)

It runs a slow algorithm like bcrypt and compares the result with the stored hash.

#### Match means login succeeds

The site only compared fingerprints. At no point did it read, or need to recover, the original password.

Notice what that last step means: **nowhere in the entire system is there a stored "your password" that could be opened.** That is the difference between transforming and storing.

## Use the right algorithm, not a homemade one

Even with salt, a "fast" hash like `sha256` is wrong for passwords, precisely because it is fast — an attacker can try billions of guesses per second. Algorithms designed for passwords are deliberately **slow**:

| Algorithm | Traits | Use |
|---|---|---|
| bcrypt | Built-in salt, adjustable cost | The most common password hash |
| scrypt | Memory-hard, resists hardware acceleration | Memory-intensive, much harder to brute-force |
| argon2 | Modern standard, tunable memory and time | One of the current first choices |

> A "homemade" password protection scheme is a red flag. Use battle-tested standards like bcrypt, scrypt, or argon2 — good engineers have already stepped on the landmines for you. Security comes from mature design, not secret inventions.

Slowness is the point of defense: the goal of password hashing is not speed but making **every single guess expensive enough that the attacker gives up.** `pass-02-cracking-101` approaches the same algorithms from the attacker's side and shows how they are actually attacked — the other half of understanding the defense.

## Common mistakes, checked one by one

Here are the most common mistakes lined up against their fixes, so you can check against them while developing:

| Common mistake | Why it is dangerous | The right fix |
|---|---|---|
| Using MD5 or SHA-1 | Too fast, and known collisions | Move to bcrypt / scrypt / argon2 |
| No salt | Rainbow tables hit in one shot | Generate a fresh random salt per signup |
| A fast hash used for passwords | Billions of guesses per second | Use an algorithm that is deliberately slow |
| A homemade "encrypted variant" | Never professionally audited, easy to get wrong | Use a standard algorithm, do not invent one |
| Pepper stored with the hashes | Leaks along with everything else | Keep pepper separate on the server |

Nearly every "password breach" tragedy checks at least one box in that table. Defense is not about perfection — it is about getting these rows right.

## The reality of database leaks: storage design is everything

Treat "the attacker gets the hash file" as a premise, not an assumption. Look back at the major breaches: what the attackers walk away with is usually the whole database, hashes and salts included. The true dividing line between safe and unsafe users is the storage design itself.

This also explains why password-leak headlines keep appearing while the corresponding accounts rarely get taken over — a good storage design turns one leak from "everything is compromised" into "each user must be cracked one by one." `pass-05-real-breaches` walks through real cases where design quality decided the scale of the disaster.

## For users, and for developers

If you are a user, this article teaches you one thing: **reusing a password across sites is putting all your eggs in one basket.** No matter how well a site protects you, you should still use unique passwords and a password manager. `pass-04-defenses` later assembles the full defense picture.

If you are a developer, this article is your checklist:

1. Never store plaintext.
2. Use bcrypt / scrypt / argon2, not a homemade scheme.
3. Rely on the algorithm's built-in salt, or add a random salt yourself.
4. When possible, add a pepper stored separately.
5. Treat "the database will leak" as inevitable and design for it from day one.

## Where this series goes next

You now know how passwords should be protected. The next step is viewing the same topic from the attack side: `pass-02-cracking-101` breaks down how attackers try to reverse these hashes, and which factors make cracking easier or harder. Understand the spear, and you understand the shield.

#### Q: Why do modern websites use hashing rather than encryption to store passwords?

* Hashing is faster, so logins do not wait

* Hashing is one-way, so the system itself cannot reverse the password and leaks are lower risk

* Encryption needs a key, and keys are too expensive

* Hashing turns long passwords into shorter ones

> 💡 Encryption is two-way: with the key you can reverse it, which means the system can recover passwords — and a leaked key equals leaked plaintext. Hashing is one-way: the system verifies a fingerprint and cannot recover the original, which is exactly what password storage needs.
