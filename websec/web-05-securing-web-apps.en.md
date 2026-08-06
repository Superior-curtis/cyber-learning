# Building a Secure Web App / API

> 📅 2026-08-05 · Getting Started
> The previous articles broke risks apart; this one puts them back together. Security is not a final pre-launch step — it is a practice that starts with a threat model and runs through development. This article gives you a process you can start using today.

---

From `web-01` to `web-04` you met the map, injection, identity, and SSRF/CSRF/upload. But there is a problem: **knowing all the risks is not knowing where to start.** This article assembles the pieces into a "secure from the start" practice.

## Security is not the last step

The most common mistake is building the app first and "adding security" right before launch. By then many problems are already baked into the architecture, and fixing them is expensive and painful.

Better: **treat security as part of the design**, running through the whole process. The good news: this does not require deep attack knowledge — it requires a disciplined process.

## Start with a threat model

Before writing code, answer four questions — the spirit of `found-04-threat-modeling`:

1. **What are we protecting?** User passwords? Payment data? Trade secrets?
2. **Who wants it?** Random drive-by attackers, or a targeted threat?
3. **Where does it come in?** Login form? API? File upload?
4. **What happens if it breaks?** How big is the impact, and how much effort is it worth?

The answers set your priorities: a site storing credit cards and a blog full of public articles call for completely different defense budgets.

## Core defense checklist

Collect the conclusions of earlier articles into a checklist every web app should have:

| Layer | Must have |
|---|---|
| Transport | HTTPS everywhere (`net-02-tls-https`) |
| Database | Parameterized queries (`web-02-injection`) |
| Output | Framework escaping + CSP (`web-02-injection`) |
| Identity | Strong passwords, MFA, server-side sessions, attempt limits (`web-03-auth-session`) |
| Password storage | argon2/bcrypt (`crypto-02-password-hashing`) |
| Requests | CSRF tokens, SSRF whitelists (`web-04`) |
| Files | Validate content, store off-webroot (`web-04`) |
| Access | Least privilege, default deny (`secplus-02-general-concepts`) |

> This checklist is not "done, therefore safe" — it is the foundation. It keeps the most prevalent problems from appearing. After that, you still need continuous testing to find what the checklist misses.

## What is special about APIs

If your service is an API (say, the backend of a mobile app), pay special attention to:

* **Authentication**: use a standard token mechanism, not a homemade "carry your identity" scheme — the warning in `web-03-auth-session` applies here too.
* **Rate limiting**: cap how many requests one user can make per window, against brute force and abuse.
* **Input validation**: validate at every entry point — type, length, range. Always assume input is untrusted.
* **Do not over-expose**: error messages must not leak stack traces or internals; `blue-02-logging-siem` covers how to log without leaking.

## Continuous testing

"Check once after writing" is not enough. Security needs to be **continuous**:

* **Dependency updates**: keep packages current and track known flaws — the topic of `cve-06-patch-management`.
* **Automated scanning**: wire scanners into CI so every change gets checked.
* **Authorized penetration testing**: periodically run (or commission) authorized tests that see the app like an attacker — the `kali-04-burp-suite` and `kali-*` series are the tools for exactly this.
* **Logging and monitoring**: incidents must be discoverable; `blue-02-logging-siem` covers this part.

## Next

The web-security map is now complete. Next, turn knowledge into feel: `kali-01-what-is-kali` introduces the security practitioner's Swiss army knife — Kali Linux — and how to use its tools (like Burp Suite) to test the systems you are authorized to test.

#### Q: What is the first step in building security into development?

* Write firewall rules first

* Do a threat model: what you protect, who wants it, how it comes in, what breaking means

* Update all packages to the latest first

* Install antivirus software

> 💡 A threat model sets priorities: different systems protect different things and justify different defense budgets. Know what you protect before deciding how hard to defend it.
