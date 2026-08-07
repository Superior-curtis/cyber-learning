# bettercap: The MITM Framework

> 📅 2026-08-05 · Deep Dive
> bettercap is the framework that modularizes 'man-in-the-middle' — ARP spoofing, sniffing, injection, all in one tool. This article explains its logic, homelab commands, and how the blue team defends against ARP spoofing.

---

`kali-19-responder-mitm` covered the MITM concept. Now meet the "MITM Swiss army knife": **bettercap** — it modularizes the MITM techniques (ARP spoofing, sniffing, injection) into one toolset.

> Security note (HOMELAB ONLY): every use of bettercap is limited to your own homelab subnet and your own devices. ARP spoofing on a real or someone else's network is MITM wiretapping — a crime.

## What bettercap is

bettercap is an open-source **MITM framework.** It splits "standing in the middle of the data flow" into modules:

* **ARP spoofing**: makes devices on the subnet believe "the attacker's MAC = the gateway," steering traffic to the attacker.
* **Sniffing**: captures plaintext data passing through.
* **Injection**: injects content into the forwarded traffic.
* **DNS spoofing**: steers name resolution to a fake target.

It turns the techniques from `kali-19` into a "menu-ized" framework.

## The logic of ARP spoofing

On a switched network, a device only sends data to "the machine it thinks is the gateway." ARP spoofing fakes that "thinks":

1. The device wants to send to the gateway → it asks ARP: whose MAC is the gateway?
2. The attacker **answers first**: "the gateway's MAC is me."
3. The device sends data to the attacker → the attacker forwards it to the real gateway.

> Keep this line: ARP spoofing fools "routing trust," not "encryption." Plaintext traffic gets read; encrypted traffic (HTTPS/SRTP) is gibberish even when steered — so encryption is the cornerstone against MITM.

## Homelab commands (your own subnet only)

On your own isolated homelab subnet, run a "communication drill" against your own devices:

```bash
# Install
sudo apt install bettercap

# Start on your own subnet interface, ARP-spoof-drill your own two test machines
sudo bettercap -iface eth0
# bettercap> set arp.spoof.targets 192.168.1.5,192.168.1.6   <- your own test machines
# bettercap> arp.spoof on
```

In the homelab the meaning is: **verify "if a malicious machine were on my subnet, would traffic be steered?"** — a defense drill, not an attack.

> Security note (again): setting arp.spoof.targets to anything outside your own subnet is MITM wiretapping — a serious crime. After the homelab drill, immediately arp.spoof off.

## The blue-team defense

| Measure | How |
|---|---|
| Static ARP binding | Gateway and critical hosts use static ARP, refusing answers |
| Switch security | Port security, Dynamic ARP Inspection (DAI) |
| Encryption | HTTPS/SRTP make steered traffic gibberish (`net-02-tls-https`) |
| Monitoring | Abnormal ARP broadcasts, gateway-MAC-change detection (`blue-02`) |

> Top defensive mindset: ARP spoofing can fool "routing," not "encryption." So the blue team's strongest move is not "fighting ARP" but "encrypting everything" — even if steered, it is unreadable.

## Security note

* ✅ Allowed: the drill above on **your own homelab subnet**, against **your own devices.**
* ❌ Not allowed: ARP spoofing or wiretapping on any real or someone else's network.
* ⚠️ Remember: MITM wiretapping is a crime; and encryption makes it "useless even when it succeeds."

## Next

The bettercap logic and defense are clear. To review the MITM concept, `kali-19-responder-mitm`; for the key countermeasure "encryption," `net-02-tls-https`.

#### Q: What does ARP spoofing fool?

* Encryption algorithms

* Routing trust — making devices send traffic to the attacker; but it cannot fool encryption, and encrypted traffic is gibberish even when steered

* User passwords

* Firewall rules

> 💡 ARP spoofing fakes 'who the gateway is,' steering traffic to the attacker; but the cornerstone against it is encryption — even if steered, it is unreadable.
