# Firewalls, VPNs, and Proxies

> 📅 2026-08-05 · Core Concepts
> Firewalls, VPNs, and proxies — three tools people lump together, though they answer three completely different questions: who gets in, is the path safe, and who runs the errand? Sort out these three roles and the skeleton of network security clicks into place.

---

Firewalls, VPNs, and proxies — these three words are almost always said together, as if they were the same kind of thing. But they answer three **completely different** questions:

* **Firewall**: who is allowed through this door?
* **VPN**: is this path safe?
* **Proxy**: who runs the errand for me?

Sort out those three roles and the skeleton of network security clicks into place. This article also clears up the most common misconception — that a VPN is an anonymity miracle cure.

## Firewall: the guard at the door

A firewall is a guard on the network boundary. Its job is simple: **apply rules to allow or drop traffic.** Rules might say "allow 443 in and out," "block IPs from a certain country," or "only let internal subnets talk to each other."

| Type | Where it runs | What it does |
|---|---|---|
| Host firewall | On one computer | Protects just that machine (Windows Firewall, ufw) |
| Network firewall | At the network edge | Protects the whole network (the one in your home router) |
| Next-gen firewall | At the edge | Reads application-layer content and adds threat protection |

The core principle fits in one line: **default deny, explicit allow.** Traffic that no rule covers is dropped, rather than "everything unspecified passes." The first item on the `blue-01-hardening` checklist is putting exactly this principle in place.

## VPN: an encrypted tunnel

VPN stands for "virtual private network." What it does is specific: **it builds an encrypted tunnel between two devices so a listener on the path cannot read the traffic.** Typical uses:

* Remote workers connect back to the office network as if they were sitting there.
* On public Wi-Fi (say, a coffee shop), traffic is encrypted first, so a rogue access point cannot read it — exactly the self-rescue tip from `net-03-wifi-security`.

**But kill this misconception now: VPN ≠ anonymity.** A VPN hides the *content* and the relationship between you and the VPN server; once your traffic reaches that server, it travels in the clear. The VPN provider can see what you do. True anonymity is a much deeper topic (Tor and friends), outside the scope of consumer VPNs.

> "I am invincible because I use a VPN" is wrong. If you type a password into a fake site, the VPN only guarantees the transport was encrypted — it does not make the site trustworthy. A VPN protects the road, not the destination.

## Proxy: the middleman who runs errands

A proxy server is your **errand runner**. Your request goes to the proxy, which forwards it to the real server and brings the response back. Along the way it "sees" what you sent, because it forwards it.

Two directions matter:

* **Forward proxy**: acts on *your* behalf to reach outside sites. Used for access control, hiding your source IP, and workplace browsing policy.
* **Reverse proxy**: sits in front of a *server*, absorbing requests, load-balancing, and terminating TLS. You will almost certainly meet one when deploying to the cloud.

> One line to separate proxy and VPN: a proxy runs your errands but can read your mail; a VPN is a sealed envelope no one on the road can open.

## The three roles at a glance

| Tool | Question it answers | Does it encrypt? | Does it hide your IP? |
|---|---|---|---|
| Firewall | Who gets in | Not necessarily | Not necessarily |
| VPN | Is the road safe | Yes (inside the tunnel) | Yes (to the outside, that server is you) |
| Proxy | Who runs the errand | Usually not | Yes, to the target site |

## Misconception cheat sheet

* **"A VPN evades all detection"**: wrong. VPN traffic has fingerprints, and enterprise firewalls often detect it.
* **"A proxy is a weaker VPN"**: wrong. They do different jobs — proxies forward at the application layer; VPNs build encrypted transport tunnels.
* **"A firewall blocks all attacks"**: wrong. A firewall is a boundary guard; it cannot stop legitimate-looking malicious requests, like flaws in a website itself. That needs the `web-01-owasp-top10` layer.

## Next

Now you can tell firewalls, VPNs, and proxies apart. Next, fit them into the bigger blueprint: `secplus-04-security-architecture` covers security architecture and design — how subnets divide, and how these tools combine into one coherent defense.

#### Q: What is wrong with the claim: a VPN makes you completely anonymous?

* A VPN does not encrypt traffic at all

* A VPN protects the transport path; the provider and the destination can still observe your behavior

* A VPN only works on phones, not computers

* A VPN always slows traffic to an unusable speed

> 💡 A VPN builds an encrypted tunnel and hides the link between you and the server, but the provider and the destination site can still see activity. It encrypts the road, not your behavior.
