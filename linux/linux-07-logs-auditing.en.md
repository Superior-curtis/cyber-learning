# Logs & Auditing (/var/log, journalctl)

> 📅 2026-08-05 · Deep Dive
> Where Linux keeps a record of what happened: the syslog and journald worlds, auth.log, dmesg, log rotation, and what to look at right after something suspicious. Logs are the blue team's memory.

---

## The system keeps a diary

Computers do not chat, but they are excellent diarists. Nearly everything that happens on your Linux machine — who logged in, which service crashed, when the network card dropped, what someone ran as root — gets written down somewhere. Those records are **logs**.

For the blue team, logs are the security cameras of the incident scene: not every tape contains a case, but on the day something actually happens, you need more than guesswork — you need the footage. This chapter shows where the tapes live, how to play them, and which segment to watch first after something goes wrong.

## The two logging worlds: syslog and journald

Two "switchboard operators" sit at the center of Linux logging and sort the messages that pour in from everywhere. Get to know both and you have a map of the log universe.

| System | Where it lives | What it is like |
|--------|----------------|-----------------|
| **syslog (rsyslog)** | `/var/log/*.log` | Plain text files; old-school, greppable by any tool, format and forwarding configurable |
| **journald** | Binary database (`/var/log/journal/`) | Built into systemd; auto-collects all service output; structured fields and instant queries |

syslog is the "text school" that has been around since the pre-systemd era — every line is a human-readable log entry. journald is systemd's "database school" — it carries structured fields (time, PID, source) alongside the text. The two usually run side by side and are often two renderings of the same story: journald can forward its messages to syslog, and syslog writes them out as classic files.

**Reading order**: start with `journalctl` for the broad picture (especially service output), then dig into `/var/log` files for system-level events. They complement each other; you want both.

## /var/log: the system's filing cabinet

`/var/log` is home to the traditional text logs. The drawers worth knowing first:

| File | What it records | Why the blue team cares |
|------|-----------------|--------------------------|
| `/var/log/auth.log` | Logins and authentication (SSH, sudo, su) | Who is trying to get into your machine — priority one |
| `/var/log/syslog` | Catch-all system and application messages | The overview when something breaks |
| `/var/log/dmesg` | Kernel boot messages and hardware events | Hardware-level anomalies, boot errors |
| `/var/log/kern.log` | Kernel logs proper | Kernel-level anomalies |
| `/var/log/apt/history.log` | Package installs and removals | Who installed what, and when |

`auth.log` is the single most important file on the blue team's shelf. Successful and failed SSH logins, `sudo` runs, `su` switches — all of it lands here. To review recent login attempts:

```bash
# Recent authentication events
tail -n 50 /var/log/auth.log

# Just the failed login attempts
grep -i "failed password" /var/log/auth.log | tail -n 20

# Who has been using sudo
grep "sudo:" /var/log/auth.log | tail -n 20
```

## journalctl: the whole system in your hands

Systemd-based distros (Ubuntu, Debian, Fedora, Arch, and friends) query the journald database with `journalctl`. Instead of opening a pile of files, you search every service from one place:

```bash
# Last 50 entries, following new ones as they arrive (like tail -f)
journalctl -n 50 -f

# Just one service (SSH, for example)
journalctl -u ssh

# What happened after a point in time
journalctl --since "2 hours ago"

# Successful SSH logins only
journalctl -u ssh | grep "Accepted"
```

> Decide how long logs are kept before you think about anything else. journald will happily eat disk space; cap its retention, or the day you need to review three months back you will find them already gone. An admin's job is making the memory itself reliable.

## dmesg: the truth from boot time

As the system boots, the kernel writes every step — hardware self-checks, mounting disks, probing devices — into the **kernel ring buffer**. `dmesg` is the tool that reads that buffer back out to you; it is usually also saved to `/var/log/dmesg`.

When do you need it? A server rebooting without warning, an external disk vanishing, a NIC not being recognized — hardware-layer mysteries have their answers in dmesg. On modern systems `journalctl -k` shows the same content:

```bash
# Read kernel messages directly
dmesg | tail -n 30

# Same content via journalctl
journalctl -k -n 30
```

## Log rotation: making logs outlive your expectations

Logs that never rotate will eventually fill the disk and drag the system down. **Log rotation** is the answer: once a log hits a configured size or age, the system renames it aside as an archive, starts a fresh file, and keeps only the most recent N copies.

The configuration lives in `/etc/logrotate.d/`, for example:

```bash
# /etc/logrotate.d/myapp
/var/log/myapp/*.log {
  weekly
  rotate 4
  compress
  missingok
}
```

This means: rotate weekly, keep 4 copies, compress the old ones. **The archived logs are an auditing treasure.** Attackers are often discovered long after the fact — if you deleted the old files before they rotated, you have destroyed the evidence yourself. When configuring rotation, make the retention period at least cover your compliance and audit needs.

## After an incident: the blue team's "look here first" checklist

Logs are not for reading every day; they are for the moment something happens. After an event, check these places in order:

1. **`/var/log/auth.log`** — did anyone log in successfully from an unfamiliar IP? A dense burst of failures? "Who got in" is question one.
2. **`journalctl --since`** — everything every service did from your suspected time onward.
3. **`last` and `lastb`** — recent login history (`lastb` shows failures), a two-second answer to the login question.
4. **`history` and shell history** — what commands were run inside a user account (if history is enabled).
5. **Scheduled tasks** — check `crontab -l` and `/etc/cron*`; persistence via a cron job is one of an attacker's favorite tricks.

> Everything in this chapter is normal administration of your own machine. Inspecting someone else's system or logs without authorization may itself be illegal — logging tools are a defender's flashlight, not a burglary kit.

## auditd: the built-in audit framework

`/var/log` records what the system *says*; but sometimes you need to audit "who touched that file, who ran that command" — that is the job of **auditd**, Linux's built-in event auditing framework. It listens to kernel-level events and writes file opens, program executions, and permission changes into audit records, answering questions at the level of "who did what, and when."

```bash
# Install and enable
sudo apt install auditd
sudo systemctl enable --now auditd

# Watch a sensitive file (/etc/shadow here; -p wa = write + attribute change)
sudo auditctl -w /etc/shadow -p wa -k shadow_watch

# Query audit records touching shadow
sudo ausearch -k shadow_watch
```

auditd's value to the blue team: even when nobody is watching logs at the moment of the event, audit records preserve the full trace of which process, which user, and which access happened at which time. Combined with the ideas from `linux-05-processes-services`, you can extend monitoring from the service layer down to the file layer — which is the real meaning of "auditing."

## Turning logs into actual defense

Logs on a single machine only take you so far. The real game is centralizing logs from every machine into one place for unified search and alerting — that is what **SIEM** (Security Information and Event Management) does. Once you can read a single machine's logs, you carry the same skills to a SIEM and can answer questions like "was anyone in the whole fleet brute-forced today?" Next, `blue-02-logging-siem` turns your logs from a single-machine drawer into a company-wide surveillance net.

#### Q: After an incident, what is the first question the blue team should answer?

* Who logged into this machine successfully, and when

* How many seconds the boot took

* Whether the kernel version is the newest

* How much free disk space remains

> 💡 Who got in, and who tried and failed, is the first question of incident response; the answer lives in /var/log/auth.log and the last command. Boot time, kernel version, and disk space are daily operations questions, not where an investigation starts.
