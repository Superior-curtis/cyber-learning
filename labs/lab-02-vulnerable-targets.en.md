# Deliberately Vulnerable Targets (Metasploitable / DVWA / Juice Shop)

> 📅 2026-08-05 · Getting Started
> To practice, you need targets. The security community built a set of deliberately vulnerable machines specifically for legal practice. Metasploitable, DVWA, and OWASP Juice Shop are the three classics. This article introduces them and what each is for.

---

In `lab-01-build-your-lab` you built the lab skeleton. Now fill in the targets: **deliberately vulnerable machines.**

The words "deliberately vulnerable" are the point — these systems are **intentionally** insecure, built by the security community specifically for training. Their existence lets you do, in complete safety, the things that would be absolutely forbidden in any normal context.

> Targets belong only in an isolated environment. Run every target inside the isolated subnet from lab-01-build-your-lab. Do not connect them to your real network or the internet — they exist to be attacked, and exposing one on a real network hands a free shooting range to any passerby.

## What a deliberately vulnerable target is

A target is a **practice system with known flaws intentionally left in.** Its value:

* **Legal**: it is built for training; testing it is fully within authorization.
* **Safe**: isolated in a VM, it cannot be broken and cannot touch your real system.
* **Scored**: every flaw is "designed," so you can check your work and learn the principle.

Without targets, you can only "study on paper"; with them, you can turn the knowledge from `web-02-injection` and `kali-07-enumeration-tools` into real, practiced skill — completely legally.

## Metasploitable: the Linux target

**Metasploitable** is a deliberately vulnerable Ubuntu VM, packed with known flaws — open services, weak passwords, exploitable components. It is the best first target for beginners:

* Practice **scanning and enumeration**: use nmap to see what it exposes.
* Practice **vulnerability verification**: cross-reference `kali-10-exploitdb-searchsploit` and `kali-06-metasploit`.
* Practice **the penetration flow**: run the full "scan → enumerate → verify" route.

## DVWA: the web target

**DVWA (Damn Vulnerable Web Application)** is a deliberately vulnerable web app running on its own web server. It is the first choice for web-security practice:

* Practice **injection**: turn the SQLi and XSS concepts from `web-02-injection` into hands-on work.
* Practice **web-testing tools**: pair it with Burp Suite (`kali-04-burp-suite`) and sqlmap (`kali-09-sqlmap`).
* Practice **graded difficulty**: it has a Security Level setting from low to impossible, progressing step by step.

## OWASP Juice Shop: the full target

**OWASP Juice Shop** is a "realistic" vulnerable web app — not as bare as DVWA, but like an actual online store, packed with flaws mapped to the OWASP Top 10:

* Practice **composite vulnerabilities**: nearly every item in `web-01-owasp-top10` has a real instance here.
* Practice **problem solving**: it ships a challenge list, with concrete "goals" like a CTF.

| Target | Kind | Best for |
|---|---|---|
| Metasploitable | Linux system | Scanning, enumeration, the pen-test flow |
| DVWA | Web app | Injection, web-testing tools |
| Juice Shop | Realistic store | OWASP Top 10 composites |

> Suggested order for beginners: Metasploitable → DVWA → Juice Shop. From system to web, from single point to composite — difficulty and breadth progress naturally.

## Install notes

Installing each is roughly the same; the key points are only three:

1. **Import the VM**: all three have official or community VM images.
2. **Put them in the isolated subnet**: follow the `lab-01-build-your-lab` topology; no real network.
3. **Boot and go**: most come preconfigured — boot, note the IP, start practicing.

## Next

Targets ready. But "practicing" needs a method: `lab-03-ctf-101` introduces how CTF (Capture The Flag) competitions work — flag formats, challenge categories, and above all the ethics — so your practice is not random flailing but methodical and goal-driven.

#### Q: Why should targets run only in an isolated environment?

* Targets consume too many resources

* Targets exist to be attacked; exposing one on a real network hands a free shooting range to strangers

* Targets need special hardware

* Targets can only be used once

> 💡 Targets intentionally carry flaws and belong in an isolated subnet for practice; on a real network you hand a free vulnerable system to anyone.
