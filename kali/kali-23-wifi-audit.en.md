# Wi-Fi Audit: Verifying Your Own AP Encryption

> 📅 2026-08-05 · Getting Started
> Is your Wi-Fi password actually strong enough? A Wi-Fi audit uses monitor mode + handshake + offline testing to verify the encryption strength of your own AP. This article explains the logic, homelab commands, and why it is limited to your own AP.

---

`net-03-wifi-security` taught you how to configure Wi-Fi encryption. Now verify it hands-on: **does your Wi-Fi password actually survive an offline test?** That is a **Wi-Fi audit.**

> Security note (HOMELAB ONLY): every step below is permitted only against your own wireless network/router. Intercepting or testing someone else's Wi-Fi signal is a serious crime in most places — even just "looking."

## What a Wi-Fi audit does

The goal in one line: **use the attacker's view to verify your own wireless encryption strength.** It takes the 4-way handshake from `net-03-wifi-security` and "runs it" — but against your own AP.

## The logic: handshake and offline testing

Remember the 4-way handshake from `net-03-wifi-security`? The audit logic builds on it:

1. **Monitor mode**: switch the wireless card to monitor mode, receiving every packet in the air.
2. **Capture a handshake**: when **your own device** connects to **your own AP**, capture that handshake.
3. **Offline testing**: run a dictionary against the handshake — the mechanism of `pass-03-cracking-tools`.

**Key: the handshake does not expose the password, but it can be tested by "guessing with a dictionary" — and the speed of the guess is decided by your password strength.**

## Homelab commands (your own AP only)

The whole flow (the three tools from `kali-13-wireless-tools`):

```bash
# 1. List your wireless card and switch to monitor mode (HOMELAB ONLY)
sudo airmon-ng start wlan0

# 2. Scan and lock onto YOUR OWN AP (note the BSSID and channel)
sudo airodump-ng wlan0mon

# 3. Capture the handshake between YOUR OWN device and YOUR OWN AP
sudo airodump-ng -c <channel> --bssid <YOUR AP BSSID> -w capture wlan0mon

# 4. Run an offline test on the handshake (verify your own password strength)
sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt capture-01.cap
```

> Security note (again): the target of these steps is only your own AP and your own devices. Running them against anyone else's Wi-Fi is illegal interception of wireless communications — a criminal offense.

## How to read the result

The offline test tells you one thing directly: **how long your password survives in front of a dictionary.**

* Cracked in seconds → the password is too weak; replace it with the "long and random" from `pass-04-defenses`.
* Not cracked → your password strength is sufficient; the `net-03` advice worked.

> This is the crypto-02-password-hashing line, in Wi-Fi form: strength comes from "long and random," not "memorable." Wi-Fi passwords are no exception.

## The blue-team defense

| Measure | How |
|---|---|
| Encryption generation | WPA3 (or WPA2-AES); reject WEP/TKIP (`net-03`) |
| Password strength | Long, random passphrase; not a word or a date |
| Disable WPS | The WPS PIN can be brute-forced (`net-03` warning) |
| Hidden features | Keep the admin panel off the public internet |

> Top defensive mindset: a Wi-Fi audit is not "attack tutorial," it is "self-check." Scan yourself with the same tools first, and you will know whether to change the password or upgrade the generation — the "know your exposure first" of blue-01-hardening, in wireless form.

## Next

The Wi-Fi audit logic and commands are clear. To review the full wireless defense chapter, `net-03-wifi-security`; for the conceptual version of these tools, `kali-13-wireless-tools`.

#### Q: In a Wi-Fi audit, 'capture the handshake, then test offline' is actually testing what?

* The AP hardware speed

* Your Wi-Fi password strength — how fast a dictionary can guess it offline

* The router brand

* Signal strength

> 💡 The offline test guesses the password in the handshake against a dictionary; the faster it guesses, the weaker the password. That turns 'is my password strong enough' into a measurable question.
