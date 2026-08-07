# A Web Attack Chain: From Scan to Root

> 📅 2026-08-05 · Deep Dive
> A real penetration test is often a chain of small steps: scan → enumerate → SQLi → get a shell → escalate. This article uses one complete attack-chain case to tie recon, web, and privilege escalation together.

---

**Case**: a Web server that "looked like nothing" ended with root. It was not broken by "one magic shot"; it was a **chain of small steps** — each harmless alone, together a complete compromise.

This article walks that chain so you can connect the `recon`, `web`, and `escalation` knowledge into one story.

> Security note: this is an authorized penetration-test chain (its target is the kind of machine in lab-02-vulnerable-targets). Applying it to any real target is intrusion. The defense for each link is given equal weight below.

## The chain

#### Scan

nmap finds 80 (HTTP) and 22 (SSH) open — a small surface, but enough to start (kali-18).

#### Enumerate

gobuster finds `/admin.php` and a `/?id=` parameter (kali-07).

#### Inject

The `?id=` parameter has SQL injection — exactly what parameterized queries should have stopped (web-02).

#### Get a shell

Injection advances to file writing, yielding a shell with the web server privileges.

#### Escalate

A too-wide sudo setting / SUID file — climbs from low-priv to root (kali-20).

**See it? No single step is a "magic vulnerability."** Each step is "the result of the previous link plus one thing not defended." A chain's strength is set by its **weakest link**.

## The defense for each step

| Link | Defense |
|---|---|
| Scan | Minimal attack surface: only necessary ports (`blue-01-hardening`) |
| Enumerate | Unneeded paths should not exist; `robots.txt` should not leak structure |
| Inject | Parameterized queries (`web-02-injection`) — cut this segment of the chain outright |
| Shell | Run the web service with least privilege, patch promptly |
| Escalate | Least privilege plus patching (`kali-20-privilege-escalation`) |

> Keep the chain mindset: defense cannot just fix "the most famous link"; every link must hold. Attackers hunt the weakest link; defenders must make every link strong.

## Detection: any link can expose the chain

Every link of the chain leaves a trace — that is what `blue-02-logging-siem` exists to catch:

* Scan: a burst of failed connections to many ports.
* Injection: abnormal SQL syntax in request parameters.
* Shell: the web process suddenly running unexpected commands.

**The blue team does not have to wait for the chain to finish — catch any one link and the chain breaks.**

## Next

This chain threads `recon-*`, `web-*`, and `kali-*` into one story. To practice link by link, `lab-07-web-challenges` has matching challenges; to see the whole chain from the blue side, `blue-02-logging-siem` and `blue-01-hardening` are the best contrast.

#### Q: What is the most accurate metaphor for the 'Web attack chain'?

* One magic vulnerability breaks straight through

* A series of small steps, each relying on the previous link; the chain is as strong as its weakest link

* Installing a firewall breaks the chain

* The attacker completes all steps at once

> 💡 The attack chain is the series of small steps scan→enumerate→inject→shell→escalate; defense must make every link strong and leave detection at any link.
