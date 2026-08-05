# Security Architecture & Design

> 📅 2026-08-05 · Core Concepts
> Good security is designed in, not bolted on. Network segmentation, secure protocols, secure baselines, and the basics of cloud and IoT security — the half of security decided at the blueprint stage.

---

## A good house starts with blueprints

Many people think security is something you buy: install a firewall, load an antivirus, and the system is safe. Real-world security is closer to building a house. Load-bearing walls, fire escapes, and plumbing are decided at the blueprint stage; trying to change them after the house is built means tearing out whole walls. **Security architecture** is that blueprint — the layer of protection your systems get from the way they are shaped, before any product is switched on.

After `secplus-03-threats-mitigations` introduced the threat landscape and its mitigations, this chapter steps one level upstream: not "what gets attacked" but "how do you design so attacks rarely land." It is not about one device; it is about the overall shape — how networks are divided, which protocols are used, what baseline machines start from, and who is responsible for cloud and IoT. Hold on to one sentence: **a badly designed system cannot be saved by more products, and a well-designed system stops many attacks before they start.**

## Five principles of secure design

Whatever system you are looking at, its architecture almost always revolves around five principles. They make a good ruler for judging a design:

| Principle | In one sentence | Real example |
|-----------|----------------|--------------|
| Least privilege | Grant only the minimum needed to do the job | A backup account cannot delete data |
| Defense in depth | More than one layer; each layer protects independently | Firewalls plus auth, encryption, and monitoring |
| Fail secure | When things break, the door closes | An access-controlled door locks on power loss |
| Zero trust | Trust no network location; verify every time | Even internal requests must prove identity |
| Secure by default | A fresh install is already safe | Unneeded services and default passwords are off |

> Least privilege is the most often skipped principle. Most intrusions are not high-tech tricks; they are one over-privileged account. Split privileges small at design time, and even a stolen account only moves the attacker a small step sideways.

These five principles run straight through to the day-to-day operations in `secplus-05-security-operations` — every task there is really about keeping these principles alive.

## Access control models: who can touch what

Another core question in architecture design is: **who can touch what?** That leads to the **access control model** — the set of rules deciding which users can do which things to which resources. Four common models:

| Model | Full name | In one sentence |
|-------|-----------|-----------------|
| DAC | Discretionary access control | The resource owner decides who gets access |
| MAC | Mandatory access control | Labels and levels are enforced by the system; users cannot override |
| RBAC | Role-based access control | Access follows a role, like accountant, engineer, admin |
| ABAC | Attribute-based access control | Access follows attributes, like time, location, device state |

Map this table back to the five principles: **least privilege** asks "how much should this role have," and **zero trust** asks "should this request be verified." The more sensitive the system, the more it leans toward system-enforced models (MAC or strict RBAC); `blue-07-iam-zero-trust` develops identity, conditional access, and zero trust in full.

## Network segmentation: make attacks unable to travel

The next thing is **segmentation**. Joining the whole internal network into one flat plane is one of the most common fatal architecture mistakes: once an attacker breaks through one machine, they can move laterally across the whole network and wander up to the database. Segmentation is simple in logic — **cut the network into blocks, put firewalls between them, and allow only necessary traffic through**.

A typical layout looks like this:

```text
Internet → outer firewall → DMZ (web, mail servers)
↓
inner firewall ←────────────────┘
↓
internal network (databases, employee desktops)
```

The **DMZ (demilitarized zone)** is a deliberately placed buffer between the public internet and the internal network: services that must face the world (web, mail) live here, so a breach there cannot reach the internal network. The internal network is then split into smaller segments by department or trust level (VLANs), and sensitive systems (for example, finance-only systems) are isolated.

| Segment | What you do | What it protects |
|---------|-------------|------------------|
| DMZ | Public services separated from the internal network | The internal network from a compromised exposed service |
| VLANs | Split by department or trust level | Limits how far lateral movement can go |
| Micro-segmentation | Fine-grained rules per server | Turns "one machine down equals the whole floor lost" into impossible |

How do packets move between these blocks? That is the home turf of `net-04-firewalls-vpn-proxy` — firewalls, VPNs, and proxies are the tools where segmentation lands. For now, remember the direction: **segmentation means one success does not buy the attacker everything.**

## Secure protocols: pick the right lane

Data traveling over the network is like driving on a road. You can take an unlit dirt road with no guardrails, or you can take an encrypted highway. **Secure protocols are the encrypted highways** — they ensure data is not read or altered in transit.

| Insecure protocol | Secure replacement | Protects |
|-------------------|--------------------|----------|
| HTTP | HTTPS (TLS) | Confidentiality and integrity of web traffic |
| Telnet | SSH | Passwords and commands in remote logins |
| FTP | SFTP / FTPS | File transfer contents |
| Plain-text mail | SMTPS / STARTTLS | Mail in transit |

> TLS is the fuse in the wall of the internet. When you see HTTPS and the padlock in your browser, the browser and the server have negotiated an encrypted channel. net-02-tls-https opens up the handshake, certificates, and the certificate trust chain in full — this fuse is worth understanding end to end.

The mindset for choosing protocols is "encrypt by default": any traffic that is public, cross-segment, or cross-network should take the encrypted lane by default; plain-text protocols are not forbidden, but they need an explicit reason and belong only inside fully controlled segments. Every lane an architect draws on the blueprint should be encrypted.

## Secure baselines: start from a clean image

Imagine one hundred new servers, each with a different out-of-box state: one still has the default account, another runs unused services, a third has a blank password. A **secure baseline** is the standard answer to "what a machine must look like once it is installed" — the recipe for every host in your environment.

A baseline usually covers: removing or disabling default accounts and passwords, turning off unneeded services and ports, applying the latest software and security patches, enabling audit and logging, and consistent key and certificate management. Write the baseline down and bake it into a **golden image**, and new machines deploy directly from that clean image — this is the starting point of `blue-01-hardening`.

At the architecture level, remember: **a baseline is not a one-time setup.** Machines drift as they run — someone changes a setting by hand, a service gets quietly enabled. So a baseline must be paired with regular checks that pull drifted machines back in line.

## Cloud security: a shared responsibility

The cloud does not change security principles; it changes **who is responsible for what**. That is the shared responsibility model: the cloud provider secures "the cloud itself," and you secure "what is inside the cloud."

| The provider owns | You own |
|-------------------|---------|
| Physical data centers, server hardware | Accounts and permissions (IAM) |
| Virtualization platform, underlying network | Configuration inside VMs and containers |
| Managed service foundations (e.g. database services) | Encryption and access control of your data |
| Patching of the cloud platform itself | Your applications and their OSes |

The most common practical mistake is assuming that moving to the cloud means the vendor secures it. In reality, a large share of public-cloud breaches start with misconfiguration: a storage bucket left unsecured, an admin console exposed to the internet, a key committed into code. Cloud security starts by getting IAM (identity and access management), encryption, and network rules (security groups) right.

## IoT: the devices everyone forgets

Printers, cameras, temperature sensors, badge readers... **IoT (Internet of Things) devices** are the most ignored corner of security architecture — and they tend to be cheap, fragile, and permanently online. Three classic problems:

* **Default credentials**: many ship as `admin/admin` and never change.
* **No updates**: no patches after leaving the factory; vulnerabilities sit there forever.
* **Cannot run protection**: too little resources even for an antivirus or proxy.

> Segment IoT with "better safe than sorry." Put IoT devices on their own network, fully isolated from desktops, phones, and databases. The cost is a little management overhead; the payoff is that a hacked camera never becomes a fallen company intranet. This is a cheap defense that architecture can solve — and most organizations skip it.

Treat IoT the way you treat people: **default distrust**. Assume any of these devices may be compromised at any time, so grant them the smallest privileges and the narrowest network paths.

## Where firewalls, VPNs, and proxies fit

Putting the last sections together, firewalls, VPNs, and proxies are really the "gates" of the architecture:

* **Firewall**: decides what crosses a segment boundary — where segmentation lands.
* **VPN**: turns the insecure internet into a temporary internal channel — the front door for remote workers.
* **Proxy**: makes requests to the outside on behalf of internal users — a filter point and a screen that hides the internal layout.

`net-04-firewalls-vpn-proxy` unpacks the rules, deployments, and common mistakes of all three. At the architecture level, remember only this: **these devices are not effective the moment you buy them; they are part of the design** — where they sit, what they allow, and what they deny by default are all decided on the blueprint.

## One paragraph of summary

Fold this chapter into one sentence: **security architecture is not about what you bought, it is about how it is designed** — networks cut fine enough, protocols on encrypted lanes, machines starting from a clean baseline, cloud and IoT responsibilities made explicit, plus least privilege and defense in depth. On that foundation, many attacks are stopped by design before they begin. The rest is handed to the daily operations of the next chapter.

## What is next

The blueprint is done; now it needs to be kept alive. `secplus-05-security-operations` covers the other half of security — not the design, but the daily work: monitoring, patching, and response. However good the design, someone has to watch over it every day.

#### Q: Why does network segmentation significantly improve overall security?

* Because it stops an attacker who broke one machine from moving freely across the whole network

* Because it can fully replace firewalls and intrusion detection

* Because it automatically encrypts all external traffic

* Because it removes the need for least privilege

> 💡 Segmentation cuts the network into isolated blocks, so even after one machine is breached the attacker can only reach machines in the same segment and cannot walk straight to the database. The other options describe things segmentation does not do.
