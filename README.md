# mesh-guides

Off-grid Reticulum + NomadNet mesh networking starter kit for Linux and desktop systems.

## What is this?

This repository collects opinionated guides for turning everyday machines into resilient off-grid mesh nodes using Reticulum and NomadNet.  
It focuses on practical, reproducible setups for desktop and laptop devices, with a bias towards CLI tools, minimal dependencies, and composable building blocks.  
You can mix these guides with radio links, local networks, or overlay networks like Tailscale or VPNs.

## Scenarios and guides

Pick the profile that best matches the role you want this node to play in your mesh:

| Scenario                | Typical device      | Guide file                    |
|-------------------------|---------------------|-------------------------------|
| Desktop daily driver    | iMac / workstation  | `DESKTOP-DAILY-DRIVER.md`     |
| Laptop daily driver     | MacBook / notebook  | `LAPTOP-DAILY-DRIVER.md`      |
| On-the-go field setup   | Mobile / travel kit | `ONTHEGO.md`                  |

Each guide is self-contained: it describes the assumptions, required software, and the suggested way to connect this node to the rest of your mesh.

## Prerequisites

Before you start, you should be comfortable with:

- Running commands in a terminal.
- Installing software using your system package manager or `pip`.
- Editing simple configuration files with a text editor.

You will also need:

- A supported operating system (Linux or desktop-class OS with Python 3 available).
- Basic networking connectivity (LAN, Wi-Fi, VPN, or an attached radio/LoRa device, depending on your use case).

For upstream documentation and deeper details, see:

- Reticulum “Get started” documentation: https://reticulum.network/start.html  
- NomadNet package and documentation: https://pypi.org/project/nomadnet/

## 30-second quickstart (conceptual)

This repository is about **guides**, not one-click installers, but the general flow looks like this:

1. Install Reticulum using your package manager or Python tooling (for example, `pip install rns`).  
2. Run Reticulum once to generate its configuration, then adjust the interfaces to match your hardware and transport (radio, Ethernet, Wi-Fi, VPN, etc.).  
3. Install NomadNet (for example, `pip install nomadnet`) and point it at your running Reticulum stack.  
4. Follow one of the profiles in this repository (`DESKTOP-DAILY-DRIVER.md`, `LAPTOP-DAILY-DRIVER.md`, or `ONTHEGO.md`) to turn your machine into a usable node in your mesh.

Always prefer the upstream docs for the exact install commands on your platform, and use these guides as a layer of opinionated integration on top.

## Why

Modern communication relies heavily on centralized infrastructure and always-on connectivity.  
Reticulum and NomadNet make it possible to build small, resilient, and privacy-respecting communication systems that can operate off-grid, over radio, or across ad-hoc links.  
These guides are meant to be a starting point for people who want to own more of their infrastructure without having to assemble all the pieces from scratch.

## License

This project is distributed under the terms described in the `LICENSE` file in this repository.
