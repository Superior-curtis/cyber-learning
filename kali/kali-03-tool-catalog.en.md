# The Tool Catalog: A Category Map

> 📅 2026-08-05 · Core Concepts
> Kali ships hundreds of tools, and beginners get lost fast. This article groups them by what they do, giving you a tool map — so whichever article you read next, you know which category to reach for.

---

Kali hands you hundreds of tools at once. Powerful, sure — and easy to get lost in. The good news: **you do not need to know them all.** The vast majority sort into a handful of categories by what they do, and remembering one or two representatives per category is enough.

This article is that map. Every later `kali-*` article maps to one of these categories.

## The six-category map

| Category | What it does | Representative tools | Related article |
|---|---|---|---|
| Recon & enumeration | Find what a target exposes | nmap, whois, dig | `recon-*` |
| Web testing | Analyze and test web apps | Burp Suite, sqlmap, nikto | `kali-04`, `kali-07`, `kali-09` |
| Password testing | Test password strength | hashcat, John, hydra | `pass-03`, `kali-08` |
| Packets & network | Understand network traffic | Wireshark, tcpdump | `kali-05` |
| Exploits & frameworks | Verify flaws on authorized systems | Metasploit, searchsploit | `kali-06`, `kali-10` |
| Forensics & OSINT | Analyze evidence, gather public info | Autopsy, exiftool, theHarvester | `kali-11`, `kali-12` |

> Learning strategy: do not try to learn every tool at once. Pick one direction — say, "I want to understand web testing first" — and master two or three tools in that category before expanding. Tools are a means, not the goal.

## How to choose a tool

When facing a task, the choice order is:

1. **Which category is this?** Scanning? Web testing? Or traffic analysis?
2. **What is the representative tool?** Start with the most famous, best-documented one.
3. **Is it enough?** If not, step up to a more advanced tool in the same category.

Most of the time, "the most common tool" is the right choice — it has the most docs, the largest community, and the most people who have already stepped on its rake.

## One tool, many roles

Another important fact: **the same tool is used by both defenders and testers.** nmap, for instance — blue teams use it to inventory their own open doors; testers use it to confirm exposure within an authorized scope. Tools do not take sides; use does.

That is also why `kali-01-what-is-kali` said "authorization is the first principle": **everyone has the tools; what matters is always where you use them and whether you have permission.**

## Next

The map is spread out. The articles that follow start with the most important: `kali-04-burp-suite` introduces Burp Suite, the core web-testing tool — it turns every web risk you learned in `web-01` through `web-05` into something you can operate and verify by hand.

#### Q: When facing a 'test this web app' task, what is the first step in choosing a tool?

* Open every tool and try them all

* Judge which category the task is, then pick the most representative, best-documented tool in that category

* Jump straight to the most advanced tool

* Grab any tool and start

> 💡 Locate the task category first, then pick its representative tool; the most common one usually has the fullest docs and the largest community — the safest start.
