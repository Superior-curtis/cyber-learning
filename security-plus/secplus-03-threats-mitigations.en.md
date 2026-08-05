# Threats, Vulnerabilities, and Mitigations

> 📅 2026-08-05 · Core Concepts
> Twenty-two percent of the exam is about the bad stuff: malware, social engineering, threat actors, and vulnerability classes. This article takes a defender view and pairs every threat with its defense.

---

Domain 1 gave you the vocabulary; Domain 2 gives you the **adversary encyclopedia**. It is the second-highest weight on SY0-701 (22%), and it is the place where candidates often "memorize a bunch of names but do not know how to use them."

The key mindset: **the star of this article is not the threat — it is the "threat → defense" pairing.** The exam mostly goes "an attack happens; pick the correct defense." So in every section below, the mitigation gets at least as much space as the threat.

The structure follows Domain 2's outline: first the malware and social engineering families (how people and programs launch attacks), then threat actors and vulnerability classes (who is attacking and where the weak points are), and finally a mitigation loop that ties it all together.

> Educational and defensive framing — this article introduces threats from a defender's point of view so you understand how attacks happen and how to stop them. Use this knowledge only on your own systems, lab environments, and explicitly authorized testing. Attacking systems that are not yours is illegal.

## The malware family: every one has an antidote

Malware is not one threat — it is a whole family. SY0-701 will ask you to recognize each member by its *behavior*, and to name the most effective defense:

| Malware | What it does | Main defense |
|---|---|---|
| Virus | attaches to files or programs and infects others when executed | antivirus, do not open unknown attachments, keep patching |
| Worm | self-replicates and spreads to other machines on its own | patch vulnerabilities, segment the network, firewall rules |
| Trojan | disguises itself as legitimate software, then acts maliciously | install only from trusted sources, check signatures and hashes |
| Ransomware | encrypts your files and demands payment | **offline backups**, least privilege, email filtering |
| Spyware | quietly records your activity, keystrokes, and data | antivirus, restrict install sources, browser permissions |
| Rootkit | burrows into the system's depths to evade detection | secure boot, integrity checks, clean reinstall |
| Botnet | herds a large number of compromised machines under one command | patching, network monitoring, block command-and-control servers |

A few exam shortcuts. First, **the virus/worm difference is whether a user has to do something**: a virus usually needs you to run a file; a worm crawls on its own. Second, **the most effective defense against ransomware is backup** — ideally an offline one, because the moment files get encrypted, your online backup may be encrypted too. Third, when you see "evades detection," think rootkit first.

Notice also that malware does not fly into a system on its own — it needs a **delivery channel**: an email attachment, a download from a malicious site, an infected USB stick, an exploit kit. See the pattern? These channels almost all revolve around *something a person does*: opening the mail, plugging in the drive, landing on the wrong page. So the first line of malware defense is often not technical — it is **not letting users open the door.**

## Social engineering: the target is people, not systems

Social engineering is one of the topics this book stresses as a *defense* topic, because it bypasses every technical control and goes straight for the softest target — people. SY0-701 will test the characteristics of each technique and its countermeasure:

| Technique | How it tricks you | How to defend |
|---|---|---|
| Phishing | mass-emails fake messages to get clicks or data | email filtering, check URLs, training, reporting |
| Spear phishing | customizes content to a specific person or group | extra vigilance for high-risk roles (finance, executives) |
| Whaling | targets senior executives as high-value prey | dual approval for large transfers, extra verification |
| Vishing | impersonates over the phone — support, bosses | call back the official number, never give sensitive info |
| Smishing | sends phishing links by SMS | do not tap unknown links, use an authenticator app |
| Pretexting | builds a believable persona ("I am IT support") first, then asks | verify the caller's identity, use official channels |
| Baiting | dangles curiosity (a free USB, a QR code prize) | never plug unknown USB drives, never scan unknown QR codes |
| Tailgating | follows someone through the door on their badge | physical access control, one badge one person, mantrap lanes |

The shared countermeasure for all of social engineering is **verify before you trust**: get a call from "your boss"? Hang up and call the official extension. An email says "update your password"? Type the site's address yourself instead of clicking the link. This is really Domain 1's zero trust applied to people.

Why are these techniques so effective? Because they do not attack a logic flaw — they attack **human defaults**: we tend to believe authority ("the boss"), urgency ("do it now"), and friendliness ("customer support"). An attacker only has to construct a scenario that *looks reasonable*, and most people will not stop to verify. That is also why "verify before you trust" is the one universal countermeasure — it does not rely on your judgment in the moment; it makes verification a fixed habit.

## Threat actors: who is attacking you

The same attack means something completely different depending on the **motivation and resources** behind it — and so does the defense. The classifications SY0-701 likes to test:

| Actor | Motivation | Resources | What you defend |
|---|---|---|---|
| Nation-state | politics, espionage, geopolitics | very high | the most patient adversary; prizes persistence and stealth |
| Cybercriminal syndicate | money | high | ransomware, fraud, stolen data resale |
| Hacktivist | ideology, protest | medium/low | website defacement, DDoS, leaked documents |
| Insider | grievance, money, or just carelessness | medium | access controls, behavior monitoring, offboarding |
| Script kiddie | fun, wanting attention | low | repurposes ready-made tools; patching basics already helps |

A practical idea: **threat modeling starts with "who would target me?"** A small company should not build for nation-state resources, but it absolutely must defend against criminal ransomware. The threat actor decides where your money goes — `found-04-threat-modeling` turns that thinking into a systematic process.

One more thing worth stressing: **these categories are not hard law.** The same group can be both a criminal syndicate and state-backed; an insider can be malicious or merely careless. The exam gives you the *most likely* answer, not an absolute one — so judge by the two axes of motivation and resources rather than memorizing the table.

## Vulnerability classes: where weaknesses come from

A vulnerability is a weakness a threat can exploit. Domain 2 does not test individual CVEs (that is `cve-01-what-is-cve`); it tests the **broad classes** of vulnerabilities:

| Class | Example | Mitigation |
|---|---|---|
| Misconfiguration | default passwords never changed, a public cloud storage bucket | configuration baselines, regular audits, automated checks |
| Unpatched software | known flaws with no update applied | patch management, auto-update, an asset inventory |
| Weak credentials | simple passwords, no MFA | password policy, MFA, block common passwords |
| Insecure design | a login with no rate limiting | secure design principles, threat modeling, testing |
| PII exposure | collecting and keeping sensitive data you do not need | data minimization, encryption, access control |
| Zero-day | a flaw even the vendor does not know about | defense in depth, anomaly monitoring, a rapid response plan |

Notice how many mitigations here are *procedural*: patch management, configuration baselines, least privilege. Vulnerabilities are not something you "scan once and finish" — they are ongoing discipline. `blue-01-hardening` covers hardening and patching more fully.

A practical note: **vulnerability scanning** is the main tool for finding these weaknesses — it scans systems, compares them against known-vulnerability databases, and produces a "where are the holes" list. But scanning is only the start: the list needs prioritization (which hole is easiest to exploit? how critical is the affected system?), a patching schedule, and verification that the patch really took. Unpatched vulnerabilities are the number-one reason companies get breached — not because the attack is clever, but because patching was never treated as ongoing discipline.

## First, separate three words: threat, vulnerability, risk

The exam loves to play with three words, so let us pin down how they relate: a **threat** is something that can cause harm (ransomware, a phisher); a **vulnerability** is a weakness you can be exploited through (an unpatched app, a weak password); **risk** is the likelihood and consequence of harm once a threat meets a vulnerability.

One sentence ties them together: **threats seep through vulnerabilities, and that is risk.** A threat with no vulnerability is low risk; a vulnerability with no threat eyeing it is also low risk — but for most systems the threat always exists, so controlling vulnerabilities is controlling risk. This three-way relationship is also the core of `found-04-threat-modeling`; it will come back again when `secplus-06-governance-risk` talks about governance.

## Supply chain and third-party risk

Domain 2 has one more category that is easy to underestimate: **supply chain risk**. The software you use comes from a third party, whose packages come from a fourth — every layer is a potential weakness. Some of the most famous supply-chain incidents smuggled malware into a popular open-source package, compromising every company that used it at once.

The defense: rely only on trusted sources, verify package integrity (signatures and hashes), keep a software bill of materials (SBOM) so you can trace fast, and treat "trusting a third party" as a managed risk rather than a default. For the exam, the intuition "supply chain risk means your security depends on people you have never met" is enough to answer most questions.

## The core mitigation loop: know first, then block

Squeeze the whole article into a process. Inventory → assess → layer → verify; this loop has no end — new threats appear, systems change, people move, and every step gets revisited:

#### Inventory your assets

If you do not know what you have, you do not know what to protect. Start with an asset list.

#### Assess the risk

Who would target you (threat actors), where your weaknesses are (vulnerability classes), and how bad it would hurt.

#### Layer the defenses

Use the Domain 1 control types — preventive, detective, corrective — and give every threat at least one layer.

#### Verify and rehearse

Confirm patches really took effect, and drill the plans so they actually run. Restore a backup for real, at least once.

> Threat-pairing practice is the most efficient way to prepare for Domain 2: when you meet a term (say, "ransomware"), do not stop there — force yourself to name the defense ("offline backups") on the spot. When threat → defense becomes a reflex, you have already won half the exam.

One final reminder: mitigation is not "do everything" — it is **do the high-impact, low-cost things first**. Change the default password, enable automatic updates, keep an offline backup. Those three are often worth more than a pile of expensive defense toys. Build the foundation, then stack upward.

## How to approach Domain 2

To wrap the whole article into three study habits:

* **Memorize the pairings, not just the names** — for every threat you meet, be able to name its defense on the spot;
* **Read behavior, not labels** — questions often describe behavior ("self-replicates and spreads") instead of giving the name; recognize the behavior and you have recognized the threat;
* **Defense first** — when you meet any threat, think about the defense before you worry about the details.

Follow those three habits, add the mixed practice in `secplus-07-practice-questions`, and Domain 2 is yours.

## Next

This article finished the taxonomy of the "bad guys." The next one climbs a level into Domain 3: **security architecture** — how networks, clouds, and systems should be designed so these threats have nowhere to land: `secplus-04-security-architecture`.

To round out systematic threat assessment first, go back to `found-04-threat-modeling`; to practice phishing defense hands-on, jump ahead to `blue-06-phishing-defense`.

#### Q: A company receives a call from someone claiming to be IT support who demands employees immediately provide their credentials to fix an emergency. Which social engineering technique is this?

* Whaling

* Pretexting

* Baiting

* Tailgating

> 💡 Pretexting builds a believable persona (here, IT support) first and then asks for information. The countermeasure is to verify the caller — call back the official number instead of trusting the claimed identity.
