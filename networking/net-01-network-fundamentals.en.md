# Network Fundamentals: TCP/IP, Subnets, and Ports

> 📅 2026-08-05 · Core Concepts
> Type a URL, hit enter, and the page appears in a second. Before security makes sense, you need to know how two computers actually talk — IP addresses, transport protocols, and the doors called ports.

---

You open a browser, type a URL, and the page appears a second later. It is so natural that almost no one stops to ask: **what actually happens in between?**

To understand security — why certain attacks work, why firewalls help, why leaving a port open is dangerous — you need to know how two computers connect. The good news: this foundation is simpler than it sounds. Think of it as sending mail.

## A network is just sending mail

When two computers talk, it works almost exactly like mailing a letter. A complete letter has three parts:

* **The address**: which building it goes to — this is the **IP address**.
* **The delivery method**: registered mail or plain mail — this is **TCP or UDP**.
* **The department**: which mailbox in the building — this is the **port**.

Master those three and the skeleton of the network is clear. They are also the three words you will hear most often in security.

## IP addresses: the street address

Every device on a network has an **IP address**, like a street number. The most common format is **IPv4** — four number groups, such as `192.168.1.10`. The larger **IPv6** is spreading, but the idea is the same.

IP addresses come in two flavors:

| Type | Purpose | Example |
|---|---|---|
| Public IP | Unique on the internet, reachable from outside | `8.8.8.8`, `104.18.12.5` |
| Private IP | Only valid inside an internal network | `192.168.x.x`, `10.x.x.x` |
| Loopback | Points at this very computer | `127.0.0.1` |

That boundary matters: **your home router has one public IP, while every device at home has a private one.** Outside connections arrive at the router, which forwards them inward — and that forwarding layer is itself a firewall-like barrier. When we get to scanning and firewalls, this public-vs-private divide comes up again and again.

## TCP vs UDP: two ways to send

Once you have an address, you pick how to send. Two main options:

* **TCP**: registered mail. It confirms the other side is there first, every message gets an acknowledgment, lost pieces are resent, and order is preserved.
* **UDP**: a broadcast or a postcard. It fires and forgets — fast, but packets can drop or arrive out of order.

| Property | TCP | UDP |
|---|---|---|
| Establishes a connection first | Yes | No |
| Guarantees delivery | Yes | No |
| Speed | Slower | Faster |
| Typical uses | Web, email, SSH | Video, DNS, games |

One line to remember: **need reliability, use TCP; need speed and can tolerate loss, use UDP.**

## Ports: the doors on the address

One server offers many services — web, email, remote login — all on the same IP. **A port is how you tell those services apart on one machine**, like departments inside the same building.

Every service usually has a default port:

| Port | Service | Purpose |
|---|---|---|
| 22 | SSH | Secure remote login |
| 53 | DNS | Turns names into IPs |
| 80 | HTTP | Unencrypted web |
| 443 | HTTPS | Encrypted web |
| 3389 | RDP | Windows remote desktop |
| 8080 | HTTP alternative | Proxy or testing |

For security, **"which ports are open" is the clue to "which doors are unlocked."** A machine exposing SSH (22) to the whole internet with a weak password is essentially inviting anyone to try. That is why `recon-03-scanning-nmap` exists — not to poke strangers, but to find out which doors you left open so you can close them.

> Think of IP, port, and TCP/UDP as the address, the department, and the delivery method. Every later network article grows from these three.

## Subnets

A **subnet** is a way to group a network into smaller segments, so machines inside the same group talk directly and everything else goes through a router. For example, `192.168.1.0/24` is a common subnet holding `192.168.1.1` through `192.168.1.254`.

Subnets matter to security because **physical segmentation is the simplest defense**:

* Keep office machines and public servers on separate subnets; if one side is breached, the other is not instantly gone.
* A home router guest network is just a subnet isolating strangers.
* Cloud subnets are the same idea; `secplus-04-security-architecture` covers segmentation in depth.

## Putting the three together

You can now answer a practical question: "which doors does my computer have open?" On Linux, `linux-08-networking-cli` shows the `ss -tlnp` command. Any service you are not using is a door worth closing.

## Next

Now that addresses and doors make sense, one question follows naturally: **how does the little lock in the browser address bar actually get locked?** That is exactly what `net-02-tls-https` unpacks.

#### Q: A server runs both a website and email. How do clients tell the two services apart?

* Two separate network cables

* Different ports, such as 80 and 25

* Different IP addresses

* Different operating systems

> 💡 Ports distinguish services on the same IP; web and email each use a default port.
