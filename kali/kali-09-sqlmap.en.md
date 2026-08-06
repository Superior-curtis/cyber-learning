# SQLmap: Automated Injection Testing

> 📅 2026-08-05 · Deep Dive
> In web-02 you learned the principles and defenses of SQL injection. sqlmap is the automated version of that principle — one command can run injection detection on an authorized target. But the stronger the automation, the more carefully it must be used.

---

`web-02-injection` taught you how SQL injection happens and the standard defense, parameterized queries. Now flip to the tool side: **sqlmap** automates "detect whether an input point has SQL injection."

> Authorization and boundaries (read first): sqlmap automatically fires a large volume of requests at the target, some of which can be destructive. It belongs only on your own systems or a scope you are authorized to test in writing. Running sqlmap against an unauthorized target is an automated attack, illegal in most places.

## What sqlmap is

sqlmap is an open-source **automated SQL injection detector**. What it does is turn the "manually test whether this parameter has injection" process you learned in `web-02` into one command plus automated judgment.

It automatically:

* Detects whether a target parameter has SQL injection.
* Determines the injection type and the underlying database.
* Under authorization, enumerates database structure or content.

**Its value and its risk are two sides of the same coin**: the stronger the automation, the more it must not be misused.

## Basic usage

The beginner invocation only needs a URL with a parameter:

```
# Injection detection on a URL parameter (illustration)
sqlmap -u "http://127.0.0.1/item?id=1" --batch

# With --dbs, enumerate databases after confirming injection (authorized only)
sqlmap -u "http://127.0.0.1/item?id=1" --dbs
```

> Always lead with --batch and a "detect only" mindset. On your own targets, first confirm with minimal flags whether injection exists, then decide whether to go further. Slow and steady beats going all-in.

## Why it is only a starting point

sqlmap tells you "this input point has injection," but it cannot replace your understanding of the problem:

* It says *where* the hole is, not *why* — real remediation requires reading the code and the parameterized queries from `web-02-injection`.
* Automation can false-positive or false-negative; the key judgment is always confirmed by hand.
* For defenders, **running sqlmap against your own site** is like running a dictionary against your own passwords — find the hole first, then fix it.

Think of sqlmap as a **metal detector**: it quickly circles where something might be buried, but digging, identifying, and handling it are on you.

## Defense counterpart

No matter how strong the automation, the defensive answer has not changed — it is the `web-02-injection` line: **parameterized queries, so input is always data, never syntax.**

If you already use parameterized queries, sqlmap gets nothing from you; if you do not, sqlmap will tell you immediately. **So the correct defender posture is to scan your own systems first and confirm the defense really works.**

## Next

Injection automation covered. Next, vulnerability intelligence: `kali-10-exploitdb-searchsploit` introduces Exploit-DB and searchsploit — the database for searching known vulnerabilities and their details, and how it relates to `cve-01` through `cve-05`.

#### Q: What is the correct way for a defender to use sqlmap?

* Scan other people sites to practice

* Scan your own site to confirm defenses like parameterized queries actually work

* Turn off database logging to avoid being found

* Only use the paid version

> 💡 sqlmap runs injection detection on authorized targets; defenders use it first on their own systems to turn 'the defense works' from assumption into measurement.
