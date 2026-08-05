# Linux Hardening Basics

> 📅 2026-08-05 · Getting Started
> A practical baseline for hardening a Linux box: keep patched, minimize services, a firewall, SSH keys with no root login, user and password policy, and automatic updates. Not an iron wall — a way to make breaking in not worth it.

---

## Hardening is just locking the doors properly

"Hardening" sounds mysterious, but it is really the reverse of what you learned in `linux-07-logs-auditing` and `linux-08-networking-cli`: reading logs taught you what to record; checking listening ports taught you which doors are open. This chapter is a **practical starting set** — no expensive equipment, just eight things you do to a system that push the cost of breaking in high enough that most attackers move on to softer targets.

Mindset matters: hardening is not about being impossible to break into. It is about making **cost of intrusion exceed value of target**. Attackers flow like water, always toward the lowest point. If you are not the lowest crack, you have already won most of the fight.

## Item one: keep the system current

It sounds like a cliché, but it is the foundation under everything else. The package management from `linux-06-shell-essentials` is not just for fun — **most intrusions exploit old versions of fixed vulnerabilities**. The moment a CVE goes public, exploit code follows within hours, and your defense window is "from patch release to update applied."

```bash
# Debian / Ubuntu
sudo apt update && sudo apt upgrade

# Fedora / RHEL family
sudo dnf upgrade
```

If you remember one sentence from this chapter, make it this: **updating takes priority over installing anything new.** Every time you log into a server, ask "are there pending updates" before you think about anything else.

## Item two: remove and minimize services

Remember `ss -tlnp`? Every listening port is one more entry point that could be broken into. The golden rule of hardening: **deny by default, allow what you need.** Anything the server does not use — printers (cups), an unused web server, a development database — gets stopped, even removed.

```bash
# See which services are running
systemctl list-units --type=service --state=running

# Stop and disable boot-time start (cups as an example)
sudo systemctl disable --now cups
```

> Attack surface scales with the number of running services. Removing a service you never needed deletes a whole class of vulnerabilities for free. It is the cheapest hardening step there is — you do not even need to understand the service.

Minimizing is not only about services. It covers user accounts too: the least-privilege principle from `linux-04-users-groups` lands here. Remove dormant accounts, take away shells nobody uses, and check `/etc/passwd` for accounts that should not exist.

## Item three: a firewall that denies first

The firewall philosophy matches service minimization: **deny by default, allow explicitly.** Ubuntu's `ufw` (Uncomplicated Firewall) is a beginner-friendly starting point — it wraps the gnarly syntax of `iptables`/`nftables` into simple commands.

```bash
# Enable ufw (careful: allow SSH first, or you will lock yourself out)
sudo ufw allow OpenSSH
sudo ufw default deny incoming
sudo ufw enable

# Allow specific services or ports
sudo ufw allow 443/tcp
sudo ufw status verbose
```

> Always allow SSH before enabling the firewall. If you enable first and allow second, your next action will likely be staring at a connection timeout and hoping the server has a console. Get the order right and the firewall protects you; get it wrong and it traps you.

The machine's externally visible listening ports should be only what genuinely needs to be public: SSH (if you need remote administration), 443/80 (if you serve the web), and any other clearly required service. Everything else gets blocked.

## Item four: SSH keys, and no root login

SSH is the remote front door of a Linux server and the most-brute-forced target there is (recall the auth.log from `linux-07-logs-auditing`). Two things about the front door are non-negotiable:

**First, switch to key-based login and disable password login.** Keys are an application of public-key cryptography (`crypto-03-asymmetric-crypto`): the server only knows your public key, and your private key stays on your machine. With no password to guess, brute force has no target at all.

```bash
# Generate a key pair on your local machine
ssh-keygen -t ed25519

# Install the public key on the server
ssh-copy-id user@your-server

# Edit /etc/ssh/sshd_config, then restart sshd
# PasswordAuthentication no
# PermitRootLogin no
sudo systemctl restart ssh
```

**Second, disable direct root login.** The root account is singular, its name is known to the entire world, and it has no lockout limit — letting root log in directly is like hanging a "welcome, try my password" sign on the door. The right pattern: log in with a normal account and escalate with `sudo` when needed.

## Item five: user password policy

Passwords are the weakest link in a server's security chain. Even with SSH password login disabled, local accounts and services on the machine may still use passwords. `pass-01-how-passwords-are-stored` covered how passwords should be stored; here is the **policy** side:

* **Length**: enforce a minimum; modern guidance is at least 12 characters.
* **Expiry and history**: rotate on a schedule, but do not push users into writing passwords on sticky notes.
* **Lockout**: lock the account for a while after N consecutive failures — the `pass-04-defenses` concept of throttling, landed at the system level.

On Ubuntu you can enforce complexity rules with `libpam-pwquality`. The point is not "how complex" but **"no weak passwords and no unlimited retries."**

## Item six: automatic updates

People forget; clocks do not. **Automatic updates** are the mechanical version of "keep current" — the system installs security patches by itself, no memory required. Ubuntu's `unattended-upgrades` is exactly this.

```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

> The trade-off: automatic updates save you the mental load of remembering to patch, at the price of "an update might break something." The pragmatic split is — security updates fully automatic, feature updates you review. Getting exploited through a publicly known hole is far more common than a one-off update mishap.

## Item seven: audit and monitor

Hardening is not "set it and forget it." The log skills from `linux-07-logs-auditing` become ongoing monitoring here: check auth.log for anomalies on a schedule, run `ss -tlnp` for listening ports you do not recognize, and `systemctl` for services that appeared out of nowhere. Build "that is odd" into an instinct, and you catch incidents on day one instead of three months after the fact.

## Item eight: build a repeatable checklist

Finally, fold the seven items into a checklist you run on every new server deployment:

* [ ] Packages updated; security updates automated
* [ ] Unneeded services and accounts stopped or removed
* [ ] Firewall denies by default, opens only what is needed
* [ ] SSH uses keys; password and root login disabled
* [ ] Password policy: minimum length, expiry, failure lockout
* [ ] Logs and listening ports audited on a schedule
* [ ] Configs and data backed up, and restore actually rehearsed

Finally, compress the eight items into a one-line summary table:

| Item | Blocks against | Core action |
|------|----------------|-------------|
| Keep patched | Exploits of known vulnerabilities | `apt/dnf upgrade` |
| Minimize services | Extra attack surface | `systemctl disable --now` |
| Firewall | Unauthorized inbound | `ufw default deny` |
| SSH keys | Password brute force | `ssh-keygen` + disable root login |
| Password policy | Weak passwords, endless retries | Minimum length + failure lockout |
| Automatic updates | Missed patch windows | `unattended-upgrades` |
| Audit and monitor | Blind spots after the fact | Scheduled log and port checks |
| Backup and restore | System damage and ransomware | Backup plus rehearsed restore |

## After hardening: where this thread goes next

The Linux series ends here — from `linux-01-why-linux` all the way to "lock the system down." But hardening a server is just one drawer in the blue team's toolbox. `blue-01-hardening` takes what you did to one machine and scales it to a whole organization — configuration baselines, patch management, asset inventory, aligning to CIS benchmarks. You now know how to lock one machine; the next step is learning to lock a whole room full of them.

#### Q: When enabling the ufw firewall, why is “allow SSH first” necessary?

* Otherwise you may lock yourself out of the server with no way back in

* Because SSH is the only service that needs to be open

* Because ufw cannot be enabled without SSH

* Because SSH needs to claim the first firewall rule

> 💡 If ufw enables with a deny-by-default for incoming traffic and SSH has not been allowed yet, your existing SSH session drops and new ones get blocked — you lock yourself out. Allowing SSH before enabling is the fundamental ordering of firewall setup.
