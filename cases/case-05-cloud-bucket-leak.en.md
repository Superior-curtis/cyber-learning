# The Public Cloud Bucket: One Setting, Millions of Records

> 📅 2026-08-05 · Deep Dive
> A backup sits in a cloud storage bucket; one 'public' checkbox unchecked, and millions of records become downloadable by anyone. This article uses one complete case to tie cloud misconfiguration, Shodan recon, and data exfiltration together.

---

**Case**: a company's database backup sits in a cloud storage bucket — set to "publicly readable" for "testing convenience." Six months later, millions of customer records in the backup are downloaded and resold by strangers.

This is not "being hacked" — it is **the door having been open all along.** This article uses the case to thread `cloud-02-cloud-misconfigs`, `recon-04-shodan-deep-dive`, and data exfiltration into one story.

> Security note: this is a defense lesson — how a misconfiguration becomes a disaster, and how to avoid it. Accessing unauthorized cloud data is a crime.

## The incident chain

#### One checkbox

The bucket is set to "publicly readable" — convenient in development, then never reverted (cloud-02).

#### Indexed

The public bucket is scanned and indexed by engines like Shodan (recon-04).

#### Discovered

A query for public buckets pulls this company and its backup out together.

#### Downloaded

Anyone can download it directly — millions of records, gone with one URL.

**Key: the attacker "intruded" into nothing.** They just "walked through a door left unlocked" — and that door is public on the internet, visible to everyone.

## Why "public" is so dangerous

* A bucket's "public" is **binary**: not "only you know," but "everyone can read."
* Backups usually hold **the most sensitive things**: database dumps, keys, customer PII.
* Public is permanent: once indexed, it is known forever.

> Keep the contrast: in development "public" is convenient; in production "public" is a disaster. Hence the rule "default private, public with reason and approval" — the first defense in cloud-02-cloud-misconfigs.

## The defense for each link

| Link | Defense |
|---|---|
| Setting | Default private; periodically audit whose buckets are public (`cloud-02`) |
| Indexing | Use Shodan to look up your own subnets and see the public view (`recon-04`) |
| Data | Encrypt backups, minimize sensitive data stored |
| Monitoring | Alerts on abnormal download volume and permission changes (`blue-02`) |

> The most important lesson: data leaks are often not "broken into" but "left open." Periodically checking your own public view with recon-04-shodan-deep-dive costs far less than the aftermath.

## Detection

* Audit records of bucket permission changes.
* Abnormal download traffic.
* "Should not be public" resources appearing on Shodan/Censys index lists.

## Next

This chain ties `cloud-02` and `recon-04` together. For the full misconfiguration list, `cloud-02-cloud-misconfigs`; for inventorying yourself with Shodan, `recon-04-shodan-deep-dive`.

#### Q: What is the 'entry point' in this cloud leak case?

* The attacker cracked the cloud account

* The bucket was set publicly readable and indexed — the attacker just walked through an unlocked door

* The cloud provider was hacked

* The data was not encrypted

> 💡 The key is a misconfiguration: public means everyone can read it, and being indexed makes it public forever; the defense is default-private plus periodically checking yourself with Shodan.
