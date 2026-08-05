# Users, Groups, and sudo

> 📅 2026-08-05 · Getting Started
> In Linux, everything runs as someone. Understanding users, groups, and sudo unlocks permissions, attack surface, and the principle of least privilege.

---

The first honest question almost everyone asks when they start using Linux is: “root can do anything, so why not just use root and skip the ceremony?” The instinct is completely reasonable — root really is all-powerful. But that is exactly what makes it dangerous. This article covers the three ideas that let you use Linux safely: **users, groups, and sudo**.

Linux is a multi-user system by design. Everything you learned in `linux-03-permissions` about rwx sits on one assumption: *who you are running as*. Get identity right, and the whole system suddenly clicks.

## Everything runs as someone

Here is the rule: **nothing in Linux exists without an identity.** Every process, every file, every running action belongs to some user. The moment you open a terminal, the shell inside it is running with the identity of some account on the system.

That is why “who am I running as” matters so much. The system never asks “is this action reasonable?” It only asks “does this identity have permission?” Checks either pass or fail — **identity is where every rule starts.**

You can look at it directly:

```bash
whoami        # who am I, in the eyes of the OS
id            # my UID, my primary group, and my other groups
```

## Where identities live: /etc/passwd and /etc/shadow

Identities live in two files. One is readable by everyone; the other only by root:

| File | What it holds | Who can read it |
|---|---|---|
| `/etc/passwd` | usernames, UIDs, home directories, default shell | everyone |
| `/etc/shadow` | password hashes, expiry and lock state | root only |

Moving password hashes from passwd to shadow was a big step in Linux history. Early systems stored hashes right in passwd, so anyone who could read the file could take them home and crack them offline. Now the hashes are locked in shadow, readable only by root — the “keep hashes tucked away” lesson from `pass-01-how-passwords-are-stored`, applied at the system level.

> Try it yourself. `cat /etc/passwd`, one account per line, fields separated by colons. You will see a bunch of accounts that do not look like people — those belong to system programs. More on that below.

A real line looks like this:

```bash
alice:x:1000:1000:Alice,,,:/home/alice:/bin/bash
```

Field by field (split on colons): username, password field, UID, primary group GID, comment, home directory, default shell. Notice the password field is just an `x` — the real hash lives in shadow. That is not an omission; it is deliberate design: **even though passwd is world-readable, it contains no password material that anyone could crack.**

## UID: the number is the real identity

Usernames are labels for humans. The system actually trusts **UIDs**. Change a name, keep the number, and permissions are completely unaffected.

| UID range | Purpose | Example |
|---|---|---|
| `0` | root, the highest privilege | root |
| `1–999` | system accounts, used by services and daemons | www-data, sshd |
| `1000+` | normal user accounts | you |

Those “not-a-person” accounts are system accounts: `www-data` runs the web server, `sshd` runs SSH. **Each service gets its own account**, so even if a service is compromised, the damage is contained to that account’s permissions. This is the principle of least privilege, implemented at the system level.

## Groups: give permission to a team

Granting permissions one user at a time would drive an administrator crazy. So Linux has **groups** — a named set of users, and permissions can be handed to the whole group at once.

Remember the rwx from `linux-03-permissions`? Every permission set is three slots: **owner, group, other**. A file’s owner sits in their primary group by default, and can join extra groups to reach more files.

| Role | Identity | Scope |
|---|---|---|
| owner (user) | the file’s owner | one person only |
| group | a set of users | a whole team |
| other | anyone else | everyone |

The power of groups is **sharing**: add a team of developers to the `developers` group and they can all read the project folder. Members of the `sudo` group can use sudo — that group matters, more below.

## The temptation of root, and its price

Back to the opening question: why not always use root? Because root carries two things at once: **unlimited power** and **unlimited blast radius**.

| What feels great | What actually happens |
|---|---|
| can install anything | one typo and the machine is gone |
| never think about permissions | permission checks are meaningless |
| nothing ever says “denied” | malware runs with the same “nothing denied” |

That last line is the key. **When you run as root, any malicious code on the system also runs as root.** You might just run a script that hides a one-line surprise, and that surprise now has your full power. Using root is not convenience — it turns “any single accident” into “a whole-system disaster.”

> Use a normal account for daily work. Never log in as root for routine tasks; the root password is the master key to the whole system — do not share it, type it into web pages, or keep it in notes apps. Even in an authorized lab or CTF environment, create a normal account and elevate with sudo when you need it.

## The principle of least privilege

All of the above condenses into the most quoted idea in security: the **principle of least privilege** — every identity gets only the minimum permissions it needs to do its job, and not one bit more.

* Users do daily work with a normal account; do not hand out admin by default.
* Each service runs under its own system account; nobody shares root.
* Lock down file permissions; make groups do the work; do not open things to “other.”

This is not bureaucracy for its own sake. It shrinks the **blast radius**. You cannot guarantee a system will never fail; you can guarantee that when it fails, the damage stays small.

## sudo: borrow the power, one command at a time

So what happens when you genuinely need admin access? The answer is **sudo** — log in as a normal user, then execute a *single* command as root on demand:

```bash
sudo apt update
```

The keyword is “single.” You live as a normal user, and only when needed do you borrow admin power for one command, then give it right back. That gives you three wins:

* **You must explicitly say “I want this”** before anything privileged happens, so accidents get fewer.
* **It leaves a record**: who ran which admin command at what time goes into the logs.
* **No shared root password**: each admin uses their own password, and you know whom to ask later.

Who gets to use sudo? That is defined in `/etc/sudoers` and by membership in the `sudo` group. It is a **prime audit target** — after every handover, after every engineer leaves, check it again. One fewer person with sudo is one fewer way a stolen account becomes a root compromise.

## The account lifecycle

Accounts are born, they live, and they die. From a security view, the problems usually come from the “death” part:

| Stage | Action | Security point |
|---|---|---|
| create | `useradd` a new account | start with least privilege from day one |
| use | daily logins, sudo | strong password, MFA where offered |
| disable | departure, no longer needed | lock the account — do not just change the password |
| delete | confirmed no longer needed | account, groups, and home directory together |

The most common gap is not “an extra account appeared.” It is “the departed employee’s account is still alive.” An intruder who gets hold of an old account has found a door that was never re-keyed. That lifecycle idea scales up into full enterprise identity management in `blue-07-iam-zero-trust`.

## Looking at users like a defender

Here is this chapter as a quick checklist:

| Check | What you are looking for |
|---|---|
| `cat /etc/passwd` | any new account you do not recognize |
| `getent group sudo` | who has sudo — and whether they should |
| account inventory | are unused accounts still alive, still active |

A very common first move by an intruder is to **create an account for themselves** or add an existing account to the sudo group. Skimming these few lines regularly is free, effective detection. These files and permissions will come back again and again in later hardening work, starting with `linux-09-hardening-basics`.

## Next

Identity is clear now — next we look at what those identities are actually *running*. `linux-05-processes-services` covers processes and services: everything happening on the system, and how systemd manages it. And knowing what “normal” looks like is the first step to spotting something wrong.

#### Q: Why do security guidelines recommend doing daily work as a normal user and elevating with sudo, instead of just logging in as root?

* Root logins are slower, so a normal account is faster

* When you run as root, any malicious code also gets root — one small accident becomes a whole-system disaster

* Root cannot install software, so a normal account is required

* A normal account actually has more permissions than root

> 💡 This is the principle of least privilege: run with the minimum you need, elevate only when necessary. Under root, every program — including malicious ones — gets the highest privilege, so the blast radius becomes unlimited.
