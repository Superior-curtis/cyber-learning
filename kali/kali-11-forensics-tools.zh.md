# 鑑識工具（strings / exiftool / binwalk / Autopsy）

> 📅 2026-08-05 · 使用教學
> 數位鑑識是「從留下的痕跡裡還原真相」。strings、exiftool、binwalk、Autopsy 是四把最常用的鑑識工具。這篇帶你認識它們各自挖出什麼，並為 labs 的鑑識題型鋪路。

---

`blue-05-forensics` 會系統地講數位鑑識。這篇先從工具面切入：Kali 裡有四把最常用的鑑識工具——**strings、exiftool、binwalk、Autopsy**——每一把挖出的線索類型都不同。先認識工具，之後的鑑識題型（`lab-05-forensics-challenges`）才有手感。

## 數位鑑識在做什麼

數位鑑識的目標一句話：**從「留下來的痕跡」還原「發生過什麼」。** 無論是調查事件（`blue-04-incident-response`）或解 CTF 鑑識題，核心都是同一件事——問「這個檔案、這張圖片、這份記憶體裡，藏著什麼線索？」

## strings：從檔案裡撈文字

`strings` 從二進位檔案裡撈出「可讀的文字片段」。圖片、程式、壓縮檔裡，常常藏著肉眼看不到的字串：

```
# 從一個檔案裡撈出所有可讀字串
strings suspicious.png | less
```

在 CTF 鑑識題裡，這常常是第一招：**旗標（flag）可能就是被塞在圖片或檔案結尾的一段文字。** 而 `lab-03-ctf-101` 會教你 flag 長什麼樣。

## exiftool：讀元資料

`exiftool` 讀取檔案的**元資料（metadata）**——拍攝時間、GPS 座標、相機型號、作者、註解等：

```
# 讀取一張圖片的元資料
exiftool photo.jpg
```

元資料是鑑識的寶藏：一張「看起來普通的照片」，可能就藏著拍攝地點或原始作者等關鍵線索。OSINT（`recon-01-recon-osint`）也常用它。

## binwalk：拆嵌入式檔案

`binwalk` 用來分析**可能嵌在另一個檔案裡的檔案**——例如圖片裡藏著一個壓縮檔，或韌體裡藏著多個部分：

```
# 分析檔案裡嵌了哪些東西
binwalk firmware.bin
```

它常與 `strings` 搭配：`strings` 找出「疑似有料」，`binwalk` 把「藏著的料」拆出來。

## Autopsy：完整鑑識平台

前三個是單點工具；**Autopsy** 是一套完整的鑑識平台，有圖形介面，能把磁碟映像檔、檔案系統、刪除檔案、瀏覽記錄等系統性地分析：

| 工具 | 擅長 | 一句話定位 |
|---|---|---|
| strings | 撈可讀字串 | 找藏著的文字 |
| exiftool | 讀元資料 | 找檔案的「身分證」 |
| binwalk | 拆嵌入式檔案 | 把藏著的檔案挖出來 |
| Autopsy | 整套磁碟鑑識 | 完整的案件分析平台 |

> 鑑識的思考順序：先問「這是什麼檔案」，再問「裡面藏了什麼」。 strings 是起手式，exiftool 補上背景，binwalk 挖深，Autopsy 處理大規模案件。

## 練習：CTF 鑑識題

Kali 的鑑識工具，最好的練習場就是 **CTF 鑑識題**——`lab-05-forensics-challenges` 會教你怎麼系統地解。原則很簡單：**拿到一個檔案，別急著猜，先讓工具說話。** strings、exiftool、binwalk 輪流上，答案往往就浮出來了。

## 下一步

工具面鋪好了。最後補上 OSINT 那一塊：`kali-12-osint-tools` 帶你認識公開資訊蒐集工具——theHarvester、recon-ng、Maltego——把 `recon-01` 與 `recon-02` 的方法變成半自動化流程。

#### Q: CTF 鑑識題拿到一個可疑檔案，最常先用的工具是哪一把？

* Autopsy

* strings——先撈出檔案裡可讀的文字線索

* Metasploit

* hashcat

> 💡 strings 是最快的起手式：從二進位檔案撈出可讀字串，旗標常就藏在其中。
