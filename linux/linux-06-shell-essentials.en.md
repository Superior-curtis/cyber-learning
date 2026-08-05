# Shell Essentials for Security Work

> 📅 2026-08-05 · Getting Started
> Real security work happens half its life in the text interface. This post sharpens the essentials: pipes, grep, find, and the feel of one-liners.

---

Real security work is not all glossy graphical interfaces. Investigating a host, digging through a log, comparing thousands of lines of text — the tool that fits best is the **shell**. It is the flashlight you carry into a dark server room: learn it, and you finally start to *see*.

This article is not about memorizing commands. It is a set of tools you can *feel*, one tool and one realistic scene per section, finished off with a few one-liners you can actually use.

## Pipes and redirection: plug commands together

The soul of the shell is chaining many small tools. The core symbols:

| Symbol | Meaning | Example |
|---|---|---|
| `\|` | send the output on the left into the input on the right | `ps aux \| grep ssh` |
| `>` | write output to a file (overwrites) | `ls > list.txt` |
| `>>` | append output to the end of a file | `echo done >> log.txt` |
| `2>` | redirect error messages elsewhere | `cmd 2> err.txt` |

Once those four click, you have combos: `ps aux | grep ssh` means “list every process, then keep only the lines mentioning ssh.” That is arguably the single most-used one-liner in security work.

## grep: find things inside text

`grep` filters lines that contain a given string. It is your search box and your first-pass filter:

```bash
grep -i error /var/log/syslog      # find error, ignoring case
grep -r "Password" /etc/           # search a whole directory recursively
grep -v "^#" /etc/ssh/sshd_config  # invert: drop lines starting with #
```

`-i` ignores case, `-r` recurses into folders, `-v` selects lines that do *not* match, `-E` turns on extended regular expressions. When reading logs, `grep` is your best friend:

```bash
grep -iE "error|fail|denied" /var/log/auth.log | tail -50
```

That one line pulls every error, failure, and denial out of the authentication log, then shows only the last 50 lines. Yes, it is that plain — and anomalies are almost always found exactly this way.

## find: find files on the filesystem

`grep` searches text; `find` searches for **files**. It walks a directory recursively and filters by conditions:

```bash
find /var/log -name "*.log" -mtime -7   # .log files changed in the last 7 days
find / -type f -size +1G                # every file over 1GB
find / -perm -4000 2>/dev/null          # programs with setuid set
```

That last one is worth remembering: a setuid program runs with its owner’s privileges, which makes it a favorite target. `2>/dev/null` quietly drops “permission denied” noise so the output stays readable.

## sort and uniq: turn noise into counts

Faced with tens of thousands of repeated lines, the real question is: “which one appears most?” The answer is two tools chained together:

```bash
cut -d" " -f6 access.log | sort | uniq -c | sort -rn | head
```

Broken down: `cut` takes column 6 of every line, `sort` orders them (so identical lines sit together), `uniq -c` counts how often each appears, `sort -rn` sorts by count, biggest first, and `head` shows the top ten. This pipeline is the standard solution to traffic analysis and to just about every “count the things” task.

## cut and wc: grab fields, count lines

* `cut -d ":" -f1 /etc/passwd` — split on `:`, take column 1, and you instantly have every username on the system.
* `wc -l file` — count the lines. It is also a quick ruler for “how big is this log.”

`cut` with `-d` (delimiter) and `-f` (field) is the standard tool for colon- or comma-separated data.

## history: your own command log

The shell keeps a record of commands you type. `history` shows it, `!123` reruns command 123. Handy for you — but there is a security point here:

> Never type passwords or keys into the command line. history files, shell records, screen recordings, even ps output can leak what looked harmless — like mysql -p secret123. When a program needs a password, let it prompt you; do not type it into the command. This is not politeness — it is one of the most common ways secrets end up on disk.

`history` pairs well with `grep` too — for example, checking what download-related commands you actually ran a moment ago:

```bash
history | grep -iE "curl|wget"
```

That is the power of pipes: one tool lists, another filters. Every tool in this chapter snaps together with every other, like building blocks.

## Reading files safely

For big files and logs, plain `cat` drowns the screen. Build these habits instead:

| Goal | Tool |
|---|---|
| page through slowly | `less file` (spacebar to page, `q` to quit) |
| see the start | `head -20 file` |
| see the end (best for logs) | `tail -20 file` |
| watch a log as it grows | `tail -f file` (follow mode, new lines appear live) |

`less` is the best way to wander through text, and `/keyword` lets you search in place. `tail -f` is the standard posture for watching live logs — a new line arrives and the screen catches up instantly.

## Real one-liners, assembled

Let us put the pieces together for the two questions that come up most in security work. First: which doors is this machine currently holding open?

```bash
ss -tlnp        # which service is listening on which port
```

`ss -tlnp` means: `-t` TCP, `-l` listening, `-n` show ports as numbers, `-p` show the program. One line tells you every open door on the box — the first stop for both investigation and hardening. And you can refine it with everything above:

```bash
ss -tlnp | grep -v "127.0.0.1"
```

This drops everything bound only to localhost, leaving only the services visible to the outside world — the actual attack surface.

## The whole toolbox on one page

| Tool | Remember it as |
|---|---|
| `\|` `>` `>>` | who eats the output, where it goes |
| `grep` | filter text |
| `find` | find files on disk |
| `sort` `uniq` `cut` `wc` | order, count, slice fields, count lines |
| `less` `head` `tail` | read files without drowning |
| `history` | what you typed |
| `ss` | who is listening on which port |

None of these is fancy, but combined they are an investigator’s daily routine. `ps aux | grep` shows you processes, `grep error /var/log` shows you anomalies, `ss -tlnp` shows you the doors — **security investigation is, most of the time, these few moves repeated.**

> How do you build the feel for these? You do not memorize. Every day, when you run into “I want to find a thing,” translate that urge into a grep, a find, or an ss question. Within a week these stop being knowledge and become reflexes.

## Next

This article was about finding things inside text. `linux-07-logs-auditing` covers the logs themselves: who wrote what, how to read dates, and how to audit a log file systematically. Then `linux-08-networking-cli` goes beyond `ss` into the rest of the network toolbelt, so you can see exactly who this machine talks to.

#### Q: You want to know which services on the system are currently listening on the network and which ports they use. What is the right one-liner?

* ps aux

* ss -tlnp

* find / -name "*"

* history

> 💡 ss -tlnp lists listening TCP services and the programs behind them (-t TCP, -l listening, -n numeric ports, -p program). ps shows processes, find locates files, history shows your own commands.
