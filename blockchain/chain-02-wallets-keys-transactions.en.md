# Wallets, Keys, and Transactions

> 📅 2026-08-05 · Core Concepts
> There are no usernames and passwords on a blockchain — your identity is a key pair. A wallet is not 'where money lives'; it is 'where your keys live.' This article unpacks addresses, signatures, and transactions, and why a leaked private key means losing everything.

---

`chain-01-blockchain-basics` explained how the ledger stays locked. But you probably still have a closer question: **how does the user side "log in" to a blockchain?**

The answer may surprise you: **there are no usernames and passwords on a blockchain. Your identity is a key pair.**

## A wallet is not a wallet

Take apart the biggest misunderstanding first: **a "wallet" is not where money lives; it is where your keys live.**

* Your assets (coins, NFTs) are recorded on the on-chain ledger — they are never in your device.
* A "wallet" merely **holds the keys that can move those assets**, and signs for you.

Think of a wallet as a **key ring**, not a billfold — losing the key ring is not "the cash in my wallet is gone," it is "I can never open the vault again."

## Addresses and keys

Remember asymmetric crypto from `crypto-03-asymmetric-crypto`? The blockchain uses it directly:

| Key | Role | Can it be public? |
|---|---|---|
| Private key | Signs, moves assets | **Never** |
| Public key | Verifies signatures | Yes |
| Address | The "mailing address" derived from the public key | Yes |

The logic of login and authorization: **you own an address = you hold its private key.** This is the "identity" from `blue-07-iam-zero-trust` — on-chain, identity is proving you hold the private key.

## Transactions and signatures

Sending a transaction is essentially **signing a statement "I transfer X to this address" with your private key**:

```
# Conceptual: a transaction's core fields
{ from: 0xmyAddress, to: 0xotherAddress, amount: 1.0, nonce: 5 }
signature = sign(transaction content, my private key)
```

Anyone given "transaction content + signature + my public key" can verify: **this really was authorized by the key holder.** If it verifies, miners/validators write it into the next block. The signature idea is identical to the digital signatures in `crypto-03-asymmetric-crypto`.

> There is no "forgot password" on-chain. Traditional systems can reset a password; the blockchain cannot — the private key is everything. Backing up your key (seed phrase) is backing up your identity and assets.

## A leaked private key = losing everything

Here is the security crux. The rule for on-chain assets:

* **Whoever holds the private key can move the assets.** The chain does not know "you"; it only knows the private key.
* If the key is stolen, the attacker transfers the assets directly, **untraceable and irreversible** — once a transaction is on-chain, it is permanent.
* So the number-one target of on-chain attacks is never "hacking the chain" but **stealing private keys**: phishing, malware, bad backups, leaked seed phrases.

The lesson of `web-03-auth-session` is amplified to the extreme: a password can be changed; **a private key cannot.**

## Nothing on-chain is secret

One last important security mindset: **"nothing on-chain is secret."**

* All transaction content, amounts, and addresses are public.
* Data you "thought was hidden," once written on-chain, is public forever.
* Analysts, law enforcement, and attackers all read the same ledger.

So any design that "puts sensitive data on-chain" is a design-level flaw — the on-chain version of the `web-04-ssrf-csrf-upload` line: "public ≠ fine."

## Next

Keys and transactions are clear. Next, the most "exciting" part of on-chain security: `chain-03-smart-contract-vulns` introduces smart-contract vulnerability classes — why code that "can never be changed once written" leaves a permanent scar when it has a bug.

#### Q: Why is a leaked private key far worse than a leaked password?

* Because private keys are longer

* Because the chain only knows the private key, not you; transactions are irreversible once on-chain, and there is no password reset

* Because private keys are harder to remember

* Because the chain auto-locks

> 💡 On-chain identity is the private key; whoever holds it owns the assets. A leaked key means assets moved irreversibly, with no 'reset password'.
