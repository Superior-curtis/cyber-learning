# Cloud Misconfigurations

> 📅 2026-08-05 · Deep Dive
> The top cause of cloud incidents is not 'being hacked' but 'misconfiguration' — public buckets, overly wide permissions, endpoints left open. This article breaks down the most common misconfiguration classes and the defense for each.

---

`cloud-01-shared-responsibility` told you "your half" is yours. And statistically, **the top cause of cloud incidents is not hacking but misconfiguration** — one unchecked box, one permission too wide.

This article breaks down the most common misconfiguration classes and the defense for each.

## Why misconfiguration is the top killer

The cloud turns "security settings" into a pile of **options**: public/private, on/off, allow/deny. More options means a higher chance of checking the wrong one. And cloud incidents are often not "an attack succeeded" but **"it was already open"** — the attacker just walks through an unlocked door.

## Common error classes

| Error | What happens | Defense |
|---|---|---|
| Public bucket | Database backups, keys, user data downloadable by anyone | Default private, periodic audits of public permissions |
| IAM too wide | Accounts with "all-resources" admin, still active after leaving | Least privilege, periodic inventory, revoke on departure |
| Open admin endpoints | Databases, SSH exposed straight to the internet | Internal/VPN only, tightened security groups |
| No encryption | Data stored in the clear | Enable default encryption |
| No audit | No logs when something happens (`blue-02-logging-siem`) | Turn on audit and centralized logging |

> Think of cloud misconfigs as "forgot to lock the door," not "the lock was picked." They need no advanced skill — just one unchecked box. So cloud defense focuses on inventory and audit, not on fighting intrusions.

## The classic: a public bucket

The "public bucket" is the poster child of cloud misconfigs: a storage service holding files is set to "publicly readable," so everything on it — backups, configs, even user data — is directly downloadable by anyone.

Why so common? "Public" is convenient during development (testing, sharing), and once set, **few people remember to change it back or check.** Defense is two things only:

1. **Default private**, with public access requiring explicit reason and approval.
2. **Periodically scan** which buckets are public.

## The two sides of the defense

Defending misconfigs is essentially repeating two things:

* **Least privilege + least exposure**: the principles from `blue-01-hardening` and `blue-07-iam-zero-trust` apply in the cloud unchanged.
* **Continuous audit**: periodically scan "which resources are public, which accounts are too wide" — the "inventory" spirit of `cve-06-patch-management`, in cloud form.

## Next

The misconfig picture is clear. Next, fold it into practice: `cloud-03-securing-cloud` walks a complete cloud security approach — identity, network, monitoring — assembled into an actionable framework.

#### Q: Why is misconfiguration the top cause of cloud incidents?

* Because cloud providers are easy to hack

* Because the cloud turns security into a pile of options; one unchecked box can make resources public — not a picked lock but a door left unlocked

* Because the cloud has no antivirus

* Because the cloud is only for big companies

> 💡 A misconfiguration is not 'being attacked' but 'already open'; the defense focus is inventory and audit, not fighting intrusions.
