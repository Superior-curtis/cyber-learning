# How CTF Works (Flag Formats, Categories, Ethics)

> 📅 2026-08-05 · Getting Started
> CTF (Capture The Flag) is one of the best ways to practice security: each challenge is a small puzzle, and solving it earns a 'flag.' This article introduces how CTF works, the common categories, and above all the ethics.

---

Your lab is built and the targets are up. But "practicing" needs a method — and one of the best-recognized methods in the security world is **CTF (Capture The Flag).**

This article introduces how CTF works: what a flag is, what categories exist, and above all the ethics.

## What CTF is

CTF is a "puzzle competition": organizers publish a batch of challenges, each a small puzzle. Your goal is to solve it and extract the **flag** — a specific piece of text hidden inside. Submit the flag and you score.

CTF's magic: **it turns "attacking" into a controlled game.** The challenges are designed, the scope is fixed, and solving them is completely legal. You can try, freely, the techniques that would be strictly forbidden in the real world — because the target exists exactly for that.

## Flags and formats

Every CTF has its own flag format, usually the organizer's name plus braces:

```
# Common examples
flag{this_is_a_flag}
CTF{you_found_me}
```

> The first step of any challenge is recognizing the flag. In many puzzles, the clue is simply "find text matching the flag format" — which is exactly what kali-11-forensics-tools's strings is for.

## Main categories

CTF challenges usually split into several families:

| Category | What it does | In this book |
|---|---|---|
| Crypto | Break encodings or encryption | `lab-04-crypto-challenges` |
| Forensics | Find clues in files/disks | `lab-05-forensics-challenges`, `kali-11` |
| OSINT | Find answers in public info | `lab-06-osint-challenges`, `recon-01` |
| Web | Exploit web flaws | `lab-07-web-challenges`, `web-01..05` |
| Pwn / Reverse | Analyze programs and binaries | `lab-08-reverse-pwn-basics` |
| Stego | Messages hidden in images/audio | Often under Forensics |

A suggested route for beginners: **Crypto → Forensics → OSINT → Web**, saving Pwn/Reverse for later. The first few lean on puzzle-solving thinking; the latter need more programming depth.

## Ethics: authorized environments only

CTF has a premise that must be stated plainly: **it is a simulation, not a license.**

* ✅ Allowed: solve the challenges and targets the organizers provide.
* ❌ Not allowed: apply CTF techniques to real systems you have no authorization to touch.
* ⚠️ Always remember: **"I know the technique" and "I have permission to use it" are two different things.** CTF teaches you how; authorization decides whether.

> This is one of the most important lines in this book. CTF is a controlled training ground, not a training camp for real targets. Using CTF skills on unauthorized systems is a crime — and it has nothing to do with whether you can solve CTF puzzles.

## Next

How CTF works and its ethics are clear. Next, start the suggested route: `lab-04-crypto-challenges` introduces the Crypto category — encodings, encryption, and those opening moves you recognize on sight.

#### Q: CTF lets us freely try attack techniques — under what premise?

* There is no premise at all

* Everything happens within the controlled challenges and targets the organizers provide

* As long as nobody finds out

* As long as the goal is learning, you may use them on any system

> 💡 CTF is a controlled simulation: fixed scope, designed targets. Applying those skills to real, unauthorized systems is a crime.
