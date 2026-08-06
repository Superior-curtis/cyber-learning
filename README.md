# Cyber Learning Hub — 一本雙語資安參考書

> A **bilingual (中文 / English) reference** that grows into a wikipedia of cybersecurity — from how passwords are really stored, to the Kali toolchain, to reading CVEs and defending real systems. **For authorized security testing, CTF practice, and blue-team defense only.**

## 目錄 / Table of Contents

| Part | Title | 篇數 |
|---|---|---|
| **00** | **Foundations** / 資安基礎 | 5 |
| **01** | **Linux Essentials** / Linux 基礎 | 9 |
| **02** | **Networks & Crypto** / 網路與密碼學 | 7 |
| **03** | **Security+ Core** / Security+ 基礎 | 7 |
| **04** | **Offensive Security (authorized / CTF)** / 攻防基礎（授權 / CTF） | 16 |
| **05** | **Tools & Kali Linux** / 工具與 Kali Linux | 14 |
| **06** | **Vulnerabilities & CVEs** / 漏洞與 CVE | 7 |
| **07** | **Practice Labs (free HackTheBox)** / 實戰練習場 | 9 |
| **08** | **Defense & Career** / 藍隊與職涯 | 10 |
| **09** | **Blockchain Security** / 區塊鏈安全 | 4 |

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

## 01 · Linux Essentials / Linux 基礎 (9 篇)

> The Linux skills every security practitioner needs.

### Linux / Linux 基礎  (9 篇)

> Filesystem, permissions, processes, logs, hardening — the Linux every security pro needs.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [為什麼資安世界被 Linux 統治](linux/linux-01-why-linux.zh.md) | [Why Linux Rules Security](linux/linux-01-why-linux.en.md) | 2026-08-05 |
| 2 | [檔案系統與目錄結構](linux/linux-02-filesystem.zh.md) | [The Linux Filesystem: Where Everything Lives](linux/linux-02-filesystem.en.md) | 2026-08-05 |
| 3 | [權限與擁有者（chmod / chown）](linux/linux-03-permissions.zh.md) | [Permissions & Ownership (chmod / chown)](linux/linux-03-permissions.en.md) | 2026-08-05 |
| 4 | [使用者、群組與 sudo](linux/linux-04-users-groups.zh.md) | [Users, Groups, and sudo](linux/linux-04-users-groups.en.md) | 2026-08-05 |
| 5 | [程序、服務與 systemd](linux/linux-05-processes-services.zh.md) | [Processes, Services, and systemd](linux/linux-05-processes-services.en.md) | 2026-08-05 |
| 6 | [Shell 指令速成（給資安人）](linux/linux-06-shell-essentials.zh.md) | [Shell Essentials for Security Work](linux/linux-06-shell-essentials.en.md) | 2026-08-05 |
| 7 | [日誌與稽核（/var/log、journalctl）](linux/linux-07-logs-auditing.zh.md) | [Logs & Auditing (/var/log, journalctl)](linux/linux-07-logs-auditing.en.md) | 2026-08-05 |
| 8 | [命令列網路工具（ip / ss / ports）](linux/linux-08-networking-cli.zh.md) | [Networking on the Command Line (ip / ss / ports)](linux/linux-08-networking-cli.en.md) | 2026-08-05 |
| 9 | [Linux 加固基礎](linux/linux-09-hardening-basics.zh.md) | [Linux Hardening Basics](linux/linux-09-hardening-basics.en.md) | 2026-08-05 |

## 02 · Networks & Crypto / 網路與密碼學 (7 篇)

> TCP/IP, TLS, Wi-Fi, and the cryptography behind password storage.

### Networks / 網路安全  (4 篇)

> TCP/IP, TLS, Wi-Fi, and firewalls/VPNs/proxies.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [TCP/IP、網段與埠](networking/net-01-network-fundamentals.zh.md) | [Network Fundamentals: TCP/IP, Subnets, and Ports](networking/net-01-network-fundamentals.en.md) | 2026-08-05 |
| 2 | [TLS / HTTPS 深潛：鎖上的鎖是怎麼鎖上的](networking/net-02-tls-https.zh.md) | [TLS/HTTPS: How the Lock Gets Locked](networking/net-02-tls-https.en.md) | 2026-08-05 |
| 3 | [Wi-Fi 安全：家用無線網路](networking/net-03-wifi-security.zh.md) | [Wi-Fi Security: Your Home Wireless Network](networking/net-03-wifi-security.en.md) | 2026-08-05 |
| 4 | [防火牆、VPN 與代理](networking/net-04-firewalls-vpn-proxy.zh.md) | [Firewalls, VPNs, and Proxies](networking/net-04-firewalls-vpn-proxy.en.md) | 2026-08-05 |

### Crypto / 密碼學  (3 篇)

> Hash vs encryption, password hashing algorithms, asymmetric crypto.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [雜湊 vs 加密：差在哪](crypto/crypto-01-hash-vs-encryption.zh.md) | [Hash vs Encryption: What's the Difference?](crypto/crypto-01-hash-vs-encryption.en.md) | 2026-08-05 |
| 2 | [密碼雜湊：bcrypt / argon2 為什麼贏](crypto/crypto-02-password-hashing.zh.md) | [Password Hashing: Why bcrypt and argon2 Win](crypto/crypto-02-password-hashing.en.md) | 2026-08-05 |
| 3 | [對稱與非對稱加密、簽章](crypto/crypto-03-asymmetric-crypto.zh.md) | [Symmetric vs Asymmetric Crypto, and Signatures](crypto/crypto-03-asymmetric-crypto.en.md) | 2026-08-05 |

## 03 · Security+ Core / Security+ 基礎 (7 篇)

> The CompTIA Security+ exam domains as a curriculum.

### Security+ / Security+ 基礎  (7 篇)

> The CompTIA Security+ exam domains as a plain-language curriculum.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [Security+ 考什麼：Domain 地圖](security-plus/secplus-01-overview.zh.md) | [What Security+ Covers: The Domain Map](security-plus/secplus-01-overview.en.md) | 2026-08-05 |
| 2 | [一般安全概念（AAA、MFA、最小權限）](security-plus/secplus-02-general-concepts.zh.md) | [General Security Concepts (AAA, MFA, Least Privilege)](security-plus/secplus-02-general-concepts.en.md) | 2026-08-05 |
| 3 | [威脅、漏洞與緩解](security-plus/secplus-03-threats-mitigations.zh.md) | [Threats, Vulnerabilities, and Mitigations](security-plus/secplus-03-threats-mitigations.en.md) | 2026-08-05 |
| 4 | [安全架構與設計](security-plus/secplus-04-security-architecture.zh.md) | [Security Architecture & Design](security-plus/secplus-04-security-architecture.en.md) | 2026-08-05 |
| 5 | [安全營運（監控、加固、應變）](security-plus/secplus-05-security-operations.zh.md) | [Security Operations (Monitoring, Hardening, Response)](security-plus/secplus-05-security-operations.en.md) | 2026-08-05 |
| 6 | [治理、風險與法遵](security-plus/secplus-06-governance-risk.zh.md) | [Governance, Risk, and Compliance](security-plus/secplus-06-governance-risk.en.md) | 2026-08-05 |
| 7 | [概念複習與練習題](security-plus/secplus-07-practice-questions.zh.md) | [Concept Review & Practice Questions](security-plus/secplus-07-practice-questions.en.md) | 2026-08-05 |

## 04 · Offensive Security (authorized / CTF) / 攻防基礎（授權 / CTF） (16 篇)

> Recon, password attacks & defenses, and web application security.

### Recon / 偵察與列舉  (3 篇)

> OSINT, search techniques, and nmap scanning & enumeration.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [偵察與 OSINT：公開資訊的力量](recon/recon-01-recon-osint.zh.md) | [Recon & OSINT: The Power of Public Information](recon/recon-01-recon-osint.en.md) | 2026-08-05 |
| 2 | [Google / Shodan 搜尋技巧](recon/recon-02-dorking.zh.md) | [Search Techniques: Google Dorks & Shodan](recon/recon-02-dorking.en.md) | 2026-08-05 |
| 3 | [掃描與列舉：nmap 入門](recon/recon-03-scanning-nmap.zh.md) | [Scanning & Enumeration: nmap for Beginners](recon/recon-03-scanning-nmap.en.md) | 2026-08-05 |

### Passwords / 密碼安全  (5 篇)

> How passwords are stored, how cracking works, and how to defend.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [密碼如何被儲存（hash / salt / pepper）](password/pass-01-how-passwords-are-stored.zh.md) | [How Passwords Are Actually Stored](password/pass-01-how-passwords-are-stored.en.md) | 2026-08-05 |
| 2 | [破解的機制：字典、暴力、規則與 GPU](password/pass-02-cracking-101.zh.md) | [How Password Cracking Works](password/pass-02-cracking-101.en.md) | 2026-08-05 |
| 3 | [hashcat 與 John the Ripper 實作](password/pass-03-cracking-tools.zh.md) | [hashcat & John the Ripper in Practice](password/pass-03-cracking-tools.en.md) | 2026-08-05 |
| 4 | [防禦：MFA、密碼管理器與鎖定策略](password/pass-04-defenses.zh.md) | [Defending: MFA, Password Managers, and Lockout](password/pass-04-defenses.en.md) | 2026-08-05 |
| 5 | [真實外洩案例教訓](password/pass-05-real-breaches.zh.md) | [Lessons from Real Password Breaches](password/pass-05-real-breaches.en.md) | 2026-08-05 |

### Web Security / Web 安全  (8 篇)

> OWASP Top 10, injection, auth, and secure development.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [OWASP Top 10 總覽](websec/web-01-owasp-top10.zh.md) | [The OWASP Top 10, Explained](websec/web-01-owasp-top10.en.md) | 2026-08-05 |
| 2 | [注入：SQLi / XSS / 命令注入與防禦](websec/web-02-injection.zh.md) | [Injection: SQLi, XSS, Command Injection & Defenses](websec/web-02-injection.en.md) | 2026-08-05 |
| 3 | [認證與 Session 安全](websec/web-03-auth-session.zh.md) | [Authentication & Session Security](websec/web-03-auth-session.en.md) | 2026-08-05 |
| 4 | [SSRF、CSRF 與檔案上傳](websec/web-04-ssrf-csrf-upload.zh.md) | [SSRF, CSRF, and File Upload](websec/web-04-ssrf-csrf-upload.en.md) | 2026-08-05 |
| 5 | [打造安全的 Web App / API](websec/web-05-securing-web-apps.zh.md) | [Building a Secure Web App / API](websec/web-05-securing-web-apps.en.md) | 2026-08-05 |
| 6 | [IDOR 與存取控制](websec/web-06-idor-access-control.zh.md) | [IDOR & Access Control](websec/web-06-idor-access-control.en.md) | 2026-08-05 |
| 7 | [業務邏輯漏洞](websec/web-07-business-logic.zh.md) | [Business Logic Flaws](websec/web-07-business-logic.en.md) | 2026-08-05 |
| 8 | [XXE 與不安全的反序列化](websec/web-08-xxe-deserialization.zh.md) | [XXE & Insecure Deserialization](websec/web-08-xxe-deserialization.en.md) | 2026-08-05 |

## 05 · Tools & Kali Linux / 工具與 Kali Linux (14 篇)

> The Kali toolchain, hands-on.

### Kali & Tools / Kali 與工具  (14 篇)

> A tour of Kali Linux and hands-on security tools.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [Kali Linux 是什麼、合法使用與授權](kali/kali-01-what-is-kali.zh.md) | [Kali Linux: What It Is, Legal Use & Authorization](kali/kali-01-what-is-kali.en.md) | 2026-08-05 |
| 2 | [安裝與建置自己的練習環境（VM / CTF）](kali/kali-02-install-lab.zh.md) | [Install & Build Your Own Practice Lab (VM / CTF)](kali/kali-02-install-lab.en.md) | 2026-08-05 |
| 3 | [工具地圖：按類別總覽](kali/kali-03-tool-catalog.zh.md) | [The Tool Catalog: A Category Map](kali/kali-03-tool-catalog.en.md) | 2026-08-05 |
| 4 | [Burp Suite：網頁測試](kali/kali-04-burp-suite.zh.md) | [Burp Suite: Web Testing](kali/kali-04-burp-suite.en.md) | 2026-08-05 |
| 5 | [Wireshark：封包分析](kali/kali-05-wireshark.zh.md) | [Wireshark: Packet Analysis](kali/kali-05-wireshark.en.md) | 2026-08-05 |
| 6 | [Metasploit：授權情境下的框架](kali/kali-06-metasploit.zh.md) | [Metasploit: The Framework, in Authorized Settings](kali/kali-06-metasploit.en.md) | 2026-08-05 |
| 7 | [列舉工具：gobuster / ffuf / nikto](kali/kali-07-enumeration-tools.zh.md) | [Enumeration Tools: gobuster / ffuf / nikto](kali/kali-07-enumeration-tools.en.md) | 2026-08-05 |
| 8 | [密碼工具實戰（hydra、hashcat）](kali/kali-08-password-tools.zh.md) | [Password Tools in Practice (hydra, hashcat)](kali/kali-08-password-tools.en.md) | 2026-08-05 |
| 9 | [SQLmap：自動化注入測試](kali/kali-09-sqlmap.zh.md) | [SQLmap: Automated Injection Testing](kali/kali-09-sqlmap.en.md) | 2026-08-05 |
| 10 | [Exploit-DB 與 searchsploit](kali/kali-10-exploitdb-searchsploit.zh.md) | [Exploit-DB & searchsploit](kali/kali-10-exploitdb-searchsploit.en.md) | 2026-08-05 |
| 11 | [鑑識工具（strings / exiftool / binwalk / Autopsy）](kali/kali-11-forensics-tools.zh.md) | [Forensics Tools (strings / exiftool / binwalk / Autopsy)](kali/kali-11-forensics-tools.en.md) | 2026-08-05 |
| 12 | [OSINT 工具（theHarvester / recon-ng / Maltego）](kali/kali-12-osint-tools.zh.md) | [OSINT Tools (theHarvester / recon-ng / Maltego)](kali/kali-12-osint-tools.en.md) | 2026-08-05 |
| 13 | [無線工具：Wi-Fi 測試](kali/kali-13-wireless-tools.zh.md) | [Wireless Tools: Wi-Fi Testing](kali/kali-13-wireless-tools.en.md) | 2026-08-05 |
| 14 | [逆向工程工具（Ghidra / radare2）](kali/kali-14-reversing-tools.zh.md) | [Reverse-Engineering Tools (Ghidra / radare2)](kali/kali-14-reversing-tools.en.md) | 2026-08-05 |

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

## 07 · Practice Labs (free HackTheBox) / 實戰練習場 (9 篇)

> Build your own lab and learn CTF categories hands-on.

### Practice Labs / 實戰練習場  (9 篇)

> Build your own lab and practice CTF categories — a free HackTheBox-style path.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [建置自己的練習實驗室（VM + Kali + 靶機）](labs/lab-01-build-your-lab.zh.md) | [Build Your Own Practice Lab (VM + Kali + Targets)](labs/lab-01-build-your-lab.en.md) | 2026-08-05 |
| 2 | [刻意脆弱的靶機（Metasploitable / DVWA / Juice Shop）](labs/lab-02-vulnerable-targets.zh.md) | [Deliberately Vulnerable Targets (Metasploitable / DVWA / Juice Shop)](labs/lab-02-vulnerable-targets.en.md) | 2026-08-05 |
| 3 | [CTF 怎麼運作（旗標格式、題型、倫理）](labs/lab-03-ctf-101.zh.md) | [How CTF Works (Flag Formats, Categories, Ethics)](labs/lab-03-ctf-101.en.md) | 2026-08-05 |
| 4 | [Crypto 題型入門](labs/lab-04-crypto-challenges.zh.md) | [Crypto Challenges, Introduced](labs/lab-04-crypto-challenges.en.md) | 2026-08-05 |
| 5 | [Forensics 題型入門](labs/lab-05-forensics-challenges.zh.md) | [Forensics Challenges, Introduced](labs/lab-05-forensics-challenges.en.md) | 2026-08-05 |
| 6 | [OSINT 題型入門](labs/lab-06-osint-challenges.zh.md) | [OSINT Challenges, Introduced](labs/lab-06-osint-challenges.en.md) | 2026-08-05 |
| 7 | [Web 題型入門](labs/lab-07-web-challenges.zh.md) | [Web Challenges, Introduced](labs/lab-07-web-challenges.en.md) | 2026-08-05 |
| 8 | [Reverse / Pwn 基礎（概念性、教育性）](labs/lab-08-reverse-pwn-basics.zh.md) | [Reverse & Pwn Basics (Conceptual, Educational)](labs/lab-08-reverse-pwn-basics.en.md) | 2026-08-05 |
| 9 | [攻略與練功的倫理準則](labs/lab-09-walkthrough-ethics.zh.md) | [Walkthroughs & Practice Ethics](labs/lab-09-walkthrough-ethics.en.md) | 2026-08-05 |

## 08 · Defense & Career / 藍隊與職涯 (10 篇)

> Hardening, detection, incident response, forensics, and how to enter the field.

### Blue Team / 藍隊防禦  (7 篇)

> Hardening, detection, incident response, and forensics.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [系統加固與安全設定](blueteam/blue-01-hardening.zh.md) | [System Hardening & Secure Config](blueteam/blue-01-hardening.en.md) | 2026-08-05 |
| 2 | [日誌與 SIEM 偵測](blueteam/blue-02-logging-siem.zh.md) | [Logs & SIEM Detection](blueteam/blue-02-logging-siem.en.md) | 2026-08-05 |
| 3 | [威脅情資](blueteam/blue-03-threat-intel.zh.md) | [Threat Intelligence](blueteam/blue-03-threat-intel.en.md) | 2026-08-05 |
| 4 | [事件應變](blueteam/blue-04-incident-response.zh.md) | [Incident Response](blueteam/blue-04-incident-response.en.md) | 2026-08-05 |
| 5 | [數位鑑識基礎](blueteam/blue-05-forensics.zh.md) | [Digital Forensics Basics](blueteam/blue-05-forensics.en.md) | 2026-08-05 |
| 6 | [釣魚與社交工程防禦](blueteam/blue-06-phishing-defense.zh.md) | [Phishing & Social Engineering Defense](blueteam/blue-06-phishing-defense.en.md) | 2026-08-05 |
| 7 | [IAM、MFA 與零信任](blueteam/blue-07-iam-zero-trust.zh.md) | [IAM, MFA, and Zero Trust](blueteam/blue-07-iam-zero-trust.en.md) | 2026-08-05 |

### Career / 職涯與倫理  (3 篇)

> Learning paths, certifications, ethics, and the law of authorized testing.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [學習路徑與練習場地](career/career-01-learning-path.zh.md) | [Learning Path & Practice Grounds](career/career-01-learning-path.en.md) | 2026-08-05 |
| 2 | [證照與職涯](career/career-02-certifications.zh.md) | [Certifications & Careers](career/career-02-certifications.en.md) | 2026-08-05 |
| 3 | [授權測試與法律 / 倫理](career/career-03-ethics-law.zh.md) | [Authorized Testing: Law & Ethics](career/career-03-ethics-law.en.md) | 2026-08-05 |

## 09 · Blockchain Security / 區塊鏈安全 (4 篇)

> Ledgers, keys, smart-contract flaws, and on-chain practice.

### Blockchain / 區塊鏈安全  (4 篇)

> Ledgers, keys, smart-contract flaws, and on-chain practice.

| # | 文章 (中文) | English | 日期 |
|---|---|---|---|
| 1 | [區塊鏈基礎：一條不能改的帳本](blockchain/chain-01-blockchain-basics.zh.md) | [Blockchain Basics: A Ledger Nobody Can Rewrite](blockchain/chain-01-blockchain-basics.en.md) | 2026-08-05 |
| 2 | [錢包、金鑰與交易](blockchain/chain-02-wallets-keys-transactions.zh.md) | [Wallets, Keys, and Transactions](blockchain/chain-02-wallets-keys-transactions.en.md) | 2026-08-05 |
| 3 | [智能合約漏洞（概念性、教育性）](blockchain/chain-03-smart-contract-vulns.zh.md) | [Smart Contract Vulnerabilities (Conceptual, Educational)](blockchain/chain-03-smart-contract-vulns.en.md) | 2026-08-05 |
| 4 | [區塊鏈安全實務](blockchain/chain-04-blockchain-security-practice.zh.md) | [Blockchain Security in Practice](blockchain/chain-04-blockchain-security-practice.en.md) | 2026-08-05 |

> ⚠️ **Legal & ethical notice:** all techniques here are for **authorized testing of your own systems, CTF labs, and defensive research**. Unauthorized testing of systems you do not own is illegal in most jurisdictions. Always get written permission first.

## 📬 Contribute

Articles are in Markdown (converted from the interactive MDX originals on the website). Feel free to open issues or PRs for corrections or new topics.

---

_Built with ❤️ — a bilingual book on cybersecurity, from the [myself](https://blog-916bd.web.app) website._
