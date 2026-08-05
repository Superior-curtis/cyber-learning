# hashcat & John the Ripper in Practice

> 📅 2026-08-05 · Tools
> A tour of hashcat and John the Ripper: hash modes, wordlists, rules, and benchmarks — the way a security team audits its own password hashes.

---

## Two names, one job

In `pass-02-cracking-101` we learned that cracking is really "guess, hash, compare." In practice, the two open-source tools that take this to an extreme are **hashcat** and **John the Ripper**. Their purpose is not "breaking in" — it is **helping you see how easily your own passwords can be guessed**, like putting your password hygiene under a microscope.

The two are positioned slightly differently. hashcat shines on GPU acceleration, chasing maximum throughput — great for large hash sets and brute force. John the Ripper (John, for short) is lightweight on a single machine, its automatic hash-format detection is strong, and it is often the first stop when you have a hash of unknown format: let John figure out what it is. Most security teams install both — John first, then hashcat.

> Repeating the authorization boundary. These tools may only be used on your own systems, your own test data, or explicitly authorized CTFs and audits. Running them against any third-party system or real user account is illegal. Every example in this article assumes you are in your own lab, processing test hashes you generated yourself.

## Hash modes: know what you are up against

The hashing world has many formats: MD5, SHA-1, bcrypt, argon2, NTLM, plus a long tail of salted variants. In the tools, each format maps to a **hash mode**. You must identify the format correctly before the tool knows how to compute — use the wrong mode and every result is wrong.

| Common hash mode | Algorithm | Typical use |
|------------------|-----------|-------------|
| 0 | MD5 | Legacy systems, checksums (never for passwords) |
| 1000 | NTLM | Windows accounts |
| 3200 | bcrypt | Modern website passwords (deliberately slow) |
| 25600 | argon2id | Modern website passwords (recommended default) |

How do you tell which one you have? Look at the length and the format characteristics — or hand it to John's auto-detection. Getting the identification right saves half your time.

## John's strength: identify "what hash is this" first

hashcat chases throughput; John the Ripper's first job is usually **identification**. When you hold a hash of unknown format — say, exported from some obscure legacy system — John auto-detects its type, which saves enormous time.

Once identified, John can also do lightweight dictionary and rule cracking on the CPU. Its default behavior is "run a first pass with the built-in wordlists and rules," which is great for a quick reconnaissance pass. A common team workflow: let John run first, and switch to hashcat only when you need GPU-class throughput.

## Wordlists: the fuel for guessing

The tool is the engine; **the wordlist is the fuel**. A wordlist is a list of candidate passwords, from huge multi-million-word lists to small custom lists built around a specific language or company naming style. The tool hashes and compares every line in the list — and this is where you start, because, as the previous article showed, most real passwords sit well within a wordlist's reach.

The logic for choosing a wordlist is simple: start broad and generic, then go small and precise. A good wordlist is not the biggest one; it is the one that most closely matches *how the people you are auditing actually set passwords*. A security team builds its own "if our users set a password, what would it look like" model from breach statistics — that is the real value of an audit.

Wordlists go stale, too: a list that worked ten years ago may have missed today's naming habits. Keeping your wordlists fresh matters as much as keeping the tools updated — because attackers are burning the same fuel.

## Rules: the wordlist's amplifier

A wordlist alone is not enough. The **rule-based mangling** from `pass-02-cracking-101` is, in the tools, just a set of rule files: `Password`, `Password1`, `P@ssw0rd`… all "grown" from the same wordlist. hashcat ships rich built-in rule sets, and a single rule stands for an entire family of transformations.

Rules can be stacked without limit — but every rule multiplies the guess count by one more wordlist worth of candidates. So practice is about balance: start with known-good built-in rule sets, watch the hit rate, then decide whether to go deeper. The tools report how many hashes you hit, and that statistic is exactly how you evaluate rule quality.

On the John side, the same job starts with one line:

```bash
# Let John auto-detect the format and start with a wordlist
john --wordlist=rockyou.txt lab-hashes.txt

# Show the test passwords that were recovered
john --show lab-hashes.txt
```

To repeat: these commands run against test hashes you generated in your own lab. The number of lines John prints tells you directly — under this wordlist-plus-rules combination — how many of your test passwords got hit.

## Benchmarks: measure your own guesses per second

Remember the equation from the previous article? `Cracking time = keyspace ÷ guesses per second`. A **benchmark** measures your own guesses-per-second so you know how fast your hardware really is against a given hash. It is also a great way to appreciate the strength of an algorithm — if bcrypt runs at a few thousand per second on your top GPU, you now have a visceral sense of what your own defenses buy you.

```bash
# 1) Create a test hash in your lab (test data, not a real account)
printf 'Lab-Password-2024' | md5sum

# 2) Run a dictionary attack against your test file (mode 0 = MD5)
hashcat -m 0 lab-hashes.txt rockyou.txt

# 3) Benchmark: see how slowly your hardware computes bcrypt
hashcat -b -m 3200
```

Every command above operates on **test hashes you generated yourself**. The point is not to memorize commands — it is to understand what each line does: build a sample, pick a format, choose the fuel, measure the speed.

## Reading the tool output

At first glance the output is a wall of statistics, but only a few fields matter:

| Output field | Meaning |
|--------------|---------|
| Candidates / Guesses | How many candidates have been tried so far |
| Speed | Guesses per second — the denominator from the previous chapter |
| Recovered / Cracked | How many test hashes were hit |
| Time.Estimated | How long the whole run will take at this speed |

These numbers are not for looking at — they are for **making decisions**: speed tells you how strong the algorithm is, hit rate tells you about wordlist and rule quality, and estimated time tells you whether to switch strategy. If you cannot read the output, the audit is not really finished.

## A complete audit workflow

Auditing your own password hashes with these tools roughly follows these steps:

#### Prepare test data

In your own lab, use a set of **test hashes you generated** or took from a CTF challenge. Never touch real user data.

#### Identify the hash type

Confirm the hash mode: is it MD5, NTLM, bcrypt, or argon2? Is it salted? Identify before you compute.

#### Guess from fast to slow

Start with a dictionary, then rules, and only then brute force. Jumping straight to brute force is not professionalism — it is a waste of electricity.

#### Read the results

A high hit rate means the sample passwords are weak: recommend migrating the algorithm or rolling out a password manager. Turn it into a report for the defense team.\n\n→ next: pass-04-defenses

The output of an audit is not "look what I cracked." It is a **risk report**: how many sample passwords fell inside a wordlist's reach, whether the algorithm is too old, and whether you should move to argon2id. Your attack surface is your own data; your output is advice for the defenders. That is blue-team thinking.

## Make the audit a routine

A one-off audit only reflects the state on the day you ran it. The more practical approach is to turn password auditing into a **recurring process**:

* When users set or reset a password, check it on the spot against breach lists and weak dictionaries, and block passwords that are already public;
* Re-run benchmarks on a schedule, so a hardware upgrade never quietly lowers your protection;
* Re-run the wordlist and rule passes periodically, and watch whether your sample passwords are trending weaker.

Some tools even expose APIs, so registration and login flows can reject a weak password at the exact moment it is chosen. That turns the breach monitoring of `pass-04-defenses` into a real-time defense instead of a yearly post-mortem.

## Common pitfalls

A few traps keep catching newcomers:

* **Wrong hash mode**: you misidentify the format, every result is wrong, and you conclude "this password is uncrackable."
* **Testing real data**: even an internal test involving real user hashes raises privacy and compliance issues. Labs use synthetic samples, period.
* **Skipping benchmarks**: without measuring speed first, you cannot estimate runtime, and your plan spirals out of control.
* **Wrong priority**: skipping the dictionary to go straight to brute force abandons your most efficient attack surface.
* **Reading only hits, not misses**: a low hit rate is not necessarily bad news — it may mean the samples are strong. What matters is comparing how the same samples behave across algorithms and rule sets.

Remember one line: the tools are not dangerous by themselves — **what you do with them** is where right and wrong live. The exact same commands are an audit in your own lab and a crime against someone else's system.

## What is next

These tools just operationalize the mechanics of `pass-02-cracking-101`. Their reason for existing is to show you **how fast your hashes can be broken** — and to force you to take defense seriously. The next article is the payoff of the whole series: `pass-04-defenses` shows you how slow hashing, password managers, passkeys, and MFA turn all that guesses-per-second from the last two chapters back into a rounding error.

#### Q: What does a hash mode refer to in hashcat?

* The hash algorithm format to use, such as MD5, bcrypt, or argon2

* The clock speed of your GPU

* The maximum size allowed for a wordlist file

* The network connection mode for an attack

> 💡 The hash mode tells the tool which algorithm to use for computing and comparing hashes. If you identify the format wrong, every result comes out wrong.
