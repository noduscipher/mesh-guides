# === MESH DAILY DRIVER — MacBook Air === ✅ (2026)
# Guia para um amigo começar do zero!
#
# 👉 Preenche os campos entre < > com os teus dados
#    e está pronto a voar! 🚀

---

## 🚀 ARRANQUE DIÁRIO

### Terminal 1 — Reticulum driver
~/start-mesh-driver.sh

### Terminal 2 — MeshChat (Liam Cottle - interface clássica)
~/start-meshchat.sh
open http://127.0.0.1:8000

### Terminal 3 — rBrowser (navegar sites NomadNet)
~/start-rbrowser.sh
open http://127.0.0.1:5500

### Terminal 4 (opcional) — MeshChatX (interface moderna)
~/start-meshchatx.sh
open http://127.0.0.1:8787

### Backup (sempre que ligares a pen USB)
~/backup-air-all.sh

---

## 🔧 ARRANQUE MANUAL (se necessário)

### Micron Composer (para editar site localmente)
cd ~/micron-composer && python3 -m http.server 8080
open http://127.0.0.1:8080/index.html

### SSH para o RPi4 (substitui <IP-DO-TEU-RPI4>)
ssh <your-username>@<IP-DO-TEU-RPI4>

---

## 🔄 CICLO DE EDIÇÃO DO SITE

# Opção 1: Editar local + enviar
code ~/.nomadnetwork/storage/pages/index.mu
scp ~/.nomadnetwork/storage/pages/index.mu <your-username>@<IP-DO-TEU-RPI4>:~/.nomadnetwork/storage/pages/index.mu

# Opção 2: Editar direto via SSH
ssh <your-username>@<IP-DO-TEU-RPI4>
nano ~/.nomadnetwork/storage/pages/index.mu

# Opção 3: Usar Micron Composer (GUI)
# Arranca o Composer, edita visualmente, depois envia via SCP

---

## 📝 AS TUAS IDENTIDADES (preenche após instalação)

| Cliente         | LXMF / ID                | Uso            |
|-----------------|--------------------------|----------------|
| MeshChat        | <teu-LXMF-meshchat>      | público        |
| MeshChatX       | <teu-LXMF-meshchatx>     | público        |
| NomadNet TUI    | <teu-LXMF-nomadnet>      | público        |
| rBrowser        | (navegação apenas)       | não precisa ID |

### NODE PÚBLICO (RPi4 ou servidor)
Hash Reticulum: <hash-do-teu-node>
LXMF público:   <lxmf-do-teu-node>
SSH:            ssh <your-username>@<IP-DO-TEU-RPI4>

---

## 🗂️ BACKUPS NA PEN USB

Estrutura automática criada por backup-air-all.sh:

/Volumes/<NOME-DA-TUA-PEN>/Air-reticulum/
/Volumes/<NOME-DA-TUA-PEN>/Air-meshchat/
/Volumes/<NOME-DA-TUA-PEN>/Air-meshchatx/
/Volumes/<NOME-DA-TUA-PEN>/Air-nomadnet/

---

## 📚 RECURSOS

Reticulum Manual:
https://reticulum.network/manual/

NomadNet Micron Markup:
https://github.com/markqvist/NomadNet

MeshChat (Liam Cottle):
https://github.com/liamcottle/reticulum-meshchat

Micron Composer:
https://github.com/markqvist/micron

---

[ licenses ]

Licensed under Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)
https://creativecommons.org/licenses/by-sa/4.0/

---

cypherpunks write code /// nature writes resilience /// we bridge both

[ walk quietly, but keep the signal alive ]

₿uilt with Love 🧡 in cooperation with Nature 🌿
