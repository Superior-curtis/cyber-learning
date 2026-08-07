# VoIP 通話嗅探：Wireshark 重組錄音

> 📅 2026-08-05 · 深入研究
> 有人問：用 Wireshark 監聽通話、存下錄音，那是 social engineering、MITM 還是 sniffing？答案是 sniffing。這篇講清分類、VoIP（SIP/RTP）的邏輯、以及「為什麼加密通話抓不到」。homelab only。

---

**問題**：「用 Wireshark 監聽通話、存下錄音檔——那是 social engineering、MITM 還是 sniffing？」

**答案**：核心是 **sniffing（封包嗅探）**。這篇把分類講清楚、把 VoIP 的邏輯拆開，也講為什麼「加密通話抓不到」。

> 安全說明（HOMELAB ONLY）：抓取你自己的測試通話（例如自己架的 SIP 伺服器、自己的兩台測試機）是練習；攔截真實或他人的通話，在多數地方是嚴重的竊聽罪。

## 先回答分類

把三種東西的界線畫清楚：

| 分類 | 做什麼 | 本例 |
|---|---|---|
| **Sniffing** | 被動抓取網路上的封包 | ✅ 主要——Wireshark 抓 SIP/RTP 並重組 |
| **MITM** | 主動把流量導向自己（如 ARP 欺騙） | ⚠️ 交換式網路上要看到他人流量才需要 |
| **Social engineering** | 騙人上鉤 | ❌ 除非「誘使對方在你能監聽的網路通話」才是 |

> 一句話記憶：監聽 = sniffing；主動把流量導向你 = MITM；騙你上鉤 = social engineering。 Wireshark 監聽通話本質是 sniffing——頂多在交換式網路上需要 MITM 的手法來「導流」。

## VoIP 的邏輯：為什麼能「重組錄音」

VoIP 通話由兩部分組成（`net-01-network-fundamentals` 的 UDP 相關）：

* **SIP**：信令——建立/結束通話（誰打給誰）。
* **RTP**：實際的音訊資料——以 UDP 封包流送出。

Wireshark 能做的：把抓到的 RTP 封包**依序重組**成連續的音訊，然後播放或存檔——`Telephony → VoIP Calls` 就是這個功能。

**前提是「不加密」**：如果通話用了 SRTP（加密的 RTP），抓到的就是亂碼——除非你有金鑰。而 WhatsApp、iMessage、一般手機通話都走加密，**根本抓不到音訊**。

## homelab 練習（只限自己的測試通話）

要練習「看懂 VoIP + 重組音訊」，正確的做法是**自己架一組不加密的 VoIP**：

```bash
# 在你自己的 homelab 上架一台 SIP 伺服器（只限測試）
sudo apt install asterisk

# 在 Wireshark 上抓你自己機器的流量，用兩台測試機打一通測試電話
wireshark
# → Telephony → VoIP Calls → 挑選通話 → Play Streams
```

用**你自己架的伺服器 + 你自己打的測試電話**，你就能親眼看到 SIP 與 RTP、並把音訊重組——完全不碰任何人的真實通話。

> 安全說明（再次強調）：這個練習的對象是你自己的 SIP 伺服器與你的測試機。對任何真實通話做這件事，就是竊聽——犯罪且可能讓你吃上官司。

## 藍隊防禦

* **加密所有 VoIP**：啟用 SRTP，讓 RTP 抓了也是亂碼。
* **網路分段**：讓敏感通話只在受控網段（`net-01` 網段）。
* **交換式網路 + 埠安全**：減少「被導流」的可能（對抗 MITM 成分）。
* **監控**：異常的大量 RTP 流量、ARP 欺騙偵測（`blue-02-logging-siem`）。

## 下一步

VoIP 嗅探的分類與邏輯清楚了。想複習封包分析的基礎，`kali-05-wireshark`；想認識 MITM 那一側，`kali-19-responder-mitm` 與 `net-01-network-fundamentals` 打底。

#### Q: 『用 Wireshark 監聽通話存錄音』最正確的分類是什麼？

* Social engineering

* Sniffing——被動抓取並重組 VoIP 封包；交換式網路上要看他人流量才額外需要 MITM

* DoS 攻擊

* 提權

> 💡 監聽本質是 sniffing；MITM 只在需要主動導流時出現；social engineering 是指『誘人上鉤』的那一層。
