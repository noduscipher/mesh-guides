# mesh-guides

Meshtastic EU868 guides for compliant community mesh operation (PT/EN/ES). This repository provides technical and beginner-friendly documentation for Meshtastic nodes running in the EU/UK 868 MHz band, with a focus on modem presets, routing roles, network planning and safe, regulation-aware use in CEPT countries. All guides are community-maintained and aim to give realistic expectations for range, clear recommendations for device roles, and practical examples that help local groups operate within legal limits.

## Languages / Idiomas

The guides are available in three languages and two levels (Technical / Beginner):

- Portuguese (PT)
- English (EN)
- Spanish (ES)

Each language has:

- a **Technical guide** (deep dive into presets, routing roles, ETSI / EU868 details)
- a **Beginner guide** (onboarding, conservative range expectations, practical do’s and don’ts)

## Documents

### Portuguese (PT)

- `docs/pt/technical/meshtastic-technical-guide.md`
- `docs/pt/beginner/meshtastic-beginner-guide.md`

### English (EN)

- `docs/en/technical/meshtastic-technical-guide.md`
- `docs/en/beginner/meshtastic-beginner-guide.md`

### Spanish (ES)

- `docs/es/technical/meshtastic-technical-guide.md`
- `docs/es/beginner/meshtastic-beginner-guide.md`

## Scope and audience

These guides are written for:

- operators running Meshtastic in the **EU/UK 868 MHz** band  
- small community groups deploying local meshes (urban, peri-urban, rural)  
- people who want **realistic** expectations for range and reliability, rather than marketing numbers  

The focus is on:

- modem presets (BW / SF / CR), airtime and practical throughput  
- routing roles and node types (CLIENT, CLIENT_MUTE, CLIENT_BASE, ROUTER, ROUTER_LATE, REPEATER, SENSOR, etc.)  
- network planning (hop limits, telemetry, nodeinfo, region codes)  
- behaviors to avoid, to reduce the risk of harmful interference in shared spectrum  

These documents are not firmware development guides and do not replace the official Meshtastic documentation. They sit **on top** of the upstream docs as opinionated, EU868-focused guidance.

## Regulatory and safety notes

The EU/UK 868 MHz band is governed by ETSI EN 300 220 and related CEPT / national regulations. These guides:

- use **conservative** range figures (urban, mixed terrain, and ideal line-of-sight)  
- emphasize how terrain, antenna placement, node height and RF noise shape real-world performance  
- recommend planning meshes within sensible geographic scopes (for example, ~30 km areas for most router roles)  
- encourage careful use of always-on roles (ROUTER, ROUTER_LATE) and favor `CLIENT_BASE` when appropriate in dense areas  

Nothing in this repository is legal advice. Always verify local regulations (for example, ANACOM in Portugal) and adjust your deployment accordingly.

## Meshtastic® trademark and branding

Meshtastic® is a registered trademark of Meshtastic LLC.

These guides:

- are independent, community-maintained documents  
- are **not** legal advice and do not explain how to register trademarks  
- do **not** grant any permission to use the Meshtastic name, logo, or visual identity commercially  

If you want to use Meshtastic branding in products, services or projects, you must follow the official Licensing & Trademark Rules:

- <https://meshtastic.org/docs/legal/licensing-and-trademark/>

## Contributions and feedback

Issues, corrections and pull requests are welcome.

Useful contributions include:

- fixes to technical details (presets, roles, parameter names)  
- clearer explanations or diagrams  
- notes from real-world deployments (urban / rural, antennas, mounting, power setups)  
- translations and language improvements within PT/EN/ES  

The goal is for these guides to remain a living reference for safe and compliant Meshtastic operation in EU868.

## Value for Value

If these guides help you build or maintain your own mesh, you can send value back in any way that makes sense to you.

The primary goal is to keep small, low-power nodes online and evolving quietly over time, not to optimize for attention or growth.

In a future update, this section may include optional details for supporting this work (for example: Bitcoin on-chain, Lightning, or other privacy-respecting ways to contribute). Until then, simply using, testing, and sharing these guides is already valuable feedback.

## License

This project is distributed under the terms described in the `LICENSE` file in this repository.
