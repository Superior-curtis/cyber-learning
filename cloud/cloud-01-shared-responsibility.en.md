# Cloud & the Shared Responsibility Model

> 📅 2026-08-05 · Core Concepts
> The cloud is not “move your servers out and you are done” — it splits security responsibility in half. The shared responsibility model decides which half is whose, and it is the first lesson of cloud security. This article breaks it down in plain terms.

---

`net-04-firewalls-vpn-proxy` and `blue-07-iam-zero-trust` covered traditional and identity security. Now the environment nearly everyone uses: **the cloud.**

The first lesson of cloud security is a defining concept: **the shared responsibility model.** Understand it and you know "whose job is it to fix what."

## The cloud is not "move out and you are done"

Many assume that going to the cloud means handing the servers to someone else — and handing over security responsibility too. **That is wrong.**

The cloud provider does own the security of the **cloud** — data centers, physical hardware, network infrastructure. But you own the security **in the cloud** — your data, your configuration, your accounts, your application. **Responsibility is split, not transferred.**

## How responsibility divides

| Layer | Whose job |
|---|---|
| Physical data center, hardware | Cloud provider |
| Virtualization, underlying network | Cloud provider |
| OS, runtime | Depends on service type |
| Your data, configuration, access | **Yours** |
| Your application and accounts | **Yours** |

> Memory aid: the provider secures the cloud; you secure what is in the cloud. Physical layers are the provider's; data and configuration are always yours. The higher you go, the more responsibility you carry.

## Service types define the split

Cloud services come in three tiers, with different responsibility boundaries:

| Type | You manage | Provider manages |
|---|---|---|
| IaaS (VMs) | Everything from the OS up | Below virtualization |
| PaaS (platform) | App and data | Platform and below |
| SaaS (software) | Data and settings | Software and below |

**IaaS carries the most responsibility (you patch the OS yourself); SaaS the least (but data responsibility is still yours).** Choosing a service type is choosing how much responsibility you take on.

## The most common cloud incidents

With the model in hand, you will find that **most cloud incidents happen on "your half":**

* A storage bucket left public, exposing sensitive data.
* IAM permissions granted too wide, stale accounts still active.
* Encryption and audit logging never turned on.

That is exactly the theme of `cloud-02-cloud-misconfigs`.

## Next

Responsibility is clear. Next, look at what most often goes wrong on your half: `cloud-02-cloud-misconfigs` introduces cloud misconfigurations — the high-frequency issues that "only need one unchecked box" to break.

#### Q: What is the essence of the shared responsibility model?

* The provider handles all security

* The provider secures the cloud; you secure what is in the cloud — data, configuration, and accounts are always your responsibility

* No security is needed after moving to the cloud

* Responsibility is decided only by the provider

> 💡 Responsibility is split: physical layers belong to the provider, data and configuration always belong to you; the higher the stack, the more you carry.
