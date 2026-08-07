# Shodan, Deeper: Searching the Internet of Devices

> 📅 2026-08-05 · Deep Dive
> recon-02 introduced Shodan; this article goes deeper — search syntax, the questions you can ask, and most importantly the defender's view: looking up your own exposure. All techniques stay within public index data and authorized scope.

---

`recon-02-dorking` introduced Shodan. This article goes deeper: **how to search, the questions you can ask, and the defender's most important move — looking up yourself.**

Shodan's essence in one line: **it is not an engine for web pages; it is an engine for devices connected to the internet.** It continuously scans public IPs, recording which ports are open, what services run, and what banners come back.

> Authorization and boundaries: Shodan indexes public service information. You may use it for public data and your own assets; but "finding a device" is not "may access it." Accessing any device or service you are not authorized for is crossing the line.

## What you can ask

Shodan's search box answers very specific questions:

| Question | Example |
|---|---|
| Which country/subnet has open SSH? | `country:TW port:22` |
| Which service is exposed on the public internet? | `product:MySQL` |
| Which software version is exposed? | `apache 2.4` |
| What does my public IP look like? | Search your own IP directly |

**The core of the syntax is filtering fields** — country, port, product, version, organization. Treat fields as filters and you can pull "the exact class you want" from hundreds of millions of records.

## The defender's use: audit your own exposure

The first thing a defender should do is not "look at others" but **"look at yourself":**

1. Look up your company's public IPs and subnets (the subnet idea from `net-01-network-fundamentals`).
2. See what Shodan shows about you: which services, which should not be public.
3. When "something that should be private" shows up in the public index, fix it immediately — the public buckets of `cloud-02-cloud-misconfigs` are exactly the kind of thing Shodan often reveals.

> Think of Shodan as the public view of your attack surface. Attackers use Shodan to find targets; you should use Shodan to find yourself first — know what is exposed before deciding what to close.

## From Shodan to full OSINT

Shodan is only one piece of OSINT. Complete public-information gathering also includes whois, DNS, archived pages, code search, and social — `recon-05-osint-sites` gives you a complete "sites and tools map," organizing the scattered sources at once.

## Next

The Shodan deep-dive is done. Next, spread out the whole OSINT toolbox: `recon-05-osint-sites` walks a full set of public-information sources — whois, DNS, archives, code, social — and how to gather with discipline.

#### Q: What should a defender do first with Shodan?

* Search competitors exposure

* Look up their own public IPs and subnets to inventory which services they expose

* Download every search result

* Try logging into the devices found

> 💡 Shodan is the public view of your attack surface; a defender first looks up themselves to see which things that should be private show up in the public index.
