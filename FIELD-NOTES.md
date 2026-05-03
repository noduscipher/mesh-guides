# field notes

These are just notes about how this mesh starter kit is evolving.  
No deadlines, no guarantees. Only what actually runs.

## Current focus – single nodes

Goal: Make it easy for a single person to turn everyday machines into reliable Reticulum + NomadNet nodes.

- Maintain clear guides for:
  - Desktop daily driver (`DESKTOP-DAILY-DRIVER.md`)
  - Laptop daily driver (`LAPTOP-DAILY-DRIVER.md`)
  - On-the-go kit (`ONTHEGO.md`)
- Keep Debian/Ubuntu and Arch/Manjaro quickstarts in the README up to date.
- Focus on reproducible, low-friction installs and minimal required dependencies.

Status: **In progress**

## Next steps – small home / friend mesh

Goal: Help people connect a handful of nodes into a resilient small mesh (home, friends, neighbours).

- Add examples and notes for:
  - Always-on nodes (home server, RPi, or similar)
  - Mixed transports (LAN, Wi-Fi, VPN, radio links)
  - Basic naming, addressing, and routing patterns in Reticulum
- Document practical topologies:
  - “home base + laptop + on-the-go kit”
  - “two homes bridged via VPN or Tailscale”
- Provide simple troubleshooting checklists (connectivity, latency, message reliability).

Status: **Planned**

## Later – field / remote nodes and V4V

Goal: Support more advanced off-grid and remote deployments, and explore a small Value-for-Value ecosystem around these guides.

- Add guidance for:
  - Remote / solar-powered nodes and low-bandwidth radio links
  - Running Reticulum and NomadNet on small SBCs and dedicated field hardware
- Integrate with a public NomadNet / Micron index:
  - Link this repository from the mesh node’s public index
  - Provide example site sections that explain the mesh to visitors
- Introduce an optional Value-for-Value (V4V) section:
  - Document ways to support the project (Lightning, BTC on-chain, etc.)
  - Keep all funding mechanisms optional and privacy-respecting.

Status: **Planned**
