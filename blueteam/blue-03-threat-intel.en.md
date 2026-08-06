# Threat Intelligence

> 📅 2026-08-05 · Deep Dive
> A blue team wants more than 'someone is knocking'; it wants to know who is outside. Threat intelligence is the craft of understanding attackers, tactics, and tools. This article covers the levels of intelligence, its sources, and its real purpose — decisions, not hoarding.

---

`blue-02-logging-siem` gave you eyes that see who is knocking. Now go further: **who is outside the door? How do they usually get in? Which door should we defend first?** The craft that answers these is **Threat Intelligence.**

## What threat intelligence is

Threat intelligence is the process of **turning attackers, tactics, tools, and indicators into actionable information.** It is not a pile of "cool hacker stories"; it is data that answers three questions:

1. **Who might attack us?** (motivation and capability)
2. **What tactics will they use?** (the threat taxonomy from `secplus-03-threats-mitigations`)
3. **What should we defend first?** (resources are finite; block the most likely first)

One line: **intelligence turns the unknown into the known, and the known into action.**

## The levels of intelligence

Intelligence is not one black box; it comes in four levels:

| Level | Answers | Who uses it |
|---|---|---|
| Strategic | Overall risk and trends | Executives |
| Tactical | Attacker tactics and behaviors | The security team |
| Operational | Attacks happening right now | Responders |
| Technical | Specific indicators (IOCs) | Detection/automation |

Example: "a ransomware gang is targeting small businesses" is strategic; "they usually start with phishing email plus weak RDP passwords" is tactical; "block these 20 IOCs" is technical. **Different roles need different levels.**

## Common sources

| Source | Provides |
|---|---|
| Public intel reports | Analyses from vendors and researchers |
| IOC lists | Malicious IPs, domains, hash values |
| Vulnerability databases | The CVE system from `cve-01` through `cve-05` |
| Threat-sharing communities | Observations exchanged between peers |
| Your own logs | The observations from `blue-02-logging-siem` — one of the best sources of all |

> The most overlooked source is your own system. Your logs hold real knocking records — the detection results from blue-02-logging-siem are themselves the intelligence closest to you.

## How to use it: decisions, not hoarding

The biggest misuse of threat intelligence is **hoarding** — downloading reports and IOCs without acting. The right use wires intelligence into decisions:

* Let IOCs enter detection rules (`blue-02-logging-siem`).
* Let common tactics shape hardening priorities (`blue-01-hardening`).
* Let "who might attack" decide how tight your defense must be.

> Intelligence has a shelf life. Attackers change IPs, tools, and tactics. Last year's IOC may be useless today — intelligence must be continuously updated and continuously used, not filed away.

## Next

You know who is outside the door and how to defend. Next, prepare for when something actually goes wrong: `blue-04-incident-response` introduces incident response — the full process from preparation to recovery, and why "having drilled it" beats "having read about it."

#### Q: What is the real purpose of threat intelligence?

* Hoarding security reports to prove professionalism

* Turning the unknown into the known, then the known into decisions and action

* Replacing the firewall

* Scaring attackers away

> 💡 Intelligence earns its value by being used: IOCs into detection, tactics into hardening, threat assessment into defense strength. Hoarding produces nothing.
