# Password Tools in Practice (hydra, hashcat)

> 📅 2026-08-05 · Getting Started
> hydra and hashcat are often lumped together, but they solve two completely different problems: one tests logins online, the other tests hashes offline. This article sorts out the split and connects it to the password mechanics you learned earlier.

---

In `pass-02` and `pass-03` you learned the mechanics of password cracking and hashcat/John. Now flip to the tools themselves: Kali ships two password tools that people constantly lump together — **hydra** and **hashcat**. They actually do two **completely different** things.

> Authorization reminder: the tools below are only for your own systems or authorized testing. Running login tests or hash cracking against an unauthorized target is an attack and is illegal in most places. Practice in the VM environment from kali-02-install-lab.

## The difference between two kinds of password attacks

Password testing splits into two families, and the tool you choose depends on "what you are facing":

| | Online | Offline |
|---|---|---|
| Facing | A login interface | A hash file |
| What you do | Send repeated login attempts | Compute hashes locally to compare |
| Representative tool | hydra | hashcat |
| Limit | The server locks/rate-limits you | Only compute power and time |

One line to remember: **hydra knocks on doors; hashcat picks the lock.** One tests real login interfaces; the other computes against hashes you already hold.

## hydra: online login testing

hydra fires a large number of login attempts at a **real login interface**, testing whether a username/password pair works:

```
# Testing a site form login (illustration)
hydra -l admin -P /usr/share/wordlists/rockyou.txt 127.0.0.1 http-post-form \
  "/login:user=^USER^&pass=^PASS^:Invalid login"
```

For defenders, hydra's existence is a reminder of how important rate limiting is: without attempt limits and lockouts, tools like this try tens of thousands of passwords a second — the reason `pass-04-defenses` stresses lockout policy.

## hashcat: offline hash cracking

hashcat faces **hash values you already hold** (say, your own database backup), computing locally at GPU speed to compare. `pass-03-cracking-tools` covered it in depth; one line refresher here:

```
# Dictionary attack on a hash file (illustration)
hashcat -m 0 hashes.txt /usr/share/wordlists/rockyou.txt
```

Its enemy is not a server but **the algorithm itself** — exactly why `crypto-02-password-hashing` says slow hashes are what carry value.

> What hydra and hashcat share is only the word "password." Online attacks are limited by the server; offline attacks are limited by the algorithm and compute. Know which one you face, and you know which tool to pick.

## How a defender reads this

Flip the lens to defense, and all these names become "check my own systems":

* Does my system have **rate limiting and lockout** to stop hydra-style online testing?
* Do I store passwords with **slow hashing + salt** (argon2/bcrypt), so hashcat gets nothing?
* Should I run the same tools over my own weak-password list first?

`pass-04-defenses` expands these into a full defense strategy. Tools let you turn "assumptions" into "measured facts."

## Next

Password tools covered. Next, another automation star: `kali-09-sqlmap` introduces automated SQL injection detection — how it turns the injection idea from `web-02-injection` into "one-command detection," and why it is a starting point, not an endpoint.

#### Q: What is the biggest difference between hydra and hashcat?

* One is a Linux tool, the other a Windows tool

* hydra tests a login interface online; hashcat computes against hashes offline

* hydra only cracks Wi-Fi

* hashcat cannot run on Kali

> 💡 hydra knocks on doors (online login testing); hashcat picks the lock (offline hash cracking). The problem you face decides the tool.
