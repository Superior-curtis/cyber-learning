# VoIP Call Sniffing: Reassembling Audio with Wireshark

> 📅 2026-08-05 · Deep Dive
> Someone asks: using Wireshark to monitor calls and save recordings — is that social engineering, MITM, or sniffing? The answer is sniffing. This article clarifies the classification, the VoIP (SIP/RTP) logic, and why encrypted calls cannot be captured. Homelab only.

---

**Question**: "Using Wireshark to monitor calls and save recordings — is that social engineering, MITM, or sniffing?"

**Answer**: the core is **sniffing (packet sniffing).** This article clarifies the classification, breaks down the VoIP logic, and explains why encrypted calls cannot be captured.

> Security note (HOMELAB ONLY): capturing your own test calls (e.g., your own SIP server, your own two test machines) is practice; intercepting real or other people's calls is serious wiretapping in most places.

## Answering the classification first

Draw the boundary between the three clearly:

| Classification | What it does | This case |
|---|---|---|
| **Sniffing** | Passively capturing packets on the network | ✅ Primary — Wireshark captures SIP/RTP and reassembles it |
| **MITM** | Actively redirecting traffic to yourself (e.g., ARP spoofing) | ⚠️ Only needed to see someone else's traffic on a switched network |
| **Social engineering** | Tricking people into taking an action | ❌ Unless the lure was "getting the target to call on a network you can monitor" |

> One-line memory: monitoring = sniffing; actively steering traffic to you = MITM; tricking you into it = social engineering. Wireshark call monitoring is fundamentally sniffing — at most, a switched network needs an MITM technique to "steer" the traffic.

## The VoIP logic: why audio can be "reassembled"

A VoIP call has two parts (the UDP ideas from `net-01-network-fundamentals`):

* **SIP**: signaling — setting up and tearing down the call (who calls whom).
* **RTP**: the actual audio data — sent as a stream of UDP packets.

What Wireshark can do: reassemble the captured RTP packets, **in order**, into continuous audio, then play or save it — the `Telephony → VoIP Calls` feature.

**The premise is "unencrypted."** If the call uses SRTP (encrypted RTP), what you capture is gibberish — unless you have the key. And WhatsApp, iMessage, and normal cellular calls are all encrypted — **their audio cannot be captured.**

## Homelab practice (your own test calls only)

To practice "reading VoIP + reassembling audio," the correct move is to **run your own unencrypted VoIP setup**:

```bash
# Install a SIP server on your own homelab (test only)
sudo apt install asterisk

# Capture your own machine traffic in Wireshark, and make a test call between two test machines
wireshark
# → Telephony → VoIP Calls → pick the call → Play Streams
```

Using **your own server and your own test calls**, you can watch SIP and RTP with your own eyes and reassemble the audio — without touching anyone's real call.

> Security note (again): the target of this exercise is your own SIP server and your own test machines. Doing this to any real call is wiretapping — a crime that can land you in legal trouble.

## The blue-team defense

* **Encrypt all VoIP**: enable SRTP, so captured RTP is gibberish.
* **Network segmentation**: keep sensitive calls on a controlled subnet (`net-01` subnets).
* **Switched networks + port security**: reduce the chance of being "steered" (against the MITM component).
* **Monitoring**: abnormal volumes of RTP traffic, ARP-spoofing detection (`blue-02-logging-siem`).

## Next

The VoIP sniffing classification and logic are clear. To review packet-analysis basics, `kali-05-wireshark`; for the MITM side, `kali-19-responder-mitm` and `net-01-network-fundamentals` lay the foundation.

#### Q: What is the most correct classification for 'monitoring calls and saving recordings with Wireshark'?

* Social engineering

* Sniffing — passively capturing and reassembling VoIP packets; a switched network may additionally need MITM to see someone else traffic

* DoS attack

* Privilege escalation

> 💡 Monitoring is fundamentally sniffing; MITM appears only when active steering is needed; social engineering refers to the 'tricking people' layer.
