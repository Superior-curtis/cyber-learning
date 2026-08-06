# Enumeration Tools: gobuster / ffuf / nikto

> 📅 2026-08-05 · Getting Started
> Scanning tells you the door is open; enumeration tells you what is behind it. gobuster, ffuf, and nikto are the three most-used tools in the enumeration phase. This article covers what each excels at, with ready-to-use examples.

---

`recon-03-scanning-nmap` taught you scanning — finding which doors are open. But once the door is open, the question only begins: **what is behind it?** The phase that answers this is **enumeration**, and its three most-used tools are the stars of this article: gobuster, ffuf, and nikto.

> Authorization reminder: these three tools belong only on your own servers, CTF targets, or an authorized scope. Brute-force enumeration fires large volumes of requests; using it against an unauthorized target is scanning an attack and is illegal in most places.

## What enumeration does

Scanning sees ports and services; enumeration goes further: **asking each service, one by one, what it actually provides.** For a web server, that means asking:

* Which directories and files does this site have?
* Which subdomains exist?
* What server software version is it, and does it have known issues?

The answers make up the detail of the attack surface — exactly the input `cve-03-reading-a-cve` needs to decide "should I worry."

## gobuster: finding directories and subdomains

gobuster uses **dictionary brute force** to guess hidden directories or subdomains on a server:

```
# Find hidden directories on a site
gobuster dir -u http://127.0.0.1/ -w /usr/share/wordlists/dirb/common.txt

# Find subdomains
gobuster dns -d example.test -w /usr/share/wordlists/dns.txt
```

The principle is plain: take a list of "common names" and ask the server, one by one, "do you have this path?" A response means it exists. For defenders this is a reminder: **those directories you assumed nobody knew about are very likely already in some dictionary.**

## ffuf: fast fuzzing

ffuf (fuzz faster you fool) is currently the fastest fuzzing tool. It is more general than gobuster — not just directories, but parameters and hidden fields:

```
# Variation testing on a parameter
ffuf -u http://127.0.0.1/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

The difference is the `FUZZ` placeholder: ffuf swaps every dictionary word into that position, firing a large batch of requests and aggregating results fast. Speed is its signature — which also means **it generates a lot of traffic, so it belongs strictly in authorized environments.**

## nikto: web server scanning

nikto does not guess paths; it **checks against a known list of server issues**:

```
# Scan a site for known issues
nikto -h http://127.0.0.1
```

It checks server software versions, known risky configs, and insecure files and directories. Think of it as a "checkup report" — a fast list of "items you might want to look at," to be confirmed later with Burp or by hand.

| Tool | Excels at | One-line positioning |
|---|---|---|
| gobuster | Directories and subdomains | Dictionary-brute paths |
| ffuf | Parameters and hidden fields | High-speed fuzzing |
| nikto | Known-issue comparison | Server checkup report |

## How they fit together

A typical enumeration flow:

#### Scan first

Use nmap to confirm open ports and services (recon-03-scanning-nmap).

#### Enumerate directories

On the web port, use gobuster or ffuf to find hidden paths.

#### Compare known issues

Use nikto for a quick pass over known risky settings on the server.

#### Verify deeper

On interesting findings, dig in with Burp (kali-04) or by hand.

> Remember the flow: scan → enumerate → compare → go deep. Each step uses the previous result to narrow the scope, which beats blind flailing by a wide margin.

## Next

Enumeration found what is behind the door. Next, another high-frequency family: `kali-08-password-tools` introduces password-testing tools — the division of labor between hydra and hashcat, and the role they play in the password mechanics from `pass-02` and `pass-03`.

#### Q: What is the shared principle behind gobuster and ffuf?

* Editing the server configuration files

* Swapping dictionary words into the target position and sending requests, revealing existing paths or parameters

* Intercepting browser requests

* Analyzing already-captured packets

> 💡 Both are dictionary-driven enumeration tools: they feed dictionary words into a target position (path or parameter), and a response reveals the item exists.
