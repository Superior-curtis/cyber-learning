# Injection: SQLi, XSS, Command Injection & Defenses

> 📅 2026-08-05 · Deep Dive
> Treating user input as executable code — that is injection, and it is OWASP A03. How do SQL injection, XSS, and command injection happen, and what defense principles do they share? This article makes it clear.

---

On the top-ten list in `web-01-owasp-top10`, A03 "Injection" is one of the most-cited names. It is a **family** — SQL injection, XSS, and command injection all belong. The shared essence fits in one sentence:

> **User input is treated as executable code.**

This article breaks down the three most common injections and explains the **defense principles they share**. Learn the principles; that matters more than memorizing vulnerability numbers.

## What injection is: input as code

Normally a program treats user input as *data* — store it, compare it. Injection happens where **data and code are not separated**: when user input is spliced directly into something that gets executed, the input becomes part of the program.

Example: a query looks for a person named Alice. If the user controls the name field and the program splices it into an SQL query, the user can type "Alice OR 1=1", turning the query into "return everyone".

> Keep this mental model: injection = the line between data and code is broken. Every injection defense is, at bottom, re-establishing that line.

## SQL injection

SQL injection happens where **database queries are assembled**. The classic defense is **parameterized queries (prepared statements)**:

```
# Wrong: splicing user input straight into the string
sql = "SELECT * FROM users WHERE name = '" + userInput + "'"

# Right: parameterized, input is always data, never syntax
sql = "SELECT * FROM users WHERE name = ?"
```

The difference: once parameterized, the database knows that position can only be a *value*. Whatever you type, it can never become new SQL syntax. **This is the standard fix for SQL injection — there is no other.**

## XSS (Cross-Site Scripting)

XSS happens where **web pages are output**: user input is echoed back verbatim, and the browser executes it as HTML or JavaScript. The usual consequence is "running your code in someone else's browser" — stealing cookies, impersonating them.

Two key defenses:

* **Output escaping**: when rendering, escape `<`, `>`, quotes and friends so the browser treats them as text, not tags.
* **CSP (Content Security Policy)**: tell the browser "this page only runs scripts from its own domain", so even injected scripts get blocked.

Modern frameworks escape output by default, so **do not disable that safe behavior by hand.**

## Command injection

Command injection happens where **operating-system commands are invoked**: user input is spliced into a shell command. For example, a "ping this address" feature that splices the address into `ping <address>` lets a user type `127.0.0.1; rm -rf /tmp/x` and run two commands.

The defense is clear:

* **Avoid calling the shell when you can** — many features do not need one at all.
* **When you must, pass arguments as an array, never a concatenated string**, and whitelist the allowed characters.

> This is a defense lesson, not an attack lesson. It only describes how injection happens and how to defend. If you practice injection against your own test environment, use a deliberately vulnerable target (DVWA, mentioned in lab-02-vulnerable-targets) and stay in scope.

## Shared defense principles

Put the three injections side by side and the defense is three sentences:

1. **Never splice user input into something that gets executed** — whether SQL, HTML, or a shell.
2. **Use the framework's safe mechanisms**: parameterized queries, automatic output escaping, restricted shell use. They have been hammered by thousands of engineers; do not roll your own.
3. **Defense in depth**: even if one layer fails, the next holds — CSP, a database account with minimal privileges, a server that never runs as root.

> The top principle: treat input as untrusted by default. Assume anything from the user may be hostile, at the system boundary. Adopt that habit and most injection bugs are prevented at write time.

## Next

Injection is handled. Next, another classic: identity. `web-03-auth-session` unpacks authentication and session security — how passwords, tokens, and sessions get stolen, and how to design so they do not.

#### Q: What is the standard defense against SQL injection?

* Uppercase all user input

* Use parameterized queries so input is always data, never syntax

* A firewall in front of the database is enough

* Make the database password longer

> 💡 Parameterized queries make the database treat that position as a value only; whatever is typed cannot become new SQL syntax.
