# Manual Técnico Meshtastic — Portugal/Europa

**Versão:** 1.0 · **Data:** 2026-05-07 · **Licença:** CC BY-SA 4.0
**Audiência:** técnicos, radioamadores, sysadmins, engenheiros de rede
**Compliance:** ETSI EN 300 220 · ANACOM Portugal

> 📄 Read this in: PT · [EN](../../en/technical/meshtastic-technical-guide.md) · [ES](../../es/technical/meshtastic-technical-guide.md)

---

## ⚠️ Aviso legal e compliance

Este manual documenta configurações Meshtastic para Portugal e Europa, conforme:

- **ETSI EN 300 220** (Short Range Devices, banda 25 MHz a 1 000 MHz)
- **ECC/REC 70-03** (recomendação CEPT para SRDs)
- **Regulamentação ANACOM Portugal** (autoridade nacional de comunicações)

**Verificado contra fontes oficiais em 2026-05-07.** A regulamentação evolui — confirma sempre contra documentação atual antes de operar.

**Responsabilidades do utilizador:**

- ✅ Verificar que a região está corretamente configurada (`EU_868` ou `EU_433`)
- ✅ Verificar que o frequency slot escolhido está dentro de uma sub-banda ETSI compatível com a potência usada
- ✅ Nunca exceder limites legais de potência ou duty cycle
- ⚠️ Usar regiões erradas (US, CN, JP, AU) em Portugal **é ilegal**
- ⚠️ Consequências: coima ANACOM, interferências em terceiros, danos no equipamento

Em caso de dúvida, consultar ANACOM, técnico qualificado ou a comunidade [meshtastic.pt](https://meshtastic.pt).

**Os autores não assumem responsabilidade por uso incorreto.**

---

## Índice

1. [Introdução](#1-introdução)
2. [Compliance e regulamentação](#2-compliance-e-regulamentação)
3. [Hardware](#3-hardware)
4. [Configuração detalhada](#4-configuração-detalhada)
5. [Modos de operação](#5-modos-de-operação)
6. [Boas práticas](#6-boas-práticas)
7. [Troubleshooting](#7-troubleshooting)
8. [Tópicos avançados](#8-tópicos-avançados)
9. [Apêndices](#9-apêndices)

---

## 1. Introdução

### 1.1. O que é Meshtastic

Meshtastic é um projeto open-source que transforma rádios LoRa baratos em dispositivos de mensagens encriptadas em rede mesh, sem dependência de internet, operadoras móveis ou infraestrutura centralizada.

Cada dispositivo (chamado "node") combina:

- Um microcontrolador (ESP32, NRF52 ou RP2040)
- Um transceiver LoRa (família Semtech SX126x, SX127x ou LR11xx)
- Uma bateria (tipicamente 18650 ou LiPo)
- Antena externa para a banda regional (868 MHz na Europa)

Conectado opcionalmente por Bluetooth a um smartphone com a app oficial Meshtastic, o node permite enviar e receber mensagens de texto numa rede mesh autoformativa.

### 1.2. Arquitetura mesh LoRa

A topologia é de **mesh dinâmica**: cada node atua simultaneamente como cliente e (se configurado) como repetidor. Mensagens propagam-se por múltiplos saltos (`hops`) até atingirem o destino, através de routing baseado em store-and-forward.

Características técnicas relevantes:

- **Camada física:** LoRa CSS (Chirp Spread Spectrum), modulação proprietária Semtech
- **Camada MAC:** ALOHA não-coordenado com retransmissões
- **Camada de rede:** flood routing com hop limit e deduplication
- **Camada aplicação:** mensagens texto, posição GPS, telemetria, sensores
- **Encriptação:** AES-256-CTR (com PSK por canal) + PKI para mensagens diretas (E2EE)

A bandwidth total é baixa (centenas de bps a kbps), o que **define o caráter da rede**: comunicação concisa, baixa frequência, alta resiliência.

### 1.3. Casos de uso

- **Comunicação off-grid:** áreas sem cobertura celular (hiking, montanha, mar costeiro)
- **Emergências:** rede comunitária resiliente em catástrofes (incêndios, terramotos)
- **Eventos:** coordenação em festivais, raids, provas desportivas
- **Mesh comunitária:** ligar bairros, vilas ou comunidades técnicas
- **Telemetria distribuída:** sensores ambientais low-power
- **Educação técnica:** introdução prática a redes mesh, RF e LoRa

### 1.4. Quem é este manual para

Pressupõe que tens:

- Confiança com configuração de dispositivos embedded (flashing de firmware, CLI)
- Compreensão básica de RF (frequência, potência, duty cycle)
- Familiaridade com networking (TCP/IP, MQTT, conceitos de routing)

Se és iniciante, começa pelo [Manual Beginner Meshtastic](../user/meshtastic-beginner-guide.md) e volta depois.

---

## 2. Compliance e regulamentação

Esta secção é a mais importante do manual. Configurar um node Meshtastic em Portugal sem entender o quadro regulatório pode ter consequências legais.

### 2.1. Quadro legal

| Norma | Âmbito | Aplicação a Meshtastic |
|---|---|---|
| **ETSI EN 300 220** | Standard técnico SRD (Short Range Devices) | Define limites RF para a banda 25 MHz – 1 000 MHz |
| **ECC/REC 70-03** | Recomendação CEPT (44 países europeus) | Define alocação de sub-bandas, potências e duty cycles |
| **ANACOM** | Autoridade portuguesa de comunicações | Aplica recomendações CEPT em território nacional |
| **CE marking + RED** | Diretiva 2014/53/UE (Radio Equipment) | Equipamentos vendidos na UE devem ser conformes |

A maioria dos dispositivos Meshtastic vendidos na UE são marcados CE, o que significa que **o hardware** é conforme. Mas o **uso** (frequência, potência, duty cycle configurados) é responsabilidade do utilizador.

**Em Portugal, o uso de SRDs nas bandas 868 MHz e 433 MHz não requer licença** desde que a operação esteja dentro dos limites de ETSI EN 300 220.

### 2.2. Sub-bandas ETSI 868 MHz (banda G: 863–870 MHz)

A banda principal usada por Meshtastic na Europa subdivide-se em 5 sub-bandas, cada uma com limites próprios:

| Sub-banda | Frequência (MHz) | Max ERP/EIRP | Duty cycle | Notas |
|---|---|---|---|---|
| **G** | 863.0 – 868.0 | ≤ 25 mW ERP | < 1% | Banda geral |
| **G1** | 868.0 – 868.6 | ≤ 25 mW ERP | < 1% | Comum em LoRaWAN uplink |
| **G2** | 868.7 – 869.2 | ≤ 25 mW ERP | < 0.1% | Restritivo |
| **G3** | **869.4 – 869.65** | **≤ 500 mW ERP** | **< 10%** | ⚡ Banda de alta potência |
| **G4** | 869.7 – 870.0 | ≤ 25 mW ERP | < 1% (ou 5 mW @ 100%) | — |

A faixa 869.65–869.7 MHz é alarm band (alarmes sociais), reservada e **não disponível** para SRD geral.

> ✓ Verificado contra ETSI TR 103 055, ETSI EN 300 220-2 V3.2.1, e múltiplas fontes técnicas independentes (Silicon Labs AN445, mobilefish.com tutorial 23, harald kreuzer).

> ⚠️ **Nomenclatura:** algumas fontes antigas usam minúsculas (g, g1, g2, g3, g4). A terminologia ETSI atual usa MAIÚSCULAS (G, G1, G2, G3, G4) com a tabela acima. Os limites são os mesmos.

### 2.3. Mapeamento Meshtastic EU_868 → sub-bandas

| Frequency Slot | Frequência central | Sub-banda ETSI | Max ERP legal | Duty cycle legal |
|---|---|---|---|---|
| **1** (default LongFast) | **869.525 MHz** | **G3** | **500 mW (27 dBm)** | **10%** |

> ✓ Verificado contra documentação oficial Meshtastic ([meshtastic.org/docs/configuration/radio/lora](https://meshtastic.org/docs/configuration/radio/lora/)) e [meshtastic.pt/getstarted](https://meshtastic.pt/getstarted).

**Implicação prática:** o LongFast slot 1 (default) **opera dentro de G3**, portanto pode legalmente transmitir até **500 mW ERP** com duty cycle de **10%**. Esta é a configuração standard em Portugal/EU e é totalmente compliant.

> ⚠️ **Outros presets** (MediumFast, ShortFast, etc.) usam frequency slots calculados algoritmicamente a partir do nome do canal. Esses slots podem cair em **G1 ou G2**, com limites de potência mais baixos (25 mW) e duty cycles mais restritos (1% ou 0.1%).
>
> Para verificar o slot exato e a sub-banda de um preset não-default, usar a [calculadora de potências meshtastic.pt](https://meshtastic.pt/tools/powercalc) ou o [calculator no GitHub heypete](https://github.com/heypete/meshtastic_frequency_slot_calculator).

### 2.4. EU_433 — informação resumida

| Parâmetro | Valor |
|---|---|
| Banda | 433.05 – 434.79 MHz |
| Frequency slot default | 4 |
| Frequência central (slot 4) | 433.875 MHz |
| Potência típica permitida | ≤ 10 mW ERP (varia por sub-banda) |
| Duty cycle aplicado pelo firmware Meshtastic | 10% |

> ⚠️ A banda 433 MHz tem **várias sub-bandas ETSI** com limites distintos (algumas permitem apenas 1 mW ERP, outras 10 mW). Para uso casual com Meshtastic em PT/EU, a configuração default cobre os casos típicos. Para deployment profissional ou comercial, **consultar ETSI EN 300 220-1/2** para a sub-banda exata.

EU_433 é minoria em Portugal: a maioria da comunidade usa 868 MHz por questões de range, ecossistema de hardware e antenas mais fáceis de obter.

### 2.5. Duty cycle: o que é e como o firmware aplica

**Duty cycle** é a percentagem máxima de tempo em que um dispositivo pode estar a transmitir, calculada numa janela temporal definida.

Para Meshtastic em EU_868 e EU_433, o firmware aplica:

- **Limite:** 10% de duty cycle
- **Janela:** 1 hora rolling, calculada por minuto

Isto significa: numa hora, o node pode transmitir no máximo **6 minutos** (10% de 60 min). Se o limite for atingido, o firmware pausa transmissões automaticamente até a janela libertar capacidade.

**Implicações:**

- A aplicação Meshtastic mostra duty cycle atual nos logs e no app
- Mensagens longas, broadcasts frequentes (telemetria, position) ou redes muito densas podem saturar
- Em caso de saturação: reduzir broadcast intervals, mudar para preset mais rápido (menor airtime), ou aumentar canais privados

### 2.6. EIRP vs ERP, ganho de antena

**Distinção importante:**

- **ERP (Effective Radiated Power):** referenciada à antena dipolo
- **EIRP (Equivalent Isotropic Radiated Power):** referenciada a antena isotrópica

Conversão: **EIRP = ERP + 2.15 dB**.

A regulamentação ETSI usa principalmente **ERP** para sub-bandas SRD. O limite de "500 mW ERP" em G3 corresponde a aproximadamente **820 mW EIRP**.

**Cuidado prático: ganho de antena conta para a potência radiada total.**

Exemplo crítico:

| Output do rádio | Antena | EIRP resultante | Compliance G3 (500 mW = 27 dBm ERP ≈ 29 dBm EIRP)? |
|---|---|---|---|
| 27 dBm | 0 dBi (isotrópica) | 29 dBm EIRP | ✅ Limite |
| 27 dBm | 3 dBi | 32 dBm EIRP | ❌ Excede |
| 27 dBm | 5 dBi | 34 dBm EIRP | ❌ Excede claramente |
| 24 dBm | 3 dBi | 29 dBm EIRP | ✅ Compliant |
| 22 dBm | 5 dBi | 29 dBm EIRP | ✅ Compliant |

> ⚠️ **Regra prática:** se usares antenas com ganho superior a 0 dBi (a maioria das antenas comerciais tem 3–5 dBi), **reduz a potência de saída do rádio** para manter EIRP total dentro do limite legal.

A maioria do hardware Meshtastic comercial tem TX power configurado para ≤ 22 dBm por defeito, deixando margem para antenas até 5 dBi. Mas se subires manualmente para 27 dBm em hardware capaz (T-Beam, RAK 4631 com PA), **verifica a antena**.

### 2.7. Outras regiões

| Região | Norma | Banda principal | Notas |
|---|---|---|---|
| **UK pós-Brexit** | Mantém ETSI EN 300 220 | 868 MHz | Compatível com EU |
| **Suíça, Noruega** | ETSI | 868 MHz | Compatíveis |
| **EUA / Canadá** | FCC Part 15 | 902–928 MHz | Incompatível com EU |
| **Japão** | ARIB STD-T108 | 920–928 MHz | Incompatível |
| **China** | SRRC | 470–510 MHz / 779–787 MHz | Incompatível |
| **Austrália / NZ** | ACMA | 915–928 MHz | Incompatível |

**Foco deste manual:** Portugal e UE (incluindo UK por compatibilidade pós-Brexit). Para outras regiões, consulta a documentação Meshtastic oficial e a regulamentação local.

### 2.8. Consequências de não-compliance

- **ANACOM:** pode aplicar coimas significativas (até dezenas de milhares de euros para uso não autorizado de espectro). Na prática, casos relacionados com SRDs e LoRa são raros, mas existem.
- **Interferências:** transmitir fora da sub-banda permitida ou acima de potência pode interferir com serviços críticos (alarmes médicos, telemetria industrial, radioastronomia).
- **Mesh degradado:** outros nodes podem ignorar os teus pacotes ou banir-te informalmente da rede comunitária.
- **Equipamento:** transmitir sem antena ou com mismatch de impedância pode danificar o estágio de RF do rádio (irreversível).

---

## 3. Hardware

### 3.1. Categorias de dispositivos

Meshtastic suporta uma matriz alargada de hardware. Os critérios essenciais são:

- **MCU:** ESP32 / ESP32-S3 (Espressif), NRF52 (Nordic), RP2040 (Raspberry Pi)
- **Rádio LoRa:** Semtech SX1262 (recomendado), SX1276 (legacy), LR1110 (avançado, com GNSS)
- **Banda:** específica para 868 MHz (variantes 433 ou 915 são incompatíveis em PT)

### 3.2. Hardware testado e usado em Portugal

> Lista alinhada com a referência da [comunidade Meshtastic PT](https://meshtastic.pt/hardware) e o [comparador](https://meshtastic.pt/comparator).

| Dispositivo | MCU | Rádio | Display | GPS | Bateria | Notas |
|---|---|---|---|---|---|---|
| **Heltec WiFi LoRa 32 V3** | ESP32-S3 | SX1262 | OLED 0.96" | ❌ | externa | Económico, popular para nodes fixos |
| **Heltec Wireless Stick Lite V3** | ESP32-S3 | SX1262 | ❌ | ❌ | externa | Headless, ideal repeaters |
| **LilyGO T-Deck Plus** | ESP32-S3 | SX1262 | TFT 2.8" táctil | L76K | 18650 | Standalone completo (teclado QWERTY) — ver [T-Deck Complete Guide](https://github.com/noduscypher/tdeck-guides) |
| **LilyGO T-Beam V1.2** | ESP32 | SX1276/SX1262 | OLED opcional | L76K | 18650 | Holder bateria integrado, popular |
| **LilyGO T3-S3** | ESP32-S3 | SX1262 | OLED | ❌ | LiPo | Compacto |
| **RAK WisBlock 4631** | NRF52840 | SX1262 | módulo | módulo | LiPo | Modular profissional |
| **SenseCAP Card Tracker T1000-E** | nRF52840 | LR1110 | E-Ink | sim | interna | Form-factor cartão crédito |
| **Seeed XIAO nRF52840** | NRF52840 | externo | ❌ | ❌ | externa | Para construções custom |

Para detalhes específicos do T-Deck Plus (instalação, touch fix, gestão de bateria), consultar:

- [T-Deck Plus Complete Guide](https://github.com/noduscypher/tdeck-guides) — instalação, configuração inicial
- [T-Deck Plus Battery Guide](https://github.com/noduscypher/tdeck-guides) — otimização energética
- [T-Deck Plus Touch Fix Guide](https://github.com/noduscypher/tdeck-guides) — correção de problemas de táctil

### 3.3. Antenas

**Frequência correta:**

- Antena 868 MHz (preferencialmente "EU 863–870 MHz") para EU_868
- Antena 433 MHz para EU_433
- ❌ Nunca usar antena 915 MHz (US) ou 923 MHz (JP) em 868 — o tuning RF é diferente e o VSWR resultante pode danificar o estágio de RF

**Conector — regra de ouro:**

| Convenção | Pino central macho | Onde se vê |
|---|---|---|
| **SMA standard** | Pino metálico | LoRa, GPS, RF profissional |
| **RP-SMA (Reverse Polarity)** | Buraco | WiFi (routers), 2.4 GHz |

**A maioria dos rádios Meshtastic usa SMA standard.** Não confundir com RP-SMA do mundo WiFi — não encaixam.

**Ganho típico:**

- 2–3 dBi: antenas pequenas (rubber duck) — bom para uso urbano e mobilidade
- 5 dBi: antenas maiores fixas — bom para nodes ROUTER em rooftop
- 7+ dBi: antenas Yagi ou colineares grandes — para deployments profissionais ou point-to-point

**Importante:** ganho contribui para EIRP. Ver secção 2.6.

> ⚠️ **NUNCA operar o rádio sem antena conectada.** A energia RF não tem para onde ir e pode destruir o estágio final do transceiver de forma irreversível. Esta é a primeira regra ANTES de ligar qualquer dispositivo Meshtastic. Não há aviso de software que evite isto — é responsabilidade do utilizador.

### 3.4. Energia

**Baterias:**

- **18650 Li-Ion** (3.7 V nominal, 2 000–3 500 mAh): standard em T-Beam, T-Deck. Comprar de marcas verificadas (Samsung, LG, Sony) — clones são frequentemente sobre-classificados.
- **LiPo packs** (1S, 3.7 V): standard em RAK WisBlock, alguns Heltec. Atenção a proteção contra overcharge/discharge.
- **LiFePO4** (3.2 V nominal): não suportado nativamente pelos chargers Meshtastic, mas viável com BMS externo. Vantagem: 4 000+ ciclos vs 500 das LiPo/Li-Ion.

**Solar (off-grid nodes):**

- Painel 10–30 W (Portugal continental, 4–6 h sol pico/dia)
- Charge controller MPPT 10 A (PWM perde ~15–20% de eficiência)
- Bateria 12 V LiFePO4 (ou 18650 com BMS) com capacidade para 3 dias autonomia
- Conversor DC-DC 12 V → 5 V/3 A

Consumo típico de um node Meshtastic ESP32 + LoRa:

- Idle (RX): ~50–80 mA
- TX a 27 dBm: ~250–350 mA (durante a transmissão)
- Deep sleep (CLIENT_MUTE): ~1–5 mA

Numa 18650 de 3 000 mAh, autonomia aproximada:

- CLIENT (uso ativo): 1–3 dias
- CLIENT_MUTE: 1–2 semanas
- ROUTER (alimentação contínua): N/A

Para optimização detalhada, consultar [T-Deck Plus Battery Guide](https://github.com/noduscypher/tdeck-guides).

---

## 4. Configuração detalhada

### 4.1. Instalação de firmware

**Métodos principais:**

#### 4.1.1. Web Flasher (recomendado)

1. Browser Chromium-based (Chrome, Edge, Brave) — Firefox não suporta WebSerial
2. Acede a <https://flasher.meshtastic.org>
3. Conecta o dispositivo via cabo USB-C de **dados** (não só carga)
4. Click "Connect" → seleciona porta serial
5. Escolhe firmware:
   - **Beta** (recomendado para uso comunitário em PT — fixes recentes incluídos)
   - **Stable** para deployments conservadores
   - ❌ **Alpha** apenas para teste de features novas (pode ter bugs)
6. **Full Erase** apenas no primeiro flash ou em caso de problemas graves; nas atualizações usa flash normal para preservar configurações

#### 4.1.2. CLI (`esptool` + `meshtastic-cli`)

```bash
# Instalar Meshtastic CLI Python (inclui esptool como dependência)
pip3 install --upgrade "meshtastic[cli]"

# Verificar versão
meshtastic --version

# Verificar dispositivo conectado
meshtastic --info
```

#### 4.1.3. Beta vs Stable — quando escolher cada

| Critério | Beta | Stable |
|---|---|---|
| Comunidade PT | ✅ Recomendado | OK |
| MQTT meshtastic.pt | ✅ Funciona melhor | OK |
| Bugs recentes corrigidos | ✅ Sim | ❌ Atraso |
| Features novas | ✅ Sim | ❌ |
| Risco de regressões | ⚠️ Pequeno | ✅ Estável |

A comunidade meshtastic.pt usa maioritariamente Beta para evitar incompatibilidades temporárias.

### 4.2. Região e LoRa Settings

**Configuração via app, web client (<https://client.meshtastic.org>) ou CLI:**

```bash
# Definir região
meshtastic --set lora.region EU_868

# Verificar configuração LoRa atual
meshtastic --get lora
```

**Settings essenciais:**

| Setting | EU_868 (recomendado PT) | EU_433 |
|---|---|---|
| `lora.region` | `EU_868` | `EU_433` |
| `lora.modem_preset` | `LONG_FAST` (default) | `LONG_FAST` |
| `lora.channel_num` | 1 (default LongFast) | 4 (default LongFast) |
| `lora.hop_limit` | 3 (default) ou 4 (convenção PT) | 3 ou 4 |
| `lora.tx_power` | Auto ou ≤ 22 dBm com antena 5 dBi | Auto |
| `lora.use_preset` | true | true |

> ⚠️ **Frequency slot afeta sub-banda → afeta compliance.** Slot 1 (LongFast EU_868) cai em G3 (500 mW, 10% OK). Outros slots podem cair em G1 ou G2 (25 mW, 1% ou 0.1%).

### 4.3. Modem Presets

Tabela comparativa dos presets standard:

| Preset | Bandwidth | SF | Coding Rate | Range típico | Airtime (typical pkt) | Throughput | Uso recomendado |
|---|---|---|---|---|---|---|---|
| **LONG_SLOW** | 125 kHz | 12 | 4/8 | ~10–15 km | ~8 s | ~37 bps | Range máximo, baixíssimo tráfego |
| **LONG_MODERATE** | 125 kHz | 11 | 4/5 | ~7–10 km | ~4 s | ~134 bps | Equilíbrio range/throughput |
| **LONG_FAST** (default) | 250 kHz | 11 | 4/5 | ~5–8 km | ~2 s | ~268 bps | Default global |
| **MEDIUM_SLOW** | 250 kHz | 10 | 4/5 | ~3–5 km | ~1.5 s | ~537 bps | Redes maiores, bom equilíbrio |
| **MEDIUM_FAST** | 250 kHz | 9 | 4/5 | ~3–5 km | ~1 s | ~1 074 bps | Urbano, redes densas |
| **SHORT_SLOW** | 250 kHz | 8 | 4/5 | ~1–3 km | ~500 ms | ~2 148 bps | Eventos, alta densidade local |
| **SHORT_FAST** | 250 kHz | 7 | 4/5 | ~1–2 km | ~200 ms | ~4 297 bps | Hub local, máximo throughput |
| **SHORT_TURBO** | 500 kHz | 7 | 4/5 | <1 km | ~100 ms | ~8 594 bps | ⚠️ Não legal em algumas regiões (BW 500 kHz) |

> ✓ Valores aproximados, baseados em documentação oficial Meshtastic e fontes técnicas (haraldkreuzer.net, openelab.io).

> ⚠️ **SHORT_TURBO usa 500 kHz de bandwidth — pode não ser legal em todas as sub-bandas EU_868.** Verificar antes de usar.

> 💡 A comunidade meshtastic.pt e a maioria das redes europeias usa **LONG_FAST** como standard. Considera mudar para MEDIUM_SLOW ou MEDIUM_FAST se a tua rede local tiver mais de 60 nodes ou estiver congestionada.

### 4.4. Hop Limit

| Configuração | Valor | Origem |
|---|---|---|
| Default Meshtastic global | **3** | Documentação oficial Meshtastic |
| Máximo absoluto | **7** | Limite firmware |
| Convenção comunidade PT | **4** em modo Client | [meshtastic.pt](https://meshtastic.pt/getstarted) |

> ⚠️ **Hop limit não é regulado por ANACOM.** A recomendação de 4 em PT é convenção de boa vizinhança da comunidade local — equilibra cobertura vs congestão.

Cada hop adicional **multiplica o airtime** consumido pela mensagem na rede. Hop limit alto em redes densas degrada throughput de todos.

### 4.5. TX Power

**Configuração via CLI:**

```bash
# Definir potência (em dBm)
meshtastic --set lora.tx_power 20

# Auto (firmware decide com base em região)
meshtastic --set lora.tx_power 0
```

**Recomendações práticas para EU_868 LongFast (G3):**

| Antena | TX Power recomendado | EIRP resultante | Compliance |
|---|---|---|---|
| 0 dBi (isotrópica teórica) | 27 dBm | 29 dBm EIRP | ✅ |
| 2 dBi (rubber duck) | 25 dBm | 29 dBm EIRP | ✅ |
| 3 dBi (whip) | 24 dBm | 29 dBm EIRP | ✅ |
| 5 dBi (whip longa) | 22 dBm | 29 dBm EIRP | ✅ |
| 7 dBi (Yagi pequena) | 20 dBm | 29 dBm EIRP | ✅ |

### 4.6. Node Configuration — User Settings

```bash
# Long name (até 39 chars)
meshtastic --set-owner "Duarte-8"

# Short name (até 4 chars)
meshtastic --set-owner-short "DUA8"
```

**Convenção Meshtastic Portugal (opcional, não obrigatória):**

Adicionar sufixo numérico ao nome do node indica a banda usada:

- `-8` para 868 MHz
- `-4` para 433 MHz
- `-24` para 2.4 GHz (futuras integrações)

Exemplos: `Duarte-8`, `João-4`, `Maria-24`.

> ✓ Verificado contra [meshtastic.pt/getstarted](https://meshtastic.pt/getstarted): "Usa-se a frequência como sufixo: -8 (868), -4 (433), -24 (2.4 GHz)".

> ⚠️ **NÃO é regra Meshtastic global** — apenas convenção da comunidade portuguesa. Útil em malhas mistas mas opcional.

### 4.7. Device Roles

| Role | Descrição | Routing | Power | Uso típico |
|---|---|---|---|---|
| **CLIENT** | Mobile node padrão | Reencaminha pacotes | Médio | Telemóvel + node pessoal |
| **CLIENT_MUTE** | Recebe mas não reencaminha | Não | Baixo | Múltiplos nodes próximos (evita feedback) |
| **CLIENT_HIDDEN** | CLIENT mas oculto na rede | Sim | Médio | Casos de privacidade |
| **TRACKER** | Foco em GPS, low-power | Limitado | Muito baixo | Tracking outdoor |
| **LOST_AND_FOUND** | Beacon de emergência | Limitado | Muito baixo | Caso de emergência |
| **TAK** | Compatibilidade ATAK/CivTAK | Sim | Médio | Operações coordenadas |
| **SENSOR** | Telemetria periódica | Limitado | Muito baixo | Sensores ambientais |
| **ROUTER** | Repetidor fixo dedicado | Sim, prioritário | Alto (alimentação contínua) | Backbone fixo |
| **ROUTER_LATE** | Router com delay (last-resort) | Sim | Alto | Cobertura redundante |
| **REPEATER** | Apenas reencaminha (sem app) | Sim | Alto | Repetidor puro |

**Regra prática:**

- Tu (mobile) → CLIENT
- Outros nodes próximos teus que não queres que congestionem → CLIENT_MUTE
- Node fixo no rooftop com alimentação contínua → ROUTER

> ⚠️ **Não configures todos os nodes como ROUTER** — sobrecarrega a malha com retransmissões redundantes. ROUTER é para nodes **fixos com alimentação contínua e antena bem posicionada**.

### 4.8. Channels

**Estrutura:**

- 1 canal **PRIMARY** (sempre ativo, índice 0)
- Até 7 canais **SECONDARY** (índices 1–7)

**Default LongFast (canal público mundial):**

- Nome: `LongFast`
- PSK: `AQ==` (chave pública conhecida — encriptação efetivamente nula)

**Canais privados:**

```bash
# Gerar PSK aleatória (256-bit)
meshtastic --ch-set name "MeuCanal" --ch-set psk random --ch-index 1
```

**Encriptação:**

- **AES-256-CTR** se PSK ≥ 32 bytes
- **AES-128-CTR** se PSK 16 bytes
- **AES-CTR (PSK pública AQ==)** = encriptação simbólica, não fornece confidencialidade

**Mensagens diretas (DM):** usam **PKI E2EE** (ECC X25519 + AES-256), independente do canal. As chaves públicas trocam-se quando dois nodes se "veem" pela primeira vez.

> ⚠️ A confidencialidade de DMs depende de o teu node não ter sido comprometido. Não há autenticação forte contra spoofing — Meshtastic não foi desenhada para uso adversarial sério. Para ameaças de modelo elevado, considera SimpleX, Signal, ou ferramentas mais robustas.

### 4.9. MQTT — meshtastic.pt

**Configurações verificadas em 2026-05-07 contra [meshtastic.pt/getstarted](https://meshtastic.pt/getstarted):**

| Parâmetro | Valor |
|---|---|
| Address | `mqtt.meshtastic.pt` |
| Username | `meshdev` |
| Password | `large4cats` |
| Root Topic | `msh` |
| Encryption | ON |
| TLS | OFF |
| Map Reporting (CLIENT) | 7 200 s (2 h) |
| Map Reporting (gateway/ROUTER) | 14 400 s (4 h) |
| Proxy to Client | ON se Bluetooth permanente |

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

> ✓ Credenciais públicas, parte do tutorial oficial meshtastic.pt.

> ⚠️ **WiFi e Bluetooth não funcionam em simultâneo nos rádios ESP32 Meshtastic** — a stack RF é partilhada. Ao ativar um, o outro desativa.

### 4.10. GPS & Position

**Settings principais:**

```bash
# GPS habilitado
meshtastic --set position.gps_enabled true

# Smart position broadcast (envia posição quando muda significativamente)
meshtastic --set position.position_broadcast_smart_enabled true

# Intervalo máximo de broadcast (mesmo sem movimento)
meshtastic --set position.position_broadcast_secs 900

# Precisão (privacidade — reduzir bits)
meshtastic --set position.position_precision 14  # ~1.5 km de imprecisão
```

**Privacidade:**

- `position_precision = 32`: precisão máxima (decimais completos)
- `position_precision = 14`: ~1.5 km de erro propositado
- `position_precision = 0`: sem broadcast de posição

Para nodes ROUTER fixos: `position.fixed_position = true` + coordenadas manuais. Poupa GPS power e evita telemetria desnecessária.

### 4.11. Network Settings

| Setting | Notas |
|---|---|
| `network.wifi_enabled` | True para acesso direto via web client. ⚠️ Conflita com Bluetooth. |
| `bluetooth.enabled` | True para conectar à app móvel (default) |
| `serial.enabled` | True para CLI/debug via USB. Não conflita com BT/WiFi. |

---

## 5. Modos de operação

### 5.1. CLIENT

- **Default para uso pessoal mobile**
- Routing on, hop limit normal (3 ou 4), MQTT uplink se desejado
- Boa escolha para 1 node por pessoa

### 5.2. CLIENT_MUTE

- Não reencaminha mensagens (saves airtime)
- Útil quando tens **múltiplos nodes próximos** (ex: telemóvel + T-Deck no carro) — evita feedback loops
- Recebe normalmente, apenas não retransmite

### 5.3. ROUTER / ROUTER_LATE

- **Apenas para nodes fixos com alimentação contínua e antena bem posicionada**
- Reencaminha pacotes com prioridade
- Alta autonomia RF não importa (alimentação contínua)
- Posicionamento: rooftop, varanda alta, com line-of-sight
- ROUTER_LATE: variante com pequeno delay artificial — usado quando há múltiplos routers e queres priorizar uns sobre outros

### 5.4. TRACKER

- GPS ON, broadcast posição frequente
- Routing limitado para poupar bateria
- Bom para uso outdoor, hiking, animais, frota

### 5.5. SENSOR

- Telemetria periódica (temperatura, humidade, etc.)
- Sem participação ativa em chat
- Ideal para sensores ambientais long-life

---

## 6. Boas práticas

### 6.1. Airtime management

**Meshtastic NÃO é WhatsApp.** A bandwidth total disponível é da ordem de centenas de bps. Comportamentos a evitar:

- ❌ Conversas longas em canais públicos (LongFast tem ~268 bps efetivos — 5 conversas simultâneas saturam)
- ❌ Mensagens > 200 caracteres (o pacote LoRa é fragmentado, multiplica airtime)
- ❌ Imagens, áudio, ficheiros (não suportado nativamente, e mesmo se fosse, seria absurdo a 268 bps)
- ❌ Bots automáticos com broadcast frequente em canal público

**Boas práticas:**

- ✅ Mensagens curtas, telegráficas
- ✅ Conversas longas → canal privado dedicado
- ✅ Telemetria → canal separado, não no canal de chat
- ✅ Cumprimentos sociais → bom senso (não é IRC)

### 6.2. Duty cycle awareness

Como ver duty cycle atual:

```bash
meshtastic --info | grep -i duty
```

Na app: `Settings → Network → LoRa → Stats`

Sintomas de duty cycle exceeded:

- Mensagens enviadas mas "Send pending" persistente
- Logs do firmware mostram "Duty cycle exceeded, deferring TX"

**Mitigação:** reduzir broadcast intervals, usar preset com menor airtime (MEDIUM em vez de LONG), separar chat e telemetria em canais diferentes.

### 6.3. Comportamentos proibidos / problemáticos

| Comportamento | Razão |
|---|---|
| ❌ Range Test mode em canal público | Spam automático, satura mesh inteira |
| ❌ HAM mode em PT | Desativa encryption — viola privacidade de outros + status legal ambíguo |
| ❌ TX Power acima do limite legal G3 (27 dBm + ganho antena) | Coima ANACOM, interferências |
| ❌ Broadcast de telemetria a < 60 s em canal público | Saturação |
| ❌ Hop limit > 4 sem coordenação prévia comunidade | Multiplica retransmissões |
| ❌ Múltiplos canais públicos ativos com PSK pública (AQ==) | Não tem ganho prático |

### 6.4. Etiqueta mesh

- **Nome único e identificável.** Evita "Node1", "Test", "Meshtastic User" — ajuda outros a identificar quem está na mesh.
- **Curto-name distintivo.** O short name (4 chars) é o que aparece em listagens — escolher algo legível.
- **Respeitar canal LongFast público.** Para conversas privadas, criar canal dedicado.
- **Educar novos users.** Direcionar para [meshtastic.pt](https://meshtastic.pt) e este manual.
- **Reportar problemas com calma.** Errors técnicos vs maus comportamentos têm respostas diferentes.

---

## 7. Troubleshooting

### 7.1. Hardware

**Device não aparece no computador:**

- Cabo USB de dados (não só carga)? Testar com outro cabo.
- Drivers (Windows): CH340/CP210x consoante o chip USB-Serial.
- LED de power aceso? Se não, problema de PSU ou switch.

**Aquecimento excessivo:**

- TX Power demasiado alto sem dissipação?
- Antena com VSWR alto (mismatch) — pode estar a refletir energia para o rádio.
- Bateria mal contactada gerando voltagem instável.

**Ecrã preto:**

- Brilho a zero (modo poupança)? Tocar num botão.
- Boot stuck? Ler logs via serial (`meshtastic --noproto`).
- Ver guias específicos de hardware (T-Deck Plus → [Touch Fix Guide](https://github.com/noduscypher/tdeck-guides)).

### 7.2. RF / mesh

**Mensagens não entregues:**

- Hop limit insuficiente para a distância?
- Outros nodes em range estão a retransmitir (configurados como CLIENT, não CLIENT_MUTE)?
- Duty cycle exceeded — a mensagem fica pendente.

**Alcance baixo:**

- Antena correta para a banda? (868 vs 915)
- Conector limpo, bem apertado?
- Posicionamento (interior vs exterior, altura, line-of-sight)?

**Duty cycle exceeded:**

- Reduzir broadcast frequency (`position.position_broadcast_secs`, `telemetry.*_update_interval`)
- Mudar para preset com menor airtime (LONG_FAST → MEDIUM_FAST)
- Separar canais

### 7.3. Software

**Firmware brick / não arranca:**

```bash
# DFU mode + Full erase
meshtastic --noproto
# Então flash firmware via web flasher com Full Erase
```

**Configurações não salvam:**

- Encadear comandos CLI numa linha:

  ```bash
  meshtastic --set lora.region EU_868 --set lora.hop_limit 3 --reboot
  ```

- Cada comando individual reboota o device, perdendo state intermédio.

**MQTT desconecta:**

- Verificar credenciais (`meshdev / large4cats` para meshtastic.pt)
- WiFi estável? Internet acessível?
- Firewall a bloquear port 1883 (MQTT default)?

**GPS no fix:**

- Ir para o exterior, vista de céu aberto.
- Cold start: 5–15 minutos primeira vez.
- Antena GPS conectada (alguns models têm cerâmica integrada, outros precisam externa).

### 7.4. Diagnóstico CLI

**Comandos essenciais:**

```bash
meshtastic --info                   # Estado geral do node
meshtastic --nodes                  # Lista nodes na mesh
meshtastic --get lora               # Config LoRa atual
meshtastic --get mqtt               # Config MQTT atual
meshtastic --noproto                # Modo debug (logs em texto)

# Mensagens via CLI
meshtastic --sendtext "olá da CLI"
meshtastic --dest '!abcd1234' --sendtext "DM via CLI"

# Reset se tudo falhar
meshtastic --factory-reset
```

---

## 8. Tópicos avançados

### 8.1. CLI completa — workflows comuns

**Setup inicial completo num comando:**

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

**Backup e restore de configurações:**

```bash
# Export configuração atual
meshtastic --export-config > my-config.yaml

# Restore
meshtastic --configure my-config.yaml
```

### 8.2. Custom firmware

- Repositório principal: <https://github.com/meshtastic/firmware>
- Build local: PlatformIO + VS Code
- Targets pré-definidos para hardware comum (`heltec_v3`, `tbeam`, `t-deck`)
- Custom plugins: API Lua existe para extensões simples

Build típico:

```bash
git clone https://github.com/meshtastic/firmware
cd firmware
pio run -e heltec-v3
# Gera firmware.bin em .pio/build/heltec-v3/
```

### 8.3. Network architecture

**Princípios para deployment de mesh comunitária:**

1. **Routers em altura, com line-of-sight inter-router** — backbone primeiro
2. **Clients distribuídos** — cobertura por densidade
3. **Hop limit conservador** (3) — multiplica airtime
4. **Múltiplos canais privados** para grupos — preserva canal público para descoberta
5. **MQTT gateway dedicado** — 1 ou 2 routers fazem uplink, não todos
6. **Documentar** topologia, frequências, presets, contactos dos node operators

Para Portugal, o [mapa nodes meshtastic.pt](https://meshtastic.pt/map) e o [Malha PT](https://malha.meshtastic.pt) ajudam a visualizar deployment existente antes de adicionar.

### 8.4. Integração externa

- **MQTT clients externos:** Mosquitto, MQTT Explorer — útil para dashboards e logging
- **API Python:** `meshtastic` package permite scripts complexos
- **Home Assistant:** integração via MQTT bridge — sensores LoRa em dashboard HA
- **Grafana + InfluxDB:** logging telemetria long-term
- **Webhooks:** firmware suporta envio de eventos para URLs externas

---

## 9. Apêndices

### Apêndice A — Region codes Meshtastic completos

| Código | Região | Banda | Notas |
|---|---|---|---|
| `EU_868` | Europa (recomendado PT) | 869.4–869.65 MHz | G3, 500 mW, 10% |
| `EU_433` | Europa (alternativa PT) | 433.05–434.79 MHz | Limites variam por sub-banda |
| `US` | EUA / Canadá | 902–928 MHz | FCC Part 15 |
| `JP` | Japão | 920–928 MHz | ARIB |
| `CN` | China | 470–510 MHz | SRRC |
| `KR` | Coreia do Sul | 920–923 MHz | — |
| `TW` | Taiwan | 920–925 MHz | — |
| `RU` | Rússia | 868.7–869.2 MHz | — |
| `IN` | Índia | 865–867 MHz | — |
| `NZ_865` | Nova Zelândia | 864–868 MHz | — |
| `TH` | Tailândia | 920–925 MHz | — |
| `LORA_24` | Globl 2.4 GHz (futuro) | 2 400–2 483.5 MHz | Hardware específico |
| `UA_433` | Ucrânia 433 | 433–434 MHz | — |
| `UA_868` | Ucrânia 868 | 868.0–868.6 MHz | — |
| `MY_433` | Malásia 433 | 433–435 MHz | — |
| `MY_919` | Malásia 919 | 919–924 MHz | — |
| `SG_923` | Singapura | 917–925 MHz | — |
| `PH_433` / `PH_868` / `PH_915` | Filipinas | várias | — |

⚠️ Em Portugal usar **apenas** `EU_868` (recomendado) ou `EU_433`. Outras = ilegal.

### Apêndice B — Modem presets (resumo técnico)

Ver tabela completa na [secção 4.3](#43-modem-presets).

### Apêndice C — EU_868 frequency slots vs sub-bandas ETSI

Ver mapeamento na [secção 2.3](#23-mapeamento-meshtastic-eu_868--sub-bandas).

Para presets diferentes de LongFast, calcular slot via:

- [Calculadora Meshtastic PT](https://meshtastic.pt/tools/powercalc)
- [Frequency Slot Calculator (heypete/GitHub)](https://github.com/heypete/meshtastic_frequency_slot_calculator)

### Apêndice D — Recursos da comunidade Meshtastic Portugal

- **Website:** <https://meshtastic.pt>
- **Primeiros passos (PT):** <https://meshtastic.pt/getstarted>
- **FAQ:** <https://meshtastic.pt/faq>
- **Hardware compatível:** <https://meshtastic.pt/hardware>
- **Comparador hardware:** <https://meshtastic.pt/comparator>
- **Mapa de nodes:** <https://meshtastic.pt/map>
- **Malha PT (network status):** <https://malha.meshtastic.pt>
- **Calculadora de potências:** <https://meshtastic.pt/tools/powercalc>
- **Canais comunitários:** <https://meshtastic.pt/channels>
- **Desbloqueio de node:** <https://meshtastic.pt/unblock>
- **Servidor IoT:** <https://meshtastic.pt/getstarted/iot>
- **Telegram:** <https://t.me/comunidademeshtasticpt>
- **Matrix:** <https://matrix.to/#/!BLCckQWdGZvhvgYrMW:matrix.org>
- **GitHub:** <https://github.com/meshtastic-pt>

### Apêndice E — Glossário

| Termo | Significado |
|---|---|
| **AES-CTR** | Advanced Encryption Standard, modo Counter — encriptação simétrica usada em canais Meshtastic |
| **ALOHA** | Protocolo MAC simples (transmissão sem coordenação prévia, retransmissão em caso de colisão) |
| **ANACOM** | Autoridade Nacional de Comunicações (Portugal) — regulador RF |
| **BW** | Bandwidth — largura espectral usada para a transmissão (kHz) |
| **CR** | Coding Rate — proporção de redundância FEC (4/5, 4/6, 4/7, 4/8) |
| **CSS** | Chirp Spread Spectrum — modulação base do LoRa |
| **Duty cycle** | Percentagem máxima de tempo permitido em transmissão (regulado por ETSI) |
| **EIRP** | Equivalent Isotropic Radiated Power — potência radiada referenciada a antena isotrópica |
| **ERP** | Effective Radiated Power — potência radiada referenciada a antena dipolo (EIRP – 2.15 dB) |
| **ETSI** | European Telecommunications Standards Institute |
| **G1/G2/G3/G4** | Sub-bandas ETSI dentro da banda 863–870 MHz |
| **Hop / Hop limit** | Número de retransmissões permitidas para uma mensagem propagar-se na mesh |
| **LBT / AFA** | Listen Before Talk / Adaptive Frequency Agility — alternativas a duty cycle |
| **LoRa** | Long Range — tecnologia RF Semtech proprietária baseada em CSS |
| **LXMF** | Lightweight Extensible Message Format (não Meshtastic — protocolo Reticulum) |
| **MQTT** | Message Queuing Telemetry Transport — protocolo pub/sub usado para uplink/downlink internet |
| **MUI** | Meshtastic UI — interface fancy nos dispositivos com display |
| **PKI** | Public Key Infrastructure — usado para encryption ponto-a-ponto (DMs) |
| **PSK** | Pre-Shared Key — chave simétrica partilhada por canal |
| **RED** | Radio Equipment Directive 2014/53/UE |
| **RSSI** | Received Signal Strength Indicator (dBm) |
| **RX / TX** | Receção / Transmissão |
| **SF** | Spreading Factor (7 a 12) — quantidade de "espalhamento" temporal (mais SF = mais range, menos throughput) |
| **SNR** | Signal-to-Noise Ratio (dB) |
| **SRD** | Short Range Device — dispositivo de baixa potência regulado por ETSI EN 300 220 |
| **SX1262 / SX1276 / LR1110** | Famílias de transceivers LoRa Semtech |
| **TOFU** | Trust On First Use — modelo de confiança usado em SimpleX (não Meshtastic) |
| **VSWR** | Voltage Standing Wave Ratio — medida de mismatch entre rádio e antena (idealmente <2:1) |

### Apêndice F — Links oficiais (reverificar antes de publicar)

- **Meshtastic.org docs:** <https://meshtastic.org/docs>
- **LoRa configuration:** <https://meshtastic.org/docs/configuration/radio/lora/>
- **Region codes:** <https://meshtastic.org/docs/configuration/radio/region/>
- **Web flasher:** <https://flasher.meshtastic.org>
- **Web client:** <https://client.meshtastic.org>
- **Firmware releases:** <https://github.com/meshtastic/firmware/releases>
- **ETSI EN 300 220 (oficial):** <https://www.etsi.org/deliver/etsi_en/300200_300299/30022002/>
- **ECC/REC 70-03:** <https://www.ecodocdb.dk/document/2229>
- **ANACOM Portugal:** <https://www.anacom.pt>

---

## Changelog

- **v1.0 (2026-05-07):** versão inicial pública. Compliance verificado contra ETSI EN 300 220 (V3.2.1 / V3.3.1) e meshtastic.pt.

---

## Notas finais

Este manual é um documento vivo. A regulamentação evolui (revisões ETSI EN 300 220 saem regularmente; ANACOM atualiza recomendações), o ecossistema Meshtastic evolui (novos presets, novos hardware, novas features), e a prática da comunidade portuguesa vai amadurecendo.

Contribuições, correções e melhorias são bem-vindas via [comunidade Meshtastic PT](https://meshtastic.pt) ou diretamente no repositório.

> **Última verificação técnica:** 2026-05-07
>
> **Próxima revisão sugerida:** após release ETSI EN 300 220-2 v3.4.x (estimado 2026-Q4) ou release Meshtastic firmware 3.0+ (estimado 2026).

---

☥ walk quietly, ₿ut keep the signal alive ⚡

*Du_arte ☥ rawmesh — Beira Baixa, Portugal*
*Licença: CC BY-SA 4.0 — <https://creativecommons.org/licenses/by-sa/4.0/>*
