# RAW MESH · ON THE GO

**Remote access · anywhere · any network**

▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓

---

## 📡 TAILSCALE NETWORK (fill with your IPs)

| Device | Tailscale IP | Local IP |
|--------|--------------|----------|
| RPi4   | `<IP-TAILSCALE-RPI4>` | 192.168.x.x |
| iMac   | `<IP-TAILSCALE-IMAC>` | 192.168.x.x |
| MacBook Air | `<IP-TAILSCALE-AIR>` | 192.168.x.x |
| iPhone | `<IP-TAILSCALE-IPHONE>` | — |

### How to get your Tailscale IPs:
```bash
# On Mac/RPi4:
tailscale ip

# Get all devices:
tailscale status
```

---

## 🔌 REMOTE ACCESS (from anywhere)

### SSH into RPi4
```bash
ssh <your-username>@<IP-TAILSCALE-RPI4>
```

### SSH into iMac
```bash
ssh <your-username>@<IP-TAILSCALE-IMAC>
```

### SSH into MacBook Air
```bash
ssh <your-username>@<IP-TAILSCALE-AIR>
```

### Quick check if devices are online
```bash
# Ping all devices:
tailscale ping <IP-TAILSCALE-RPI4>
tailscale ping <IP-TAILSCALE-IMAC>
tailscale ping <IP-TAILSCALE-AIR>
```

---

## 🌐 UPDATE NOMADNET SITE REMOTELY

### Option 1: Edit on Mac + Send to RPi4
```bash
# Edit locally with VS Code
code ~/.nomadnetwork/storage/pages/index.mu

# Send to RPi4 via Tailscale
scp ~/.nomadnetwork/storage/pages/index.mu <your-username>@<IP-TAILSCALE-RPI4>:~/.nomadnetwork/storage/pages/index.mu
```

### Option 2: Edit directly on RPi4 via SSH
```bash
# Connect to RPi4
ssh <your-username>@<IP-TAILSCALE-RPI4>

# Edit with nano
nano ~/.nomadnetwork/storage/pages/index.mu

# Save: Ctrl+O, Enter
# Exit: Ctrl+X
```

### Option 3: Use Micron Composer + Upload
```bash
# On Mac, start Composer:
cd ~/micron-composer && python3 -m http.server 8080
open http://127.0.0.1:8080/index.html

# Edit visually, then upload via SCP (Option 1)
```

---

## 🚀 QUICK START (café / hotspot / mobile)

### MacBook Air (portable setup)
```bash
~/start-mesh-driver.sh    # Reticulum
~/start-meshchat.sh        # MeshChat → http://127.0.0.1:8000
~/start-rbrowser.sh        # rBrowser → http://127.0.0.1:5500
~/backup-air-all.sh        # Backup to USB pen
```

### iMac (when at home/office)
```bash
~/start-mesh-driver.sh     # Reticulum
~/start-meshchatx.sh       # MeshChatX → http://127.0.0.1:8787
~/start-rbrowser.sh        # rBrowser → http://127.0.0.1:5500
~/backup-imac-all.sh       # Backup to USB pen
```

### iPhone (mesh on mobile)
**Reticulum via Sideband:**
1. Open Sideband app
2. Connect via RNode or network transport
3. LXMF messages work anywhere

**Remote access to RPi4 via Termius:**
1. Open Termius app
2. Select RPi4 saved connection (Tailscale IP)
3. SSH session active — run commands remotely

---

## 🗺️ NETWORK MAP (fill with your data)

| Node | Service | LXMF / Hash | Usage |
|------|---------|-------------|-------|
| **RPi4** | NomadNet public node | `<hash-do-teu-node>` | Public site, always-on |
| **iMac** | MeshChatX | `<teu-LXMF-meshchatx-imac>` | Messaging when home |
| **iMac** | NomadNet TUI | `<teu-LXMF-nomadnet-imac>` | Terminal client |
| **MacBook Air** | MeshChat | `<teu-LXMF-meshchat-air>` | Messaging on the go |
| **MacBook Air** | MeshChatX | `<teu-LXMF-meshchatx-air>` | Alternative UI |
| **iPhone** | Sideband | `<teu-LXMF-sideband>` | Mobile LXMF |

---

## 🛠️ EMERGENCIES ON THE GO

### MeshChat won't start
```bash
# Increase file descriptor limit
ulimit -n 10240

# Start manually
cd ~/reticulum-meshchat
source venv_novo/bin/activate
python3 meshchat.py --headless --port 8000
```

### RPi4 not responding via Tailscale
```bash
# Check Tailscale status on iPhone app
# Confirm RPi4 is online in device list

# Restart Tailscale on RPi4 (if you can access locally):
sudo tailscale down
sudo tailscale up
```

### Port already in use
```bash
# Find and kill process on port 8000:
lsof -i :8000
kill -9 <PID>

# Or force kill all on common ports:
lsof -i :8000 | awk 'NR>1 {print $2}' | xargs kill -9
lsof -i :5500 | awk 'NR>1 {print $2}' | xargs kill -9
lsof -i :8787 | awk 'NR>1 {print $2}' | xargs kill -9
```

### Restart Reticulum daemon
```bash
# Stop all Reticulum processes
pkill -f rnsd

# Wait for clean shutdown
sleep 2

# Start fresh
rnsd &

# Or use systemd (on RPi4):
sudo systemctl restart rnsd
```

### Check Reticulum connectivity
```bash
# List known destinations
rnstatus

# Ping a known node
rnprobe <destination-hash>

# Check interface status
rnpath <destination-hash>
```

---

## 🗂️ BACKUPS ON USB PEN

Automatic backup structure (created by backup scripts):

```
/Volumes/<USB-PEN-NAME>/
├── BACKUPID/              ← iMac complete backup
│   ├── iMac-reticulum/
│   ├── iMac-meshchatx/
│   └── iMac-nomadnet/
├── BACKUPIDAIR/           ← MacBook Air complete backup
│   ├── Air-reticulum/
│   ├── Air-meshchat/
│   ├── Air-meshchatx/
│   └── Air-nomadnet/
└── PARA-AMIGOS/           ← Guides to share with friends
    ├── MESH-DAILY-DRIVER-iMac.md
    ├── MESH-DAILY-DRIVER-MacBookAir.md
    └── ONTHEGO.md
```

---

## 📱 MOBILE WORKFLOW

**Best practices for mesh on the go:**

1. **Before leaving home:**
   - Run backup scripts on Mac
   - Verify RPi4 is online and reachable via Tailscale
   - Sync latest site content from RPi4 to Mac

2. **On mobile (café/hotspot):**
   - Connect MacBook Air to WiFi/hotspot
   - Run `~/start-mesh-driver.sh` (Reticulum auto-connects via Tailscale)
   - Use MeshChat for messaging
   - Use rBrowser to check your site and others

3. **On iPhone (anywhere):**
   - Sideband for LXMF messages
   - Termius for SSH to RPi4 (admin tasks)
   - No need for laptop — full mesh access from phone

4. **When back home:**
   - Sync any local edits to RPi4: `scp ... <your-username>@<IP-TAILSCALE-RPI4>:...`
   - Run backup scripts again

---

## 🌍 USE CASES

| Scenario | Solution |
|----------|----------|
| **At a café** | MacBook Air + MeshChat + Tailscale to RPi4 |
| **On a hike** | iPhone Sideband + local LoRa RNode (if available) |
| **Traveling abroad** | Tailscale keeps you connected to home node |
| **Friend wants to try** | Share guide from PARA-AMIGOS/ on USB pen |
| **Power outage at home** | RPi4 on solar keeps node alive, access via Tailscale |
| **No internet** | Local mesh via LoRa (T-Deck, RNode devices) |

---

## 📚 RESOURCES

- **Tailscale Docs:** https://tailscale.com/kb/
- **Reticulum Manual:** https://reticulum.network/manual/
- **Sideband (iOS/Android):** https://github.com/markqvist/Sideband
- **NomadNet:** https://github.com/markqvist/NomadNet
- **MeshChat:** https://github.com/liamcottle/reticulum-meshchat

---

## 📜 LICENSE

Licensed under Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)

https://creativecommons.org/licenses/by-sa/4.0/

---

cypherpunks write code /// nature writes resilience /// we bridge both

**walk quietly, but keep the signal alive.**

₿uilt with Love 🧡 in cooperation with Nature 🌿
