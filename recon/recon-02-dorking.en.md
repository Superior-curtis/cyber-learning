# Search Techniques: Google Dorks & Shodan

> 📅 2026-08-05 · Getting Started
> Search engines are not just for answers — they are a giant public database. Google search operators and Shodan can pull out scattered-but-public information in one pass. This article shows you how to dig, and where the line is.

---

Google is a database you use every day but rarely actually use. Ordinary searches touch only its surface; its real power lives in **search operators** — command-like codes that let you pull one specific class of public page out of billions.

Add **Shodan** — an engine that continuously scans the internet and records which devices are connected — and you have a powerful public-intelligence kit. This article shows how to use it, and most importantly, **where the line is.**

> Authorization and boundaries: these techniques are for your own assets, CTF practice, or an authorized scope. Search operators are harmless by themselves; what you do after seeing the results is the decision that matters. Accessing data you have no permission for is crossing the line.

## Google search operators

Google supports special commands that restrict results to a site, a file type, or a place in the text. The common ones:

| Operator | What it does | Example |
|---|---|---|
| `site:` | Search only one site | `site:example.com` |
| `filetype:` | Search only one file type | `filetype:pdf` |
| `intitle:` / `inurl:` | Keyword in title or URL | `intitle:"login"` |
| `intext:` | Keyword in the body | `intext:"admin"` |
| `-` | Exclude a word | `example.com -filetype:pdf` |

Combined, they let you ask very specific questions, like "how many public PDFs does this domain host?" All of it is data in the public index — Google already lists it; you are just finding it with a sharper syntax.

> One easy-to-miss point: being findable by Google does not mean it should be public. Many security incidents begin with a config file or backup accidentally placed where it is public. For defenders, running these same operators against yourself is the cheapest, most effective self-audit there is.

## Shodan: a map of internet-connected devices

Shodan is a search engine, but its database is not pages — it is **services open on the internet**. It continuously scans public IPs and records which port is open, what service runs, and what banners come back.

That means you can search for:

* Open SSH, databases, and cameras in a country or subnet.
* Devices exposed to the public internet that were never meant to be.
* How widely a particular software version is deployed.

For defenders, the classic Shodan move is: **look up your own public IP and see what it shows.** If Shodan has indexed one of your services, that is usually the signal to lock or close it.

## Why this information matters

One record is nothing; combined, they let you answer three questions fast:

1. **What is exposed?** Which services are visible on the public internet.
2. **How current are they?** Old service versions often have known flaws — the topic of `cve-01-what-is-cve`.
3. **Where to start?** Treat public information as a priority list instead of blind sweeping scans.

## The ethical line: seeing is not touching

This is the most important part. A search engine "finding" a result does **not** mean you may "access" it:

* ✅ Allowed: use public search syntax against the public index, read public banner info.
* ❌ Not allowed: try to log into indexed devices, access content that needs permissions, take any active action against third-party assets.
* ⚠️ Remember: **public information ≠ authorization to use it.** Seeing something is not permission to go in.

> The most common trap is "it is public, so I can use it." That does not hold up ethically or legally. Treat search skills as looking first; leave any active action for the scope you are authorized to touch.

## Next

Search and Shodan give you a silhouette of the target. Next, `recon-03-scanning-nmap` moves to the active phase: using nmap on your own (or authorized) machines to turn that silhouette into an exact list of which doors are open.

#### Q: What is the correct mindset for Google search operators?

* If you can find it, you may use it

* They are just a sharper syntax for retrieving already-public indexed data

* They can bypass website login permissions

* They exist only for hacking other systems

> 💡 Search operators retrieve pages Google already indexes publicly; finding something is not authorization to use it, so confirm scope before any active action.
