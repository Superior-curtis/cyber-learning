# OSINT Tools (theHarvester / recon-ng / Maltego)

> 📅 2026-08-05 · Getting Started
> recon-01 and recon-02 taught you the OSINT method; these three tools semi-automate it. theHarvester collects emails and subdomains, recon-ng is a modular framework, and Maltego draws the relationships. This article introduces them, and the line you must not cross.

---

`recon-01-recon-osint` and `recon-02-dorking` taught you the OSINT method and search skills. This article turns the method into tools: **theHarvester, recon-ng, and Maltego** — three tools that semi-automate public-information gathering.

> Authorization and boundaries: these three tools only touch publicly available information, but they may also surface things you are not authorized to access. Confirm your scope before use, and only access data you are already entitled to. A tool can query; it does not give you a license to use.

## What OSINT tools do

Manual OSINT is tiring: query whois, query DNS, crawl search engines, jot down leads. OSINT tools **semi-automate** these steps — give one a domain or company name, and it runs a batch of public queries, organizing the results into lists.

The key word is "semi": **the tool collects; you handle judgment and boundaries.**

## theHarvester: collecting emails, subdomains, and IPs

theHarvester is the one most often picked up first: give it a domain, and it gathers **email addresses, subdomains, hostnames, and IPs** from multiple public sources:

```
# Public info gathering on a domain (illustration)
theHarvester -d example.test -b all
```

Its output is a list. How a defender uses it: **see what public information your domain leaks** — employee emails and subdomains are raw material for later social engineering and attack. Know what you expose before you defend.

## recon-ng: a modular OSINT framework

recon-ng is like "the Metasploit of OSINT" — a framework that wraps public data sources into **modules**. You load a module, set the source, run it, and it stores results in a built-in database for later cross-referencing.

Its strength is **composability**: chain whois, DNS, and social-media modules into an automated collection pipeline.

## Maltego: graphing the relationships

Maltego draws OSINT results as a **graph**: people, domains, mailboxes, and organizations are nodes, connected by lines showing their relationships.

> Positioning the three: theHarvester pulls lists, recon-ng chains pipelines, Maltego draws maps. Pull, chain, then draw — visualization often reveals relationships that stay hidden inside a list.

## The line: finding it is not using it

The temptation of OSINT tools is that they look easy, but the line does not disappear because of a tool:

* ✅ Allowed: query public whois, DNS, and search results; visit public pages you are entitled to.
* ❌ Not allowed: phish the addresses you found, scan subdomains without authorization, access anything requiring permissions.
* ⚠️ Remember: **"public information" is not "authorization to use it."** A tool can find it; that does not let you do anything with it.

The ethics section of `recon-01-recon-osint` applies here unchanged — the tool just searches faster; the line has not moved an inch.

## Next

The Kali tool series is complete. Next, fold all of it into practice: `lab-01-build-your-lab` walks you through building your own complete practice lab — integrating Kali, targets, and CTF categories into a single "free HackTheBox" learning path.

#### Q: When using OSINT tools, which mindset is correct?

* Whatever the tool finds, you may use directly

* The tool collects public information; usage permission and boundaries are still on you

* OSINT tools can bypass login permissions

* As long as nobody finds out, it is fine

> 💡 OSINT tools only semi-automate collection of public info; 'public' is not 'authorized,' and the line does not vanish because a tool exists.
