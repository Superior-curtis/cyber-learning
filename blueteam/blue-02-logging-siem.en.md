# Logs & SIEM Detection

> 📅 2026-08-05 · Deep Dive
> Your system is tightened, but can you see who is knocking? Logs are the defender's memory; SIEM is the tool that turns that memory into eyes. This article covers log types, what SIEM does, and the basic thinking behind detection.

---

`blue-01-hardening` tightened the system. But defense is not just closing doors — you also need to know **who is knocking, how often, and at which door.** That takes two things: **logs** (the traces) and **SIEM** (the tool that turns traces into eyes).

`linux-07-logs-auditing` already taught you how to read Linux logs. This article pulls up a level: how logs become detection capability the whole organization can see.

## Why logs matter

One line: **without logs, an incident never happened.** When something goes wrong, what do you use to reconstruct it? Logs. The first step of incident response in `blue-04-incident-response` is preserving and reviewing logs.

Logs have two sides of value:

* **Post-hoc reconstruction**: after an incident, rebuild "when, who, what" from logs.
* **Proactive detection**: set rules so "abnormal traces" fire alerts on their own.

## Types of logs

Systems produce many kinds of logs:

| Log | What it records |
|---|---|
| Authentication | Login success/failure, who came from where |
| System | Service start/stop, kernel messages |
| Application | Application-level events and errors |
| Network | Firewall and traffic records |
| Audit | Sensitive actions like permission changes and file access |

A single machine's logs are "one person's memory." But an enterprise usually runs hundreds of machines — to connect all that memory, you need SIEM.

## What SIEM is

**SIEM** (Security Information and Event Management) is a **centralized** security log platform. Its job splits in two:

* **Central collection**: gather logs from all machines into one place.
* **Correlation analysis**: run rules over the mass of logs to find patterns invisible on any single machine.

A classic SIEM story: "a server's login failures suddenly spike, and the same account starts logging in across machines." Viewed on any single box it is just "a few failures"; SIEM connects the two and reveals a credential attack.

## The thinking behind detection

Good detection is not "keep all the logs"; it is **hunting for anomalies with a target.** Three basic ideas:

1. **Baseline**: know what normal looks like first, so abnormal stands out.
2. **Rules**: write the alert-worthy patterns, like "more than five login failures in ten minutes."
3. **Trend**: do not just catch single events; watch whether they are increasing.

> The detection trade-off: no false negatives vs no false positives. Rules set too loose miss real attacks (false negatives); too tight, you drown in normal behavior (false positives). Good detection is tuned iteratively, not written once.

## Where to start

As an individual or small team, you do not need SIEM immediately. A practical path:

1. First make sure **logs are on and centralized** (the approach in `linux-07-logs-auditing`).
2. First watch the **most valuable signals**: logins, permission changes, abnormal outbound connections.
3. When the scale grows, bring in a SIEM platform.

## Next

You now have eyes that see who is knocking. Next, learn to understand who is outside the door: `blue-03-threat-intel` introduces threat intelligence — understanding attackers, tactics, and tools, so detection and response have direction.

#### Q: What is SIEM's core value?

* Automatically patch every vulnerability

* Centralize scattered logs and run correlation over the mass, revealing patterns no single machine shows

* Replace the firewall

* Encrypt all data

> 💡 SIEM = centralized logs + correlation: piecing traces from many machines together is what reveals cross-machine, cross-time attack patterns.
