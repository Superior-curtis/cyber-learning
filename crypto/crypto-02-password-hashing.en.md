# Password Hashing: Why bcrypt and argon2 Win

> 📅 2026-08-05 · Core Concepts
> They are all hashes, so why is MD5 long gone, bcrypt still standing, and argon2 the new favorite? It comes down to four words: slow, salted, and memory-hungry. This article takes you from hash basics to choosing the right algorithm yourself.

---

`pass-01-how-passwords-are-stored` covered the basics: never store passwords in plaintext, store hashes. But "use a hash" and "use the right hash" are two different things — pick the wrong algorithm and you have replaced a safe with a birthday lock.

They are all hashes, so why is MD5 long gone, bcrypt still standing, and argon2 the new favorite? The answer compresses into four words: **slow, salted, and memory-hungry.**

## Fast is bad: a gift to the attacker

Start with something counterintuitive. Normally we like fast algorithms — the faster a page loads, the better. But password hashing is the opposite: **for passwords, fast is bad.**

Why? Because the attacker does not get your password; they get the hash, and they have to *guess*. Guessing means hashing candidate passwords and comparing — `pass-02-cracking-101` walks through the whole mechanism. Here, keep the crux:

> **The faster the algorithm, the more guesses per second; the more guesses per second, the sooner your password falls.**

Storing passwords with MD5 is basically turning cracking speed to maximum. A modern GPU computes billions of MD5 hashes per second; an ordinary password is gone in minutes.

## Four candidates

To be "slow," researchers designed algorithms specifically for passwords. Here is the lineup:

| Algorithm | Born | Signature | Current standing |
|---|---|---|---|
| PBKDF2 | 2000 | Repeats one hash hundreds of thousands of times | Common, but no built-in GPU resistance |
| bcrypt | 1999 | Built-in salt, tunable cost factor, resists GPUs better | Old but strong; default in many frameworks |
| scrypt | 2009 | Requires memory as well as compute | Stronger against ASIC/GPU |
| argon2 | 2015 | Modern memory-hard design, competition winner | The current best choice |

## Why bcrypt is so popular

bcrypt has survived twenty years on two design choices:

1. **Built-in random salt**: every hash includes its own random salt, so even two users with the same password get different hashes — which makes precomputed rainbow tables useless.
2. **A tunable cost factor**: you can make it "slower on purpose." Computers are far faster than in 1999, so you raise the cost parameter until a single computation still takes a few hundred milliseconds, and the attacker is down to a handful of guesses per second.

```
# Illustration: bcrypt cost factor controls the slowness
# At cost = 12, one verification takes about 0.3 s
# The attacker gets ~3 guesses per second, not hundreds of millions
```

> The golden target for password hashing: about 100–300 ms per verification. To a user it is just a moment; to an attacker it is a few guesses per second. A too-fast hash is an unlocked door.

## argon2: the current champion

argon2 won the 2015 Password Hashing Competition. It pushes "slow" to its extreme — not just CPU, but **heavy memory use**. Memory is the hardest resource for GPUs and custom chips to supply in bulk, so argon2 resists "mine it with a rig" attacks especially well.

argon2 comes in variants: **argon2id** is the defender's first choice, resisting both side-channel and GPU attacks. In practice, if you are choosing today: **argon2id first, bcrypt second, scrypt third.**

## How to choose: a decision table

| Your situation | Recommendation |
|---|---|
| New project, no legacy | **argon2id** |
| Framework already ships bcrypt (common) | Use it; raise the cost factor to 100ms+ |
| Old system, want to upgrade | Re-hash on login, migrate gradually to argon2 |
| Any situation | Never store passwords with MD5 / SHA-1 / unsalted SHA-256 |

## The three most common mistakes

* **Storing passwords with ordinary hashes**: MD5, SHA-1, even unsalted SHA-256 are built for data integrity, not passwords — they are far too fast. This is the root of several disasters in `pass-05-real-breaches`.
* **Sharing one salt**: the salt must be unique and random per user. One site-wide salt is no salt.
* **Inventing your own algorithm**: the first rule of crypto is do not roll your own. bcrypt and argon2 have been hammered and attacked by cryptographers for years, which is exactly why they are trustworthy.

## Next

You now know which algorithm to reach for. Next, connect this knowledge to the adversarial side: `pass-02-cracking-101` shows from the attacker's perspective just how valuable the slowness is — understand the enemy, and you know how to tune the defense.

#### Q: Why is being fast actually bad for a password hash?

* Fast algorithms use too much CPU

* The faster the algorithm, the more password guesses an attacker can try per second

* Fast algorithms are always less secure

* Only slow algorithms can use a salt

> 💡 Password cracking is guess-then-hash-then-compare; the faster the algorithm, the more guesses per second, so weak passwords fall sooner.
