# Install & Build Your Own Practice Lab (VM / CTF)

> 📅 2026-08-05 · Getting Started
> Where you practice matters more than the practice itself. This article shows how to build a fully isolated, safe, and free practice environment in virtual machines — Kali plus a few deliberately vulnerable targets is your own little HackTheBox.

---

In `kali-01-what-is-kali` you met the tools and the line. Now the question is: **where do you practice without crossing that line?**

The answer is simple and important: **practice in your own virtual machines.** A VM is "a computer inside your computer" — anything you do inside it cannot affect your real system or your real network. This article shows how to build it, for free.

## Why VM / isolation

Practicing security tools, the worst move is "trying things on a real system." Three reasons you must use VMs:

1. **Safety**: scanning, testing, malicious files — all sealed inside the VM, unable to harm your real system.
2. **Disposable**: break it, delete it, rebuild in five minutes.
3. **Legal**: you have 100% authorization over your own VM — you can never cross the line by accident.

> Iron rule: the practice environment must be isolated from your real network. If a VM connects straight into your home LAN, it could scan other real devices in your house — that is no longer practice. The network setup below exists to prevent exactly that.

## Installing Kali: the overview

Full install steps vary by version; here is the **conceptual skeleton that matters**:

| Step | Key point |
|---|---|
| 1. Install a hypervisor | Free VirtualBox or VMware Workstation Player both work |
| 2. Download the Kali image | Use the official prebuilt VM image — already configured, far less work |
| 3. Import | "Import" the image in your hypervisor and boot |
| 4. Update | `sudo apt update && sudo apt upgrade` to refresh the tools |
| 5. Snapshot | Take a snapshot after setup so you can restore anytime |

For beginners, **using the official prebuilt VM image** beats installing from ISO by a mile — it skips a pile of drivers and config.

## Practice targets: deliberately vulnerable machines

Kali alone is not enough; you need "targets" to practice on. These **deliberately insecure** machines are built by the security community precisely for training:

| Target | What you practice |
|---|---|
| Metasploitable | A Linux box full of known flaws, great for beginners |
| DVWA | A deliberately vulnerable web app, for injection and XSS |
| OWASP Juice Shop | A fuller vulnerable web app, for the OWASP Top 10 |

`lab-02-vulnerable-targets` covers installing and using them. Here, just remember: **they all live inside VMs, fully isolated from your real system.**

## Network setup: the step that matters

The most overlooked part of a practice lab is networking. Your hypervisor offers several modes:

* **NAT**: the VM reaches the internet through your host, and outsiders cannot see it — **recommended for practice**.
* **Host-Only**: only your host and the VMs can reach each other; no outside connectivity.
* **Bridged**: the VM joins your LAN like a real device — **avoid for practice** unless you know exactly what you are doing.

> Recommended combo: Kali on NAT, targets on Host-Only, sharing one isolated subnet. Kali can scan the targets but cannot touch your real home network — practice with peace of mind, and legally.

## Next

Environment ready. Now meet your toolbox: `kali-03-tool-catalog` organizes Kali's hundreds of tools by category, so whichever skill you practice next, the right tool is obvious.

#### Q: Why must you practice security tools in an isolated virtual machine?

* Virtual machines run faster

* It contains the testing, cannot affect the real system or network, and you have full authorization over your own VM

* Kali cannot be installed on a real system

* Virtual machines are cheaper

> 💡 VMs give isolation (no impact on the real system), disposability (rebuild when broken), and clear authorization (your own machine).
