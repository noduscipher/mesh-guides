# Meshtastic for Beginners — Portugal/Europe

**Version:** 1.0 · **Date:** 2026-05-07 · **License:** CC BY-SA 4.0
**For people who've never heard of Meshtastic and want to get started.**

> 📄 Read this in: [PT](../../pt/user/meshtastic-beginner-guide.md) · EN · [ES](../../es/user/meshtastic-beginner-guide.md)

---

## ⚠️ Important before you start

**This guide is for Portugal and Europe (868 MHz band).**

Simple rules:

- ✅ Always set the region to **"EU_868"** (Europe 868 MHz)
- ❌ **Never** use other regions (US, Japan, China, Australia) in Portugal
- ⚠️ Wrong region = illegal in Portugal and works poorly

If you have doubts, ask the [Meshtastic PT community](https://meshtastic.pt) or join the [Telegram group](https://t.me/comunidademeshtasticpt).

---

## Table of Contents

1. [Welcome to Meshtastic](#1-welcome-to-meshtastic)
2. [First steps — what do I need?](#2-first-steps--what-do-i-need)
3. [Step-by-step installation](#3-step-by-step-installation)
4. [Daily use](#4-daily-use)
5. [Simple settings](#5-simple-settings)
6. [Common problems (FAQ)](#6-common-problems-faq)
7. [Useful tips](#7-useful-tips)
8. [Join the community](#8-join-the-community)
9. [Quick FAQ](#9-quick-faq)

---

## 1. Welcome to Meshtastic

### What is it?

Imagine **smart walkie-talkies** that help each other relay messages. Each device receives a message, and if it's not for them, **passes it to the next** until it reaches its destination.

All of this **without internet, without a carrier, without a SIM card**.

### Why use it?

- 📵 **Works without mobile network** — useful in mountains, sea, events, emergencies
- 🔒 **Private messages** — automatically encrypted
- 🌍 **Long range** — typically 5 to 15 km, more with elevation
- 💰 **Cheap hardware** — between €30 and €100 per device
- 🔋 **Low consumption** — battery lasts days
- 🤝 **Community** — Portugal already has hundreds of users

### How it works (simply)

1. Your Meshtastic device is a small radio with a battery
2. It connects to your phone via Bluetooth
3. You write messages in the phone app
4. The device sends by radio (868 MHz band, long range)
5. Other nearby devices receive or **forward** to those further away
6. When it arrives at the destination, it appears in that person's app

The "**mesh**" means **all devices help all the others** — like a network of friends where each one passes the word along.

---

## 2. First steps — what do I need?

### The essentials

| Item | What it is | Approximate cost |
|---|---|---|
| 1× Meshtastic device | Small hardware with LoRa radio | €30–80 |
| 1× 868 MHz antenna | Critical! Included in most | (included) |
| 1× Smartphone | Recent Android or iOS | (you have it) |
| 1× USB-C data cable | For first-time configuration | €5 |

### Where to buy?

**Online stores shipping to PT:**

- [Tinytronics (NL)](https://www.tinytronics.nl) — good price, EU without customs
- [OpenELab (DE)](https://eu.openelab.io) — European Heltec distributor
- [PTRobotics](https://www.ptrobotics.com) — Portugal
- [Mauser](https://mauser.pt) — Portugal

### Which device to choose?

**To start (cheapest):**
- **Heltec WiFi LoRa 32 V3** (~€25–30) — small, with OLED display
- **Heltec Wireless Stick Lite V3** (~€20) — no display, even cheaper

**For full use (with keyboard and screen):**
- **LilyGO T-Deck Plus** (~€70) — standalone, QWERTY keyboard, touchscreen
  - For specifics, see [T-Deck Plus Complete Guide](https://github.com/noduscypher/tdeck-guides)

**For backpack and GPS:**
- **LilyGO T-Beam V1.2** (~€40) — with built-in GPS

> 💡 For a detailed comparison, see the [meshtastic.pt hardware comparator](https://meshtastic.pt/comparator).

### What to check before buying?

- ✅ **868 MHz band** (NOT 915 MHz which is US, NOT 433 MHz)
- ✅ Antenna included (most include, but confirm)
- ✅ USB-C cable (some come without)

---

## 3. Step-by-step installation

### ⚠️ RULE #1 before anything

**ALWAYS connect the antenna BEFORE turning on the device.**

If you turn it on without an antenna, you can **permanently damage** the radio part. Without warning. Without recovery. Screw the antenna firmly before plugging in USB or battery.

### 3.1. Install firmware (the device's "soul")

> 💡 "Firmware" is the program that makes the device work. You install it the first time (and update occasionally).

1. **Computer with Google Chrome** (not Firefox — doesn't work here)
2. Connect the device to the computer via a USB-C cable **(data cable, not charge-only)**
3. Open <https://flasher.meshtastic.org>
4. Click **"Connect"** and select your device from the list
5. Choose the **latest Beta version** (recommended — has recent fixes)
6. Check **"Full Erase"** only the first time
7. Click **"Flash"** and wait (1–3 minutes)
8. **Don't unplug** the cable until it's done
9. When done, turn the device off and on again

### 3.2. Install the app on the phone

**Android:**

- [Meshtastic on Play Store](https://play.google.com/store/apps/details?id=com.geeksville.mesh)

**iOS:**

- [Meshtastic on App Store](https://apps.apple.com/pt/app/meshtastic/id1586432531)

Install and open.

### 3.3. Connect the phone to the device

1. **On the device:** turn it on (power button)
2. **On the phone:** go to the Meshtastic app → "+" → "Connect"
3. Select your device (name like "Meshtastic_xxxx")
4. Accept Bluetooth pairing if asked

### 3.4. Basic configuration (CRITICAL)

This is the most important part. Without this, your node won't work properly in Portugal.

**In the app, go to Settings (gear icon) → Radio Configuration → LoRa:**

| Setting | Value |
|---|---|
| **Region** | **Europe 868** ⚠️ mandatory in PT |
| **Modem Preset** | LongFast (leave default) |
| **Hop Limit** | 3 (default) or 4 (PT convention) |

**Go to User:**

- **Long Name:** your name or nickname (e.g., `Duarte-8`)
- **Short Name:** 4-character abbreviation (e.g., `DUA8`)

> 💡 **Convention in Portugal:** many users add `-8` to the end of the name to indicate they're using 868 MHz. It's optional but helps identify who's on which band. Not mandatory.

**Go to MQTT** (to connect to the Portuguese community network):

| Setting | Value |
|---|---|
| **Address** | `mqtt.meshtastic.pt` |
| **Username** | `meshdev` |
| **Password** | `large4cats` |
| **Root Topic** | `msh` |
| **Encryption Enabled** | ✅ ON |
| **TLS Enabled** | ❌ OFF |

> ✓ These are the official public credentials of the meshtastic.pt network, verified on 2026-05-07.

**Save / Apply.** The device reboots.

Done! You're on the network.

---

## 4. Daily use

### 4.1. Send messages

1. In the app, go to **"Channels"** (channel list)
2. The default channel is called **"LongFast"** — it's the public channel, everyone sees
3. Type the message
4. Send

> ⚠️ **Important:** messages on "LongFast" are seen by **ALL Meshtastic users** within range and on the MQTT network. **It's not private.**

### 4.2. See other nodes

- **List:** shows all devices yours has detected
- **Map:** shows them geographically (if they have GPS active)
- For each node you see: distance, signal (RSSI), last activity

### 4.3. Direct messages (DM)

1. Tap a specific node in the list
2. Tap "Direct Message"
3. Type — only that person receives

> 🔒 **Direct messages are end-to-end encrypted automatically.** Even when relayed by other devices, no one in the middle can read them.

### 4.4. Short messages, please!

Meshtastic is **not WhatsApp**. Bandwidth is very low.

- ✅ Short messages (up to ~200 characters)
- ✅ Spaced conversations
- ❌ Don't send photos (not supported)
- ❌ Don't continuously chat on the public channel

---

## 5. Simple settings

### 5.1. Privacy

**I don't want to share location:**

- Go to **Position** → disable "GPS Enabled"

**I want to share but with less precision:**

- Position → "Position Precision" → reduce to 14 (~1.5 km error)

### 5.2. Save battery

- **CLIENT_MUTE mode:** if you have multiple close nodes of yours, set this one to receive but not retransmit → saves battery
- **Reduce broadcast intervals:** Position Broadcast at 900 s (15 min) instead of 30 s

### 5.3. Improve range

- **Better external antenna** (5 dBi is a good choice)
- **Elevated position** — balcony, high window, rooftop
- **Line of sight** with other nodes — no buildings between you
- **Not in basement** — RF doesn't pass through cement + earth well

---

## 6. Common problems (FAQ)

### "I don't see messages / I don't receive"

**Check:**

1. Is region set to **EU_868**?
2. Antenna connected (even just to receive)?
3. Are there other nodes in your area? See the [meshtastic.pt map](https://meshtastic.pt/map)
4. Device actually on and listening?

### "Device won't connect to the app"

**Check:**

1. Bluetooth enabled on the phone?
2. Device on with LED blinking?
3. Restart the app
4. "Forget" the old pairing in phone Bluetooth and redo

### "Battery drains too fast"

**Solutions:**

1. CLIENT_MUTE mode if multiple close nodes
2. Disable GPS if you don't need it
3. Disable WiFi (just use Bluetooth)
4. Reduce TX Power to 17 dBm instead of 27 dBm (range decreases proportionally)

### "Very low range"

**Check:**

1. Antenna correct for 868 MHz (not 915, not 433)?
2. Connector clean and well-tightened?
3. Position (interior vs exterior, height)?
4. Other nodes too far? (map)

### "T-Deck Plus with touch problems"

→ See [T-Deck Plus Touch Fix Guide](https://github.com/noduscypher/tdeck-guides) — dedicated document.

---

## 7. Useful tips

### Good mesh citizenship

- ✅ **Short, spaced messages** — everyone shares the channel
- ✅ **Long conversations → dedicated private channel**
- ✅ **Unique and identifiable name** — avoid "Node1", "Test"
- ✅ **Help new users** — you were once a newbie too
- ❌ **No spam, no bots on public channel**
- ❌ **Don't test Range Test mode on public channel** (sends automatic spam)

### When NOT to use Meshtastic

- ❌ For serious emergencies (use 112)
- ❌ For long conversations (use Signal/Telegram)
- ❌ To send photos or files
- ❌ For WhatsApp-style chat

### When it IS perfect

- ✅ Coordination on hiking without coverage
- ✅ Quick status updates ("arrived", "I'm here")
- ✅ Events with lots of people (festivals, raids)
- ✅ Local technical community
- ✅ Backup for when mobile network fails

---

## 8. Join the community

The Meshtastic Portugal community is active, welcoming, and helps newcomers.

| Resource | Link |
|---|---|
| **Official PT website** | <https://meshtastic.pt> |
| **PT getting started** | <https://meshtastic.pt/getstarted> |
| **PT FAQ** | <https://meshtastic.pt/faq> |
| **PT nodes map** | <https://meshtastic.pt/map> |
| **Network status** | <https://malha.meshtastic.pt> |
| **Community Telegram** | <https://t.me/comunidademeshtasticpt> |
| **Matrix** | <https://matrix.to/#/!BLCckQWdGZvhvgYrMW:matrix.org> |
| **Instagram** | <https://www.instagram.com/meshtastic.pt> |
| **Facebook group** | <https://www.facebook.com/groups/www.meshtastic.pt/> |
| **YouTube** | <https://www.youtube.com/channel/UC3awqON_H724kV4prigjPSA> |

Occasional events (meetups, demos, hackathons) announced on the [meshtastic.pt calendar](https://meshtastic.pt/eventos).

---

## 9. Quick FAQ

**Is it legal to use Meshtastic in Portugal?**
Yes, no licence required, as long as you use the `EU_868` or `EU_433` region and don't change power/duty cycle above defaults. ANACOM allows SRDs in these bands freely for personal use.

**Do I need a ham radio licence?**
No. Meshtastic in standard mode is a Short Range Device (SRD) — doesn't require a licence.

**Does it work without internet?**
Yes, that's the main point. Devices communicate directly by radio. Internet is only needed if you want to connect to the global MQTT network (optional).

**What's the range?**
Highly terrain- and antenna-dependent:
- Dense city: typically 1–3 km
- Suburban: 3–8 km
- Rural with elevation: 10–20 km+
- World records: 200+ km with extreme elevation (mountain-to-sea)

**Can I use it in the city?**
Yes, but range is reduced by buildings. In dense mesh networks (many people with nodes), messages travel further via retransmission.

**How much does it cost to start?**
Minimum: €30 (Heltec V3 + antenna, you already have cable and phone). Comfortable setup: €50–100.

**How much battery does it use?**
In normal CLIENT mode: 1–3 days with 18650 battery. In CLIENT_MUTE: 1–2 weeks. Fixed routers: continuous power.

**Can I use it traveling in Europe?**
Yes, EU_868 is harmonized across all EU + UK + Switzerland + Norway. In Portugal, Spain, France, Germany, etc. — works the same.

**And outside Europe?**
**You have to change the region** to that of the country you're in (`US`, `JP`, `AU`, etc.). 868 MHz antennas work poorly at other frequencies — may need a new antenna.

**Is it safe / private?**
- Direct messages (DM): end-to-end encrypted (PKI), only recipient reads
- Private channels (with strong PSK): encrypted, only those with the key read
- Public "LongFast" channel: anyone in range or on the MQTT network reads

For serious privacy needs (advanced threats), use Signal or SimpleX. Meshtastic is good for casual use but wasn't designed for adversarial use.

---

## Next steps

When you want to dig deeper:

- [Meshtastic Technical Manual](../technical/meshtastic-technical-guide.md) — to understand ETSI compliance, advanced modes, detailed configurations
- [T-Deck Plus Guides](https://github.com/noduscypher/tdeck-guides) — specific to T-Deck hardware
- [meshtastic.pt](https://meshtastic.pt) — living community resource

---

☥ walk quietly, ₿ut keep the signal alive ⚡

*noduscypher-rawmesh node*
*License: CC BY-SA 4.0*
