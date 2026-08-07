# hping & DoS/DDoS: Commands, Logic, and the Blue-Team Defense

> 📅 2026-08-05 · Deep Dive
> hping3 is a packet-crafting tool and the classic way to test DoS resilience. This article explains flood logic, gives homelab-only stress-test commands, and covers the blue-team detection and mitigation in equal depth.

---

DoS/DDoS is an "availability attack" — the goal is not stealing data but **making a service unusable.** `hping3` is the classic Kali tool for this kind of "resilience testing." This article explains the logic behind it, gives **homelab-only** test commands, and puts the blue-team defense on equal footing.

> Security note (HOMELAB ONLY): every command on this page is permitted only against your own homelab server, in an isolated environment. Launching DoS/DDoS against any real target is criminal and highly destructive, carrying serious criminal liability in most jurisdictions. This is a defense lesson.

## What DoS/DDoS is

* **DoS (Denial of Service)**: a single source makes the target service unusable.
* **DDoS (Distributed DoS)**: many sources strike together, far harder to absorb.

It knocks out the "Availability" pillar of `found-02-cia`: the service is still there, but **legitimate users cannot get in.**

## Why it works: the resource-exhaustion logic

Every service has finite resources — connection-state table, bandwidth, CPU. **The logic of a flood: send more than it can handle, so it exhausts itself processing junk and has no capacity left for real users.**

Two main attack lines:

* **Exhaust state**: fill the server's "connection-state table" so new legitimate connections cannot get in.
* **Exhaust bandwidth**: flood the pipe so all traffic is stuck in the queue.

## Common techniques

| Technique | Exhausts | One line |
|---|---|---|
| SYN flood | Connection-state table | Massive half-open connections fill the table |
| UDP flood | Bandwidth/processing | A storm of UDP packets |
| ICMP flood | Bandwidth | A storm of pings |
| Slowloris | Connection count | Slowly open and hold many connections |
| Amplification | Bandwidth | Reflection/amplification (DNS/NTP) |

## The logic of a SYN flood (TCP handshake)

SYN flood is the classic, built on the TCP three-way handshake from `net-01-network-fundamentals`:

1. Client sends `SYN`; the server replies `SYN-ACK` and **holds a "half-open connection"** waiting for the final `ACK`.
2. A flood sends many `SYN`s but **never completes the handshake** → half-open connections pile up.
3. The state table fills → real users cannot even get a `SYN` in.

**Key: the server "trusts every SYN will complete," which is exactly what exhausts it.** The mitigation logic therefore follows directly — see "Blue-team defense."

## hping3: the packet-crafting tool

`hping3` can build and send **arbitrary packets** — which makes it a powerful network-testing tool in legitimate hands (testing firewalls, TCP stacks) and a flood generator in the wrong ones. Install is simple:

```bash
# Install
sudo apt install hping3
```

> hping3's power comes from "customizable + massable." It lets you tune every field of a packet and (in authorized settings) send it in volume. Understand those two words — "customizable" and "massable" — and you understand both the flood and where the mitigation applies.

## Homelab stress-test commands (your own server only)

Start with the **most harmless** thing: send **one** test packet to your own localhost and observe the response.

```bash
# Send one SYN packet to your own localhost and observe the TCP reply (harmless)
hping3 -S -p 80 127.0.0.1
```

To verify **your own server's** resilience to a SYN flood, do it only on an isolated homelab network, against **your own** server IP:

```bash
# HOMELAB ONLY — only your own server IP, from a separate test machine
hping3 -S --flood -p 80 192.168.1.10
```

> Security note (again): --flood sends a huge volume of packets very fast. Only against your own homelab server, only on an isolated subnet. Running this against anything that is not yours is launching a DoS attack — illegal and likely to bring you criminal trouble. After the test, immediately inspect your own logs and connection table to verify the mitigation works.

## The blue-team defense

Defense splits into "detect" and "mitigate":

**Detection (the baseline mindset from `blue-02-logging-siem`):**

```bash
# Count half-open (SYN-RECV) connections — a spike is the SYN-flood tell
ss -tan state syn-recv | wc -l

# Capture packets that have SYN but no ACK
tcpdump -i eth0 "tcp[tcpflags] & tcp-syn != 0 and tcp[tcpflags] & tcp-ack == 0"
```

**Mitigation:**

| Measure | How |
|---|---|
| SYN cookies | Do not hold state; answer by computation instead |
| Rate limiting | Cap the connection-establishment rate per source |
| Firewall | Block abnormal sources and protocols (`net-04-firewalls-vpn-proxy`) |
| CDN / DDoS protection | Divert and scrub under heavy traffic |
| Redundant architecture | Spread load across nodes so one target does not die alone |

> Defense mindset: the attack pressures "resources and rates," so the defense limits "resources and rates." First get a baseline (blue-02-logging-siem) so you know what abnormal looks like; then add mitigation so you can survive when abnormal arrives.

## Security note

* ✅ Allowed: resilience-testing **your own homelab**, against **your own server**, observing and verifying the defense.
* ❌ Not allowed: launching DoS/DDoS against any real or third-party target — whatever the "pure" motive.
* ⚠️ Remember: DoS is a destructive attack; unauthorized launch is a serious crime in most places (`career-03-ethics-law`).

## Next

The DoS logic and defense are clear. To see how anomalies get discovered, `blue-02-logging-siem` teaches baselines and detection; to harden the server itself, `blue-01-hardening` is the best start.

#### Q: What is the key reason a SYN flood can take a server down?

* The server compute is too weak

* The server trusts every SYN will complete the handshake, so it holds many half-open connections until the state table fills

* The server has no firewall

* SYN packets are too large

> 💡 The server holds a half-open connection state for every SYN while awaiting ACK; a flood sends many SYNs without completing, and once the table fills, legitimate connections can no longer get in.
