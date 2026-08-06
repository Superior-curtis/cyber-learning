# Forensics Challenges, Introduced

> 📅 2026-08-05 · Getting Started
> Forensics is CTF's most hands-on category: you get a file or a disk image with a flag hidden inside. This article teaches the opening set — file, strings, exiftool, binwalk — to go from 'got a file' to 'dug out the flag.'

---

`kali-11-forensics-tools` introduced the four forensics tools. Now put them to work: **the Forensics category** — CTF's most hands-on type.

The rule is simple: the organizer hands you a file (image, document, disk image…), and the flag is hidden inside. Your job is to dig it out with tools. This article gives you an opening set you can use immediately.

## What forensics challenges do

Forensics challenges are, at heart, **finding clues inside files.** The flag may hide in:

* A piece of text tucked at the end of a file.
* The metadata of an image or document.
* A file "containing another file."
* Deleted or hidden files.

The matching tools are the four from `kali-11-forensics-tools` — and the opening sequence is nearly always the same.

## Opening set: four things first

```bash
# 1. First see what this file really is (do not trust the extension)
file mystery.bin

# 2. Pull out all readable strings
strings mystery.bin | less

# 3. Read the metadata
exiftool mystery.bin

# 4. Check whether another file is embedded
binwalk mystery.bin
```

> The forensics rule: let the tools speak before you guess. After these four commands, the "direction" of most challenges floats up — what remains is usually following the lead deeper.

## Common exam points

| Point | What it looks like | Solution |
|---|---|---|
| Hidden strings | The flag sits right in the file | `strings` |
| Hidden files | One file embedded in another | `binwalk` unpack, then process |
| Metadata | Clues in author/comment/GPS | `exiftool` |
| Disk images | A whole disk to analyze | Autopsy or mounting the image |
| Stego | Message hidden in image pixels/audio | Category-specific tools (stegsolve etc.) |

"Stego (steganography)" often sits under Forensics — messages hidden in the details of an image or sound, invisible to the eye, recoverable only with specific tools.

## Practice mindset

* **Follow the flow**: `file` → `strings` → `exiftool` → `binwalk` every time; it makes you much faster.
* **Do not trust the extension**: a `.png` can be disguised; `file` is the truth.
* **Remember the flag format**: `lab-03-ctf-101` said — finding "text that matches the flag format" is often the answer.

> An honest reminder: when stuck, first ask "did I skip the simplest step?" Many forensics flags sit right in the strings output — you were just too busy hunting for the complicated solution to look.

## Next

Digging clues out of files is practiced. Next, a completely different brain exercise: `lab-06-osint-challenges` introduces the OSINT category — touching no system, relying only on public information and reasoning to piece the answer together.

#### Q: Given a file in a Forensics challenge, what is the standard opening?

* Open it as a text file directly

* Use file to confirm the type, then check strings / exiftool / binwalk in turn

* Re-download the challenge

* Guess an answer first

> 💡 file reveals the truth, strings pulls strings, exiftool reads metadata, binwalk unpacks embedded files — the standard forensics opening four.
