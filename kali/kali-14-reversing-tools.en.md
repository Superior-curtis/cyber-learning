# Reverse-Engineering Tools (Ghidra / radare2)

> 📅 2026-08-05 · Getting Started
> Without source code, how do you understand what a program does? Reverse engineering is that craft, and Ghidra and radare2 are its two most-used tools. This article conceptually introduces them and ties back to the Reverse/Pwn challenges in lab-08.

---

`lab-08-reverse-pwn-basics` introduced Reverse/Pwn conceptually. Now meet their hands: **reverse-engineering tools** — **Ghidra** and **radare2**, the two most-used in Kali for "understanding a program without source."

> Authorization reminder: reverse-analyzing your own programs, open-source software, or authorized targets is legitimate learning; reverse-engineering proprietary software to bypass licensing or make cracks violates laws and terms in many places. The target and the purpose decide legality.

## What reverse engineering does

Reverse engineering is the craft of **"understanding program logic without source code."** Typical tasks:

* See what a program does (encrypt? verify? communicate?).
* Find the key or flag the program hides.
* Understand which logic a vulnerability lives in — exactly the start of `lab-08-reverse-pwn-basics`.

It splits into **static analysis** (read the code and structure, without running) and **dynamic analysis** (run it, observe behavior).

## The two tools

| Tool | Feature | Best for |
|---|---|---|
| Ghidra | NSA's open-source graphical disassembler, with a GUI | Visual, beginner-friendly |
| radare2 | A command-line-oriented reverse-engineering framework | Scriptable, advanced |

**Ghidra's** biggest edge is **free + graphical**: import an executable and it auto-disassembles, draws the call flow, and labels functions — beginners can "see" what the program does.

**radare2** goes the opposite way: everything is driven by commands and scripts. The learning curve is steep, but it is **repeatable and automatable** — the tool for solving challenges and deep analysis.

> Beginner advice: Ghidra first, radare2 second. Build the "read a program" feel with the graphical tool first, then learn the precision and scriptability of the command-line one.

## Its relation to CTF

`lab-08-reverse-pwn-basics` mentioned that many Reverse challenges "pass with strings." When strings is not enough, bring out the heavy tools:

* **Find the main logic**: locate `main` in Ghidra and follow the calls to see how input is verified.
* **Recover the crypto**: see which algorithm the program uses and where the key hides.
* **Confirm dynamically**: run it under radare2 or a debugger and check whether the actual behavior matches your hypothesis.

## Learning boundaries

Reverse is a double-edged sword; while learning, keep two lines:

* ✅ Allowed: analyze open-source software, your own programs, CTF-provided binaries, public teaching material.
* ❌ Not allowed: reverse proprietary software to bypass licensing or crack it; use the skill on unauthorized targets.
* ⚠️ Remember: **"ability to analyze" is not "permission to analyze."** The line from `career-03-ethics-law` applies here too.

## Next

With the reversing tools covered, the common Kali tool families (scanning, web, password, packets, frameworks, forensics, OSINT, wireless, reversing) are all on the map. To practice, follow the route in `lab-08-reverse-pwn-basics`; to review, the tool map in `kali-03-tool-catalog` is your index.

#### Q: What is the main difference between Ghidra and radare2?

* One is free, one is paid

* Ghidra is a graphical disassembler (beginner-friendly); radare2 is a command-line framework (scriptable, advanced)

* One only analyzes Windows, one only Linux

* They are identical

> 💡 Ghidra is free and graphical, good for building feel; radare2 is command-line and scriptable, good for advanced and automated work.
