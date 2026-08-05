# Cyber Learning Hub — 一本雙語資安參考書

> A **bilingual (中文 / English) reference** that grows into a wikipedia of cybersecurity — from how passwords are really stored, to the Kali toolchain, to reading CVEs and defending real systems. **For authorized security testing, CTF practice, and blue-team defense only.**

## 目錄 / Table of Contents

| Part | Title | 篇數 |
|---|---|---|
| **00** | **Foundations** / 資安基礎 | 5 |
| **01** | **Linux Essentials** / Linux 基礎 | 0 |
| **02** | **Networks & Crypto** / 網路與密碼學 | 0 |
| **03** | **Security+ Core** / Security+ 基礎 | 0 |
| **04** | **Offensive Security (authorized / CTF)** / 攻防基礎（授權 / CTF） | 5 |
| **05** | **Tools & Kali Linux** / 工具與 Kali Linux | 0 |
| **06** | **Vulnerabilities & CVEs** / 漏洞與 CVE | 7 |
| **07** | **Practice Labs (free HackTheBox)** / 實戰練習場 | 0 |
| **08** | **Defense & Career** / 藍隊與職涯 | 0 |

## 00 · Foundations / 資安基礎 (5 篇)

> What security is, the CIA triad, threat modeling, and how the web works.

### Foundations / 資安基礎  (5 篇)

> What security is, the CIA triad, glossary, threat modeling, and the web.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [資安是什麼：威脅、漏洞與風險](foundations/found-01-what-is-security.zh.md) | [What Is Security: Threats, Vulnerabilities, and Risk](foundations/found-01-what-is-security.en.md) | 2026-08-05 |
| 2 | [CIA 三元組與資安原則](foundations/found-02-cia.zh.md) | [The CIA Triad: Confidentiality, Integrity, Availability](foundations/found-02-cia.en.md) | 2026-08-05 |
| 3 | [白話資安名詞表](foundations/found-03-glossary.zh.md) | [A Plain-Language Security Glossary](foundations/found-03-glossary.en.md) | 2026-08-05 |
| 4 | [威脅模型入門](foundations/found-04-threat-modeling.zh.md) | [Threat Modeling for Beginners](foundations/found-04-threat-modeling.en.md) | 2026-08-05 |
| 5 | [網路與 Web 運作基礎（HTTP / DNS / IP / Port）](foundations/found-05-how-the-web-works.zh.md) | [How the Web Works: HTTP, DNS, IP, and Ports](foundations/found-05-how-the-web-works.en.md) | 2026-08-05 |

## 01 · Linux Essentials / Linux 基礎 (0 篇)

> The Linux skills every security practitioner needs.

## 02 · Networks & Crypto / 網路與密碼學 (0 篇)

> TCP/IP, TLS, Wi-Fi, and the cryptography behind password storage.

## 03 · Security+ Core / Security+ 基礎 (0 篇)

> The CompTIA Security+ exam domains as a curriculum.

## 04 · Offensive Security (authorized / CTF) / 攻防基礎（授權 / CTF） (5 篇)

> Recon, password attacks & defenses, and web application security.

### Passwords / 密碼安全  (5 篇)

> How passwords are stored, how cracking works, and how to defend.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [密碼如何被儲存（hash / salt / pepper）](password/pass-01-how-passwords-are-stored.zh.md) | [How Passwords Are Actually Stored](password/pass-01-how-passwords-are-stored.en.md) | 2026-08-05 |
| 2 | [破解的機制：字典、暴力、規則與 GPU](password/pass-02-cracking-101.zh.md) | [How Password Cracking Works](password/pass-02-cracking-101.en.md) | 2026-08-05 |
| 3 | [hashcat 與 John the Ripper 實作](password/pass-03-cracking-tools.zh.md) | [hashcat & John the Ripper in Practice](password/pass-03-cracking-tools.en.md) | 2026-08-05 |
| 4 | [防禦：MFA、密碼管理器與鎖定策略](password/pass-04-defenses.zh.md) | [Defending: MFA, Password Managers, and Lockout](password/pass-04-defenses.en.md) | 2026-08-05 |
| 5 | [真實外洩案例教訓](password/pass-05-real-breaches.zh.md) | [Lessons from Real Password Breaches](password/pass-05-real-breaches.en.md) | 2026-08-05 |

## 05 · Tools & Kali Linux / 工具與 Kali Linux (0 篇)

> The Kali toolchain, hands-on.

## 06 · Vulnerabilities & CVEs / 漏洞與 CVE (7 篇)

> How the CVE system works, reading advisories, famous vulnerabilities, and bug bounty.

### CVEs / 漏洞與 CVE  (7 篇)

> CVE/CWE/CVSS, disclosure, famous vulnerabilities, and bug bounty.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [CVE / CWE / CVSS 是什麼](cve/cve-01-what-is-cve.zh.md) | [What Are CVEs, CWEs, and CVSS?](cve/cve-01-what-is-cve.en.md) | 2026-08-05 |
| 2 | [漏洞生命週期與揭露](cve/cve-02-lifecycle-disclosure.zh.md) | [The Vulnerability Lifecycle & Disclosure](cve/cve-02-lifecycle-disclosure.en.md) | 2026-08-05 |
| 3 | [怎麼讀一份 CVE 報告](cve/cve-03-reading-a-cve.zh.md) | [How to Read a CVE Advisory](cve/cve-03-reading-a-cve.en.md) | 2026-08-05 |
| 4 | [經典漏洞 I：Heartbleed、Shellshock、EternalBlue](cve/cve-04-famous-1.zh.md) | [Famous Vulnerabilities I: Heartbleed, Shellshock, EternalBlue](cve/cve-04-famous-1.en.md) | 2026-08-05 |
| 5 | [經典漏洞 II：Log4Shell、ProxyLogon、SolarWinds](cve/cve-05-famous-2.zh.md) | [Famous Vulnerabilities II: Log4Shell, ProxyLogon, SolarWinds](cve/cve-05-famous-2.en.md) | 2026-08-05 |
| 6 | [修補管理與供應鏈風險](cve/cve-06-patch-management.zh.md) | [Patch Management & Supply-Chain Risk](cve/cve-06-patch-management.en.md) | 2026-08-05 |
| 7 | [Bug Bounty 基礎](cve/cve-07-bug-bounty.zh.md) | [Bug Bounty Basics](cve/cve-07-bug-bounty.en.md) | 2026-08-05 |

## 07 · Practice Labs (free HackTheBox) / 實戰練習場 (0 篇)

> Build your own lab and learn CTF categories hands-on.

## 08 · Defense & Career / 藍隊與職涯 (0 篇)

> Hardening, detection, incident response, forensics, and how to enter the field.

> ⚠️ **Legal & ethical notice:** all techniques here are for **authorized testing of your own systems, CTF labs, and defensive research**. Unauthorized testing of systems you do not own is illegal in most jurisdictions. Always get written permission first.

## 📬 Contribute

Articles are in Markdown (converted from the interactive MDX originals on the website). Feel free to open issues or PRs for corrections or new topics.

---

_Built with ❤️ — a bilingual book on cybersecurity, from the [myself](https://blog-916bd.web.app) website._
