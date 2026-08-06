# IAM, MFA, and Zero Trust

> 📅 2026-08-05 · Core Concepts
> Modern security's focus is moving from the boundary to identity. IAM manages who gets in, MFA confirms it is really you, and zero trust changes the premise of trust. This article connects the three into the new identity-security framework.

---

Security is undergoing a paradigm shift: **the center of gravity is moving from the boundary to identity.**

In the past, we assumed "inside the firewall = trusted." But the cloud, remote work, and login-from-anywhere made "inside" meaningless. Three words were born: **IAM, MFA, and Zero Trust.** This article connects them.

## IAM: Identity and Access Management

**IAM (Identity and Access Management)** is the whole practice of "managing identities and permissions":

* **Identity**: who? (accounts, roles, sources)
* **Permissions**: what can they touch? (the least privilege from `secplus-02-general-concepts`)
* **Lifecycle**: join, change, leave — permissions follow the person; when they leave, permissions go.

IAM's core question is simple: **"does this person actually have permission to do this?"** Answer it well, and you block a large class of "access control failures" — exactly A01 in `web-01-owasp-top10`.

## MFA: confirming it is really you

`pass-04-defenses` and `blue-06-phishing-defense` both mentioned MFA. In the identity framework, it is the **upgrade to the proof that it is you**:

* Password only = one proof of "something you know."
* Add phone/key = a second proof of "something you have."
* Being tricked out of a password no longer means being tricked out of your identity.

**MFA is the last lock that makes a stolen password not enough** — the attacker gets the password, but not the second factor.

## Zero Trust: changing the premise of trust

**Zero Trust** is a security philosophy in one line:

> **Always verify, trust nothing by default — whether the request comes from inside or outside.**

The traditional model assumes "inside the wall is trusted"; Zero Trust deletes that assumption and replaces it with "every request verifies identity, checks permission, confirms the device." Its three pillars:

| Pillar | Meaning |
|---|---|
| Identity | Verify who you are every time |
| Device | Confirm the device is clean and trusted |
| Network | Being on the intranet is no longer enough to pass |

> Zero Trust is not a product; it is a set of principles. In practice: least privilege, continuous verification, fine-grained segmentation. You do not need to adopt it all at once — turning on MFA and tightening permissions is already moving toward Zero Trust.

## How the three connect

IAM, MFA, and Zero Trust are not three separate things — they are **one logical chain**:

1. **IAM** decides what permissions an identity should have.
2. **MFA** confirms it is really that identity.
3. **Zero Trust** extends "verification" from one login to every request.

Put them together and you have the skeleton of modern identity security: **every request, verified every time, with minimal privilege.**

## A practical starting point

A full IAM + Zero Trust architecture is large, but you do not need to go all-in. A practical priority:

1. **Turn on MFA everywhere** — especially admins: cheapest, highest-impact.
2. **Tighten permissions** — extend the least-privilege principle from `blue-01-hardening` to the account layer.
3. **Revoke on departure** — confirm "person leaves, permissions vanish immediately" — the most-overlooked item.
4. When the scale justifies it, adopt formal IAM platforms and Zero Trust architecture.

## Next

The identity framework is clear, and the whole blue-team series is nearly done. Next, return to the human: `career-01-learning-path` gathers the knowledge of this book from start to finish into a learning path and practice ground that is yours.

#### Q: What is the core of the Zero Trust philosophy?

* All intranet traffic is trusted

* Always verify, trust nothing by default — every request verifies identity and permission

* Trust all devices

* Only put firewalls at the boundary

> 💡 Zero Trust deletes the 'inside the wall = trusted' assumption, replacing it with verifying identity, permissions, and device on every request.
