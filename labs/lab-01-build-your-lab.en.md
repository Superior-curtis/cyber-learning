# Build Your Own Practice Lab (VM + Kali + Targets)

> 📅 2026-08-05 · Getting Started
> Fold everything you have learned into your own little HackTheBox. This article shows how to use free virtual machines to assemble Kali, targets, and an isolated subnet into a complete learning path.

---

From `found-01` all the way to `kali-12`, you now have the map, the principles, and the tools. It is time to bring them together: **assemble them into your own practice lab — a free, legal, fully isolated "little HackTheBox."**

This article has a concrete goal: teach you to combine **Kali (the attacker-view tools), targets (practice objectives), and an isolated subnet (the safety boundary)** into one setup.

## Why your own lab

Why "your own"? Because practicing security fears two things above all:

* **Accidentally touching someone else's system** — so practice belongs in a fully isolated, fully owned environment.
* **Breaking your real system** — so it belongs in disposable virtual machines.

Your own lab zeroes both risks at once: **you have 100% authorization over everything in it, and you can restore in seconds when you break it.**

## What you need

| Component | Role | Free source |
|---|---|---|
| Hypervisor | Runs the VMs | VirtualBox / VMware Workstation Player |
| Kali Linux | The tool-loaded attacker-view machine | Official VM image |
| Targets | Practice objectives | Metasploitable / DVWA / Juice Shop |
| Isolated subnet | The safety boundary | Built into the hypervisor |

An ordinary laptop or desktop is enough — no special hardware required. Practice labs do not need speed; they need **isolation and reproducibility**.

## Network topology: one picture

The soul of a lab is its network settings. The safest, most common topology looks like this:

```text
[ Your real computer ]
│
│  NAT (Kali reaches the internet through your host)
▼
[ Kali VM ]
│
│  Host-Only subnet (fully isolated from the outside)
▼
[ Target 1 ] [ Target 2 ]   ← only talks to Kali
```

The key: **Kali can scan the targets, but the whole practice subnet cannot touch your real network or the outside.** Practice can never spill into the real world.

## Building it step by step

#### Install the hypervisor

Install VirtualBox or VMware Workstation Player (both free).

#### Import Kali

Download the official Kali VM image and import it (far easier than installing from ISO).

#### Add targets

Import practice targets as VMs too (introduced in lab-02-vulnerable-targets).

#### Set up the subnet

Put Kali on NAT and targets on Host-Only, sharing one isolated subnet.

#### Take snapshots

Once configured, snapshot each machine clean, so you can restore anytime.

> The snapshot is your undo button. Take one before practicing, restore when you break it, and you are back to a clean state in five seconds. That makes bold experimentation cost nothing.

## The mindset for practice

The lab is built; how you use it is the point. Three suggestions:

1. **Follow the book's route**: every article from `recon-01` to `kali-12` pairs with a matching practice in this lab.
2. **One skill at a time**: web first (DVWA + Burp), then scanning (nmap + Metasploitable). Do not light too many fires at once.
3. **Failure is progress**: the point of practice is not "breaking in"; it is understanding why. Stuck? Go back to the matching chapter.

## Next

The lab skeleton is up. Next, fill in the targets: `lab-02-vulnerable-targets` introduces the three most-used deliberately vulnerable machines — Metasploitable, DVWA, and OWASP Juice Shop — and what each is for.

#### Q: What is the core principle of a practice lab's network setup?

* Connect every machine straight to the internet

* Kali can reach the targets, but the whole practice subnet is fully isolated from the real network

* Give the VMs the same IP as your real computer

* Just turn off the firewall when practicing

> 💡 Isolation is the soul of a lab: the practice environment cannot touch the real network, so it can never spill into the real world or other people.
