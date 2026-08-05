# How the Web Works: HTTP, DNS, IP, and Ports

> 📅 2026-08-05 · Core Concepts
> What happens between typing a URL and seeing a page? DNS turns a domain into an IP, TCP/IP builds the connection, ports pick the service, and HTTP exchanges the messages. This is the foundation for everything else.

---

You open a browser, type a URL, and a page appears in about a second. But in that second your computer runs a relay race: it asks for directions (DNS), opens a call (TCP/IP), finds the right door (Port), and finally trades data in a shared language (HTTP). Nearly every attack and defense you will meet in security writing is built on these layers.

Think of this article as the prerequisite course for the whole book. Whether you later read `net-02-tls-https`, `recon-03-scanning-nmap`, or `web-02-injection`, you will need this first. Without this layer, a lot of security discussion sounds like a foreign language.

## The journey of one click

See the big picture first, then we pull it apart. After you type a URL and press Enter, roughly this happens:

```text
browser types a URL
↓
ask DNS: what IP is example.com?
↓
get back 93.184.216.34
↓
open a TCP connection to port 443 (TLS encrypted)
↓
send HTTP GET /index.html
↓
receive 200 OK + HTML, browser starts rendering
```

Each step can be broken into finer mechanics. This article explains every leg of the relay.

## DNS: translating names into addresses

Humans remember domain names; the network only understands IPs. DNS (the Domain Name System) is the internet's phone book: it translates `example.com` into an address like `93.184.216.34`.

The lookup is not a single trip — it goes up the chain:

#### The browser checks its cache

Did we look this up recently? If so, use it. Fastest path.

#### It asks a recursive resolver

Usually your ISP or a public DNS like 8.8.8.8, which runs the rest of the journey for you.

#### Root servers and top-level domains

Root servers point to the .com servers, which point to the authoritative server that owns example.com.

#### The authoritative answer

The authoritative server returns the real IP, and the resolver caches it for a while.

> DNS is the invisible foundation of the entire internet — and one of the most abused targets. Later, blue-02-logging-siem and blue-03-threat-intel show how DNS gets misused and how defenders detect it. For now, remember: DNS does the name-to-address translation.

## IP and TCP: the address and the delivery guarantee

Once you have the IP, your computer has to set up a connection with that server. Two layers each do their own job:

* **IP (Internet Protocol)** handles addressing and forwarding. Every connected device has an IP, like a mailing address. IPv4 looks like `93.184.216.34`; the world is gradually moving to the longer IPv6 as addresses run out.
* **TCP (Transmission Control Protocol)** guarantees reliable delivery. It chops data into numbered segments, sends them, and waits for the other side to confirm; anything missing gets resent. Web content cannot afford to be lost, so the web runs on TCP rather than on UDP, which offers no delivery guarantee.

| Trait | IP | TCP |
|---|---|---|
| Role | Find the device | Make sure data arrives intact |
| Analogy | The address on the envelope | The receipt on registered mail |
| Handles | Addressing, routing | Splitting, numbering, resending |
| Used by | Every network | HTTP, SSH, email |

`net-01-network-fundamentals` goes deeper into this layer, including how packets hop between routers.

## Ports: the same address, different doors

One server runs many services at once: a website, SSH, email. IP finds the building, but you still need to know the room. A port is the door number.

The ports worth memorizing show up in security writing constantly:

| Port | Service | Encrypted? | Why security writing mentions it |
|---|---|---|---|
| 22 | SSH remote admin | Yes | The backdoor key to a server; scanning and brute-force hotspot |
| 53 | DNS queries | Special | The whole world looks up names through it; often abused |
| 80 | HTTP websites | No | Being replaced by 443, but old sites still use it |
| 443 | HTTPS websites | Yes | The default entrance to the modern web; TLS's stage |

> recon-03-scanning-nmap will teach you to scan for open doors. That ability belongs only on your own systems, CTF labs, and authorized testing. Scanning other people's computers without permission is illegal in many countries. This book stays defensive and educational.

## HTTP: the web's conversation language

With the connection up, the browser and server talk HTTP. It is a request/response exchange: the browser sends a request, the server sends a response.

A request looks roughly like this:

```http
GET /index.html HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Cookie: session=abc123
```

* **The first line**: method (GET), path (/index.html), version (HTTP/1.1).
* **Headers**: lines of "field: value". `Host` tells the server which site you want; `Cookie` carries your login state.
* **Body**: usually absent for GET; present when POSTing a form.

A response looks like this:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: session=xyz789; HttpOnly

<html>...</html>
```

Status codes report the outcome as numbers. In security writing, know these well:

| Code | Meaning | Security relevance |
|---|---|---|
| 200 OK | Success | All normal |
| 301/302 | Redirect | Can be used for phishing or open-redirect attacks |
| 401/403 | Not logged in / denied | Direct signal from access control |
| 404 | Not found | During directory brute-forcing, differing responses leak clues |
| 500 | Server error | Often accompanies injection attacks that crash queries |

## Cookies and sessions: how the server remembers you

HTTP itself is stateless: every request is an independent event and the server does not remember who you are. Cookies exist to solve that.

* The server sets `Set-Cookie` in a response, and the browser automatically sends it back with every later request.
* The most common use is a Session ID: a random value that means "you are logged in."
* Security details matter: `HttpOnly` stops JavaScript from reading it, `Secure` sends it only over HTTPS, and `SameSite` blocks cross-site requests.

`web-03-auth-session` covers logins, sessions, cookies, and the associated attacks in one place; `web-01-owasp-top10` also ranks session management among the most important issues. For now, hold onto one sentence: **a cookie is the credential the server uses to recognize you — stolen, and you are impersonated.**

## HTTPS: turning plaintext into ciphertext

Everything described above in HTTP is plaintext: any router or malicious WiFi in between can read it. HTTPS wraps HTTP in a layer of TLS encryption so the content becomes ciphertext.

* The URL changes from http:// to https:// and the browser shows a padlock.
* Encryption provides both confidentiality (others cannot read it) and integrity (the content was not tampered with).
* The server uses a digital certificate to prove it really is example.com — that is where trust comes from.

HTTPS is the pillar of modern web security and deserves a whole article: `net-02-tls-https` is the next stop in this series, going deep into the TLS handshake, certificates, and common attacks.

## Where this series goes next

You now have the full foundation: DNS finds the address, TCP/IP builds the connection, ports pick the door, HTTP exchanges the messages, and HTTPS protects them. These concepts will follow you through every article in this book.

The most valuable next step is the secure connection itself — `net-02-tls-https` explains exactly how HTTPS protects your data.

#### Q: After you type a URL in the browser, what is DNS responsible for?

* Splitting the data into packets for delivery

* Translating the domain name into an IP address

* Encrypting the messages between browser and server

* Deciding which status code the page should return

> 💡 DNS is the internet phone book: it translates a human-readable domain like example.com into an IP address like 93.184.216.34, so the connection has a destination.
