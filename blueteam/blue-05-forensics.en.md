# Digital Forensics Basics

> 📅 2026-08-05 · Deep Dive
> After an incident, the truth hides in the evidence. Digital forensics is the craft of systematically collecting, preserving, and analyzing digital evidence. This article covers the forensics flow, what to examine, and why the chain of custody matters.

---

`blue-04-incident-response` said to contain first, understand later. And the raw material for "understanding" is **evidence.** Digital forensics is the craft of **systematically collecting, preserving, and analyzing digital evidence** — the conclusions of response must stand on evidence, not guesses.

You already played with forensics tools in `kali-11-forensics-tools`. This article places them inside the blue-team methodology.

## What digital forensics does

The goal of digital forensics in one line: **reconstruct "what happened" from digital traces, with evidence that holds up to scrutiny.** It has two main scenarios:

* **Incident response**: after a breach, find out how the attacker got in, what they touched, and what they took.
* **Investigation**: support legal or internal investigations, with conclusions that could stand in court.

Both share one bottom line: **evidence must not be contaminated.**

## Collect & preserve: the chain of custody

The most overlooked — and most important — part of forensics is **preservation.** Once evidence is altered, the conclusion becomes unreliable. Basic principles:

| Principle | How |
|---|---|
| Photograph/document the state first | Record the system state before touching it |
| Copy read-only | Analyze the "copy," never the "original" |
| Record every step | Who, when, what — forming a chain of custody |
| Store safely | Seal the original against tampering |

> Remember the forensics rule: protect the evidence before analyzing it. See a suspicious machine? Do not rush to poke around on it — every operation can contaminate evidence. The correct move is to copy it and analyze the copy.

## What to examine

Forensics analysis splits into three main objects:

| Object | What to look for |
|---|---|
| Disk | Deleted files, hidden data, suspicious programs (Autopsy from `kali-11`) |
| Memory | Running programs, keys, suspicious processes |
| Network | Connection records, transferred content (Wireshark from `kali-05`) |

In incident response, **the usual priority is memory > disk > network** — what is in memory is most "current" and disappears fastest.

## Tools

The four tools from `kali-11-forensics-tools` are a great start: strings for text, exiftool for metadata, binwalk for embedded files, Autopsy for full disk analysis. Large investigations use more professional platforms, but the concepts are the same.

## From blue team to practice

Digital forensics is not only a blue-team thing — **CTF's Forensics category (`lab-05-forensics-challenges`) is its miniature version**: hand you a file, dig the hidden clue out. The "find the clue" skill you train there uses the same muscles as blue-team forensics.

## Next

Evidence and truth are clear. Next, return to the most human side of defense: `blue-06-phishing-defense` introduces phishing and social-engineering defense — why "people" are the most-broken link, and how to defend.

#### Q: What is the most important first step in digital forensics?

* Start searching on the suspicious machine immediately

* Protect the evidence: document the state, copy read-only, analyze on the copy

* Format the hard drive

* Format the disk right away

> 💡 Once evidence is contaminated, conclusions become unreliable; the right order is protect first, analyze second, working on a copy.
