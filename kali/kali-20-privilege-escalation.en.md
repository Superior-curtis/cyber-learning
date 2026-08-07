# Privilege Escalation: The Logic from Low-Priv to root, and Defense

> 📅 2026-08-05 · Deep Dive
> Midway through a pentest, you often only hold low privileges. Privilege escalation exploits misconfigurations or flaws to push privileges upward — from a normal user to root. This article covers the routes, homelab commands, and the blue-team defense.

---

A penetration test often does not reach root in one step; it is: **get low privileges first → then push upward.** That "push upward" phase is **privilege escalation (privesc).**

> Security note (HOMELAB ONLY): every command on this page is practiced only on your own homelab host (for example Metasploitable, from lab-02-vulnerable-targets). Escalating on a real system is attack.

## What privilege escalation does

The goal in one line: **go from "normal user" to "root (or admin)."** Why it matters:

* Low privilege opens a limited set of doors; root opens nearly all of them.
* The end of a pentest is often "prove I can reach the highest privilege" — not actually doing harm, but **proving it needs fixing.**
* For defenders, understanding privesc means understanding "why least privilege and patching matter so much."

## Why "low → root" is even possible

Root should be the hardest thing to get, so why can it often be "pushed up to"? Because systems have a few common "ladders":

| Ladder | What happens |
|---|---|
| Sudo too wide | A normal user can "run certain programs as root" |
| SUID files | Some programs run with the owner's privilege and can be abused |
| Weak services | A service running as root has a flaw or is abused |
| Unpatched flaws | Kernel/program has a known privesc CVE (`cve-*`) |
| Over-writable files | Config files that can be written, then executed by root |

> Remember the common thread: privesc almost always exploits "permissions given too wide" or "something that should have been patched." Every ladder corresponds to a hole the defender should close.

## Homelab commands (your own host only)

Practice on your own homelab host (e.g., Metasploitable); first "become a low-privilege user," then start reconnaissance:

```bash
# See what programs my user can run with sudo (homelab only)
sudo -l

# Find SUID files (homelab only)
find / -perm -4000 -type f 2>/dev/null

# Run the automated enumeration script linpeas (homelab only)
./linpeas.sh
```

> Security note (again): on your homelab target these commands are "practicing a defense lesson"; on a real system they are unauthorized privilege escalation. The difference is always whether you have permission to touch that machine.

## The blue-team defense

Every privesc ladder has a matching lock:

| Ladder | Defense |
|---|---|
| Sudo too wide | Minimize `sudoers`: only the needed commands, never `ALL` |
| SUID | Remove unneeded SUID bits, audit regularly |
| Weak services | Do not run services as root, least privilege (`blue-01-hardening`) |
| Unpatched flaws | Patch (`cve-06-patch-management`) |
| Over-writable files | Correct file ownership and permissions (`linux-03-permissions`) |

> Defense mindset: privesc's root is "excess privilege + unpatched." Do least privilege and patching well, and even if an attacker gets in, they cannot climb up — blue-01-hardening is the full version of this.

## Security note

* ✅ Allowed: practicing the privesc flow on **your own homelab target**, to verify and understand the defense.
* ❌ Not allowed: escalating on any real or unauthorized system.
* ⚠️ Remember: unauthorized escalation is intrusion; on a well-defended system this phase should "fail to climb" — that is what you should verify.

## Next

The privesc logic and defense are clear. To close it off from the hardening side, `blue-01-hardening` is the best chapter; to review the mechanism of permissions themselves, `linux-03-permissions` and `linux-04-users-groups` lay the foundation.

#### Q: Why is 'permissions too wide' the most common root of privilege escalation?

* Because the root password is too weak

* Because systems grant root to normal users by default

* Because sudo too wide, too many SUIDs, and services running as root each let a low-privilege user borrow higher privilege

* Because antivirus is off

> 💡 The privesc ladders (wide sudo, SUID, root services, unpatched flaws) are all essentially 'excess privilege + unpatched'; defense that does least privilege and patching stops the climb.
