# The Linux Filesystem: Where Everything Lives

> 📅 2026-08-05 · Getting Started
> On Linux, every file lives on one tree growing out of the root. Understand what lives in /etc, /var, /home, and /tmp, and you know exactly where evidence, configs, and suspicious files belong.

---

On Windows, your files live on drive letters like C: and D:. On Linux, **every file lives on one tree that grows out of a single root `/`.** That is not wordplay — it is the first map you need before touching any server. And in security, that map tells you directly where the evidence is, where the configs are, and where suspicious files are likely to hide.

This article is about reading the map. I will hand you the handful of small tools every regular uses (`pwd`, `ls`, `cd`, `find`), then wire "where a file lives" directly into your security instincts.

## Everything is a file, hanging on one tree

The Linux philosophy is "**everything is a file**": regular documents are files, directories are files, and even your hard drive, a running program, or the keyboard input get treated as files. All of them are hung on one upside-down tree.

The top of the tree is the **root directory `/`**. Below it, directories branch out, and each level is one step of a path. For example `/etc/ssh/sshd_config` means: from the root (`/`) enter `etc`, then `ssh`, and finally the file `sshd_config`.

The biggest difference from Windows: **there are no drive letters.** A hard disk, a USB stick, or a network mount can be attached ("mounted") to any directory on the tree. To the user, it all just looks like paths on one tree.

## Mounting: a world without drive letters

Because there are no drive letters, "plugging in a new device" has another name on Linux: **mounting**. A USB stick, a new disk, a network drive — each gets attached to some directory on the tree, for example `/mnt/usb`. The mount point is an empty spot on the tree, and once something is mounted there, that path leads into the device.

For you (the future security person) this has a practical payoff: **you do not need to know which physical disk the data is on — only the path.** On a strange machine, the `mount` command tells you which device each mount point corresponds to, and "something odd mounted somewhere odd" is itself a possible anomaly. The concept of mounting will come back later when we talk about services and isolation (for example in `linux-05-processes-services`).

## The directories worth knowing

You do not need to memorize all directories, but these are the landmarks a security worker hits every single day:

| Directory | What it holds | Why it matters for security |
|---|---|---|
| `/` | the tree root, where everything starts | the origin of every path |
| `/etc` | system configuration files | **weaknesses and misconfigurations mostly live here** |
| `/var` | variable data: logs, queues, caches | **logs (`/var/log`) are where incident investigation starts** |
| `/home` | every user's personal folder | user files, `~/.ssh` keys |
| `/tmp` | a world-writable scratch space | a favorite place for suspicious files (star of the next chapter) |
| `/usr` | installed programs and libraries | the body that actually runs the system |
| `/bin`, `/sbin` | executable programs | the commands the system boots with |

The point is not to memorize everything at once. The point is to build the feeling that **everything has a rightful place.** A core security instinct is this: when a file shows up somewhere it should not be, or a config file contains something it should not contain, something is wrong.

## Hidden files: the leading dot

Linux has an odd little rule: **filenames that start with a `.` are hidden by default** — `ls` will not show them; you need `ls -a`. These "dotfiles" are not secret (it is just a historical convention), but they often hold important things:

| Dotfile | Usually holds |
|---|---|
| `~/.bashrc` | your shell configuration |
| `~/.ssh/` | SSH keys and settings (the whole directory is a dot directory) |
| `~/.profile` | environment settings loaded at login |

Two security lessons here. First, **to see a directory fully, remember `ls -a`** — miss the dotfiles and you miss half the clues. Second, attackers love to hide tools or backdoors in dotfiles because they are invisible by default. `linux-07-logs-auditing` and `linux-08-networking-cli` will lean on this instinct.

## Absolute vs relative paths

There are two ways to point at a place on the tree:

* **Absolute path**: the full route from the root, always starting with `/`. For example `/var/log/auth.log`.
* **Relative path**: starts from "where you are right now." If you are already inside `/var`, just write `log/auth.log`.

Think of it as "my address is 10 Main Street" (absolute) versus "take the second lane on your right after leaving the door" (relative). Both are used constantly, but **mixing them up reads the wrong file** — not a paperwork issue, but a classic source of config bugs: a program using a relative path and started from the wrong working directory loads a completely different config.

## Shortcuts: symbolic links

Linux's "shortcut" is the **symbolic link (symlink)**: a filename that merely points to another file. You create one with `ln -s target link`, and reading the link reads the target.

```
ln -s /etc/nginx/nginx.conf /tmp/web_config
```

Symlinks have two faces in security. Legitimate use: linking configs and programs to convenient locations so you do not have to duplicate them. Dangerous use: **if a high-privilege program follows a link to read or write a file, an attacker can re-point the link at a file they want** — that is the classic symlink race vulnerability. The safe practice is simple: check whether a file is a link before writing to it, and stay alert in world-writable directories like `/tmp`.

`ls -l` marks symlinks with an `l` and shows `->` for where the link points.

## Moving around and finding things: your first commands

Time to get hands-on. Open a terminal (or any Linux box) and build muscle memory with these:

| Command | What it does |
|---|---|
| `pwd` | print the directory you are in (Print Working Directory) |
| `ls` | list the contents of the current directory |
| `cd /etc` | move into `/etc` |
| `cd ..` | go up one level |
| `cd ~` | go back to your home directory |
| `find / -name "*.conf"` | from the root, find every `.conf` file |
| `locate sshd` | fast search using an index (needs `updatedb` to have run) |

`find` is the real Swiss army knife: it does not just search names — it filters by size, time, permissions, and owner. Later chapters will use it to hunt for suspicious files on **your own test machines or CTF environments**. `locate` is fast but depends on an index, so it is good for everyday lookups and less flexible.

```bash
pwd
# /home/tom
ls
# Desktop  Documents  notes.txt
cd /var/log
ls | head
# auth.log  boot.log  dmesg  syslog  ...
pwd
# /var/log
```

Notice what happened: after `cd /var/log`, `pwd` changes its answer. **Where you stand decides what a relative path points at** — which is the root of many config bugs, and the first thing to check when you follow someone's instructions and nothing works.

One level deeper, `find` can search by things other than the name:

```bash
# find config files in /etc modified in the last 3 days
find /etc -name "*.conf" -mtime -3

# find files writable by anyone (a risk signal)
find / -type f -perm -0002 2>/dev/null

# find files larger than 100MB
find /home -size +100M
```

`-mtime -3` means "modified within three days," and `-perm -0002` means "writable by others." These show up again in the audit checklist of `linux-09-hardening-basics` — keep the impression.

## Why this matters for security

"Which directory a file lives in" is not a detail in security — it is a trail of evidence. Three lines of the trail are especially useful:

* **`/etc` is the home of configuration.** Want to check whether a machine has been tampered with, or whether a strange service got enabled? Walk through `/etc`.
* **`/var/log` is the home of events.** Who logged in, when did they fail, did a program crash — it is all here. `linux-07-logs-auditing` will teach you to read it.
* **`/tmp` is world-writable.** Any user can create files in `/tmp`, which makes it a favorite hiding spot — and the star of the next chapter on permissions.

Once you get used to thinking in directories, any server stops being a messy pile. The tree appears in your head, along with what belongs at each landmark.

> A path is a language. Reading /var/log/auth.log stacks three kinds of knowledge: /var means "variable data," log means "logs," and auth means "authentication." Learn to unpack paths, and you can read what a machine is telling you it has done.

## Next

You now know where every file lives. The next chapter answers a sharper question: **who can read, who can write, and who can run** — permissions and ownership (`chmod` / `chown`). It is also the official answer to why a world-writable `/tmp` is dangerous. That is `linux-03-permissions`.

#### Q: A Linux server had a security incident, and you want to check who tried and failed to log in at 3 AM. Where is your first stop?

* In /etc, looking for login settings

* In /var/log, looking for authentication logs

* In /home, digging through every user folder

* In /tmp, looking at scratch files

> 💡 Login and authentication events are recorded under /var/log (for example auth.log). /etc holds configuration, /home holds user data, and /tmp is a world-writable scratch space — none of them record login attempts.
