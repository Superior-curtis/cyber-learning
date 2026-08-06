# System Hardening & Secure Config

> 📅 2026-08-05 · Getting Started
> The defender's basic skill starts with tightening the system. Hardening is not one magic product; it is a chain of settings that shrink the attack surface and minimize privileges. This article gives you a ready-to-use general checklist.

---

You have practiced the attacker's view for a long time. Now switch sides: **the defender's basic skill starts with tightening the system** — that is **hardening.**

Hardening has no magic: it is not one product that makes you "secure with a click"; it is a chain of settings that shrink the attack surface and minimize privileges. The good news: you already saw its seeds in `linux-09-hardening-basics` and `net-04-firewalls-vpn-proxy`; this article organizes it into a full methodology.

## What hardening is

Hardening in one line: **adjust the system to a state of "keep only what is necessary; close everything else."**

Why it works: attackers look for exploitable gaps, and most gaps are "extra services, extra privileges, extra back doors." Hardening removes those "extras" one by one — **the leaner the system, the fewer places to hit.**

## Core principles

Three principles underlie every hardening step:

* **Minimal attack surface**: turn off unused services, block unused ports.
* **Least privilege**: give every person and program only what the job requires.
* **Default deny**: anything not explicitly allowed is blocked.

You heard these in `secplus-02-general-concepts`; now turn them into settings.

## A general checklist

| Category | Action |
|---|---|
| Updates | Keep packages and firmware patched (`cve-06-patch-management`) |
| Services | Disable unneeded services and daemons, close extra ports |
| Firewall | Default deny, explicit allow; the principle from `net-04-firewalls-vpn-proxy` |
| SSH | Disable direct root login, use keys, restrict source IPs |
| Users | Remove stale accounts, strong passwords, audit `sudo` regularly |
| Config | Check world-writable files, disable directory browsing, remove default credentials |
| Logging | Ensure auditing is on, so incidents leave a trail (`linux-07-logs-auditing`) |

> Hardening mindset: not "what to add," but "what to remove." Every service closed and every privilege taken away shrinks the attack surface. Ask "is this really needed?" — if not, turn it off.

## Hardening vs patching vs antivirus

Separate three easily-confused ideas:

| Practice | What it does |
|---|---|
| Patching | Fixes known vulnerabilities (`cve-06-patch-management`) |
| Hardening | Reduces the exploitable entry points themselves |
| Antivirus/EDR | Detects and stops threats that already occurred |

All three are needed, but **hardening is the foundation**: patches fix the known holes; hardening makes even unknown holes hard to find — fewer entry points means every attack struggles to gain purchase.

## Next

The system is tightened. Next, learning to see: `blue-02-logging-siem` introduces logs and SIEM — turning the traces a system leaves behind into eyes that can tell who is knocking.

#### Q: What is the core mindset of hardening?

* Install as much security software as possible

* Minimize the attack surface and privileges: turn off the unused, block the extra, default to deny

* Make the system as fast as possible

* Change the operating system periodically

> 💡 Hardening is about 'what to remove', not 'what to install': fewer entry points and privileges leave attackers little to work with.
