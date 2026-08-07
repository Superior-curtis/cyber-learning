# OSINT Sites & Tools Map

> 📅 2026-08-05 · Getting Started
> OSINT sources are scattered — whois, DNS, archived pages, code, search engines, social platforms. This article organizes the common public sources into a map, and how to gather with discipline. Public information and authorized scope only.

---

`recon-01` through `recon-04` covered the OSINT mindset, search, tools, and Shodan. This article spreads out the whole toolbox: **a map of OSINT sites and tools** — which sources, what questions they answer, and how to gather with discipline.

> Boundary reminder: all of the following are public sources. "Finding" something is not "may use it" — accessing anything requiring permissions, or running harassing investigations on real individuals, is crossing the line. The ethics section of recon-01-recon-osint applies here unchanged.

## Sources by category

| Category | What it does | Representative sources |
|---|---|---|
| Registration & DNS | Info behind a domain | whois, dig, SecurityTrails |
| Search | Publicly indexed pages and files | Google dorks, Bing |
| History | Old pages and deleted content | Wayback Machine |
| Code | Public programs and configs | GitHub, GitLab |
| Devices | Internet-connected services | Shodan, Censys |
| Social | Public personal and org info | Public platform pages |
| Images | Pictures and their origins | Reverse image search |

## The essential public sources

Narrow the list to the most-used:

| Tool/site | What you can ask |
|---|---|
| whois / RDAP | Domain registration info, DNS |
| Wayback Machine | What a site looked like, deleted pages |
| Shodan / Censys | Public devices and services (`recon-04-shodan-deep-dive`) |
| GitHub search | Configs and keys leaked in public code |
| Google dorks | `site:`, `filetype:` (`recon-02-dorking`) |
| Reverse image search | The origin and location of an image |

> Think of sources as "channels for asking questions." Each source is best at one kind of question — decide "what do I want to know" first, then pick the matching source. That beats wandering across sites with no aim.

## A disciplined gathering flow

OSINT gathering is not "browsing around"; it is a sequenced process:

#### Define the question

What do you want to know? A target domain, a person, or a photo?

#### Go passive first

whois, DNS, search engines, archives — without touching the target at all.

#### Then active

Within scope, visit public pages and run deeper queries.

#### Record and cross-check

Write down leads and cross-verify across sources to find connections.

## Ethics: the gathering boundary

No matter how convenient the tool, the line does not move:

* ✅ Allowed: public whois, DNS, search results, archived pages, public code.
* ❌ Not allowed: bypassing permissions, accessing private content, harassing real individuals, turning "found" into "used."
* ⚠️ Remember: **public information is not authorization to use it.** The line from `kali-12-osint-tools` and `recon-01-recon-osint` is the most important lesson you take away.

## Next

The OSINT map is spread. To practice, `lab-06-osint-challenges` has exercises; to meet automated collection, `kali-12-osint-tools` introduces theHarvester and recon-ng.

#### Q: What is almost always the first step of OSINT gathering?

* Accessing the target system directly

* Defining the question first, then starting passive collection with the matching public sources (whois, search, history)

* Downloading every available tool

* Investigating real individuals

> 💡 Ask clearly what you need to know, then collect passively from the matching sources — a disciplined flow is both more efficient and safer than random browsing.
