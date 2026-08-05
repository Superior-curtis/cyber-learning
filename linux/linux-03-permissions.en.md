# Permissions & Ownership (chmod / chown)

> 📅 2026-08-05 · Getting Started
> The Linux permission system is the most basic lock on the whole machine: three actions — read, write, execute — applied to owner, group, and everyone else. One wrong set of permissions is an open door.

---

Every door on a computer has a lock, and the keys come in three flavors: who you are, which group you belong to, and everyone else. That is the Linux permission system — the most basic lock on the entire machine, and one of the easiest things to get wrong and one of the most commonly abused. This article takes that lock completely apart, and by the end you will see clearly why "wrong permissions" is itself a vulnerability.

## rwx: three actions

For every file or directory, three actions are defined for three classes of people:

| Action | On a file | On a directory |
|---|---|---|
| **r**ead | view the contents | list what is inside |
| **w**rite | change the contents | create or delete files inside |
| e**x**ecute | run it as a program | walk into the directory |

The three classes are **owner (user)**, **group**, and **other**. A file's permissions can therefore be written as three `rwx` groups, like this:

```
-rwxr-xr-x    tom   devs    program
```

Reading it left to right:

* `-`: it is a regular file (a `d` would mean directory).
* `rwx`: the **owner**, tom, can read, write, and execute.
* `r-x`: the **group**, devs, can read and execute, but not write.
* `r-x`: **everyone else** can read and execute, but not write.

The order is fixed: `u` (user) → `g` (group) → `o` (other), each three characters `rwx`. The first column printed by `ls -l` is exactly this string.

## Reading the full `ls -l` line

The permission string is the first column of `ls -l`, but the whole line carries more security information. Take this example:

```
-rwxr-xr-x  1 tom  devs  2048  Aug  5 10:30  program
```

The fields, in order:

| Field | Example | Meaning |
|---|---|---|
| type and permissions | `-rwxr-xr-x` | file type (`-` or `d`) plus three rwx groups |
| link count | `1` | how many names this file has |
| owner | `tom` | who owns it |
| group | `devs` | which group it belongs to |
| size | `2048` | bytes |
| modification time | `Aug 5 10:30` | when it was last changed |
| name | `program` | the filename |

For an investigation, "last modified time" is often the key clue: **the time a system file was changed is frequently the time the incident happened.** Get used to scanning the whole line, and you will see not "a string of text" but a timeline of the machine.

## Numeric mode: why everyone says 755 and 644

Symbols are good for reading, but commands use numbers. Each `r`, `w`, `x` maps to a number — present means 1, absent means 0:

```
r = 4    w = 2    x = 1
```

Add up each group of three and you get a digit from 0 to 7: `rwx` = 4+2+1 = 7, `r-x` = 4+0+1 = 5, `r--` = 4. Three digits together are the full permission:

| Digits | Expanded | Meaning |
|---|---|---|
| `755` | `rwxr-xr-x` | owner read/write/execute; everyone else read/execute |
| `644` | `rw-r--r--` | owner read/write; everyone else read only |
| `600` | `rw-------` | only the owner can read and write — the standard for private files |
| `777` | `rwxrwxrwx` | everyone can read, write, and execute — almost never right |

The logic is plain: **the bigger the number, the more doors it opens.** `777` means removing the locks entirely, and very few files deserve that.

```bash
# make script.sh 755: owner can write, others read and run
chmod 755 script.sh

# make secret.key 600: only the owner can read and write
chmod 600 secret.key

# symbolic mode: give the group read access
chmod g+r report.txt

# recursively set the whole directory to 644
chmod -R 644 docs/
```

The symbolic form is just as handy: `u`/`g`/`o` plus `+`/`-` plus `r`/`w`/`x`, for example `chmod o-w file` means "take away write permission for everyone else."

## chown: who owns the lock

Permissions are the lock, and the **owner is the one who decides about the lock** — only the owner (or root) can change a file's permissions or hand it to someone else. Changing the owner is what `chown` does:

| Command | Effect |
|---|---|
| `chown tom file` | make tom the owner of file |
| `chown tom:devs file` | set owner tom and group devs at once |
| `chown -R tom:devs docs/` | recurse through the whole directory |

Most of the time you will not touch `chown` much, but it is the necessary half of the story: **the tightest permissions mean nothing if the owner is wrong — you just handed the key to the wrong person.**

## umask: the permission a new file is born with

A new file is born with a default permission, decided by **umask**. The umask is a "deduct this" value: `umask 022` means "by default, remove the write bit for everyone else," so:

* new files: `666 - 022 = 644` (`rw-r--r--`)
* new directories: `777 - 022 = 755` (`rwxr-xr-x`)

You can change the umask in `~/.bashrc` or in system config. **A too-loose umask makes every new file world-writable by default** — and "default too loose" is exactly the kind of finding security audits love to flag.

Common umask values and what they produce:

| umask | New files | New directories | Strictness |
|---|---|---|---|
| `022` | `644` | `755` | common default: readable by others |
| `027` | `640` | `750` | group can read, others see nothing |
| `077` | `600` | `700` | most private: only you |

For most machines `022` is fine. But on a box holding sensitive data (keys, password files, customer records), tightening the umask to `077` — making "new files readable only by me" the default — is an extremely cheap layer of protection.

## The sticky bit: turning /tmp into a shared living room

`/tmp` has to be world-writable, but that also means **any user could delete another user's scratch files** — a disaster on a shared host. The solution is the **sticky bit**, shown as a `t` at the end of the permission string:

```
drwxrwxrwt   /tmp
```

A directory with the sticky bit allows everyone to create files, but **only the owner of a file (or root) can delete it.** That is exactly how `/tmp` looks on real systems. When you see a trailing `t` or `T` in `ls -l`, that is it.

## The security meaning: whoever can write can replace the content

Stack the whole mechanism together and the security meaning fits in one sentence: **permissions govern who can change what on this machine — which is precisely what an attacker wants.** A few classic problems:

| Problem | Why it is dangerous | Defense |
|---|---|---|
| A config file is `777` | any user can change system settings | set it to `644` or tighter |
| A code directory is world-writable | an attacker plants malicious content that the system later runs | remove group/other write |
| A private key is `644` | other users can read your `~/.ssh/id_rsa` | set it to `600` |
| `/tmp` without the sticky bit | anyone can delete another user's files and disturb evidence | make sure the sticky bit is present |

Flip it around and you can see why **bad permissions are an entire vulnerability class by themselves**: OWASP's "broken access control" and the CVE list's shelf of "overly permissive defaults" are the same story. The defensive principle is plain — **least privilege: give only the people who need it, only as much as they need.** Sweep with `find / -perm 777` or a long `ls -l` during an audit, and you will immediately see which doors this machine left open.

## Permission auditing in practice: a ten-minute sweep

Turning this chapter's knowledge into action takes just a few commands — a small audit:

```bash
# find files that are writable by anyone
find / -perm -0002 -type f 2>/dev/null

# find overly loose permissions inside the config tree
find /etc -perm -002 -type f 2>/dev/null

# check whether your keys are readable by everyone
ls -l ~/.ssh/
```

Once the sweep returns results, the question is simple: **does it really need to be this way?** If not, tighten it — a three-second `chmod` that closes a door someone was planning to walk through. `linux-09-hardening-basics` will turn this sweep into a full hardening process.

> Permissions are not a "config detail" — they are the boundary. A config file only root can read, a key only you can read, a directory nobody can write: these quiet 600 and 755 entries are where defense actually lands. When checking whether a system is secure, check who can write first.

## Next

In the three permission classes, the sharpest question is: **who counts as a user, and who counts as a group?** The next chapter enters the world of users — accounts, groups, root, sudo, and why you should never do daily work as root. That is `linux-04-users-groups`.

#### Q: Your private SSH key (~/.ssh/id_rsa) shows permissions of 644, or rw-r--r--. What should you do?

* Leave it alone, because a key is not a config file

* Change it to 600 so only you can read and write it

* Change it to 777 so any program can read it

* Move it to /tmp, since that folder is world-writable

> 💡 A private key is your identity. With 644, every other user on the machine can read it. Setting it to 600 (rw-------) is the standard fix, so only you can touch it.
