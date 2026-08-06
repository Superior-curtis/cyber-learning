# Forensics Tools (strings / exiftool / binwalk / Autopsy)

> 📅 2026-08-05 · Getting Started
> Digital forensics is reconstructing the truth from what was left behind. strings, exiftool, binwalk, and Autopsy are the four most-used forensics tools. This article covers what each digs up, and paves the way for the forensics challenges in labs.

---

`blue-05-forensics` will systematically cover digital forensics. This article starts on the tool side: Kali ships four of the most-used forensics tools — **strings, exiftool, binwalk, and Autopsy** — and each digs up a different kind of clue. Know the tools first, and the forensics challenges in `lab-05-forensics-challenges` will feel natural.

## What digital forensics does

The goal of digital forensics fits in one line: **reconstruct "what happened" from the traces left behind.** Whether investigating an incident (`blue-04-incident-response`) or solving a CTF forensics challenge, the core is the same — asking "what clues does this file, this image, this memory hide?"

## strings: pulling readable text out of a file

`strings` extracts the readable text fragments from a binary file. Images, programs, and archives often hide strings invisible to the naked eye:

```
# Pull all readable strings out of a file
strings suspicious.png | less
```

In CTF forensics, this is often the first move: **the flag may just be a piece of text tucked into the end of an image or file.** And `lab-03-ctf-101` teaches you what a flag looks like.

## exiftool: reading metadata

`exiftool` reads a file's **metadata** — capture time, GPS coordinates, camera model, author, comments, and more:

```
# Read the metadata of an image
exiftool photo.jpg
```

Metadata is a treasure trove for forensics: an "ordinary-looking photo" can hide the location or original author — key clues. OSINT (`recon-01-recon-osint`) uses it often too.

## binwalk: unpacking embedded files

`binwalk` analyzes **files that may be embedded inside other files** — a compressed archive hidden inside an image, or a firmware image with multiple parts:

```
# Analyze what is embedded in a file
binwalk firmware.bin
```

It pairs well with `strings`: `strings` finds "something looks interesting," and `binwalk` unpacks the hidden content.

## Autopsy: the full forensics platform

The first three are point tools; **Autopsy** is a full forensics platform with a GUI that systematically analyzes disk images, file systems, deleted files, browser history, and more:

| Tool | Excels at | One-line positioning |
|---|---|---|
| strings | Pulling readable strings | Find hidden text |
| exiftool | Reading metadata | Find a file's ID card |
| binwalk | Unpacking embedded files | Dig out hidden archives |
| Autopsy | Whole-disk forensics | Full case-analysis platform |

> Forensic thinking order: first ask 'what file is this', then 'what is inside it.' strings is the opener, exiftool adds background, binwalk digs deep, Autopsy handles large cases.

## Practice: CTF forensics challenges

Kali's forensics tools find their best practice ground in **CTF forensics challenges** — `lab-05-forensics-challenges` shows you how to solve them systematically. The principle is simple: **when handed a file, do not guess — let the tools speak.** Run strings, exiftool, binwalk in turn, and the answer tends to float up.

## Next

The tool side is laid out. Finally, the OSINT piece: `kali-12-osint-tools` introduces public-information gathering tools — theHarvester, recon-ng, and Maltego — turning the methods of `recon-01` and `recon-02` into a semi-automated workflow.

#### Q: Given a suspicious file in a CTF forensics challenge, which tool is most often used first?

* Autopsy

* strings — pull the readable text clues out of the file first

* Metasploit

* hashcat

> 💡 strings is the fastest opener: it extracts readable strings from a binary, and the flag is often among them.
