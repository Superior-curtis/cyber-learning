# Blockchain Basics: A Ledger Nobody Can Rewrite

> 📅 2026-08-05 · Core Concepts
> Blockchain is often made to sound mysterious, but its core is plain: a ledger everyone writes to and nobody can rewrite. This article unpacks blocks, hashes, and consensus in plain terms — and why it is a security topic at all.

---

You have probably heard of blockchain — Bitcoin, Ethereum, NFTs, Web3. Each name has its own story, but the underlying technology is startlingly plain: **a ledger everyone writes to, and nobody can secretly rewrite.**

This article unpacks three ideas in plain terms: **blocks, hashes, and consensus.** Master them and you will understand the keys and transactions in `chain-02`, and see exactly where the "vulnerabilities" in `chain-03` break.

## A shared ledger

Think of the blockchain as a **public ledger**:

* Every transaction is recorded on it, and it is **public** — anyone can read it.
* The ledger is maintained by many people, not held by one company.
* Once a page is written, it is **almost impossible** to secretly alter.

Its design goal is one thing: **make forging or rewriting records extremely hard.** That "hard" comes from two mechanisms — hashes and consensus.

## Blocks and hashes: why it cannot be tampered with

Each page of the ledger is a **block**. Besides its transactions, every block records one more thing: **the fingerprint of the previous page.**

That "fingerprint" is a hash — the one-way function from `crypto-01-hash-vs-encryption`:

* Same page content → always the same fingerprint.
* Change one character → the fingerprint changes completely.

So the ledger is a **chain**: every page is glued to the previous page's fingerprint. Try to edit a transaction on page 5, and page 5's fingerprint changes; page 6 records "the old page-5 fingerprint," so it is caught immediately — you would have to recompute **every page after 5** to make it fit. That is where the name "blockchain" comes from: **every page is locked to the previous one by its hash.**

> Think of the blockchain as a ledger where every page is stapled to the one before. Edit any middle page and the staple shows. Hashes alone already make tampering expensive; consensus makes it costlier.

## Distribution and consensus: why cheating is hard

If the ledger lived on one server, hacking that server would mean rewriting the ledger. So the blockchain **copies the ledger to many people**, each holding a full copy.

When someone wants to "write a new page," all copies must **agree** — that is **consensus.** The most common method is "proof of work": the writer pays a large computational cost (mining), and everyone else verifies the new page before accepting it.

The result: to tamper, you must not only change your own copy, but rewrite history on **more than half the network**, with more compute than everyone else combined. The cost makes it not worth it — not "impossible," but "too expensive to bother."

## What it means for a security practitioner

For a security professional, blockchain matters because **its trust model is the opposite of traditional systems**:

| Traditional system | Blockchain |
|---|---|
| Trust is centralized (bank, server) | Trust is spread across everyone |
| Edit data and nobody may notice | Tampering is exposed instantly by the hash chain |
| Security rests on access control | Security rests on keys and consensus |

That inversion is exactly the theme of `chain-02` and `chain-03`: when access control gives way to **keys**, a leaked key is fatal; when code is written on-chain and can never be changed, a bug is permanent. **On-chain there is no "fix it later" — only "a stumble is a lifetime mistake."**

## Next

You now understand how the ledger stays locked. Next: `chain-02-wallets-keys-transactions` covers the user side — wallets, keys, and transactions — and why "your private key is your identity."

#### Q: Why is it almost impossible to secretly tamper with a blockchain record?

* Because records are very complexly encrypted

* Because every block is glued to the previous page hash, so any edit shows, and you must fight the whole network consensus

* Because only banks can change it

* Because the data is not public at all

> 💡 The hash chain exposes any tampering instantly, and consensus makes rewriting history too expensive to be worth it.
