# Meshtastic para Iniciantes — Portugal

**Versão:** 1.0 · **Data:** 2026-05-07 · **Licença:** CC BY-SA 4.0
**Para quem nunca ouviu falar de Meshtastic e quer começar.**

> 📄 Lê isto em: PT · [EN](../../en/user/meshtastic-beginner-guide.md) · [ES](../../es/user/meshtastic-beginner-guide.md)

---

## ⚠️ Importante antes de começar

**Este guia é para Portugal e Europa (banda 868 MHz).**

Regras simples:

- ✅ Configura sempre a região como **"EU_868"** (Europa 868 MHz)
- ❌ **Nunca** uses outras regiões (US, Japão, China, Austrália) em Portugal
- ⚠️ Região errada = ilegal em Portugal e funciona mal

Se tiveres dúvidas, pede ajuda à [comunidade Meshtastic PT](https://meshtastic.pt) ou no [Telegram do grupo](https://t.me/comunidademeshtasticpt).

---

## Índice

1. [Bem-vindo ao Meshtastic](#1-bem-vindo-ao-meshtastic)
2. [Primeiros passos — o que preciso?](#2-primeiros-passos--o-que-preciso)
3. [Instalação passo-a-passo](#3-instalação-passo-a-passo)
4. [Usar no dia-a-dia](#4-usar-no-dia-a-dia)
5. [Configurações simples](#5-configurações-simples)
6. [Problemas comuns (FAQ)](#6-problemas-comuns-faq)
7. [Dicas úteis](#7-dicas-úteis)
8. [Junta-te à comunidade](#8-junta-te-à-comunidade)
9. [FAQ rápido](#9-faq-rápido)

---

## 1. Bem-vindo ao Meshtastic

### O que é?

Imagina **walkie-talkies inteligentes** que se ajudam uns aos outros para passar mensagens. Cada dispositivo recebe uma mensagem, e se não for para ele, **passa-a ao seguinte** até chegar ao destino.

Tudo isto **sem internet, sem operadora, sem cartão SIM**.

### Por que usar?

- 📵 **Funciona sem rede móvel** — útil em montanha, mar, eventos, emergências
- 🔒 **Mensagens privadas** — encriptadas automaticamente
- 🌍 **Longo alcance** — 5 a 15 km típicos, mais com elevação
- 💰 **Hardware barato** — entre 30 € e 100 € por dispositivo
- 🔋 **Baixo consumo** — bateria dura dias
- 🤝 **Comunidade** — em Portugal há já centenas de utilizadores

### Como funciona (sem complicar)?

1. O teu dispositivo Meshtastic é um pequeno rádio com bateria
2. Liga-se ao teu telemóvel via Bluetooth
3. Escreves mensagens na app do telemóvel
4. O dispositivo envia por rádio (banda 868 MHz, longo alcance)
5. Outros dispositivos próximos recebem ou **retransmitem** para outros mais longe
6. Quando chega ao destino, aparece na app dessa pessoa

A "**mesh**" significa que **todos os dispositivos ajudam todos os outros** — como uma rede de amigos onde cada um passa o recado.

---

## 2. Primeiros passos — o que preciso?

### O essencial

| Item | O que é | Custo aproximado |
|---|---|---|
| 1× Dispositivo Meshtastic | Pequeno hardware com rádio LoRa | 30–80 € |
| 1× Antena 868 MHz | Crítica! Vem incluída na maioria | (incluído) |
| 1× Smartphone | Android ou iOS recente | (já tens) |
| 1× Cabo USB-C de dados | Para configurar a 1ª vez | 5 € |

### Onde comprar?

**Lojas online com envio para PT:**

- [Tinytronics (NL)](https://www.tinytronics.nl) — bom preço, EU sem alfândega
- [OpenELab (DE)](https://eu.openelab.io) — distribuidor europeu Heltec
- [PTRobotics](https://www.ptrobotics.com) — Portugal
- [Mauser](https://mauser.pt) — Portugal

### Que dispositivo escolher?

**Para começar (mais barato):**
- **Heltec WiFi LoRa 32 V3** (~25–30 €) — pequeno, com display OLED
- **Heltec Wireless Stick Lite V3** (~20 €) — sem display, ainda mais barato

**Para uso completo (com teclado e ecrã):**
- **LilyGO T-Deck Plus** (~70 €) — standalone, teclado QWERTY, ecrã táctil
  - Para detalhes específicos, ver [T-Deck Plus Complete Guide](https://github.com/noduscypher/tdeck-guides)

**Para mochila e GPS:**
- **LilyGO T-Beam V1.2** (~40 €) — com GPS integrado

> 💡 Para uma comparação detalhada, vê o [comparador hardware meshtastic.pt](https://meshtastic.pt/comparator).

### O que verificar antes de comprar?

- ✅ **Banda 868 MHz** (NÃO 915 MHz que é dos EUA, NÃO 433 MHz)
- ✅ Antena incluída (a maioria inclui, mas confirma)
- ✅ Cabo USB-C (alguns vêm sem)

---

## 3. Instalação passo-a-passo

### ⚠️ REGRA #1 antes de tudo

**Conecta SEMPRE a antena ANTES de ligar o dispositivo.**

Se ligares sem antena, podes **danificar permanentemente** a parte de rádio. Sem aviso. Sem retorno. Aparafusa a antena bem firme antes de ligar USB ou bateria.

### 3.1. Instalar firmware (a "alma" do dispositivo)

> 💡 "Firmware" é o programa que faz o dispositivo funcionar. Tens de instalar a primeira vez (e atualizar de vez em quando).

1. **Computador com Google Chrome** (não Firefox — não funciona aqui)
2. Liga o dispositivo ao computador via cabo USB-C **(de dados, não só carga)**
3. Abre <https://flasher.meshtastic.org>
4. Clica em **"Connect"** e seleciona o teu dispositivo na lista
5. Escolhe a **última versão Beta** (recomendado — tem correções recentes)
6. Marca **"Full Erase"** apenas na primeira vez
7. Clica em **"Flash"** e espera (1–3 minutos)
8. **Não desligues** o cabo enquanto não terminar
9. Quando terminar, desliga e volta a ligar o dispositivo

### 3.2. Instalar a app no telemóvel

**Android:**

- [Meshtastic na Play Store](https://play.google.com/store/apps/details?id=com.geeksville.mesh)

**iOS:**

- [Meshtastic na App Store](https://apps.apple.com/pt/app/meshtastic/id1586432531)

Instala e abre.

### 3.3. Ligar o telemóvel ao dispositivo

1. **No dispositivo:** liga-o (botão de power)
2. **No telemóvel:** vai à app Meshtastic → "+" → "Connect"
3. Seleciona o teu dispositivo (nome tipo "Meshtastic_xxxx")
4. Aceita o pairing Bluetooth se pedir

### 3.4. Configuração básica (CRÍTICA)

Esta é a parte mais importante. Sem isto, o teu node não funciona corretamente em Portugal.

**Na app, vai a Settings (engrenagem) → Radio Configuration → LoRa:**

| Configuração | Valor |
|---|---|
| **Region** | **Europe 868** ⚠️ obrigatório em PT |
| **Modem Preset** | LongFast (deixa default) |
| **Hop Limit** | 3 (default) ou 4 (convenção PT) |

**Vai a User:**

- **Long Name:** o teu nome ou apelido (ex: `Duarte-8`)
- **Short Name:** abreviatura de 4 caracteres (ex: `DUA8`)

> 💡 **Convenção em Portugal:** muitos utilizadores adicionam `-8` ao fim do nome para indicar que usam 868 MHz. É opcional mas ajuda a identificar quem está em que banda. Não é obrigatório.

**Vai a MQTT** (para ligar à rede comunitária portuguesa):

| Configuração | Valor |
|---|---|
| **Address** | `mqtt.meshtastic.pt` |
| **Username** | `meshdev` |
| **Password** | `large4cats` |
| **Root Topic** | `msh` |
| **Encryption Enabled** | ✅ ON |
| **TLS Enabled** | ❌ OFF |

> ✓ Estes são as credenciais públicas oficiais da rede meshtastic.pt, verificadas em 2026-05-07.

**Save / Apply.** O dispositivo reinicia.

Pronto! Estás na rede.

---

## 4. Usar no dia-a-dia

### 4.1. Enviar mensagens

1. Na app, vai a **"Channels"** (lista de canais)
2. O canal padrão chama-se **"LongFast"** — é o canal público, todos veem
3. Escreve a mensagem
4. Envia

> ⚠️ **Importante:** mensagens em "LongFast" são vistas por **TODOS os utilizadores Meshtastic** dentro do alcance e na rede MQTT. **Não é privado.**

### 4.2. Ver outros nodes

- **Lista:** mostra todos os dispositivos que o teu detetou
- **Mapa:** mostra-os geograficamente (se tiverem GPS ativo)
- Para cada node vês: distância, sinal (RSSI), última atividade

### 4.3. Mensagens privadas (DM)

1. Toca num node específico na lista
2. Toca em "Direct Message"
3. Escreve — só essa pessoa recebe

> 🔒 **Mensagens diretas são encriptadas end-to-end automaticamente.** Mesmo retransmitidas por outros dispositivos, ninguém no meio consegue ler.

### 4.4. Mensagens curtas, por favor!

Meshtastic **não é WhatsApp**. A largura de banda é muito baixa.

- ✅ Mensagens curtas (até ~200 caracteres)
- ✅ Conversas espaçadas
- ❌ Não envies fotos (não suporta)
- ❌ Não faças chat contínuo no canal público

---

## 5. Configurações simples

### 5.1. Privacidade

**Não quero partilhar localização:**

- Vai a **Position** → desativa "GPS Enabled"

**Quero partilhar mas com menos precisão:**

- Position → "Position Precision" → reduz para 14 (~1.5 km de erro)

### 5.2. Poupar bateria

- **Modo CLIENT_MUTE:** se tens vários nodes próximos teus, configura este para receber mas não retransmitir → poupa bateria
- **Reduz broadcast intervals:** Position Broadcast a 900 s (15 min) em vez de 30 s

### 5.3. Melhorar alcance

- **Antena externa de melhor qualidade** (5 dBi é boa escolha)
- **Posição elevada** — varanda, janela alta, rooftop
- **Linha de vista** com outros nodes — sem prédios entre vocês
- **Não em cave** — RF não atravessa bem cimento + terra

---

## 6. Problemas comuns (FAQ)

### "Não vejo mensagens / não recebo"

**Verificar:**

1. Região está em **EU_868**?
2. Antena conectada (mesmo só para receber)?
3. Há outros nodes na tua área? Vê o [mapa meshtastic.pt](https://meshtastic.pt/map)
4. Dispositivo realmente ligado e à escuta?

### "Dispositivo não conecta à app"

**Verificar:**

1. Bluetooth ativado no telemóvel?
2. Dispositivo ligado e LED a piscar?
3. Reiniciar a app
4. "Forget" o pairing antigo no Bluetooth do telemóvel e refazer

### "Bateria acaba muito rápido"

**Soluções:**

1. Modo CLIENT_MUTE se múltiplos nodes próximos
2. Desativar GPS se não precisas
3. Desativar WiFi (só usa Bluetooth)
4. Reduzir TX Power para 17 dBm em vez de 27 dBm (alcance baixa proporcionalmente)

### "Alcance muito baixo"

**Verificar:**

1. Antena correta para 868 MHz (não 915, não 433)?
2. Conector limpo e bem apertado?
3. Posição (interior vs exterior, altura)?
4. Outros nodes longe demais? (mapa)

### "T-Deck Plus com problemas de táctil"

→ Ver [T-Deck Plus Touch Fix Guide](https://github.com/noduscypher/tdeck-guides) — documento dedicado.

---

## 7. Dicas úteis

### Boa cidadania mesh

- ✅ **Mensagens curtas e espaçadas** — todos partilham o canal
- ✅ **Conversas longas → canal privado dedicado**
- ✅ **Nome único e identificável** — evita "Node1", "Test"
- ✅ **Ajuda novos utilizadores** — tu também já foste novato
- ❌ **Não spam, não bots em canal público**
- ❌ **Não testes Range Test mode no canal público** (envia spam automático)

### Quando NÃO usar Meshtastic

- ❌ Para emergências sérias (usa 112)
- ❌ Para conversas longas (usa Signal/Telegram)
- ❌ Para enviar fotos ou ficheiros
- ❌ Para chat tipo WhatsApp

### Quando É perfeito

- ✅ Coordenação em hiking sem cobertura
- ✅ Status updates rápidos ("cheguei", "estou aqui")
- ✅ Eventos com muita gente (festivais, raids)
- ✅ Comunidade técnica local
- ✅ Backup para quando rede móvel falha

---

## 8. Junta-te à comunidade

A comunidade Meshtastic Portugal é ativa, acolhedora e ajuda quem começa.

| Recurso | Link |
|---|---|
| **Website oficial PT** | <https://meshtastic.pt> |
| **Primeiros passos PT** | <https://meshtastic.pt/getstarted> |
| **FAQ PT** | <https://meshtastic.pt/faq> |
| **Mapa de nodes PT** | <https://meshtastic.pt/map> |
| **Estado da rede** | <https://malha.meshtastic.pt> |
| **Telegram comunidade** | <https://t.me/comunidademeshtasticpt> |
| **Matrix** | <https://matrix.to/#/!BLCckQWdGZvhvgYrMW:matrix.org> |
| **Instagram** | <https://www.instagram.com/meshtastic.pt> |
| **Facebook grupo** | <https://www.facebook.com/groups/www.meshtastic.pt/> |
| **YouTube** | <https://www.youtube.com/channel/UC3awqON_H724kV4prigjPSA> |

Eventos ocasionais (meetups, demos, hackathons) anunciados no [calendário meshtastic.pt](https://meshtastic.pt/eventos).

---

## 9. FAQ rápido

**É legal usar Meshtastic em Portugal?**
Sim, sem necessidade de licença, desde que uses a região `EU_868` ou `EU_433` e não alteres potência/duty cycle acima dos defaults. ANACOM permite SRDs nestas bandas livremente para uso pessoal.

**Preciso de licença de radioamador?**
Não. Meshtastic em modo standard é Short Range Device (SRD) — não requer licença.

**Funciona sem internet?**
Sim, é o ponto principal. Os dispositivos comunicam diretamente por rádio. A internet só é precisa se quiseres ligar à rede MQTT global (opcional).

**Qual é o alcance?**
Depende muito do terreno e antenas:
- Cidade densa: 1–3 km típico
- Suburbano: 3–8 km
- Rural com elevação: 10–20 km+
- Recordes mundiais: 200+ km com elevação extrema (montanha-mar)

**Posso usar na cidade?**
Sim, mas o alcance é reduzido por edifícios. Em redes mesh densas (muitas pessoas com nodes), as mensagens chegam mais longe via retransmissão.

**Quanto custa começar?**
Mínimo: 30 € (Heltec V3 + antena, já tens cabo e telemóvel). Setup confortável: 50–100 €.

**Quanto consome de bateria?**
Em modo CLIENT normal: 1–3 dias com bateria 18650. Em CLIENT_MUTE: 1–2 semanas. Routers fixos: alimentação contínua.

**Posso usar em viagem na Europa?**
Sim, EU_868 é harmonizado em toda a UE + UK + Suíça + Noruega. Em Portugal, Espanha, França, Alemanha, etc. — funciona igual.

**E fora da Europa?**
**Tens de mudar a região** para a do país onde estás (`US`, `JP`, `AU`, etc.). Antenas 868 MHz funcionam mal em outras frequências — pode precisar de antena nova.

**É seguro / privado?**
- Mensagens diretas (DM): encriptadas end-to-end (PKI), só destinatário lê
- Canais privados (com PSK forte): encriptados, só quem tem a chave lê
- Canal público "LongFast": qualquer pessoa em alcance ou na rede MQTT lê

Para necessidades de privacidade séria (ameaças avançadas), usa Signal ou SimpleX. Meshtastic é boa para casual mas não foi desenhada para uso adversarial.

---

## Próximos passos

Quando quiseres aprofundar:

- [Manual Técnico Meshtastic](../technical/meshtastic-technical-guide.md) — para entender compliance ETSI, modos avançados, configurações detalhadas
- [Guides T-Deck Plus](https://github.com/noduscypher/tdeck-guides) — específicos do hardware T-Deck
- [meshtastic.pt](https://meshtastic.pt) — recurso vivo da comunidade

---

☥ walk quietly, ₿ut keep the signal alive ⚡

*noduscypher-rawmesh node*
*Licença: CC BY-SA 4.0*
