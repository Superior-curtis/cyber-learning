# A LAN Attack Chain: From Broadcast to Crack

> 📅 2026-08-05 · Deep Dive
> Once an attacker is inside your LAN, the typical next steps are a broadcast-driven chain: Responder grabs hashes → offline cracking → lateral movement with the same password. This article turns the typical post-intrusion next steps into a complete case.

---

**Case**: a laptop falls to phishing and the attacker enters the LAN. But the real "prize" is not that laptop — it is what happens next: **a chain that moves laterally through the network.**

This article turns "after the intruder is inside" into a complete case, connecting `kali-19-responder-mitm`, `pass-03-cracking-tools`, and `blue-07-iam-zero-trust`.

> Security note: everything in this chain is limited to your own homelab subnet (lab-01-build-your-lab). Reproducing it on a real or someone else's network is a crime. The defense is given equal weight at every link.

## The chain

#### Foothold

Phishing puts the attacker on a low-privilege machine (the blue-06 scenario).

#### Grab hashes

Run Responder on the LAN, answer name-resolution broadcasts, and capture credential hashes other machines send (kali-19).

#### Crack offline

Crack the captured hashes offline with hashcat — triggering no account lockouts (pass-03).

#### Move laterally

Use the cracked password to log into other machines — because passwords are reused, one key opens many doors.

**Key insight: every step "borrows" behavior the system already allows.** Name-resolution broadcasts are legitimate, hashes can be computed offline, password reuse is a user habit — the attacker simply strings them together.

## Why LAN attacks are especially "quiet"

The hardest part of this chain is that **it barely triggers traditional alerts**:

* Responder does not "intrude"; it just answers broadcasts.
* Offline cracking never touches the target server — no failed-login records.
* Lateral movement uses "legitimate logins" that look like a normal user.

> This is exactly why blue-07-iam-zero-trust exists: assume the network is already breached, and verify every request — rather than trusting "inside the wall is trusted."

## The defense for each link

| Link | Defense |
|---|---|
| Foothold | Phishing defense: training, MFA, least privilege (`blue-06`, `pass-04`) |
| Grab hashes | Disable LLMNR/NBT-NS, enable SMB signing (`kali-19-responder-mitm`) |
| Crack offline | Slow hashing + salt, strong passwords (`crypto-02-password-hashing`) |
| Lateral movement | No password reuse, MFA, zero-trust-style verification (`blue-07-iam-zero-trust`) |

> The most important lesson: this chain's success rests on two habits — "trust the LAN by default" and "reuse passwords." Disable the extra protocols, move to a password manager, and verify with zero trust — three things that make the chain nearly fail.

## Detection

Every link leaves a trace (`blue-02-logging-siem`):

* Abnormal name-resolution broadcast volume.
* One machine suddenly logging into many others (the lateral-movement tell).
* The same credential appearing in logins across many places after a crack.

## Next

This chain threads the LAN, passwords, and identity together. For Responder details, `kali-19-responder-mitm`; for identity defense, `blue-07-iam-zero-trust`.

#### Q: Why is this LAN attack chain especially 'quiet' and hard for traditional alerts to catch?

* Because it sends no network packets at all

* Because it borrows behaviors the system already allows: answering broadcasts, computing hashes offline, and logging in with real credentials

* Because the LAN has no logs

* Because it takes only one action

> 💡 Every link is 'legitimate': Responder only answers broadcasts, offline cracking never touches the server, lateral movement uses real credentials — so zero-trust 'verify every time' is the cure.
