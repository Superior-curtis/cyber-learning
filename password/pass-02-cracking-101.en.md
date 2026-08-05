# How Password Cracking Works

> 📅 2026-08-05 · Core Concepts
> The mechanics of password cracking: how attackers work from the hash, and how dictionaries, brute force, rules, and GPUs shorten the race.

---

## The short version: cracking is guessing, not decrypting

Security teams occasionally end up with a file of their own password hashes, or practice in a CTF lab. In those situations, what they hold is almost never a plaintext list of passwords. It is a string of seemingly meaningless characters — a **hash** of each password. And "cracking" is the process of pushing that hash backward to the original password.

One crucial premise comes from `crypto-02-password-hashing`: **a good hash is one-way.** Given a password, computing the hash is easy; given a hash, no key can "decrypt" it back. So the person doing the cracking is really guessing — taking a candidate, hashing it the same way, and comparing the result. The whole story of cracking is one loop — "guess, hash, compare" — plus every trick in the book for making that loop run faster.

> Authorized use only. This series explains cracking as concepts and principles, so defenders understand the threat and so you can practice on your own systems, CTF labs, or explicitly authorized tests. Running password testing against any system, network, or account you do not own is illegal — and a criminal offense in many countries.

## The guessing loop: the basic unit of cracking

Reduced to its smallest form, cracking has just three steps:

1. **Guess a password** (for example, `sunshine`).
2. **Hash it** with the same hash function.
3. **Compare** it to the target hash. A match means success; otherwise, back to step 1.

The efficiency of this loop comes down to three variables:

* **What you guess** — the quality of the guess source;
* **How many guesses per second** — the speed of the hardware and algorithm;
* **How many guesses it takes to hit** — the size of the keyspace.

Every attack technique you are about to see is really "changing one of these variables." Dictionary attacks improve what gets guessed, brute force fights the keyspace, GPUs raise guesses per second, and rule-based mangling improves both the quality and the range of what gets guessed.

## Offline vs online: two very different guesses per second

Before going further, get one distinction straight, because it decides which defenses work.

Most of what this book calls cracking is **offline cracking**: the attacker already has the hash file and runs it on their own (or rented) GPUs, with no server in the loop. That is the worst case — the attacker is limited by nothing and can compute for as long as they like. The only ceiling is how hard the hash itself is.

The other kind is **online cracking**: trying passwords against a login page directly, where every attempt passes through the server. This is slower by several orders of magnitude, and the server can interfere — locking accounts, throttling, adding captchas.

Why does the distinction matter? Because it explains why the lockout strategies in `pass-04-defenses` only fight online cracking. Offline cracking has to be stopped by "a strong enough password plus a slow enough hash." A defender must handle both threats, not just pick one.

## Dictionary attack: humans are not random number generators

The oldest — and often the most effective — method is the **dictionary attack**. The attacker builds a list of candidates — common words, names, birthdays, `123456`, `password`, the company name, pet names, band names — then hashes and compares every entry on the list.

The dictionary attack works so well because **the way real people choose passwords is highly predictable.** Years of leaked password databases have been analyzed over and over; the top ten entries are the same old faces, and the top hundred account for a striking percentage of all passwords. When people are told to "make a secure password," the most common reaction is to add an exclamation mark, a digit, or swap an `e` for a `3` — and the dictionary and its rules are already waiting for those moves.

> A fun fact: in real breaches, a password that *looks* secure is usually far more dangerous than a truly random one — because it is guessable.

## Brute force: walking the entire keyspace

When the dictionary and its rules come up empty, the attacker falls back to the dumbest and most thorough method — **brute force**: try every combination of some character set (say lowercase letters plus digits), from first to last.

What decides how hard brute force is, is the **keyspace** — the total number of combinations:

```
8 chars, lowercase only:     26^8     ≈ 208 billion
8 chars, upper + lower + digits: 62^8 ≈ 2.18 trillion
12 chars, all 95 printable:  95^12    ≈ 5.4 × 10^23
```

See the pattern? **Length matters far more than complexity.** Each extra character multiplies the keyspace by the size of the character set; a complexity rule usually multiplies it by just two or three. That is why `pass-04-defenses` keeps hammering on "long, random passwords" — the math is decided up front.

## Rule-based mangling: making the dictionary come alive

A dictionary is static, but **rules** bring it to life. Rule-based mangling means applying a set of transformations to every word in the dictionary: capitalize the first letter, append two digits, swap `o` for `0`, swap `s` for `$`, tack on a year… exactly the "security hardening" real humans do.

A ten-thousand-word dictionary, dressed up with a few dozen common rules, becomes millions of guesses that track real human habits far better than random text. hashcat and John the Ripper ship with large rule sets, and a single rule directive can stand for "transform, mangle, append digits." It is the leverage point of cracking efficiency — a small input producing an enormous, and far more realistic, guess space.

| Method | Guess source | Speed | Defense it fears most |
|--------|-------------|-------|----------------------|
| Dictionary | List of common passwords | Very fast | Long, random passwords |
| Brute force | All combinations of a charset | Depends on space | Long password + slow hash |
| Rule mangling | Dictionary × transforms | Fast | Passwords that follow no pattern |
| Rainbow table | Precomputed lookup table | Instant lookup | Per-account salt |

## Rainbow tables: trading space for time

There is one more completely different approach: the **rainbow table**. If a hash has no salt, the same password always produces the same hash. So someone can precompute a mapping of "lots of common passwords → hashes," store it on disk, and on demand just look the answer up — near-instant.

This is the classic **time/space trade-off**: spend a lot of storage and upfront computation to make the actual cracking take almost no time. A full unsalted MD5 rainbow table can eat hundreds of gigabytes, but a single lookup takes milliseconds. Its fatal weakness is just as clear: if every user gets a random **salt**, the same table becomes completely useless. That is why `crypto-02-password-hashing` insists salt is not optional — it is a requirement.

## GPUs and throughput: speed is the real weapon

Hash functions are designed to be *fast*, and for an attacker that is a gift. A modern GPU packs thousands of cores and can compute billions of hashes per second, turning brute force from "theoretically possible" into "actually practical."

> This is the first big defensive lesson: choosing the hash algorithm is choosing a speed gate. General-purpose hashes (MD5, SHA-1) are GPU candy, with hundreds of millions of tries per second; purpose-built password hashes (bcrypt, scrypt, argon2) deliberately slow down to hundreds — or fewer. On the same GPU, argon2 allows a tiny fraction of the attempts MD5 does.

Why does "guesses per second" matter so much? Because cracking time equals the number of combinations you must try, divided by the number you can try per second. Anything that drops the denominator by several orders of magnitude is equivalent to making every password several characters longer. That is the fundamental difference between a password hash and an ordinary hash.

## A sense of scale across hash families

Different hashes allow wildly different guess rates on a GPU. The numbers below are conceptual magnitudes — they climb every year as hardware improves, but the **relative gaps stay stable**:

| Algorithm | Approx. guesses/second (magnitude) | Designed for |
|-----------|------------------------------------|--------------|
| MD5 | Billions | Checksums (never passwords) |
| SHA-256 | Billions | Signatures, certificates (never passwords) |
| bcrypt | Tens of thousands | Password hashing |
| argon2id | Thousands | Password hashing (modern default) |

The same GPU that can try 3 billion MD5s per second is reduced to a few thousand argon2id attempts. That is a gap of nearly seven orders of magnitude — more protection than any complexity rule has ever delivered. And note that "guesses per second" here means hash computations — for an attacker, that is basically the same thing as guesses.

## Where the hashes come from: the role of breach data

The raw material — those strings of hashes — usually arrives via a **database breach**. A site stores password hashes in its database; when the database leaks, the attacker carries the whole batch home to crunch. That is why `pass-05-real-breaches` is worth reading: one major breach hands the attacker millions of ready-to-crack targets at once.

For the defender, this yields a very practical lesson: **monitoring known breaches pays off.** Take your own internal hashes, compare them against public breach lists, find the passwords that are already common knowledge, and force a reset. `pass-04-defenses` will unfold this defense in detail.

## Tying it together: what an audit looks like

Put the concepts together and watch a real audit unfold (again: against your own systems and test data):

1. First, **confirm which algorithm the hashes use** — if it is MD5 or SHA-1, log that problem right away.
2. Run a **dictionary** pass and see how many test passwords hit directly.
3. Add **rules** and run again, watching how much the hit rate improves.
4. For what remains, use **brute force** where it makes sense — but only after benchmarking, so you know how long it will take.
5. Write up the "style of the passwords that got hit" in a report: they look like passwords a real human would pick, which means policy and training need to change.

Those five steps are exactly what `pass-03-cracking-tools` covers — and the conclusions you carry out of it (how many hit, which algorithm, should we switch) are precisely where `pass-04-defenses` begins.

## One page summary: offense and defense pull on the same equation

Compress the whole chapter into one sentence: the attacker tries to **raise the guess rate** (dictionaries, rules, GPUs, rainbow tables); the defender tries to **raise the cost per guess** (long passwords, salt, slow hashes — and later, MFA). Both sides tug on the same equation:

```
Cracking time = keyspace size ÷ guesses per second
```

Understand that equation and you can see the logic behind every defensive recommendation — each one either grows the numerator or shrinks the denominator.

A concrete example: say a password has a keyspace of 10^12 combinations. Against MD5 at a billion guesses per second, the average time to hit is a few minutes; against argon2id at a thousand guesses per second, it becomes decades. **The same password is worth dramatically different amounts under different defense levels** — which is why the defender always has work to do.

## What is next

With the mechanics in hand, the next step is practice. `pass-03-cracking-tools` introduces hashcat and John the Ripper — how a security team benchmarks its hardware, audits its own password hashes, and evaluates rule quality, all inside its own lab. And the real payoff is `pass-04-defenses`, which systematically closes every weakness this chapter exposed.

#### Q: Why is cracking a password not the same as decrypting it?

* Hashes are one-way: no key can reverse them, so cracking means guessing and comparing

* Passwords are never stored, so there is nothing to decrypt

* Attackers simply read plaintext out of the database

* Every hash has an inverse function that restores the original

> 💡 Password hashes are one-way functions: you can compute the hash from the password but never the password from the hash. Cracking is therefore repeated guessing, hashing, and comparing — not decryption.
