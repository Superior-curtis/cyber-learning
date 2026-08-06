# Web Challenges, Introduced

> 📅 2026-08-05 · Getting Started
> Web is CTF's most frequently set category — and the field where you already have tool feel. This article introduces how web challenges play: opening moves, common exam points, and turning web-01 through web-05 knowledge into actual solving.

---

Web is CTF's most frequently set category, and one of the most covered in this book — `web-01` through `web-05`, `kali-04`, `kali-07`, and `kali-09` all paved the way for it. Time to use them.

> Authorization reminder: the "targets" of web challenges are the organizers' controlled sites. Solving techniques belong to those challenges alone; using them on real sites you have no authorization for is illegal.

## What web challenges do

A web challenge hands you a **deliberately flawed site**, and you use its flaws to find the flag. It is not about "breaking the whole site" — usually **one challenge equals one vulnerability**:

* Is this one testing SQL injection? Then some input point is the key.
* Testing permissions? Then a page "nobody was supposed to see" is the entry.
* Testing cookies? Then identity management has a flaw.

Challenges usually only ask you to find that flaw and use it to grab the flag — small scope, clear goal.

## Opening moves

| Move | With what |
|---|---|
| Look at the page and code | Browser DevTools |
| Look at requests and responses | Burp Suite (`kali-04-burp-suite`) |
| Find hidden paths | gobuster / ffuf (`kali-07-enumeration-tools`) |
| Test injection | Manual + sqlmap (`kali-09-sqlmap`) |

> The first step of any web challenge is recon: read the source, read the cookies, read the request structure, then choose your angle. The spirit of recon-01-recon-osint applies here — look first, then act.

## Common exam points

| Point | Clue | Related knowledge |
|---|---|---|
| Parameter tampering | Changing URL/form params gives different results | `web-02-injection` |
| Cookie tampering | Changing a cookie changes identity | `web-03-auth-session` |
| Access bypass | Paths that "should be blocked" are reachable | `web-01-owasp-top10` A01 |
| Hidden features | Entrances hidden in code or directories | `kali-07-enumeration-tools` |
| SSRF/CSRF | Requests are being manipulated | `web-04-ssrf-csrf-upload` |

## Practice mindset

* **Read the source first**: many web challenges drop their hint in an HTML comment or JS — "view source" is always the cheapest, highest-payoff first move.
* **Change one variable at a time**: change a parameter, read the response, change again — the Repeater loop from `kali-04-burp-suite`, finding the pattern in small steps.
* **Cross-reference the knowledge**: when stuck, ask "which `web-02` idea does this look like?" — challenges are designed to map to a chapter.

## Next

You have web feel. Next, the hardest and most fascinating pair: `lab-08-reverse-pwn-basics` gives you a **conceptual** introduction to Reverse and Pwn — without diving into the binary deep end, understanding what they do and what background they need.

#### Q: What is the first step in almost every web challenge?

* Brute force directly

* Recon first: read the source, cookies, and request structure, then choose the angle

* Reload the page until it works

* Close the browser and use another tool

> 💡 The web challenge opener is recon — source, cookies, and request structure are all clues; see clearly first, then pick the angle.
