# === MESH DAILY DRIVER — iMAC === ✅ (2026)
# Guia para um amigo começar do zero!
#
# 👉 Preenche os campos entre < > com os teus dados
#    e está pronto a voar! 🚀

---

## 🚀 ARRANQUE DIÁRIO

### Terminal 1 — Reticulum driver
~/start-mesh-driver.sh

### Terminal 2 — MeshChatX (mensagens + navegação)
~/start-meshchatx.sh
open http://127.0.0.1:8787

### Terminal 3 — rBrowser (navegar sites NomadNet)
~/start-rbrowser.sh
open http://127.0.0.1:5500

### Terminal 4 (opcional) — NomadNet TUI
source ~/nomadnet-venv/bin/activate
nomadnet --textui

### Backup (sempre que ligares a pen USB)
~/backup-imac-all.sh

---

## 🔄 CICLO DE EDIÇÃO DO SITE

# 1. Edita a tua página no VS Code
code ~/.nomadnetwork/storage/pages/index.mu

# 2. Envia para o teu RPi4 (substitui <IP-DO-TEU-RPI4>)
scp ~/.nomadnetwork/storage/pages/index.mu <your-username>@<IP-DO-TEU-RPI4>:~/.nomadnetwork/storage/pages/index.mu

# 3. Vê o resultado no rBrowser
# Navega para: <HASH-DO-TEU-NODE>:/page/index.mu

---

## 📝 AS TUAS IDENTIDADES (preenche após instalação)

| Cliente       | LXMF / ID              | Uso            |
|---------------|------------------------|----------------|
| NomadNet TUI  | <teu-LXMF-nomadnet>    | público        |
| MeshChatX     | <teu-LXMF-meshchatx>   | público        |
| rBrowser      | (navegação apenas)     | não precisa ID |

### NODE PÚBLICO (RPi4 ou servidor)
Hash Reticulum: <hash-do-teu-node>
LXMF público:   <lxmf-do-teu-node>
SSH:            ssh <your-username>@<IP-DO-TEU-RPI4>

---

## 🗂️ BACKUPS NA PEN USB

Estrutura automática criada por backup-imac-all.sh:

/Volumes/<NOME-DA-TUA-PEN>/iMac-reticulum/
/Volumes/<NOME-DA-TUA-PEN>/iMac-meshchatx/
/Volumes/<NOME-DA-TUA-PEN>/iMac-nomadnet/

---

## 📚 RECURSOS

Reticulum Manual:
https://reticulum.network/manual/

NomadNet Micron Markup:
https://github.com/markqvist/NomadNet

MeshChatX (Liam Cottle):
https://github.com/liamcottle/reticulum-meshchat

---

[ licenses ]

Licensed under Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)
https://creativecommons.org/licenses/by-sa/4.0/

---

cypherpunks write code /// nature writes resilience /// we bridge both

[ walk quietly, but keep the signal alive ]

₿uilt with Love 🧡 in cooperation with Nature 🌿
