# OSINT Challenges, Introduced

> 📅 2026-08-05 · Getting Started
> OSINT challenges touch no system — they rely only on public information and reasoning: find the place, the person, the connection. This article introduces how OSINT challenges play, the opening moves, and the 'public data only' line.

---

`recon-01-recon-osint` taught you that OSINT is "assembling a picture from public information." CTF's **OSINT category** turns that into a game: **touch no system; rely only on public information and reasoning to piece together the answer.**

This article introduces how OSINT challenges play and their opening moves — and once more draws the line clearly.

## What OSINT challenges do

An OSINT challenge hands you a "thread" and asks you to follow it to an answer. Common forms:

* **Find the place**: this photo — where was it taken?
* **Find the person**: an account, a piece of text — who is behind it?
* **Find the connection**: two seemingly unrelated things — how are they linked?
* **Find the history**: what does a site or person's past hide?

The core skill is one: **know how to ask questions and use public tools to answer them.**

## Opening moves

| Tool / method | Use |
|---|---|
| Reverse image search | Find the source and location of a photo |
| Advanced search syntax | The `site:` / `filetype:` from `recon-02-dorking` |
| Maps & street view | Locate from photo details (signs, terrain) |
| whois / DNS | Look up info behind a domain |
| Social & timezones | Clues from post times, geotags |

> OSINT mindset: break the question down. "Where was this photo taken" is too broad. Ask first — what do the signs say, what is the weather, what terrain is the ground — and break the big question into a chain of small ones, conquering each.

## Common exam points

* **Photo geolocation**: sign language, street style, license-plate style, landmarks — all locating clues.
* **Person identity**: assemble someone's picture from public accounts, post content, shared tags.
* **Time and events**: reconstruct a story from a "when, where, with whom" timeline.

What these share: **public information is more plentiful than you think** — "most useful information is already public" from `recon-01-recon-osint` becomes the game rule, amplified.

## Ethics reminder: public data only

The easiest line to cross in OSINT is slipping from public into intrusive. Remember:

* ✅ Allowed: search public pages, public accounts, whois, maps.
* ❌ Not allowed: try to log in, bypass permissions, access private content, run harassing investigations on real people.
* ⚠️ Remember: **challenges are designed to be played; real people are not.** The lines from `recon-01-recon-osint` and `kali-12-osint-tools` hold outside CTF too.

> The target of a CTF OSINT challenge is the answer, not the person. Reason freely while solving; but applying the same methods to real people and invading their privacy is an entirely different matter.

## Next

The OSINT reasoning is practiced. Next, return to the field where you already have tool feel: `lab-07-web-challenges` introduces the Web category — turning the knowledge from `web-01` through `web-05` into actual solving on target machines.

#### Q: What is the biggest difference between a CTF OSINT challenge and real-world OSINT investigation?

* CTF targets a designed "answer"; real investigation faces real people and their privacy

* CTF allows any tool, real-world does not

* They are exactly the same

* CTF needs no reasoning

> 💡 CTF OSINT is a controlled game aimed at an answer; applying the same methods to real people and invading privacy is a different matter.
