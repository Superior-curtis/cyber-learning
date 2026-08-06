# Wireshark: Packet Analysis

> 📅 2026-08-05 · Getting Started
> Burp lets you read web requests; Wireshark lets you see every piece of data that actually crossed the network. This article introduces packet analysis — the interface, basic filtering, and how to use it to find suspicious connections.

---

`kali-04-burp-suite` let you read the "application layer" requests. But the network world has an even lower landscape: **packets** — the small pieces of data that actually cross the network.

**Wireshark** is the tool for viewing that landscape. It is the recognized standard for packet analysis: capture traffic, inspect packets one by one, and use filters to pull the few you want out of thousands.

## What a packet is

When computers send data, they do not ship it as one giant blob; they cut it into **packets**. Each packet is like a letter, containing:

* **Source and destination**: who sent it, to whom (IP and ports).
* **Protocol info**: the "format" of the letter — TCP, UDP, HTTP, and so on.
* **Content**: the actual data.

Wireshark opens those letters for you, one by one: source, destination, protocol used, and content.

## Interface overview

Wireshark's interface looks intimidating at first, but you only need three panes:

| Pane | What it shows |
|---|---|
| Packet list | One row per packet: time, source, destination, protocol, summary |
| Packet details | Open a packet, fields unfold by layer |
| Hex view | The raw bytes of the packet |

A beginner's first move: **capture a little traffic, click a few packets, and watch the protocol layers unfold.** You will see with your own eyes what an HTTP request looks like in the details.

## Basic filtering: the needle from the haystack

Captured traffic easily runs to thousands of packets. **Display filters** are the key to pulling your target out:

```
# Only traffic to/from a specific IP
ip.addr == 192.168.1.10

# Only HTTP requests
http

# Only DNS queries
dns

# Combine conditions
http and ip.addr == 192.168.1.10
```

> Remember the order: capture, filter, inspect. The beginner mistake is trying to capture "exactly the right traffic." Instead, capture everything, then use filters to fish it out. The filter is your search box.

## Security uses

Both defenders and learners use Wireshark regularly:

* **Understand protocols**: turn the TCP, TLS, DNS ideas from `net-01` through `net-04` into visible packets.
* **Find suspicious connections**: bursts of outbound traffic, odd ports, unexpected flows all show up in the list.
* **Verify encryption**: confirm traffic really goes over HTTPS (content is encrypted) rather than in the clear.
* **Forensic analysis**: `blue-05-forensics` covers how network incident analysis often begins with packets.

For practice, **only capture traffic in your own VM environment** — the isolated subnet from `kali-02-install-lab` exists exactly for this. Do not capture on other people's networks.

## Next

You can now read packets. Next, pull the view up a level: `kali-06-metasploit` introduces the "framework" of penetration testing — a platform that modularizes vulnerability verification — and how to use it correctly in authorized settings.

#### Q: From thousands of packets, what is the most effective way to find HTTP traffic from one IP?

* Read every packet one by one

* Use a display filter such as http and ip.addr == 192.168.1.10

* Re-capture the traffic

* Open the capture file in a text editor

> 💡 Display filters are Wireshark's search box; capture everything, then filter by conditions — the right way to find the needle in the haystack.
