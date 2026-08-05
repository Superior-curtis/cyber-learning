# A Plain-Language Security Glossary

> 📅 2026-08-05 · Deep Dive
> CVE, 0-day, botnet, SIEM, EDR, spear-phishing — the jargon wall, torn down. Short, plain entries for every term you will meet, grouped into four families.

---

You open a security article and within three sentences you hit “CVE, CVSS 9.8, RAT, and a botnet was used to exfiltrate credentials.” It is not you — the field genuinely is a jargon factory.

The good news: the terms cluster into four families, and each family has maybe five members. This post is the lookup table — one or two plain lines per term, grouped so they stick, with pointers to where the book goes deep.

## Family 1: who is doing it

The people and machines behind an attack.

| Term | Plain meaning | In depth |
|---|---|---|
| **Threat actor** | Fancy word for “the bad guys” — an individual, a group, or a state | `found-04-threat-modeling` |
| **RAT** (Remote Access Trojan) | Malware that quietly gives the attacker remote control of your machine | `cve-04-famous-1` |
| **Botnet** | A swarm of hijacked machines (“bots”) that an attacker controls together — often rented out to run DDoS or spam | `cve-04-famous-1` |
| **Trojan** | Software that looks useful or innocent but has a hidden malicious job | `cve-04-famous-1` |
| **Hacker** | A word the media has run into the ground; in this book we mostly say “attacker” — the precise term | `found-04-threat-modeling` |
| **APT** (Advanced Persistent Threat) | A patient, well-funded attacker (usually a state group) that gets in and stays hidden for months | `cve-04-famous-1` |

The mental model: **the threat actor is the human, the botnet is their army, and the RAT and trojan are the tools for recruiting machines into that army.**

## Family 2: how the attack arrives

The techniques used to get in and to do damage.

| Term | Plain meaning | In depth |
|---|---|---|
| **Phishing** | Bulk fake messages (email, SMS, chat) that try to trick you into clicking or revealing secrets | `blue-06-phishing-defense` |
| **Spear-phishing** | Phishing aimed at one specific person, personalized with details about them — far more dangerous than bulk spam | `blue-06-phishing-defense` |
| **Social engineering** | Exploiting people instead of code — persuasion, impersonation, fake urgency | `found-04-threat-modeling` |
| **Ransomware** | Malware that encrypts your files and demands payment to unlock them | `blue-04-incident-response` |
| **DDoS** (Distributed Denial of Service) | Flooding a service with traffic until real users cannot reach it — an availability attack | `found-02-cia` |
| **0-day** | A vulnerability unknown to the vendor, so there is “zero days” of warning — no fix exists yet | `cve-01-what-is-cve` |
| **Exploit** | The code or technique that actually makes a vulnerability do harm | `cve-01-what-is-cve` |
| **PoC** (Proof of Concept) | A small, often harmless demonstration that a vulnerability can be exploited — proof that the bug is real | `cve-07-bug-bounty` |
| **Backdoor** | A hidden way into a system that bypasses normal login — often planted by malware, sometimes shipped by a vendor | `blue-01-hardening` |
| **Keylogger** | Software that records every key you press, silently harvesting passwords | `cve-04-famous-1` |

The mental model: **the vulnerability is the unlocked door, the exploit is the thing that opens it, the 0-day is an unlocked door nobody knows about yet, and phishing and social engineering are picking the lock of a person instead.**

## Family 3: how the industry files vulnerabilities

The system that names, describes, and scores bugs.

| Term | Plain meaning | In depth |
|---|---|---|
| **CVE** (Common Vulnerabilities and Exposures) | The industry database that gives each known vulnerability a unique ID — like CVE-2024-1234 | `cve-01-what-is-cve` |
| **CWE** (Common Weakness Enumeration) | A catalog of *types* of weakness — “SQL injection”, “weak password” — the genus that many CVEs share | `cve-01-what-is-cve` |
| **CVSS** (Common Vulnerability Scoring System) | The 0.0–10.0 severity score you see on vulnerability reports (9.8 means “very bad, patch now”) | `cve-03-reading-a-cve` |
| **Patch** | The vendor’s fix for a vulnerability — the whole reason CVEs exist is so you know what to patch | `cve-06-patch-management` |
| **Disclosure** | The process of telling the vendor about a bug before going public — responsible disclosure gives them time to fix it | `cve-02-lifecycle-disclosure` |

The mental model: **CWE is the disease, CVE is the individual patient, CVSS is how sick the patient is, and the patch is the medicine.** The whole machine exists so defenders know what to fix first.

## Family 4: the defenders’ toolbox

The tools and practices that live on the defensive side.

| Term | Plain meaning | In depth |
|---|---|---|
| **SIEM** (Security Information and Event Management) | A system that collects all the logs and alerts from your network into one place and helps analysts spot attacks | `blue-02-logging-siem` |
| **SOC** (Security Operations Center) | The team and room that watch the network around the clock and react to alerts | `blue-02-logging-siem` |
| **EDR** (Endpoint Detection and Response) | Security software on each computer that watches for bad behavior and can respond — the modern upgrade of antivirus | `blue-01-hardening` |
| **MFA** (Multi-Factor Authentication) | Logging in with two or more different kinds of proof — password plus a code from your phone | `blue-07-iam-zero-trust` |
| **Zero trust** | A philosophy: trust nobody by default, verify every request, even inside the network | `blue-07-iam-zero-trust` |
| **Sandbox** | An isolated environment where suspicious files run safely — if it is malware, it only escapes into the sandbox | `blue-05-forensics` |
| **Honeypot** | A deliberately fake, attractive target set up to lure attackers and watch what they do | `blue-03-threat-intel` |
| **IoC** (Indicator of Compromise) | A trace an attack leaves behind — an IP, a filename, a hash — used to detect other victims | `blue-03-threat-intel` |
| **IR** (Incident Response) | The plan and playbook for handling an attack once it happens — contain, eradicate, recover | `blue-04-incident-response` |
| **VPN** | An encrypted tunnel for your traffic — useful for privacy on public Wi-Fi, not a magic shield | `net-04-firewalls-vpn-proxy` |

The mental model: **the SIEM is the security camera room, the SOC is the team in it, the EDR is the guard on each floor, MFA is the extra lock on every door, zero trust is the “everyone shows ID” policy, the sandbox is the sealed room where you open suspicious mail, and the honeypot is the decoy vault.**

## A sixty-second self-test

Skip the definitions and recognize the scenes instead. You have heard these lines at work:

| What you hear | It is probably |
|---|---|
| “A 9.8-severity issue is being exploited in the wild” | CVSS score + exploit (maybe a 0-day) |
| “Our accounts are flooding in from the same set of countries” | botnet / DDoS or credential stuffing |
| “The CEO got an email from ‘IT’ asking for the password list” | spear-phishing / social engineering |
| “The vendor released an update; we should schedule it” | patch / patch management |

Being able to label those puts you ahead of most news readers.

## Terms that sound alike

Half the confusion in security is not new ideas — it is pairs of terms that look similar. Quick separators:

| Confused pair | The separator |
|---|---|
| CVE vs CWE vs CVSS | CVE = one bug, CWE = the type of bug, CVSS = how bad that one bug is |
| Phishing vs spear-phishing | one sprays, the other aims — the aim is what makes it dangerous |
| RAT vs trojan | a trojan is a delivery method; a RAT is what gets delivered (remote control) |
| EDR vs antivirus | antivirus matches known signatures; EDR watches behavior and can respond |
| VPN vs proxy | a proxy forwards your traffic; a VPN encrypts it too |
| SIEM vs EDR | SIEM watches the whole network’s logs; EDR watches each endpoint |

When two terms feel interchangeable, ask which question each one answers. The family table above is the same trick at a larger scale.

## The whole glossary in one breath

If you only have twenty seconds, hold onto these four lines:

```The&#x20;20-second&#x20;glossary
Who: threat actor, botnet, RAT.
How: phishing, ransomware, DDoS, exploit, 0-day.
Filing: CVE names it, CWE types it, CVSS scores it.
Defense: SIEM, EDR, MFA, sandbox, honeypot.
```

Every other term is a detail of one of these lines.

## How to actually use this glossary

Three habits will make the jargon stick faster than memorizing:

1. **Translate, then store.** When you meet a new term, force yourself to say it in one plain sentence. “RAT = remote-control malware.” If you cannot, you have not understood it.
2. **Ask which family.** Is it a *who* (family 1), a *how* (family 2), a *filing* (family 3), or a *tool* (family 4)? The family tells you the part it plays.
3. **Point it at a real story.** Take any breach headline and name: the threat actor, the technique (phishing?), the CVE if named, and the defender tool that could have caught it. One headline, four family members — the whole field in one exercise.

> The glossary is a map, not the territory. Every term here has a whole article behind it. When a term keeps coming up, that is the sign to go read its home article — the table above tells you where.

## Next

You now have the vocabulary. The next article turns it into a method: **threat modeling** — the discipline of thinking about your own systems the way an attacker would, *before* they do. That is `found-04-threat-modeling`.

#### Q: A personalized email pretending to be your boss, using real details about your project, tries to get you to reveal your login. What is this attack?

* A general phishing blast to many people

* Spear-phishing — phishing aimed at one specific target

* A 0-day exploit

* A botnet attack

> 💡 Phishing that is personalized for one named target is spear-phishing — the most dangerous form because it is hard to spot as fake. A 0-day is an unknown vulnerability, and a botnet is a swarm of hijacked machines.
