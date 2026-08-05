# Processes, Services, and systemd

> 📅 2026-08-05 · Getting Started
> Every Linux box is a collection of hundreds of programs running at once. Learn to read processes and manage services, and you have the first blue-team skill: knowing what normal looks like.

---

A server looks quiet — no windows, no animation, just a slowly spinning fan. But inside that quiet black box, several hundred programs are running at the same time. This article is about how to *see* them, manage them, and — most importantly — how **knowing what normal looks like is the first step to spotting something wrong.**

In Linux, running programs go by several names: processes, services, daemons. They are related but not the same. Once you can tell them apart, you have the vital signs of the machine.

## What a process is

A **process** is a program that is actually running — one live instance of it. Open three windows of the same editor and you have one program but three processes. A program is a file on disk; a process is the living thing in memory.

Every process has an identity:

| Term | Full name | Meaning |
|---|---|---|
| PID | Process ID | the unique number the system gives every process |
| PPID | Parent PID | who started this process |
| UID | User ID | whose identity this process runs as |

Processes have children: the things you start spawn sub-processes, forming a family tree. That tree matters — when you meet an unfamiliar process, tracing its parents usually shows you where it came from.

```
systemd (PID 1)
 └── sshd ──── sshd ──── bash ──── ps
 └── nginx
```

## Snapshot with ps

To take a picture of every process on the system, use `ps`. The most useful form:

```bash
ps aux
```

The letters `aux` have no deep meaning — just remember them. It prints a very wide table; the columns that matter:

| Column | Meaning |
|---|---|
| `USER` | whose identity this process runs as |
| `PID` | process number |
| `%CPU` `%MEM` | how much CPU and memory it is using |
| `COMMAND` | the actual command being executed |

`ps` is a snapshot — it shows the instant you pressed Enter. For live motion, read on.

The first column of `ps aux` is `USER`. Connect this to the previous chapter: **that column tells you whose identity the process runs as.** A web server running as root and the same web server running as a normal user have completely different blast radius if either is compromised. When you read a process, always read its USER too — this is the most practical application of `linux-04-users-groups`.

## Watch live with top / htop

`top` opens a continuously updating screen, sorted by CPU usage, refreshed every few seconds. `htop` is a friendlier version (may need installing). In security terms its job is clear: **which process is spiking the CPU? Which one ate all the memory?**

“A process suddenly pinning the CPU at 100%” is a classic alarm. It might be a legitimate batch job — or someone mining on your machine. Do not panic-kill; first figure out what it is. That habit is the whole point of this chapter.

## Signals and kill: asking politely, then not

You do not “delete” a process — you send it a **signal**. The everyday tool is `kill` with a PID:

```bash
kill 1234        # sends SIGTERM, asks the process to finish gracefully
kill -9 1234     # sends SIGKILL, hard stop
```

| Signal | Behavior | When to use |
|---|---|---|
| `SIGTERM` (default) | tells the process “time to end”, lets it clean up | try this first, always |
| `SIGKILL` | the kernel ends it outright, no cleanup possible | only when SIGTERM does nothing |

Send SIGTERM first so the process can save state and close files; escalate to SIGKILL only if it is stuck. **Before killing anything, be sure that PID is really the thing you want gone** — killing the wrong service is worse than letting a thousand harmless processes live.

## Services and systemd

Not every program should be started by hand. Some are designed to **run in the background and serve the whole system** — those are called **services** or **daemons**: web servers, databases, SSH, schedulers.

Modern Linux manages them with **systemd**. systemd is the very first program to run at boot (PID 1); it starts, supervises, and restarts services. You talk to it with `systemctl`:

```bash
sudo systemctl status sshd     # how is this service right now
sudo systemctl start sshd      # start it now
sudo systemctl stop sshd       # stop it now
sudo systemctl enable sshd     # make it start at boot
sudo systemctl disable sshd    # stop it from starting at boot
```

See the pattern? `status / start / stop` control “now”; `enable / disable` control “after the next boot.” **These five verbs are among the most important buttons in security** — because disabling an unused service is the cheapest attack-surface reduction there is.

> A pairing worth remembering: `active / inactive` describes *now*; `enabled / disabled` describes *at boot*. Services that auto-start (enabled) are the real attack surface; services you only start by hand when needed (disabled) are a much smaller risk.

## What actually starts at boot

“What does this machine auto-start when it boots?” is a question every defender keeps asking:

| To see | Command |
|---|---|
| services loaded for the current boot | `systemctl list-units` |
| services set to start at boot | `systemctl list-unit-files --state=enabled` |
| recent logs for one service | `journalctl -u sshd` |

Beyond systemd, remember two old roads: `cron` schedules (`crontab -l`, `/etc/cron.*`), and “what is really listening on the network” (`ss -tlnp`). Every single thing that auto-starts is a door someone could knock on.

> Blue-team fundamental: build a “normal” baseline. On a machine you trust, save the output of systemctl list-unit-files --state=enabled. Next time the box feels off, compare — the services that are new are the ones to investigate first. You do not need to understand everything; you only need to recognize “not like usual.”

## Meeting an unfamiliar process: investigate, then act

When you spot a process you do not recognize, the dangerous response is panic. The right workflow:

#### Look first, do not kill

Find the PID with ps aux, then inspect ps -fp \<PID> to see its parent and who launched it.

#### Check logs and config

Read its logs with journalctl -u \<service>, grep its config files, and find out who installed it and why.

#### Act only once sure

If it really is unwanted, stop it and remove it from boot: sudo systemctl disable \<service>.

That “investigate first, act second” sequence is incident response in miniature. You will see the same steps scaled up into a whole team process in `blue-04-incident-response`.

## Reading processes like a defender

This chapter, reduced to actions:

| Check | What you want to know |
|---|---|
| scan `ps aux` | any processes you do not recognize |
| watch `top` for a bit | any process eating an unusual amount of CPU |
| `ss -tlnp` | which services are listening on the network |
| `systemctl list-unit-files --state=enabled` | what auto-starts at boot |

“Less is more” is the general rule: **fewer services means a smaller attack surface.** Each extra listening service is one more entrance that could be exploited. Disabling what you do not need and closing ports you do not use is the least glamorous — and most effective — hardening move there is. It is also lesson one of `linux-09-hardening-basics`.

## Next

Now that you can see processes, the next tool in your belt is **finding things fast in walls of text.** `linux-06-shell-essentials` covers grep, find, and pipes — turning “a few hundred processes” and “tens of thousands of log lines” into one line you can actually read.

#### Q: The system suddenly slows down, and top shows an unfamiliar process using 99% of the CPU. What is the safest sequence of actions?

* Immediately run kill -9 on that PID, speed matters most

* First figure out what it is (ps, check its parent, read logs), then act once it is confirmed

* Just reboot the machine and pretend it never happened

* Kill every PID at once so everything is cleanly gone

> 💡 Investigate first, act second. An unfamiliar process could be an alarm or a false positive, and killing the wrong PID can take down a real service. Find out where it came from before deciding what to do.
