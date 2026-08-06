# Symmetric vs Asymmetric Crypto, and Signatures

> 📅 2026-08-05 · Core Concepts
> AES, RSA, ECC, digital signatures — behind all these terms is really one question: why are two keys better than one? With everyday analogies, this article makes symmetric, asymmetric, and signatures click, and ties back to the TLS handshake you met earlier.

---

You met the handshake in `net-02-tls-https`: the server hands over a public key, and both sides arrive at the same key. But what exactly are those two keys? And how does a "digital signature" actually sign?

This article makes the most-misunderstood chunk of cryptography clear: **symmetric encryption, asymmetric encryption, and digital signatures.** Three concepts, one core question: why are two keys better than one?

## Symmetric encryption: one key, one box

**Symmetric encryption** is the intuitive one: a single key both encrypts and decrypts. A box locked with that key opens only with the same key. AES is the most common symmetric algorithm today.

Its strength is **speed** — modern CPUs have dedicated instructions and can encrypt gigabytes per second. Its weakness is fatal: **you have to get the key to the other party.** And securely delivering the key is itself a hard problem — you cannot exactly mail the key and the lock in the same envelope.

## Asymmetric encryption: a key pair

**Asymmetric encryption** (public-key crypto), invented in the 1970s, was a milestone. Instead of one key, it uses a **pair**:

* **Public key**: safe to share with the world.
* **Private key**: known only to its owner.

The pair has a special property: **something encrypted with the public key opens only with the private key.** The reverse also holds — sign with the private key, and anyone with the public key can verify.

| Property | Symmetric (AES) | Asymmetric (RSA / ECC) |
|---|---|---|
| Number of keys | One | A pair (public + private) |
| Speed | Fast | Slow (orders of magnitude slower) |
| Main problem | How to deliver the key safely | Slow compute, more key management |
| Typical use | Bulk data encryption | Key exchange, signatures |

Famous implementations include **RSA** (old, widespread) and **ECC** (elliptic curves — shorter keys and faster at the same security level).

## Key exchange: the knot two keys untie

Now the core question has its answer: **why are two keys better than one?** Because they solve the key-delivery problem.

In the `net-02-tls-https` handshake, in plain terms: the browser and server do not need to ship a shared key back and forth. Instead, each uses its own private key with the other's public key to arrive at **the same** symmetric key. A listener sees both public keys exchanged but, lacking a private key, cannot derive the shared one.

In math terms this is the **Diffie-Hellman key exchange**. It changed the internet — without it, HTTPS would have no fast-and-secure encryption foundation.

## Digital signatures: proving it came from you

Public-key crypto also gave birth to a killer application: **digital signatures**. The logic runs asymmetric crypto in reverse:

* The sender signs the document (usually its hash) with **their own private key**.
* The receiver verifies the signature with the sender's **public key**.

A successful verification proves two things:

1. **It really came from that person** (only they hold the private key).
2. **The content was not altered** (any change breaks the verification).

This is exactly how certificates work in `net-02-tls-https`: a CA signs a certificate with its private key, and the browser verifies with the CA's public key. The stamp on the certificate is that digital signature.

> Keep this mapping: encrypt with the other's public key = only they can read (secrecy); sign with your private key = anyone can verify it came from you (identity + integrity).

## How the real world combines them

Algorithms rarely work alone. A typical TLS connection divides the work like this:

* **Asymmetric** handles the precious part: key exchange and identity, once per session.
* **Symmetric** carries the actual bulk: pages, files, video — fast and sustainable.

This "asymmetric to open, symmetric to cruise" hybrid is the real meaning behind the `net-02-tls-https` handshake. Cryptography rarely asks you to invent anything new — but you need to know which tool was built to solve which problem.

## Next

Know the crypto trio — symmetric, asymmetric, signatures — and the web's encryption story is complete. Revisit `net-02-tls-https` and read the handshake once more; this time you can see what every key is doing.

#### Q: Which two things does a digital signature prove?

* The document is encrypted and very large

* It was signed by the private-key holder, and the content was not altered

* The document is backed up and cannot be lost

* The signer has a fast computer

> 💡 A signature is produced with the sender private key and verified with the public key; a passing check proves both identity and content integrity.
