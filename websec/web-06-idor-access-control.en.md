# IDOR & Access Control

> 📅 2026-08-05 · Deep Dive
> "Change one number in the URL and see someone else's data" — that is IDOR, the most common form of broken access control. This article unpacks how it happens, why hiding is not enough, and the one real defense.

---

The top of `web-01-owasp-top10` is A01 "Broken Access Control." Its most common, easiest-to-understand form is **IDOR** — change an id in a URL or request and see data you should not.

This article makes it clear: how it happens, why "nobody will guess it" is not a defense, and what actually works.

## What IDOR is

**IDOR (Insecure Direct Object Reference)** happens when a program uses a **user-supplied id** to fetch data directly, **without checking whether this user is allowed to see it.**

```
# Conceptual: querying directly by id, no permission check
GET /orders?id=123    → 200 {"order":"my order"}
GET /orders?id=124    → 200 {"order":"someone else's order"}   # the problem
```

As long as the database has id=124 and the program does not check "is 124 yours," you see someone else's orders, files, or messages. **The issue is not whether the id can be guessed; it is that the server never performs an authorization check.**

## Why "hiding it" is not enough

This is the most-misunderstood point. Many assume "the id is long and random, so nobody can guess it — safe." **That is wrong**, for three reasons:

1. **Ids are often enumerable**: `?id=123` → try `124`, `125` — a short sweep covers them.
2. **Ids leak elsewhere**: old messages, shared links, social screenshots can all carry someone else's id.
3. **Security must not rest on secrets**: `web-05-securing-web-apps` said the right mindset is "input is untrusted."

**Security = server-side authorization.** A user-supplied id is just "I want to access this record"; whether they may is decided by the server, using "who is logged in" plus "do they have permission to this record."

## The broken-access-control family

IDOR is one form of A01. The whole family points at the same thing: **permissions not checked.**

| Form | One line |
|---|---|
| IDOR | Directly alter an object id |
| Privilege escalation | A normal account does admin actions |
| Method bypass | Using POST where GET is expected gets around limits |
| Vertical/horizontal | Cross-role / cross-user access |

> Memorize this test: any program that accesses a resource by a user-supplied identifier must first ask "does this user have permission to this resource?" Skip that step, and it is A01.

## Defense

Defense is not "hide the id" but **building authorization checks on the server**:

1. **Verify permission on every resource access**: bind "the current user" to "the resource owner/authorized party" in the query.
2. **Use opaque references**: use internal random IDs rather than enumerable sequence numbers.
3. **Default deny**: the principle from `blue-01-hardening` — if not explicitly allowed, block.
4. **Test it actively**: log in as user A and try to reach user B's resources — a mandatory item on the test list.

## Next

IDOR is "the server did not check permissions." The next class is trickier: `web-07-business-logic` introduces business-logic flaws — the program follows the rules, but the rules themselves can be bypassed or abused.

#### Q: What is wrong with the idea 'make the id long and random, and nobody can guess it'?

* Ids always leak

* Security must not rest on secrecy; the real defense is the server checking whether this user has permission to this resource

* Long ids slow the site down

* There is nothing wrong with it

> 💡 Ids are enumerable and leak elsewhere, and security should not depend on secrets; the correct defense is server-side authorization.
