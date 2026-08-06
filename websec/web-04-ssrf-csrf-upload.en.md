# SSRF, CSRF, and File Upload

> 📅 2026-08-05 · Deep Dive
> Three often-underestimated risks: making the server fetch internal resources (SSRF), sneaking a request out of your browser (CSRF), and uploading an unexpected file. This article unpacks how each happens and how to defend.

---

`web-01` through `web-03` built the web-security map, injection, and identity. This article closes with three often-underestimated risks: **SSRF, CSRF, and file upload.** They are not as famous as SQL injection, but the damage they do is just as real.

## SSRF: making the server fetch your internals

SSRF (Server-Side Request Forgery) happens where **the server fetches a resource based on a user-supplied URL** — for example, "give me an image URL and I will fetch it back."

Where is the problem? A server usually sits on an internal network and can reach services **only insiders can reach**. If the user controls the fetch target, they can make the server visit things that should never be public: internal admin panels, cloud metadata services, local services.

Key defenses:

* **Whitelist**: only allow fetching the hosts or domains you expect, not any URL.
* **Block internal addresses**: reject `127.0.0.1`, private IPs, and cloud metadata addresses.
* **Least privilege**: whatever can be fetched, the fewer services it can reach, the better.

> Think of SSRF as "hiring an employee who lives inside the building to run your errand" — they have internal access you do not, so be very careful where you send them.

## CSRF: sneaking a request out of your browser

CSRF (Cross-Site Request Forgery) is the reverse problem. The attacker does **not steal your identity**; they exploit the fact that **you are already logged in**: a malicious site quietly makes your browser send a request to a site you are logged into.

Because the browser automatically attaches your session cookie, the server assumes "this is the real user" — so "change password" or "transfer money" gets executed without your knowledge.

The standard defense is the **CSRF token**: the server embeds a random token in each form, known only to your own pages, and verifies it on receipt. The attacker's site cannot obtain that token, so the request is rejected.

```
# Concept: the form carries a random token the server knows
<input type="hidden" name="csrf_token" value="random value">
```

> Remember the difference: XSS steals your identity; CSRF borrows it. So the CSRF defense is "verify the request really came from your own page," and modern frameworks ship this protection built in — do not turn it off.

## File upload: the unexpected file

Websites allow uploads all the time — avatars, attachments, data imports. The risk: **if an uploaded file is executed rather than stored, it becomes an attack surface.**

Typical problems:

* Uploading a "looks like an image, really a program" file that the server executes.
* Files with no type or size validation, filling the disk or draining resources.
* Uploaded files that, when downloaded by others, get executed as HTML/scripts (an XSS hidden inside, say, an `.svg`).

Key defenses:

* **Validate by content**, not by extension or `Content-Type`.
* **Store outside the webroot or in a dedicated directory**, with random filenames, so path is not a predictable executable.
* **Serve as attachments**: set the correct `Content-Disposition` so browsers do not render the file as a page.

## The shared mindset

SSRF, CSRF, and file upload all point at the same thing: **both input and requests are untrustworthy, and must be validated at the system boundary.**

| Risk | One-line defense |
|---|---|
| SSRF | Server fetches only whitelisted targets |
| CSRF | Random token in forms; verify request origin |
| File upload | Validate content, store off-webroot, serve as attachment |

Put these together with the parameterized queries, output escaping, and server-side sessions from earlier articles, and you hold every building block `web-05-securing-web-apps` needs.

## Next

You have seen the individual defenses. Finally, assemble them: `web-05-securing-web-apps` teaches a "threat model → defense checklist → continuous testing" approach to building a web app that is secure from the start.

#### Q: What does a CSRF attacker exploit?

* Stealing the user password

* The fact that the user is already logged in, tricking the browser into sending an authenticated request

* Running arbitrary code directly on the server

* Tampering with the user session cookie

> 💡 CSRF does not steal identity; it borrows it — a malicious page makes the logged-in browser send a request the server assumes is the real user.
