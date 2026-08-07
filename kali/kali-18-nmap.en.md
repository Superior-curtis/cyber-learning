# nmap, Fully: Host Discovery to NSE Scripts

> 📅 2026-08-05 · Getting Started
> nmap is the single most important tool in Kali — nearly all recon starts with it. This article covers host discovery, scan techniques, service detection, NSE scripts, and output formats in full, with homelab-only commands.

---

`recon-03-scanning-nmap` introduced nmap basics. But by pentest-bible standards, nmap deserves a **full hands-on article** — it is the most important tool in recon, and nearly everything starts with it.

> Security note (HOMELAB ONLY): every command on this page targets your own homelab network and servers. Scanning any real or third-party target is illegal in most places — even just "seeing which ports are open."

## Install

```bash
sudo apt install nmap
nmap --version
```

## Host discovery

First find "who is on the subnet" — the usual first step is `-sn` (ping only, no port scan):

```bash
# Discover live hosts on the subnet (ping only, no port scan)
nmap -sn 192.168.1.0/24
```

In the homelab this means: **inventory the devices on your own network** and spot "who is extra" — an unknown device is often a warning sign.

## Scan techniques

Now that you know "who is there," ask "which doors are open":

```bash
# SYN half-open scan (fast, most common, needs root)
sudo nmap -sS -p- 192.168.1.10

# TCP connect scan (no root, slower)
nmap -sT -p 22,80,443 192.168.1.10

# UDP scan (slow; only specific ports)
sudo nmap -sU -p 53,161 192.168.1.10
```

**SYN scan (`-sS`) is the default first choice**: it sends SYN, reads the response, and never completes the handshake — fast and relatively low-key. `-p-` scans all 65535 ports.

## Service & version detection

`net-01-network-fundamentals` said ports are doors; `-sV` tells you "what service and version is behind the door":

```bash
# Version + default scripts + OS detection (the common "full" scan)
sudo nmap -sV -sC -O 192.168.1.10
```

**Version detection is the input for CVE judgment**: scan `apache 2.4.49`, and you can look up in `cve-03-reading-a-cve` or `kali-10-exploitdb-searchsploit` whether it has known issues.

## NSE scripts: nmap's "plugins"

Half of nmap's power is **NSE (Nmap Scripting Engine)** — hundreds of built-in scripts:

```bash
# List all scripts
ls /usr/share/nmap/scripts/

# Run a specific script against a service (e.g., web title)
nmap -p 80 --script http-title 192.168.1.10

# Run a set of common scripts by category
sudo nmap -sV --script=vuln 192.168.1.10
```

> NSE turns nmap from a "scanner" into a "recon toolbox": scripts like --script=vuln run an initial known-vulnerability check against your own hosts — moving from inventory toward assessment.

## Output formats

Scan results are worth saving — `-oA` writes three formats at once:

```bash
# Output normal/grepable/XML in one shot
sudo nmap -sV -oA lab-scan 192.168.1.10
```

Save them, and you get the `blue-02-logging-siem` mindset — **know what normal looks like, so you can spot abnormal.**

## Security note

* ✅ Allowed: any of the above scans against **your own homelab subnet**, on **your own devices.**
* ❌ Not allowed: scanning any unauthorized subnet or host — even `-sn`.
* ⚠️ Remember: scanning actively touches the target and leaves traces in its logs; unauthorized scanning is illegal in most places (`career-03-ethics-law`).

## Next

nmap is the core of recon. To revisit the conceptual view, `recon-03-scanning-nmap`; to see what a scan looks like in the logs from the defender side, `blue-02-logging-siem` and `blue-01-hardening` are the best contrast.

#### Q: What is the main job of nmap's `-sV` flag?

* Speed up the scan

* Identify the service and version behind a port — the input for looking up known vulnerabilities (CVEs)

* Hide the scan behavior

* Directly run the exploit

> 💡 `-sV` identifies service and version, letting you map 'what is open' to 'does it have known flaws'; the key step from recon to assessment.
