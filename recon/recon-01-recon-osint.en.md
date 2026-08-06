# Recon & OSINT: The Power of Public Information

> 📅 2026-08-05 · Getting Started
> Before anyone touches a system, the first move in a security test is not attack — it is observation. Recon and OSINT teach you how to assemble a usable map of a target from public information alone, without ever crossing a line.

---

The starting point of a security test sounds counterintuitive: **not attack — observation.**

Before you make a single move against a system, you spend time on "recon" — collecting publicly available information and assembling a map of the target. Done well, every later step has direction; done sloppily, it is like driving blindfolded.

This article covers the basics of recon and OSINT (Open Source Intelligence). The core idea fits in one line: **most of the useful information is already public.**

> Authorization and boundaries (read first): every technique in this series applies only to your own systems, CTF lab environments, or a scope you are authorized to test in writing. Unauthorized scanning or probing of any system is illegal in most jurisdictions. This book only teaches how to read public information — it provides no operational procedures aimed at third parties.

## What recon is

In an authorized test, recon is the first phase: **collecting every piece of public information that helps you understand the target, without touching it.**

Its value is that it makes the later steps cheaper and safer:

* Knowing which technologies a company uses lets you focus the checks that matter.
* Knowing the subdomains and services narrows the scan range.
* Knowing employees' public details lets you assess social-engineering risk (see `blue-06-phishing-defense`).

For defenders, recon means the mirror image: **other people can see this about you, so you should look first and find out what you are exposing.**

## Passive vs active

Recon splits into two kinds, with a clear line:

| Type | What it does | Example | Touches the target? |
|---|---|---|---|
| Passive | Reads only what others already published | whois, DNS records, search engines | No |
| Active | Sends requests to the target | Visiting a public site, querying its DNS | Yes (lightly) |

**Passive first, active later** is the iron rule. Passive recon never touches the target and is always safe; even visiting a public page leaves a trace in the target's logs, so active recon requires confirmed scope.

## OSINT: assembling a picture from public pieces

OSINT breaks into no system — it just uses public information **more systematically**. Common sources:

| Source | What you can get |
|---|---|
| WHOIS | Domain registrant, registration date, DNS servers |
| DNS records | Subdomains, mail servers, IPs |
| Search engines | Indexed public pages, documents, exposed config files |
| Social media | People's public profiles, roles, technology habits |
| Public documents | Slide decks, manuals, job postings — often leak the stack |

Each seems trivial; together they are powerful. `recon-02-dorking` teaches the advanced search syntax that pulls this scattered-but-public information out in one pass.

> The first rule of OSINT: collect only what was already public. If you need to bypass a permission, guess a password, or access something you should not, you have crossed the line — that is not OSINT.

## Legal and ethical boundaries

State the boundaries clearly so you do not trip over them:

* ✅ Allowed: public websites, whois, DNS, public social profiles, search-engine caches.
* ❌ Not allowed: bypassing logins, brute-forcing, phishing real users, accessing explicitly blocked resources.
* ⚠️ Mandatory: confirm scope. Before anything, ask "am I authorized to touch this?"

## Next

You now have the mindset for passive recon. Next, `recon-02-dorking` teaches the first practical tool: using Google search operators and Shodan to dig the public information out faster and deeper.

#### Q: Which of these is passive recon?

* Directly scanning every port of the target

* Querying the WHOIS and DNS records of the target domain

* Trying to log into the target admin panel

* Sending phishing emails to target employees

> 💡 Querying WHOIS, DNS, and other public records sends no request to the target, so it is passive recon. Scanning, login attempts, and phishing all touch the target and need explicit authorization.
