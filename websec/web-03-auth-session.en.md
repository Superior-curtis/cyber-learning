# Authentication & Session Security

> 📅 2026-08-05 · Core Concepts
> How does a site know you are you? Passwords are only half the story — sessions and cookies are the other half. This article unpacks authentication and session mechanics, common attacks, and how to design so your identity does not get stolen.

---

You log into a site, close the tab, and reopen it the next day — it still knows who you are. That is not magic; it is the work of a **session**. But if a session gets stolen, an attacker can "pretend to be you."

`web-01` and `web-02` gave you the map and the injection defenses. This article answers a more personal question: **how does a site confirm identity, and how does it keep that identity from being impersonated?**

## Authentication vs authorization

First, separate two easily-confused ideas:

* **Authentication**: who are you? — logging in, proving identity.
* **Authorization**: what can you do? — after identity is proven, whether you are allowed to touch something.

Even perfect authentication fails if authorization is sloppy — that is OWASP A01 "Broken Access Control", the same line as `web-01-owasp-top10`.

## The password is only half

A password proves what you *know*. But relying on passwords alone has a big problem: **if a password leaks (phishing, database breach), the attacker logs straight in.**

Modern authentication therefore stresses multiple factors — at least two different kinds of proof:

| Factor type | Example |
|---|---|
| Something you know | Password, PIN |
| Something you have | Phone, security key, one-time code |
| Something you are | Fingerprint, face |

`pass-04-defenses` covers this thoroughly. Keep the conclusion here: **add MFA whenever you can, especially for admin accounts.**

## Sessions and cookies: remembering who you are

Here is the problem: HTTP is "stateless" — the server does not automatically remember the previous request. For a site to "remember you," it needs a **credential that identifies you.**

The common approach: after a successful login, the server issues a **session ID** stored in a **cookie**. Every later request carries it, and the server knows "this is the person who just logged in."

```
# Concept: the session cookie the server hands the browser after login
Set-Cookie: session=abc123; HttpOnly; Secure
```

The key point: **that little ID is the key to your identity.** Whoever holds it can impersonate you.

> Think of the session ID as a hotel key card: logging in issues the card, and you swipe it to enter. If the card is lost, whoever has it can enter your room. So the card (session) must be hard to copy, must expire, and must not travel where it is easy to steal.

## Common attacks

* **Session theft**: stealing the cookie via XSS (covered in `web-02-injection`) or malware on the device. Getting the session is getting the identity.
* **Password brute force**: attackers try many common passwords. Defenses are attempt limits and MFA.
* **Replay**: intercepting someone else's request and resending it later. Defenses are timestamps, nonces, and HTTPS.
* **Session fixation**: an attacker obtains a session ID first, tricks the user into logging in with it, then shares the identity. The defense is rotating the session ID after login.

## Secure design checklist

Turn the risks above into a short design checklist:

| Principle | How |
|---|---|
| HttpOnly cookie | JavaScript cannot read it, lowering the XSS-theft risk |
| Secure cookie | Sent over HTTPS only |
| Session timeout | Auto-logout after inactivity |
| Rotate session ID on login | Blocks session fixation |
| Attempt limits + MFA | Blocks brute force |
| HTTPS end to end | Blocks interception and replay |

> One of the most common mistakes: putting the user's identity in a cookie for the user to carry. Storing the username or role directly in a cookie is handing the key to the user to edit. Always use server-managed sessions, never client-tamperable data.

## Next

Identity and sessions are solid. Next, several interaction classes that often break: `web-04-ssrf-csrf-upload` unpacks SSRF, CSRF, and file upload — classic risks at the request and data layer.

#### Q: Why should you not store a user's identity in a cookie the user can edit?

* Cookies can only hold very short content

* That is handing the key to the user to change, so identity can be forged

* Browsers do not support cookies

* Cookies cannot be encrypted

> 💡 Identity should be managed server-side; putting the username or role into an editable cookie lets the user decide who they are.
