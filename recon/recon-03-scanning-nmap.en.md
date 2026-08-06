# Scanning & Enumeration: nmap for Beginners

> 📅 2026-08-05 · Getting Started
> "Which ports are open, and what runs there" — the classic first question in security. nmap is the standard tool for answering it. This article takes you from zero to scanning your own machines and reading the output, plus what a scan looks like to the defender.

---

`net-01-network-fundamentals` taught you that ports are doors, and "which ports are open" is the classic first question in security. The standard tool for answering it is **nmap** — a network scanner used by nearly every security professional.

This article takes you from zero: how to use nmap, how to read its output, and above all — **scan only machines you are authorized to scan.** At the end we look at a scan from the defender's side, so you know what it looks like in your logs.

> Authorization and boundaries: nmap actively sends network requests to the target, and every scan leaves a trace on the target. Scan only your own machines, CTF practice targets, or a scope you are authorized in writing. Unauthorized scanning is illegal in most places.

## What scanning does

Imagine standing outside a neighborhood and wanting to know how many doors each house has open and what is written on them. Scanning is exactly that: **you send connection requests to a target IP, observe which ports respond, and try to identify the services behind them.**

For defenders the same information is precious: **which doors does your machine show to the world?** Know it first, fix it first.

## Your first scan

The basic nmap usage is simple — give it a target:

```bash
# Scan your own machine for common ports
nmap 127.0.0.1

# Add -sV to identify service versions (slower, more useful)
nmap -sV 127.0.0.1

# Scan specific ports
nmap -p 22,80,443 192.168.1.10
```

The first run usually takes seconds. Output looks like this:

```
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
```

The `STATE` column is the key: `open` means the door is open — the service is accepting connections. This is part of the attack surface.

## Common flags

| Flag | What it does |
|---|---|
| `-sV` | Identify service versions |
| `-p 80,443` | Specify ports; `-p-` scans all ports |
| `-O` | Try to detect the OS (needs privileges, slower) |
| `-oA output` | Save results to files |
| `-T4` | Speed the scan up (use with care — faster is more detectable) |

> Less is more. The classic beginner mistake is going all-in with all-ports-and-max-speed. On a machine you own, scanning the common ports and reading the services carefully beats blasting through tens of thousands of ports.

## What it looks like to the defender

This is the lesson people skip: **scanning is not invisible.**

Every connection request you send appears in the target's logs — firewall records, service logs, `/var/log` all carry the trace. The logs in `linux-07-logs-auditing` are exactly what records "who came knocking." A defender with `blue-02-logging-siem` in place will usually raise an alert on a burst of failed connections across many ports in a short window.

Two consequences:

1. For an attacker, the scan itself may be the alarm — which is why tools deliberately slow down or obfuscate.
2. For a defender, **you should know what your own logs look like**, so when a scan happens you recognize it.

## Enumeration: after the scan

Scanning says "the door is open"; finding out what is behind it is **enumeration** — asking the service itself deeper questions: what version runs, does it have a welcome banner, does a known vulnerability exist. The "scan → enumerate → judge vulnerability" flow is what `kali-07-enumeration-tools` and `cve-03-reading-a-cve` pick up next.

## Next

You can now scan your own machines. Next, walk to the defensive side: `blue-02-logging-siem` teaches you to turn logs into eyes that see who is knocking — know your doors and know the knocking, and both of security's feet are planted.

#### Q: Why is using nmap against systems you do not own a serious problem?

* nmap destroys the target system

* Scanning actively contacts the target, leaves a trace, and is illegal without authorization

* nmap only scans Windows

* Scan results are never accurate

> 💡 nmap sends active requests to the target, leaving a trace in its logs every time; unauthorized scanning is prohibited both legally and ethically.
