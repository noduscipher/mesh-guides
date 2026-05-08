# Meshtastic Technical Manual — Portugal/Europe

**Version:** 1.0 · **Date:** 2026-05-07 · **License:** CC BY-SA 4.0
**Audience:** technicians, ham radio operators, sysadmins, network engineers
**Compliance:** ETSI EN 300 220 · ANACOM Portugal

> 📄 Read this in: [PT](../../pt/technical/meshtastic-technical-guide.md) · EN · [ES](../../es/technical/meshtastic-technical-guide.md)

---

## ⚠️ Legal notice and compliance

This manual documents Meshtastic configurations for Portugal and Europe, in accordance with:

- **ETSI EN 300 220** (Short Range Devices, 25 MHz to 1 000 MHz band)
- **ECC/REC 70-03** (CEPT recommendation for SRDs)
- **ANACOM Portugal regulations** (Portuguese national communications authority)

**Verified against official sources on 2026-05-07.** Regulations evolve — always confirm against current documentation before operating.

**User responsibilities:**

- ✅ Verify the region is correctly configured (`EU_868` or `EU_433`)
- ✅ Verify the chosen frequency slot falls within an ETSI sub-band compatible with the power used
- ✅ Never exceed legal power or duty cycle limits
- ⚠️ Using the wrong region (US, CN, JP, AU) in Portugal **is illegal**
- ⚠️ Consequences: ANACOM fine, interference with third parties, equipment damage

When in doubt, consult ANACOM, a qualified technician, or the [meshtastic.pt community](https://meshtastic.pt).

**The authors assume no responsibility for incorrect use.**

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [Compliance and regulation](#2-compliance-and-regulation)
3. [Hardware](#3-hardware)
4. [Detailed configuration](#4-detailed-configuration)
5. [Operating modes](#5-operating-modes)
6. [Best practices](#6-best-practices)
7. [Troubleshooting](#7-troubleshooting)
8. [Advanced topics](#8-advanced-topics)
9. [Appendices](#9-appendices)

---

## 1. Introduction

### 1.1. What is Meshtastic

Meshtastic is an open-source project that turns inexpensive LoRa radios into encrypted mesh-networked messaging devices, with no dependency on internet, mobile carriers, or centralized infrastructure.

Each device (called a "node") combines:

- A microcontroller (ESP32, NRF52, or RP2040)
- A LoRa transceiver (Semtech SX126x, SX127x, or LR11xx family)
- A battery (typically 18650 or LiPo)
- An external antenna for the regional band (868 MHz in Europe)

Optionally connected via Bluetooth to a smartphone running the official Meshtastic app, the node lets you send and receive text messages on a self-forming mesh network.

### 1.2. LoRa mesh architecture

The topology is a **dynamic mesh**: each node acts simultaneously as client and (if configured) repeater. Messages propagate through multiple hops to reach their destination, via store-and-forward routing.

Relevant technical characteristics:

- **Physical layer:** LoRa CSS (Chirp Spread Spectrum), proprietary Semtech modulation
- **MAC layer:** uncoordinated ALOHA with retransmissions
- **Network layer:** flood routing with hop limit and deduplication
- **Application layer:** text messages, GPS position, telemetry, sensors
- **Encryption:** AES-256-CTR (with per-channel PSK) + PKI for direct messages (E2EE)

Total bandwidth is low (hundreds of bps to kbps), which **defines the network's character**: concise, low-frequency, highly resilient communication.

### 1.3. Use cases

- **Off-grid communication:** areas without cellular coverage (hiking, mountain, coastal sea)
- **Emergencies:** resilient community network in disasters (fires, earthquakes)
- **Events:** coordination at festivals, raids, sports events
- **Community mesh:** linking neighborhoods, villages, or technical communities
- **Distributed telemetry:** low-power environmental sensors
- **Technical education:** practical introduction to mesh networks, RF, and LoRa

### 1.4. Who this manual is for

It assumes you have:

- Confidence with embedded device configuration (firmware flashing, CLI)
- Basic understanding of RF (frequency, power, duty cycle)
- Familiarity with networking (TCP/IP, MQTT, routing concepts)

If you're a beginner, start with the [Meshtastic Beginner Manual](../user/meshtastic-beginner-guide.md) and come back later.

---

## 2. Compliance and regulation

This is the most important section of the manual. Configuring a Meshtastic node in Portugal without understanding the regulatory framework can have legal consequences.

### 2.1. Legal framework

| Norm | Scope | Application to Meshtastic |
|---|---|---|
| **ETSI EN 300 220** | SRD (Short Range Device) technical standard | Defines RF limits for the 25 MHz – 1 000 MHz band |
| **ECC/REC 70-03** | CEPT recommendation (44 European countries) | Defines sub-band allocation, power, and duty cycles |
| **ANACOM** | Portuguese communications authority | Applies CEPT recommendations in national territory |
| **CE marking + RED** | Directive 2014/53/EU (Radio Equipment) | Equipment sold in EU must be compliant |

Most Meshtastic devices sold in the EU are CE marked, meaning **the hardware** is compliant. But the **use** (configured frequency, power, duty cycle) is the user's responsibility.

**In Portugal, SRD use in 868 MHz and 433 MHz bands does not require a licence** as long as operation stays within ETSI EN 300 220 limits.

### 2.2. ETSI 868 MHz sub-bands (band G: 863–870 MHz)

The main band used by Meshtastic in Europe is subdivided into 5 sub-bands, each with its own limits:

| Sub-band | Frequency (MHz) | Max ERP/EIRP | Duty cycle | Notes |
|---|---|---|---|---|
| **G** | 863.0 – 868.0 | ≤ 25 mW ERP | < 1% | General band |
| **G1** | 868.0 – 868.6 | ≤ 25 mW ERP | < 1% | Common in LoRaWAN uplink |
| **G2** | 868.7 – 869.2 | ≤ 25 mW ERP | < 0.1% | Restrictive |
| **G3** | **869.4 – 869.65** | **≤ 500 mW ERP** | **< 10%** | ⚡ High-power band |
| **G4** | 869.7 – 870.0 | ≤ 25 mW ERP | < 1% (or 5 mW @ 100%) | — |

The 869.65–869.7 MHz range is alarm band (social alarms), reserved and **not available** for general SRD use.

> ✓ Verified against ETSI TR 103 055, ETSI EN 300 220-2 V3.2.1, and multiple independent technical sources (Silicon Labs AN445, mobilefish.com tutorial 23, harald kreuzer).

> ⚠️ **Naming:** some older sources use lowercase (g, g1, g2, g3, g4). Current ETSI terminology uses UPPERCASE (G, G1, G2, G3, G4) with the table above. The limits are the same.

### 2.3. Meshtastic EU_868 → sub-band mapping

| Frequency Slot | Center frequency | ETSI sub-band | Legal max ERP | Legal duty cycle |
|---|---|---|---|---|
| **1** (default LongFast) | **869.525 MHz** | **G3** | **500 mW (27 dBm)** | **10%** |

> ✓ Verified against official Meshtastic documentation ([meshtastic.org/docs/configuration/radio/lora](https://meshtastic.org/docs/configuration/radio/lora/)) and [meshtastic.pt/getstarted](https://meshtastic.pt/getstarted).

**Practical implication:** LongFast slot 1 (default) **operates within G3**, so it can legally transmit up to **500 mW ERP** with **10%** duty cycle. This is the standard configuration in Portugal/EU and is fully compliant.

> ⚠️ **Other presets** (MediumFast, ShortFast, etc.) use frequency slots calculated algorithmically from the channel name. Those slots may fall in **G1 or G2**, with lower power limits (25 mW) and more restrictive duty cycles (1% or 0.1%).
>
> To check the exact slot and sub-band for a non-default preset, use the [meshtastic.pt power calculator](https://meshtastic.pt/tools/powercalc) or the [GitHub heypete calculator](https://github.com/heypete/meshtastic_frequency_slot_calculator).

### 2.4. EU_433 — summary

| Parameter | Value |
|---|---|
| Band | 433.05 – 434.79 MHz |
| Default frequency slot | 4 |
| Center frequency (slot 4) | 433.875 MHz |
| Typical permitted power | ≤ 10 mW ERP (varies by sub-band) |
| Duty cycle applied by Meshtastic firmware | 10% |

> ⚠️ The 433 MHz band has **multiple ETSI sub-bands** with distinct limits (some allow only 1 mW ERP, others 10 mW). For casual use with Meshtastic in PT/EU, the default configuration covers typical cases. For professional or commercial deployment, **consult ETSI EN 300 220-1/2** for the exact sub-band.

EU_433 is a minority choice in Portugal: most of the community uses 868 MHz due to range, hardware ecosystem, and easier-to-find antennas.

### 2.5. Duty cycle: what it is and how the firmware applies it

**Duty cycle** is the maximum percentage of time a device may transmit, calculated within a defined time window.

For Meshtastic in EU_868 and EU_433, the firmware applies:

- **Limit:** 10% duty cycle
- **Window:** rolling 1 hour, calculated per minute

This means: in one hour, the node can transmit at most **6 minutes** (10% of 60 min). If the limit is reached, the firmware automatically pauses transmissions until the window frees up.

**Implications:**

- The Meshtastic app shows current duty cycle in logs and in the app
- Long messages, frequent broadcasts (telemetry, position), or very dense networks may saturate
- If saturated: reduce broadcast intervals, switch to a faster preset (lower airtime), or add private channels

### 2.6. EIRP vs ERP, antenna gain

**Important distinction:**

- **ERP (Effective Radiated Power):** referenced to a dipole antenna
- **EIRP (Equivalent Isotropic Radiated Power):** referenced to an isotropic antenna

Conversion: **EIRP = ERP + 2.15 dB**.

ETSI regulation primarily uses **ERP** for SRD sub-bands. The "500 mW ERP" limit in G3 corresponds to approximately **820 mW EIRP**.

**Practical caution: antenna gain counts towards total radiated power.**

Critical example:

| Radio output | Antenna | Resulting EIRP | G3 compliant (500 mW = 27 dBm ERP ≈ 29 dBm EIRP)? |
|---|---|---|---|
| 27 dBm | 0 dBi (isotropic) | 29 dBm EIRP | ✅ At limit |
| 27 dBm | 3 dBi | 32 dBm EIRP | ❌ Exceeds |
| 27 dBm | 5 dBi | 34 dBm EIRP | ❌ Clearly exceeds |
| 24 dBm | 3 dBi | 29 dBm EIRP | ✅ Compliant |
| 22 dBm | 5 dBi | 29 dBm EIRP | ✅ Compliant |

> ⚠️ **Rule of thumb:** if you use antennas with gain above 0 dBi (most commercial antennas have 3–5 dBi), **reduce the radio's output power** to keep total EIRP within the legal limit.

Most commercial Meshtastic hardware ships with TX power configured at ≤ 22 dBm by default, leaving margin for antennas up to 5 dBi. But if you manually raise to 27 dBm on capable hardware (T-Beam, RAK 4631 with PA), **check the antenna**.

### 2.7. Other regions

| Region | Norm | Main band | Notes |
|---|---|---|---|
| **UK post-Brexit** | Maintains ETSI EN 300 220 | 868 MHz | Compatible with EU |
| **Switzerland, Norway** | ETSI | 868 MHz | Compatible |
| **USA / Canada** | FCC Part 15 | 902–928 MHz | Incompatible with EU |
| **Japan** | ARIB STD-T108 | 920–928 MHz | Incompatible |
| **China** | SRRC | 470–510 MHz / 779–787 MHz | Incompatible |
| **Australia / NZ** | ACMA | 915–928 MHz | Incompatible |

**This manual's focus:** Portugal and EU (including UK due to post-Brexit compatibility). For other regions, consult official Meshtastic documentation and local regulations.

### 2.8. Consequences of non-compliance

- **ANACOM:** can apply significant fines (up to tens of thousands of euros for unauthorized spectrum use). In practice, cases involving SRDs and LoRa are rare, but they exist.
- **Interference:** transmitting outside the permitted sub-band or above power limits can interfere with critical services (medical alarms, industrial telemetry, radio astronomy).
- **Degraded mesh:** other nodes may ignore your packets or informally ban you from the community network.
- **Equipment:** transmitting without an antenna or with impedance mismatch can damage the radio's RF stage (irreversible).

---

## 3. Hardware

### 3.1. Device categories

Meshtastic supports a wide hardware matrix. Essential criteria:

- **MCU:** ESP32 / ESP32-S3 (Espressif), NRF52 (Nordic), RP2040 (Raspberry Pi)
- **LoRa radio:** Semtech SX1262 (recommended), SX1276 (legacy), LR1110 (advanced, with GNSS)
- **Band:** specific to 868 MHz (433 or 915 variants are incompatible in PT)

### 3.2. Hardware tested and used in Portugal

> List aligned with the [Meshtastic PT community](https://meshtastic.pt/hardware) reference and the [comparator](https://meshtastic.pt/comparator).

| Device | MCU | Radio | Display | GPS | Battery | Notes |
|---|---|---|---|---|---|---|
| **Heltec WiFi LoRa 32 V3** | ESP32-S3 | SX1262 | OLED 0.96" | ❌ | external | Inexpensive, popular for fixed nodes |
| **Heltec Wireless Stick Lite V3** | ESP32-S3 | SX1262 | ❌ | ❌ | external | Headless, ideal for repeaters |
| **LilyGO T-Deck Plus** | ESP32-S3 | SX1262 | TFT 2.8" touch | L76K | 18650 | Complete standalone (QWERTY keyboard) — see [T-Deck Complete Guide](https://github.com/noduscypher/tdeck-guides) |
| **LilyGO T-Beam V1.2** | ESP32 | SX1276/SX1262 | optional OLED | L76K | 18650 | Built-in battery holder, popular |
| **LilyGO T3-S3** | ESP32-S3 | SX1262 | OLED | ❌ | LiPo | Compact |
| **RAK WisBlock 4631** | NRF52840 | SX1262 | module | module | LiPo | Modular, professional |
| **SenseCAP Card Tracker T1000-E** | nRF52840 | LR1110 | E-Ink | yes | internal | Credit-card form factor |
| **Seeed XIAO nRF52840** | NRF52840 | external | ❌ | ❌ | external | For custom builds |

For T-Deck Plus specifics (installation, touch fix, battery management), see:

- [T-Deck Plus Complete Guide](https://github.com/noduscypher/tdeck-guides) — installation, initial configuration
- [T-Deck Plus Battery Guide](https://github.com/noduscypher/tdeck-guides) — power optimization
- [T-Deck Plus Touch Fix Guide](https://github.com/noduscypher/tdeck-guides) — touchscreen problem fixes

### 3.3. Antennas

**Correct frequency:**

- 868 MHz antenna (preferably "EU 863–870 MHz") for EU_868
- 433 MHz antenna for EU_433
- ❌ Never use a 915 MHz (US) or 923 MHz (JP) antenna at 868 — RF tuning is different and the resulting VSWR can damage the RF stage

**Connector — rule of thumb:**

| Convention | Center pin (male) | Where seen |
|---|---|---|
| **SMA standard** | Metal pin | LoRa, GPS, professional RF |
| **RP-SMA (Reverse Polarity)** | Hole | WiFi (routers), 2.4 GHz |

**Most Meshtastic radios use standard SMA.** Don't confuse with WiFi RP-SMA — they don't fit.

**Typical gain:**

- 2–3 dBi: small antennas (rubber duck) — good for urban use and mobility
- 5 dBi: larger fixed antennas — good for ROUTER nodes on rooftops
- 7+ dBi: large Yagi or collinear antennas — for professional or point-to-point deployments

**Important:** gain contributes to EIRP. See section 2.6.

> ⚠️ **NEVER operate the radio without the antenna connected.** RF energy has nowhere to go and can irreversibly destroy the transceiver's final stage. This is the first rule BEFORE turning on any Meshtastic device. There's no software warning that prevents this — it's the user's responsibility.

### 3.4. Power

**Batteries:**

- **18650 Li-Ion** (3.7 V nominal, 2 000–3 500 mAh): standard in T-Beam, T-Deck. Buy from verified brands (Samsung, LG, Sony) — clones are often overrated.
- **LiPo packs** (1S, 3.7 V): standard in RAK WisBlock, some Heltec. Watch for overcharge/discharge protection.
- **LiFePO4** (3.2 V nominal): not natively supported by Meshtastic chargers, but viable with external BMS. Advantage: 4 000+ cycles vs 500 for LiPo/Li-Ion.

**Solar (off-grid nodes):**

- Panel 10–30 W (continental Portugal, 4–6 h peak sun/day)
- MPPT 10 A charge controller (PWM loses ~15–20% efficiency)
- 12 V LiFePO4 battery (or 18650 with BMS) sized for 3 days autonomy
- DC-DC 12 V → 5 V/3 A converter

Typical consumption of a Meshtastic ESP32 + LoRa node:

- Idle (RX): ~50–80 mA
- TX at 27 dBm: ~250–350 mA (during transmission)
- Deep sleep (CLIENT_MUTE): ~1–5 mA

On a 3 000 mAh 18650, approximate autonomy:

- CLIENT (active use): 1–3 days
- CLIENT_MUTE: 1–2 weeks
- ROUTER (continuous power): N/A

For detailed optimization, see [T-Deck Plus Battery Guide](https://github.com/noduscypher/tdeck-guides).

---

## 4. Detailed configuration

### 4.1. Firmware installation

**Main methods:**

#### 4.1.1. Web Flasher (recommended)

1. Chromium-based browser (Chrome, Edge, Brave) — Firefox doesn't support WebSerial
2. Visit <https://flasher.meshtastic.org>
3. Connect the device via a USB-C **data** cable (not charge-only)
4. Click "Connect" → select serial port
5. Choose firmware:
   - **Beta** (recommended for community use in PT — recent fixes included)
   - **Stable** for conservative deployments
   - ❌ **Alpha** only for testing new features (may have bugs)
6. **Full Erase** only on first flash or in case of serious problems; for updates use normal flash to preserve configurations

#### 4.1.2. CLI (`esptool` + `meshtastic-cli`)

```bash
# Install Meshtastic Python CLI (includes esptool as dependency)
pip3 install --upgrade "meshtastic[cli]"

# Check version
meshtastic --version

# Check connected device
meshtastic --info
```

#### 4.1.3. Beta vs Stable — when to choose each

| Criterion | Beta | Stable |
|---|---|---|
| PT community | ✅ Recommended | OK |
| MQTT meshtastic.pt | ✅ Works better | OK |
| Recent bugs fixed | ✅ Yes | ❌ Lagged |
| New features | ✅ Yes | ❌ |
| Regression risk | ⚠️ Small | ✅ Stable |

The meshtastic.pt community uses mostly Beta to avoid temporary incompatibilities.

### 4.2. Region and LoRa settings

**Configuration via app, web client (<https://client.meshtastic.org>) or CLI:**

```bash
# Set region
meshtastic --set lora.region EU_868

# Check current LoRa configuration
meshtastic --get lora
```

**Essential settings:**

| Setting | EU_868 (recommended PT) | EU_433 |
|---|---|---|
| `lora.region` | `EU_868` | `EU_433` |
| `lora.modem_preset` | `LONG_FAST` (default) | `LONG_FAST` |
| `lora.channel_num` | 1 (default LongFast) | 4 (default LongFast) |
| `lora.hop_limit` | 3 (default) or 4 (PT convention) | 3 or 4 |
| `lora.tx_power` | Auto or ≤ 22 dBm with 5 dBi antenna | Auto |
| `lora.use_preset` | true | true |

> ⚠️ **Frequency slot affects sub-band → affects compliance.** Slot 1 (LongFast EU_868) falls in G3 (500 mW, 10% OK). Other slots may fall in G1 or G2 (25 mW, 1% or 0.1%).

### 4.3. Modem Presets

Comparison of standard presets:

| Preset | Bandwidth | SF | Coding Rate | Typical range | Airtime (typical pkt) | Throughput | Recommended use |
|---|---|---|---|---|---|---|---|
| **LONG_SLOW** | 125 kHz | 12 | 4/8 | ~10–15 km | ~8 s | ~37 bps | Maximum range, very low traffic |
| **LONG_MODERATE** | 125 kHz | 11 | 4/5 | ~7–10 km | ~4 s | ~134 bps | Balance range/throughput |
| **LONG_FAST** (default) | 250 kHz | 11 | 4/5 | ~5–8 km | ~2 s | ~268 bps | Global default |
| **MEDIUM_SLOW** | 250 kHz | 10 | 4/5 | ~3–5 km | ~1.5 s | ~537 bps | Larger networks, good balance |
| **MEDIUM_FAST** | 250 kHz | 9 | 4/5 | ~3–5 km | ~1 s | ~1 074 bps | Urban, dense networks |
| **SHORT_SLOW** | 250 kHz | 8 | 4/5 | ~1–3 km | ~500 ms | ~2 148 bps | Events, high local density |
| **SHORT_FAST** | 250 kHz | 7 | 4/5 | ~1–2 km | ~200 ms | ~4 297 bps | Local hub, maximum throughput |
| **SHORT_TURBO** | 500 kHz | 7 | 4/5 | <1 km | ~100 ms | ~8 594 bps | ⚠️ Not legal in some regions (BW 500 kHz) |

> ✓ Approximate values, based on official Meshtastic documentation and technical sources (haraldkreuzer.net, openelab.io).

> ⚠️ **SHORT_TURBO uses 500 kHz bandwidth — may not be legal in all EU_868 sub-bands.** Verify before use.

> 💡 The meshtastic.pt community and most European networks use **LONG_FAST** as standard. Consider switching to MEDIUM_SLOW or MEDIUM_FAST if your local network has more than 60 nodes or is congested.

### 4.4. Hop Limit

| Configuration | Value | Source |
|---|---|---|
| Meshtastic global default | **3** | Official Meshtastic documentation |
| Absolute maximum | **7** | Firmware limit |
| PT community convention | **4** in Client mode | [meshtastic.pt](https://meshtastic.pt/getstarted) |

> ⚠️ **Hop limit is not regulated by ANACOM.** The PT recommendation of 4 is local community convention — it balances coverage vs congestion.

Each additional hop **multiplies the airtime** consumed by the message on the network. High hop limit in dense networks degrades throughput for everyone.

### 4.5. TX Power

**Configuration via CLI:**

```bash
# Set power (in dBm)
meshtastic --set lora.tx_power 20

# Auto (firmware decides based on region)
meshtastic --set lora.tx_power 0
```

**Practical recommendations for EU_868 LongFast (G3):**

| Antenna | Recommended TX Power | Resulting EIRP | Compliance |
|---|---|---|---|
| 0 dBi (theoretical isotropic) | 27 dBm | 29 dBm EIRP | ✅ |
| 2 dBi (rubber duck) | 25 dBm | 29 dBm EIRP | ✅ |
| 3 dBi (whip) | 24 dBm | 29 dBm EIRP | ✅ |
| 5 dBi (long whip) | 22 dBm | 29 dBm EIRP | ✅ |
| 7 dBi (small Yagi) | 20 dBm | 29 dBm EIRP | ✅ |

### 4.6. Node Configuration — User Settings

```bash
# Long name (up to 39 chars)
meshtastic --set-owner "Duarte-8"

# Short name (up to 4 chars)
meshtastic --set-owner-short "DUA8"
```

**Meshtastic Portugal convention (optional, not mandatory):**

Adding a numeric suffix to the node name indicates the band used:

- `-8` for 868 MHz
- `-4` for 433 MHz
- `-24` for 2.4 GHz (future integrations)

Examples: `Duarte-8`, `João-4`, `Maria-24`.

> ✓ Verified against [meshtastic.pt/getstarted](https://meshtastic.pt/getstarted): "The frequency is used as a suffix: -8 (868), -4 (433), -24 (2.4 GHz)".

> ⚠️ **NOT a Meshtastic global rule** — only a Portuguese community convention. Useful in mixed meshes but optional.

### 4.7. Device Roles

| Role | Description | Routing | Power | Typical use |
|---|---|---|---|---|
| **CLIENT** | Standard mobile node | Forwards packets | Medium | Phone + personal node |
| **CLIENT_MUTE** | Receives but doesn't forward | No | Low | Multiple close nodes (avoids feedback) |
| **CLIENT_HIDDEN** | CLIENT but hidden on network | Yes | Medium | Privacy cases |
| **TRACKER** | GPS focus, low-power | Limited | Very low | Outdoor tracking |
| **LOST_AND_FOUND** | Emergency beacon | Limited | Very low | Emergency case |
| **TAK** | ATAK/CivTAK compatibility | Yes | Medium | Coordinated operations |
| **SENSOR** | Periodic telemetry | Limited | Very low | Environmental sensors |
| **ROUTER** | Dedicated fixed repeater | Yes, prioritized | High (continuous power) | Fixed backbone |
| **ROUTER_LATE** | Router with delay (last-resort) | Yes | High | Redundant coverage |
| **REPEATER** | Forwarding only (no app) | Yes | High | Pure repeater |

**Practical rule:**

- You (mobile) → CLIENT
- Other close nodes you don't want to congest → CLIENT_MUTE
- Fixed rooftop node with continuous power → ROUTER

> ⚠️ **Don't configure all nodes as ROUTER** — overloads the mesh with redundant retransmissions. ROUTER is for **fixed nodes with continuous power and well-positioned antenna**.

### 4.8. Channels

**Structure:**

- 1 **PRIMARY** channel (always active, index 0)
- Up to 7 **SECONDARY** channels (indices 1–7)

**LongFast default (worldwide public channel):**

- Name: `LongFast`
- PSK: `AQ==` (publicly known key — effectively no encryption)

**Private channels:**

```bash
# Generate random PSK (256-bit)
meshtastic --ch-set name "MyChannel" --ch-set psk random --ch-index 1
```

**Encryption:**

- **AES-256-CTR** if PSK ≥ 32 bytes
- **AES-128-CTR** if PSK 16 bytes
- **AES-CTR (public PSK AQ==)** = symbolic encryption, doesn't provide confidentiality

**Direct messages (DM):** use **PKI E2EE** (ECC X25519 + AES-256), independent of channel. Public keys are exchanged when two nodes "see" each other for the first time.

> ⚠️ DM confidentiality depends on your node not being compromised. There's no strong authentication against spoofing — Meshtastic wasn't designed for serious adversarial use. For high-threat models, consider SimpleX, Signal, or more robust tools.

### 4.9. MQTT — meshtastic.pt

**Settings verified on 2026-05-07 against [meshtastic.pt/getstarted](https://meshtastic.pt/getstarted):**

| Parameter | Value |
|---|---|
| Address | `mqtt.meshtastic.pt` |
| Username | `meshdev` |
| Password | `large4cats` |
| Root Topic | `msh` |
| Encryption | ON |
| TLS | OFF |
| Map Reporting (CLIENT) | 7 200 s (2 h) |
| Map Reporting (gateway/ROUTER) | 14 400 s (4 h) |
| Proxy to Client | ON if Bluetooth permanent |

**CLI:**

```bash
meshtastic --set mqtt.address mqtt.meshtastic.pt
meshtastic --set mqtt.username meshdev
meshtastic --set mqtt.password large4cats
meshtastic --set mqtt.root msh
meshtastic --set mqtt.encryption_enabled true
meshtastic --set mqtt.tls_enabled false
meshtastic --set mqtt.proxy_to_client_enabled true
```

> ✓ Public credentials, part of the official meshtastic.pt tutorial.

> ⚠️ **WiFi and Bluetooth don't work simultaneously on Meshtastic ESP32 radios** — the RF stack is shared. Activating one disables the other.

### 4.10. GPS & Position

**Main settings:**

```bash
# GPS enabled
meshtastic --set position.gps_enabled true

# Smart position broadcast (sends position when significantly changed)
meshtastic --set position.position_broadcast_smart_enabled true

# Maximum broadcast interval (even without movement)
meshtastic --set position.position_broadcast_secs 900

# Precision (privacy — reduce bits)
meshtastic --set position.position_precision 14  # ~1.5 km imprecision
```

**Privacy:**

- `position_precision = 32`: maximum precision (full decimals)
- `position_precision = 14`: ~1.5 km deliberate error
- `position_precision = 0`: no position broadcast

For fixed ROUTER nodes: `position.fixed_position = true` + manual coordinates. Saves GPS power and avoids unnecessary telemetry.

### 4.11. Network Settings

| Setting | Notes |
|---|---|
| `network.wifi_enabled` | True for direct web client access. ⚠️ Conflicts with Bluetooth. |
| `bluetooth.enabled` | True to connect to mobile app (default) |
| `serial.enabled` | True for CLI/debug via USB. Doesn't conflict with BT/WiFi. |

---

## 5. Operating modes

### 5.1. CLIENT

- **Default for mobile personal use**
- Routing on, normal hop limit (3 or 4), MQTT uplink if desired
- Good choice for 1 node per person

### 5.2. CLIENT_MUTE

- Doesn't forward messages (saves airtime)
- Useful when you have **multiple close nodes** (e.g., phone + T-Deck in car) — avoids feedback loops
- Receives normally, just doesn't retransmit

### 5.3. ROUTER / ROUTER_LATE

- **Only for fixed nodes with continuous power and well-positioned antenna**
- Forwards packets with priority
- High RF autonomy doesn't matter (continuous power)
- Positioning: rooftop, high balcony, with line-of-sight
- ROUTER_LATE: variant with small artificial delay — used when there are multiple routers and you want to prioritize some over others

### 5.4. TRACKER

- GPS ON, frequent position broadcast
- Limited routing to save battery
- Good for outdoor, hiking, animals, fleet tracking

### 5.5. SENSOR

- Periodic telemetry (temperature, humidity, etc.)
- No active chat participation
- Ideal for long-life environmental sensors

---

## 6. Best practices

### 6.1. Airtime management

**Meshtastic is NOT WhatsApp.** Total available bandwidth is in the hundreds of bps. Behaviors to avoid:

- ❌ Long conversations on public channels (LongFast has ~268 bps effective — 5 simultaneous conversations saturate)
- ❌ Messages > 200 characters (the LoRa packet is fragmented, multiplies airtime)
- ❌ Images, audio, files (not natively supported, and even if they were, it would be absurd at 268 bps)
- ❌ Automated bots with frequent broadcast on public channel

**Good practices:**

- ✅ Short, telegraphic messages
- ✅ Long conversations → dedicated private channel
- ✅ Telemetry → separate channel, not in chat channel
- ✅ Social greetings → common sense (it's not IRC)

### 6.2. Duty cycle awareness

How to see current duty cycle:

```bash
meshtastic --info | grep -i duty
```

In the app: `Settings → Network → LoRa → Stats`

Duty cycle exceeded symptoms:

- Sent messages but persistent "Send pending"
- Firmware logs show "Duty cycle exceeded, deferring TX"

**Mitigation:** reduce broadcast intervals, use a preset with lower airtime (MEDIUM instead of LONG), separate chat and telemetry into different channels.

### 6.3. Prohibited / problematic behaviors

| Behavior | Reason |
|---|---|
| ❌ Range Test mode on public channel | Automatic spam, saturates entire mesh |
| ❌ HAM mode in PT | Disables encryption — violates others' privacy + ambiguous legal status |
| ❌ TX Power above legal G3 limit (27 dBm + antenna gain) | ANACOM fine, interference |
| ❌ Telemetry broadcast at < 60 s on public channel | Saturation |
| ❌ Hop limit > 4 without prior community coordination | Multiplies retransmissions |
| ❌ Multiple active public channels with public PSK (AQ==) | No practical gain |

### 6.4. Mesh etiquette

- **Unique and identifiable name.** Avoid "Node1", "Test", "Meshtastic User" — helps others identify who's on the mesh.
- **Distinctive short-name.** The short name (4 chars) is what appears in listings — choose something readable.
- **Respect the public LongFast channel.** For private conversations, create a dedicated channel.
- **Educate new users.** Direct them to [meshtastic.pt](https://meshtastic.pt) and this manual.
- **Report problems calmly.** Technical errors vs bad behaviors require different responses.

---

## 7. Troubleshooting

### 7.1. Hardware

**Device doesn't show on computer:**

- Data USB cable (not charge-only)? Try another cable.
- Drivers (Windows): CH340/CP210x depending on USB-Serial chip.
- Power LED on? If not, PSU or switch problem.

**Excessive heating:**

- TX Power too high without dissipation?
- Antenna with high VSWR (mismatch) — may be reflecting energy back to the radio.
- Battery poorly contacted generating unstable voltage.

**Black screen:**

- Brightness at zero (power saving mode)? Touch a button.
- Boot stuck? Read logs via serial (`meshtastic --noproto`).
- See hardware-specific guides (T-Deck Plus → [Touch Fix Guide](https://github.com/noduscypher/tdeck-guides)).

### 7.2. RF / mesh

**Messages not delivered:**

- Hop limit insufficient for the distance?
- Other nodes in range are forwarding (configured as CLIENT, not CLIENT_MUTE)?
- Duty cycle exceeded — message stays pending.

**Low range:**

- Correct antenna for the band? (868 vs 915)
- Connector clean, well-tightened?
- Positioning (interior vs exterior, height, line-of-sight)?

**Duty cycle exceeded:**

- Reduce broadcast frequency (`position.position_broadcast_secs`, `telemetry.*_update_interval`)
- Switch to a preset with lower airtime (LONG_FAST → MEDIUM_FAST)
- Separate channels

### 7.3. Software

**Firmware brick / won't boot:**

```bash
# DFU mode + Full erase
meshtastic --noproto
# Then flash firmware via web flasher with Full Erase
```

**Settings don't save:**

- Chain CLI commands in one line:

  ```bash
  meshtastic --set lora.region EU_868 --set lora.hop_limit 3 --reboot
  ```

- Each individual command reboots the device, losing intermediate state.

**MQTT disconnects:**

- Check credentials (`meshdev / large4cats` for meshtastic.pt)
- Stable WiFi? Internet accessible?
- Firewall blocking port 1883 (MQTT default)?

**No GPS fix:**

- Go outdoors, clear sky view.
- Cold start: 5–15 minutes first time.
- GPS antenna connected (some models have integrated ceramic, others need external).

### 7.4. CLI diagnostics

**Essential commands:**

```bash
meshtastic --info                   # General node state
meshtastic --nodes                  # List nodes on mesh
meshtastic --get lora               # Current LoRa config
meshtastic --get mqtt               # Current MQTT config
meshtastic --noproto                # Debug mode (text logs)

# Messages via CLI
meshtastic --sendtext "hi from CLI"
meshtastic --dest '!abcd1234' --sendtext "DM via CLI"

# Reset if all else fails
meshtastic --factory-reset
```

---

## 8. Advanced topics

### 8.1. Full CLI — common workflows

**Complete initial setup in one command:**

```bash
meshtastic \
  --set lora.region EU_868 \
  --set lora.hop_limit 3 \
  --set-owner "Duarte-8" \
  --set-owner-short "DUA8" \
  --set mqtt.address mqtt.meshtastic.pt \
  --set mqtt.username meshdev \
  --set mqtt.password large4cats \
  --set mqtt.encryption_enabled true \
  --set mqtt.tls_enabled false \
  --reboot
```

**Configuration backup and restore:**

```bash
# Export current configuration
meshtastic --export-config > my-config.yaml

# Restore
meshtastic --configure my-config.yaml
```

### 8.2. Custom firmware

- Main repository: <https://github.com/meshtastic/firmware>
- Local build: PlatformIO + VS Code
- Pre-defined targets for common hardware (`heltec_v3`, `tbeam`, `t-deck`)
- Custom plugins: Lua API exists for simple extensions

Typical build:

```bash
git clone https://github.com/meshtastic/firmware
cd firmware
pio run -e heltec-v3
# Generates firmware.bin in .pio/build/heltec-v3/
```

### 8.3. Network architecture

**Principles for community mesh deployment:**

1. **Routers high up, with inter-router line-of-sight** — backbone first
2. **Distributed clients** — coverage by density
3. **Conservative hop limit** (3) — multiplies airtime
4. **Multiple private channels** for groups — preserves public channel for discovery
5. **Dedicated MQTT gateway** — 1 or 2 routers do uplink, not all
6. **Document** topology, frequencies, presets, node operator contacts

For Portugal, the [meshtastic.pt nodes map](https://meshtastic.pt/map) and [Malha PT](https://malha.meshtastic.pt) help visualize existing deployment before adding.

### 8.4. External integration

- **External MQTT clients:** Mosquitto, MQTT Explorer — useful for dashboards and logging
- **Python API:** `meshtastic` package allows complex scripts
- **Home Assistant:** integration via MQTT bridge — LoRa sensors in HA dashboard
- **Grafana + InfluxDB:** long-term telemetry logging
- **Webhooks:** firmware supports sending events to external URLs

---

## 9. Appendices

### Appendix A — Complete Meshtastic region codes

| Code | Region | Band | Notes |
|---|---|---|---|
| `EU_868` | Europe (recommended PT) | 869.4–869.65 MHz | G3, 500 mW, 10% |
| `EU_433` | Europe (PT alternative) | 433.05–434.79 MHz | Limits vary by sub-band |
| `US` | USA / Canada | 902–928 MHz | FCC Part 15 |
| `JP` | Japan | 920–928 MHz | ARIB |
| `CN` | China | 470–510 MHz | SRRC |
| `KR` | South Korea | 920–923 MHz | — |
| `TW` | Taiwan | 920–925 MHz | — |
| `RU` | Russia | 868.7–869.2 MHz | — |
| `IN` | India | 865–867 MHz | — |
| `NZ_865` | New Zealand | 864–868 MHz | — |
| `TH` | Thailand | 920–925 MHz | — |
| `LORA_24` | Global 2.4 GHz (future) | 2 400–2 483.5 MHz | Specific hardware |
| `UA_433` | Ukraine 433 | 433–434 MHz | — |
| `UA_868` | Ukraine 868 | 868.0–868.6 MHz | — |
| `MY_433` | Malaysia 433 | 433–435 MHz | — |
| `MY_919` | Malaysia 919 | 919–924 MHz | — |
| `SG_923` | Singapore | 917–925 MHz | — |
| `PH_433` / `PH_868` / `PH_915` | Philippines | various | — |

⚠️ In Portugal use **only** `EU_868` (recommended) or `EU_433`. Others = illegal.

### Appendix B — Modem presets (technical summary)

See full table in [section 4.3](#43-modem-presets).

### Appendix C — EU_868 frequency slots vs ETSI sub-bands

See mapping in [section 2.3](#23-meshtastic-eu_868--sub-band-mapping).

For presets other than LongFast, calculate slot via:

- [Meshtastic PT calculator](https://meshtastic.pt/tools/powercalc)
- [Frequency Slot Calculator (heypete/GitHub)](https://github.com/heypete/meshtastic_frequency_slot_calculator)

### Appendix D — Meshtastic Portugal community resources

- **Website:** <https://meshtastic.pt>
- **Getting started (PT):** <https://meshtastic.pt/getstarted>
- **FAQ:** <https://meshtastic.pt/faq>
- **Compatible hardware:** <https://meshtastic.pt/hardware>
- **Hardware comparator:** <https://meshtastic.pt/comparator>
- **Nodes map:** <https://meshtastic.pt/map>
- **Malha PT (network status):** <https://malha.meshtastic.pt>
- **Power calculator:** <https://meshtastic.pt/tools/powercalc>
- **Community channels:** <https://meshtastic.pt/channels>
- **Node unblock:** <https://meshtastic.pt/unblock>
- **IoT server:** <https://meshtastic.pt/getstarted/iot>
- **Telegram:** <https://t.me/comunidademeshtasticpt>
- **Matrix:** <https://matrix.to/#/!BLCckQWdGZvhvgYrMW:matrix.org>
- **GitHub:** <https://github.com/meshtastic-pt>

### Appendix E — Glossary

| Term | Meaning |
|---|---|
| **AES-CTR** | Advanced Encryption Standard, Counter mode — symmetric encryption used in Meshtastic channels |
| **ALOHA** | Simple MAC protocol (transmission without prior coordination, retransmission on collision) |
| **ANACOM** | Autoridade Nacional de Comunicações (Portugal) — RF regulator |
| **BW** | Bandwidth — spectral width used for transmission (kHz) |
| **CR** | Coding Rate — proportion of FEC redundancy (4/5, 4/6, 4/7, 4/8) |
| **CSS** | Chirp Spread Spectrum — base modulation of LoRa |
| **Duty cycle** | Maximum percentage of time allowed in transmission (regulated by ETSI) |
| **EIRP** | Equivalent Isotropic Radiated Power — radiated power referenced to isotropic antenna |
| **ERP** | Effective Radiated Power — radiated power referenced to dipole antenna (EIRP – 2.15 dB) |
| **ETSI** | European Telecommunications Standards Institute |
| **G1/G2/G3/G4** | ETSI sub-bands within the 863–870 MHz band |
| **Hop / Hop limit** | Number of retransmissions allowed for a message to propagate on the mesh |
| **LBT / AFA** | Listen Before Talk / Adaptive Frequency Agility — alternatives to duty cycle |
| **LoRa** | Long Range — proprietary Semtech RF technology based on CSS |
| **LXMF** | Lightweight Extensible Message Format (not Meshtastic — Reticulum protocol) |
| **MQTT** | Message Queuing Telemetry Transport — pub/sub protocol used for internet uplink/downlink |
| **MUI** | Meshtastic UI — fancy interface on devices with display |
| **PKI** | Public Key Infrastructure — used for end-to-end encryption (DMs) |
| **PSK** | Pre-Shared Key — symmetric key shared per channel |
| **RED** | Radio Equipment Directive 2014/53/EU |
| **RSSI** | Received Signal Strength Indicator (dBm) |
| **RX / TX** | Reception / Transmission |
| **SF** | Spreading Factor (7 to 12) — amount of temporal "spreading" (more SF = more range, less throughput) |
| **SNR** | Signal-to-Noise Ratio (dB) |
| **SRD** | Short Range Device — low-power device regulated by ETSI EN 300 220 |
| **SX1262 / SX1276 / LR1110** | Semtech LoRa transceiver families |
| **TOFU** | Trust On First Use — trust model used in SimpleX (not Meshtastic) |
| **VSWR** | Voltage Standing Wave Ratio — measure of mismatch between radio and antenna (ideally <2:1) |

### Appendix F — Official links (re-verify before publishing)

- **Meshtastic.org docs:** <https://meshtastic.org/docs>
- **LoRa configuration:** <https://meshtastic.org/docs/configuration/radio/lora/>
- **Region codes:** <https://meshtastic.org/docs/configuration/radio/region/>
- **Web flasher:** <https://flasher.meshtastic.org>
- **Web client:** <https://client.meshtastic.org>
- **Firmware releases:** <https://github.com/meshtastic/firmware/releases>
- **ETSI EN 300 220 (official):** <https://www.etsi.org/deliver/etsi_en/300200_300299/30022002/>
- **ECC/REC 70-03:** <https://www.ecodocdb.dk/document/2229>
- **ANACOM Portugal:** <https://www.anacom.pt>

---

## Changelog

- **v1.0 (2026-05-07):** initial public version. Compliance verified against ETSI EN 300 220 (V3.2.1 / V3.3.1) and meshtastic.pt.

---

## Final notes

This manual is a living document. Regulation evolves (ETSI EN 300 220 revisions are released regularly; ANACOM updates recommendations), the Meshtastic ecosystem evolves (new presets, new hardware, new features), and the practice of the Portuguese community matures over time.

Contributions, corrections, and improvements are welcome via the [Meshtastic PT community](https://meshtastic.pt) or directly in the repository.

> **Last technical verification:** 2026-05-07
>
> **Suggested next review:** after ETSI EN 300 220-2 v3.4.x release (estimated 2026-Q4) or Meshtastic firmware 3.0+ release (estimated 2026).

---

☥ walk quietly, ₿ut keep the signal alive ⚡

*noduscypher-rawmesh node*
*License: CC BY-SA 4.0 — <https://creativecommons.org/licenses/by-sa/4.0/>*
