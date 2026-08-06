# Business Logic Flaws

> 📅 2026-08-05 · Deep Dive
> The program follows the rules, but the rules themselves can be bypassed or abused — that is a business-logic flaw. It has no fixed signature, scanners miss it, yet it often costs real money or breaks the flow. This article unpacks it with examples and gives defense directions.

---

The previous vulnerabilities (injection, IDOR) all have "clearly bad input." But one class has **input that looks perfectly normal, with the program following its rules — except the rules themselves can be abused.** That is a **business-logic flaw.**

It is especially tricky: no fixed signature, almost invisible to scanners, yet it often costs real money or breaks the flow.

## What a business-logic flaw is

Business logic is "how this system is supposed to operate": how discounts calculate, how purchase limits work, how many steps the flow has. A flaw happens when **the rules can be bypassed or abused**, and the program does not notice.

The contrast with technical flaws:

| | Technical (e.g., injection) | Business-logic |
|---|---|---|
| Input | Clearly abnormal | Looks perfectly normal |
| Scanners | Might catch | Almost never |
| Root cause | Program failed to validate input | The rules themselves have a hole |
| Impact | Data/system breached | Money/flow abused |

## Classic examples

* **Discount abuse**: reusing a "one-time discount code" to the extreme — every input is legal, but the result violates the "once per person" intent.
* **Negative quantity**: set the cart quantity to `-1` and the total goes negative — no lower-bound check.
* **Skipping a step**: checkout is "pick items → pay → ship," but calling the "ship" step directly skips payment.
* **Race conditions**: one coupon used twice simultaneously, both "checks" pass — the window problem from `web-04-ssrf-csrf-upload`.

> The shared root: the program checks "technically correct" but not "business-sensible." Every field is legal; the combination violates the business rule.

## Why scanners miss it

Automated scanners find flaws by "bad-input signatures" — malicious SQL, abnormal lengths. But a business-logic flaw's input is **entirely normal**; it is broken at the level of *intent* and *rules*:

* The scanner does not know "one discount per person" is a rule.
* The scanner does not know "quantity cannot be negative" is a rule.
* These rules live only in **business knowledge**.

So it needs a human: someone who understands the business, thinking during testing, "what if I walk this flow differently?"

## Defense directions

Business-logic flaws have no single patch, but there are general defense directions:

1. **Enforce business rules server-side**: discount counts, quantity ranges, state transitions — not just in the frontend (which can be bypassed).
2. **Give flows state**: every checkout step must verify the previous one really completed.
3. **Idempotency for non-monotonic actions**: use idempotency keys so the same action cannot take effect twice.
4. **Have a human walk the flow maliciously**: someone who knows the business walks normal flows backwards, skipping and repeating, like an attacker.

> Reminder: this is not teaching you to cause losses — it is telling defenders where to check. Testing your own systems with a "walk the normal flow maliciously" mindset is the most effective way to find these issues.

## Next

Business logic is covered. Next, close out two more technical classics: `web-08-xxe-deserialization` introduces XXE and insecure deserialization — two problems of "parsing what should not be parsed."

#### Q: Why are business-logic flaws almost invisible to automated scanners?

* Because they crash the scanner

* Because their input is perfectly normal; the flaw lives in intent and rules, which exist only in business knowledge

* Because they only affect paid features

* Because scanners lack enough memory

> 💡 Business-logic flaws have legal input and no signature; scanners do not understand rules like 'one use per person,' so only a person who understands the business can test for them.
