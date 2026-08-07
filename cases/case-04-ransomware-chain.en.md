# The Ransomware Infection Chain

> 📅 2026-08-05 · Deep Dive
> Ransomware is not 'a file encrypts and it is over' — it is a chain: phishing attachment → execution → lateral movement → encryption → ransom. This article walks the whole chain and dissects it from both the analyst view (static/dynamic) and the blue-team view (detect/respond).

---

**Case**: an "invoice" email gets opened; three hours later the whole company's files become `.locked`, and a ransom note appears on screen.

Ransomware is not "a file encrypts and it is over" — it is a chain. This article walks the whole chain and dissects it from both the **analyst** and **blue-team** views.

> Security note: this is a defense-and-analysis lesson — understanding the chain, how to analyze, and how to respond. This book does not provide ransomware code or delivery methods.

## The infection chain

#### Phishing attachment

An "invoice" email carries a malicious document — phishing is the #1 entry (blue-06).

#### Execution

The user opens the attachment; macros or commands run and drop a loader.

#### Lateral movement

Stolen credentials spread it to other machines (the lateral idea from case-03).

#### Encryption

Files are encrypted in bulk; a ransom note is written.

#### Ransom

Payment is demanded in exchange for the decryption key.

**See it? What is valuable is not "the encryption step" but the parts before — "how it got in, how it spread."** Stop any link and the chain breaks.

## The analyst view: static → dynamic

If you get a suspicious sample (say, an attachment from a suspicious email), the analysis flow from `mal-02` and `mal-03` kicks in:

1. **Static**: `file`, `sha256sum`, `strings` — see what you can learn without executing (`mal-02-static-analysis`).
2. **Dynamic**: run it in a sandbox (`mal-03-sandbox-dynamic`) and observe where it connects and what it changes.

> Analyst mindset: read statically first, confirm dynamically — isolated throughout. A sample may "sleep" for a long time in the sandbox before acting, so dynamic analysis needs patience.

## The blue-team view: detection and response

| Phase | Blue-team action |
|---|---|
| Detect | Abnormal file-encryption activity, mass renames — the baseline of `blue-02-logging-siem` |
| Contain | Isolate infected machines and cut the network immediately (`blue-04-incident-response`) |
| Eradicate | Remove the loader and close the entry |
| Recover | Restore from clean backups — the only reliable "cure" for ransomware |
| Lessons | Why did someone click? Strengthen training and defenses |

> The two most important ransomware lessons: (1) backup, backup, backup — a clean offline backup is the only reliable recovery; (2) stop the entry — phishing training and MFA keep the chain from ever reaching the encryption step.

## Next

This chain ties `blue-06`, `mal-*`, and `blue-04` together. For the analysis flow, `mal-02-static-analysis` and `mal-03-sandbox-dynamic`; for the response flow, `blue-04-incident-response`.

#### Q: Why is the 'valuable' part of ransomware not the final encryption but the earlier steps?

* Because encryption is easy

* Because what decides the outcome is how it gets in and spreads — stop the entry or lateral movement and the chain never reaches encryption

* Because the ransom amount does not matter

* Because encryption cannot be detected

> 💡 Stop the phishing entry or lateral movement and the chain breaks before encryption; so defense focuses on phishing training, MFA, and backups rather than 'fighting encryption.'
