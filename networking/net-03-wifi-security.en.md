# Wi-Fi Security: Your Home Wireless Network

> 📅 2026-08-05 · Getting Started
> That little router in your home is actually a digital front door open to the whole world. What do WEP, WPA2, and WPA3 each protect? How does the WPA2 handshake work? How do you harden your router, and how do you spot a rogue access point? This chapter walks you through home wireless security from zero.

---

## Your home has a door that never closes

Think about this for a second: your router has handed your Wi-Fi password to every visiting phone, laptop, smart TV, and robot vacuum. That router is also broadcasting to the whole street — "I am here, this is my name, this is how I encrypt." If someone parks at the curb and tries to connect… **can it actually stop them?**

In `net-01-network-fundamentals` we learned how the network packs, addresses, and delivers data. This chapter answers a closer-to-home question: **what protects the last mile of the wireless network?** The answer lives in three words: WEP, WPA2, WPA3.

## Three generations of encryption: from paper doors to steel doors

The history of wireless encryption is really a history of "find the flaw → install a harder lock." Every generation solves the same core problem: **on completely unprotected radio waves, how do you make sure only people who know the password can understand the traffic?**

| Generation | Era | What it protects | Its fate |
|---|---|---|---|
| WEP | 1999 | Encrypts data with a fixed key | Key too short, cracked in minutes, fully obsolete |
| WPA | 2003 | Stronger TKIP algorithm | A transition, still breakable |
| WPA2 | 2004 | AES-CCMP and the 4-way handshake | The long-time standard; weak passwords still at risk |
| WPA3 | 2018 | Hardened handshake, resists offline brute force | The current best choice, default on new routers |

> If your router offers WEP or "WPA/WPA2 (TKIP)", choose WPA2 (AES) or WPA3. This is not a preference — it is a security tier. WEP encryption is broken in minutes by modern tools, which is the same as leaving the door unlocked.

Password cracking itself is a whole field — `pass-02-cracking-101` dissects how those tools work. Here, keep the conclusion: **the lock tier decides how much effort an attacker must spend.**

## The WPA2 handshake: a passwordless challenge

WPA2's core mechanism is the **4-way handshake**. That sounds technical, but an everyday analogy helps: you walk into a members-only bar, and the bouncer does not ask "what is the password?" Instead they ask a question **only someone who knows the password can answer**, and you ask one back — both sides confirm they belong.

A high-level view of the flow:

```text
AP ────ANonce (random)───→ device
AP ←──SNonce + MIC────── device
AP ────MIC────────────────→ device
AP ←────ACK──────────────── device
→ both derive the same session key
```

The key point: **the Wi-Fi passphrase never travels over the air.** Both sides feed "the known password plus both random numbers" into a key-derivation function and arrive at the same temporary encryption key; everything after is encrypted with it.

> But this does not mean a strong password lets you ignore everything else. An attacker can record the entire handshake and then brute-force the password offline on their own machine. If the password is short or common — like password123 — it is tried out in hours. So use a long, random passphrase, not a short word.

WPA3 goes further with its **SAE (Simultaneous Authentication of Equals)** handshake, which makes offline brute-force nearly useless — even a recorded handshake cannot be tried offline. That is why "choose WPA3" is not a gimmick; it is a real upgrade.

## Harden your router: five things you can do now

Your router is the front door of your home network, and factory defaults are rarely secure. These are the five most important, easiest defensive moves:

| Item | Why it matters | How |
|---|---|---|
| Change the admin password | Factory passwords are public knowledge | Log in and set a unique admin password |
| Update the firmware | Firmware flaws are a common entry point | Turn on auto-update or check manually |
| WPA2/WPA3 + strong password | Encryption tier and length set the difficulty | Pick WPA3 (or WPA2-AES), 12+ characters |
| Create a guest network | Visitors and smart devices should not reach your main gear | Enable Guest Network, isolate IoT |
| Turn off WPS | The WPS PIN can be brute-forced in hours | Disable Wi-Fi Protected Setup |

> WPS is the most ignored backdoor on home routers. It was designed for one-button pairing, but it has an 8-digit PIN that can be guessed. An attacker can crack that PIN in hours and recover the password directly. If you do not use WPS, turn it off.

These moves are hardening in miniature — `blue-01-hardening` expands the same idea to whole systems.

## Rogue access points: identical fake doors

Is there an attack that works even with a strong password? Yes — the **rogue access point**, also called an **evil twin**. An attacker sets up an access point near a coffee shop or your building with the same name (SSID) as the real one. Your devices cheerfully connect to it, and the attacker sees everything you send.

How do you tell them apart? A few practical checks:

* **Two Wi-Fi networks with the same name**: one is real, one is fake — something is wrong.
* **Full bars but oddly slow**: fake access points usually have tiny bandwidth.
* **Unexpected login pages or certificate warnings**: a fake AP often intercepts the sites you meant to visit.
* **Frequent drops and reconnects**: you may be getting kicked toward the fake.

> In public places, assume the Wi-Fi may be fake. Use a VPN on public networks, and avoid logging into banking or typing passwords. For the full encryption and identity story, net-02-tls-https explains why HTTPS is the critical part of this defense.

## One-line summary

Pack this chapter into a sentence: **home wireless security = pick the right generation (WPA3/WPA2) + a strong passphrase + a hardened router + permanent suspicion of unknown access points.** WEP is a paper door, WPA2 is a steel door, and WPA3 is an electronic lock that makes brute-forcers give up — but no lock stops you from setting the key to `12345678`.

## Next

The door is guarded; now look at the whole building's access control. `net-04-firewalls-vpn-proxy` covers firewalls, VPNs, and proxies — three tools people constantly confuse, each with its own job in network security.

#### Q: Even with a strong Wi-Fi password, why should you avoid logging into important sites on a public rogue access point?

* A rogue access point can impersonate any site and intercept your login

* Public Wi-Fi is always slower than home

* A rogue access point only affects phones, not laptops

* Public networks forbid encrypted protocols

> 💡 A rogue access point (evil twin) impersonates the service you meant to reach and intercepts your traffic and credentials. A strong password protects your own Wi-Fi; it does not make outside fake access points trustworthy.
