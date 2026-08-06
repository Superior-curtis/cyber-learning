# Burp Suite: Web Testing

> 📅 2026-08-05 · Getting Started
> Want to see exactly what requests a web app really sends? Burp Suite is the core web-testing tool — it intercepts, inspects, and modifies every request between browser and server. This article introduces it, and where it belongs.

---

In `web-01` through `web-05` you learned the web risks. But you have surely wondered: **what do these risks look like in a real request?** The standard web-testing tool, **Burp Suite**, exists exactly for this — it lets you "intercept" every request between browser and server, see it with your own eyes, and modify it by hand.

> Authorization reminder: Burp is for testing your own web apps, or ones you are authorized to test. Point your practice environment at the virtual machines from kali-02-install-lab (like DVWA), and you will never touch someone else's system by accident.

## What Burp is: an intercepting proxy

At its core, Burp is an **intercepting proxy**. It sits between browser and server: the browser connects to Burp first, and Burp forwards to the server. That means **every request and response passes under Burp's eyes**, and you can stop, inspect, modify, and release them.

```
Browser ──→ Burp (intercept, inspect, modify) ──→ Server
```

For a tester this is a huge edge: you do not guess what the site sends — you look at it. For a defender, understanding Burp means understanding how a tester sees your site.

## Core features

| Feature | What it does | Your use case |
|---|---|---|
| Proxy | Intercept and inspect requests/responses | See what the app actually transmits |
| Repeater | Resend a request, modified | Manually verify one input point |
| Intruder | Automate many variations | Dictionaries, parameters, brute force |
| Scanner | Auto-scan common flaws | Quick inventory (paid tier) |

Beginners should master two things first: **use Proxy to read a request, and use Repeater to modify one.** That alone turns the injection ideas of `web-02-injection` into a visible, hands-on experiment.

## How to use it: one flow

Walk through "reading one request":

#### Set up the proxy

Point your browser at Burp (usually 127.0.0.1:8080) and install Burp certificate so HTTPS can be intercepted too.

#### Turn on interception

With Intercept on, requests sent by the browser pause in Burp for you to inspect.

#### Inspect and modify

Look at headers, parameters, cookies — this is what the site really receives. You can edit parameters and release.

#### Experiment with Repeater

Send a request to Repeater, change one value, watch the response, and confirm your hypothesis.

This "grab request → see structure → change parameter → watch response" loop is the basic skill of web testing. The secure-testing described in `web-05-securing-web-apps` is, in practice, exactly this — verified case by case.

## What to practice against

Burp should only ever point at sites you have permission to touch. The friendliest starting target is **DVWA** — `lab-02-vulnerable-targets` shows how to stand it up in a virtual machine. It is deliberately weak, so you can run every Burp feature in a fully legal, fully isolated environment.

## Next

You can now "read web requests." Next, go one layer deeper: `kali-05-wireshark` takes you below the application layer to the packets that actually crossed the network — analyzing traffic with Wireshark and finding the problems hidden in the details.

#### Q: What is Burp Suite's core role?

* An antivirus suite

* An intercepting proxy: intercept, inspect, and modify requests between browser and server

* A password cracking tool

* An operating system

> 💡 Burp is an intercepting proxy that sits between browser and server, letting you see and modify every request.
