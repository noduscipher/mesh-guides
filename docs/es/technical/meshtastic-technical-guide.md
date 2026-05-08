# Manual Técnico Meshtastic — Portugal/Europa

**Versión:** 1.0 · **Fecha:** 2026-05-07 · **Licencia:** CC BY-SA 4.0
**Audiencia:** técnicos, radioaficionados, sysadmins, ingenieros de red
**Compliance:** ETSI EN 300 220 · ANACOM Portugal

> 📄 Léelo en: [PT](../../pt/technical/meshtastic-technical-guide.md) · [EN](../../en/technical/meshtastic-technical-guide.md) · ES

---

## ⚠️ Aviso legal y compliance

Este manual documenta configuraciones Meshtastic para Portugal y Europa, conforme:

- **ETSI EN 300 220** (Short Range Devices, banda 25 MHz a 1 000 MHz)
- **ECC/REC 70-03** (recomendación CEPT para SRDs)
- **Regulación ANACOM Portugal** (autoridad nacional de comunicaciones portuguesa)

**Verificado contra fuentes oficiales en 2026-05-07.** La regulación evoluciona — confirma siempre contra documentación actualizada antes de operar.

> 💡 **Nota para lectores hispanohablantes:** este manual está orientado al marco legal portugués (ANACOM). En España la autoridad equivalente es la **Secretaría de Estado de Telecomunicaciones e Infraestructuras Digitales (SETID)**, antes Ministerio de Asuntos Económicos y Transformación Digital. Las normas técnicas ETSI EN 300 220 son **idénticas en todo el espacio CEPT** (44 países, incluyendo España, México vía adopción, y mayoría de países hispanohablantes europeos). Verifica con tu autoridad nacional para detalles administrativos locales.

**Responsabilidades del usuario:**

- ✅ Verificar que la región esté correctamente configurada (`EU_868` o `EU_433`)
- ✅ Verificar que el frequency slot escogido esté dentro de una sub-banda ETSI compatible con la potencia usada
- ✅ Nunca exceder los límites legales de potencia o duty cycle
- ⚠️ Usar regiones erróneas (US, CN, JP, AU) en territorio CEPT **es ilegal**
- ⚠️ Consecuencias: multa administrativa, interferencias a terceros, daños al equipo

En caso de duda, consulta a tu autoridad nacional (ANACOM, SETID, etc.), un técnico cualificado o la [comunidad meshtastic.pt](https://meshtastic.pt).

**Los autores no asumen responsabilidad por uso incorrecto.**

---

## Índice

1. [Introducción](#1-introducción)
2. [Compliance y regulación](#2-compliance-y-regulación)
3. [Hardware](#3-hardware)
4. [Configuración detallada](#4-configuración-detallada)
5. [Modos de operación](#5-modos-de-operación)
6. [Buenas prácticas](#6-buenas-prácticas)
7. [Troubleshooting](#7-troubleshooting)
8. [Temas avanzados](#8-temas-avanzados)
9. [Apéndices](#9-apéndices)

---

## 1. Introducción

### 1.1. ¿Qué es Meshtastic?

Meshtastic es un proyecto open-source que transforma radios LoRa baratas en dispositivos de mensajería encriptada en red mesh, sin dependencia de internet, operadoras móviles o infraestructura centralizada.

Cada dispositivo (llamado "node") combina:

- Un microcontrolador (ESP32, NRF52 o RP2040)
- Un transceiver LoRa (familia Semtech SX126x, SX127x o LR11xx)
- Una batería (típicamente 18650 o LiPo)
- Antena externa para la banda regional (868 MHz en Europa)

Conectado opcionalmente por Bluetooth a un smartphone con la app oficial Meshtastic, el node permite enviar y recibir mensajes de texto en una red mesh autoformativa.

### 1.2. Arquitectura mesh LoRa

La topología es de **mesh dinámica**: cada node actúa simultáneamente como cliente y (si está configurado) como repetidor. Los mensajes se propagan por múltiples saltos (`hops`) hasta alcanzar su destino, mediante routing basado en store-and-forward.

Características técnicas relevantes:

- **Capa física:** LoRa CSS (Chirp Spread Spectrum), modulación propietaria Semtech
- **Capa MAC:** ALOHA no coordinado con retransmisiones
- **Capa de red:** flood routing con hop limit y deduplicación
- **Capa aplicación:** mensajes texto, posición GPS, telemetría, sensores
- **Encripción:** AES-256-CTR (con PSK por canal) + PKI para mensajes directos (E2EE)

El ancho de banda total es bajo (cientos de bps a kbps), lo cual **define el carácter de la red**: comunicación concisa, baja frecuencia, alta resiliencia.

### 1.3. Casos de uso

- **Comunicación off-grid:** áreas sin cobertura celular (senderismo, montaña, mar costero)
- **Emergencias:** red comunitaria resiliente en catástrofes (incendios, terremotos)
- **Eventos:** coordinación en festivales, raids, pruebas deportivas
- **Mesh comunitaria:** conectar barrios, pueblos o comunidades técnicas
- **Telemetría distribuida:** sensores ambientales low-power
- **Educación técnica:** introducción práctica a redes mesh, RF y LoRa

### 1.4. Para quién es este manual

Asume que tienes:

- Soltura con configuración de dispositivos embedded (flashing de firmware, CLI)
- Comprensión básica de RF (frecuencia, potencia, duty cycle)
- Familiaridad con networking (TCP/IP, MQTT, conceptos de routing)

Si eres principiante, empieza por el [Manual Beginner Meshtastic](../user/meshtastic-beginner-guide.md) y vuelve después.

---

## 2. Compliance y regulación

Esta sección es la más importante del manual. Configurar un node Meshtastic en territorio CEPT sin entender el marco regulatorio puede tener consecuencias legales.

### 2.1. Marco legal

| Norma | Ámbito | Aplicación a Meshtastic |
|---|---|---|
| **ETSI EN 300 220** | Estándar técnico SRD (Short Range Devices) | Define límites RF para la banda 25 MHz – 1 000 MHz |
| **ECC/REC 70-03** | Recomendación CEPT (44 países europeos) | Define asignación de sub-bandas, potencias y duty cycles |
| **ANACOM** | Autoridad portuguesa de comunicaciones | Aplica recomendaciones CEPT en territorio nacional |
| **CE marking + RED** | Directiva 2014/53/UE (Radio Equipment) | Equipos vendidos en UE deben ser conformes |

La mayoría de dispositivos Meshtastic vendidos en la UE están marcados CE, lo que significa que **el hardware** es conforme. Pero el **uso** (frecuencia, potencia, duty cycle configurados) es responsabilidad del usuario.

**En Portugal y España, el uso de SRDs en las bandas 868 MHz y 433 MHz no requiere licencia** siempre que la operación esté dentro de los límites de ETSI EN 300 220.

### 2.2. Sub-bandas ETSI 868 MHz (banda G: 863–870 MHz)

La banda principal usada por Meshtastic en Europa se subdivide en 5 sub-bandas, cada una con sus propios límites:

| Sub-banda | Frecuencia (MHz) | Max ERP/EIRP | Duty cycle | Notas |
|---|---|---|---|---|
| **G** | 863.0 – 868.0 | ≤ 25 mW ERP | < 1% | Banda general |
| **G1** | 868.0 – 868.6 | ≤ 25 mW ERP | < 1% | Común en LoRaWAN uplink |
| **G2** | 868.7 – 869.2 | ≤ 25 mW ERP | < 0.1% | Restrictiva |
| **G3** | **869.4 – 869.65** | **≤ 500 mW ERP** | **< 10%** | ⚡ Banda de alta potencia |
| **G4** | 869.7 – 870.0 | ≤ 25 mW ERP | < 1% (o 5 mW @ 100%) | — |

La franja 869.65–869.7 MHz es alarm band (alarmas sociales), reservada y **no disponible** para SRD general.

> ✓ Verificado contra ETSI TR 103 055, ETSI EN 300 220-2 V3.2.1, y múltiples fuentes técnicas independientes (Silicon Labs AN445, mobilefish.com tutorial 23, harald kreuzer).

> ⚠️ **Nomenclatura:** algunas fuentes antiguas usan minúsculas (g, g1, g2, g3, g4). La terminología ETSI actual usa MAYÚSCULAS (G, G1, G2, G3, G4) con la tabla anterior. Los límites son los mismos.

### 2.3. Mapeo Meshtastic EU_868 → sub-bandas

| Frequency Slot | Frecuencia central | Sub-banda ETSI | Max ERP legal | Duty cycle legal |
|---|---|---|---|---|
| **1** (default LongFast) | **869.525 MHz** | **G3** | **500 mW (27 dBm)** | **10%** |

> ✓ Verificado contra documentación oficial Meshtastic ([meshtastic.org/docs/configuration/radio/lora](https://meshtastic.org/docs/configuration/radio/lora/)) y [meshtastic.pt/getstarted](https://meshtastic.pt/getstarted).

**Implicación práctica:** el LongFast slot 1 (default) **opera dentro de G3**, por lo tanto puede legalmente transmitir hasta **500 mW ERP** con duty cycle del **10%**. Esta es la configuración estándar en territorio CEPT y es totalmente compliant.

> ⚠️ **Otros presets** (MediumFast, ShortFast, etc.) usan frequency slots calculados algorítmicamente a partir del nombre del canal. Esos slots pueden caer en **G1 o G2**, con límites de potencia más bajos (25 mW) y duty cycles más restrictivos (1% o 0.1%).
>
> Para verificar el slot exacto y la sub-banda de un preset no-default, usar la [calculadora de potencias meshtastic.pt](https://meshtastic.pt/tools/powercalc) o el [calculator en GitHub heypete](https://github.com/heypete/meshtastic_frequency_slot_calculator).

### 2.4. EU_433 — información resumida

| Parámetro | Valor |
|---|---|
| Banda | 433.05 – 434.79 MHz |
| Frequency slot por defecto | 4 |
| Frecuencia central (slot 4) | 433.875 MHz |
| Potencia típica permitida | ≤ 10 mW ERP (varía por sub-banda) |
| Duty cycle aplicado por el firmware Meshtastic | 10% |

> ⚠️ La banda 433 MHz tiene **varias sub-bandas ETSI** con límites distintos (algunas permiten solo 1 mW ERP, otras 10 mW). Para uso casual con Meshtastic en territorio CEPT, la configuración default cubre los casos típicos. Para deployment profesional o comercial, **consultar ETSI EN 300 220-1/2** para la sub-banda exacta.

EU_433 es minoritario: la mayoría de la comunidad usa 868 MHz por cuestiones de range, ecosistema de hardware y antenas más fáciles de obtener.

### 2.5. Duty cycle: qué es y cómo lo aplica el firmware

**Duty cycle** es el porcentaje máximo de tiempo en el que un dispositivo puede estar transmitiendo, calculado en una ventana temporal definida.

Para Meshtastic en EU_868 y EU_433, el firmware aplica:

- **Límite:** 10% de duty cycle
- **Ventana:** 1 hora rolling, calculada por minuto

Esto significa: en una hora, el node puede transmitir como máximo **6 minutos** (10% de 60 min). Si se alcanza el límite, el firmware pausa transmisiones automáticamente hasta que la ventana libere capacidad.

**Implicaciones:**

- La aplicación Meshtastic muestra duty cycle actual en los logs y en la app
- Mensajes largos, broadcasts frecuentes (telemetría, position) o redes muy densas pueden saturar
- En caso de saturación: reducir broadcast intervals, cambiar a preset más rápido (menor airtime), o añadir canales privados

### 2.6. EIRP vs ERP, ganancia de antena

**Distinción importante:**

- **ERP (Effective Radiated Power):** referenciada a antena dipolo
- **EIRP (Equivalent Isotropic Radiated Power):** referenciada a antena isotrópica

Conversión: **EIRP = ERP + 2.15 dB**.

La regulación ETSI usa principalmente **ERP** para sub-bandas SRD. El límite de "500 mW ERP" en G3 corresponde a aproximadamente **820 mW EIRP**.

**Cuidado práctico: la ganancia de antena cuenta para la potencia radiada total.**

Ejemplo crítico:

| Output del radio | Antena | EIRP resultante | Compliance G3 (500 mW = 27 dBm ERP ≈ 29 dBm EIRP)? |
|---|---|---|---|
| 27 dBm | 0 dBi (isotrópica) | 29 dBm EIRP | ✅ Al límite |
| 27 dBm | 3 dBi | 32 dBm EIRP | ❌ Excede |
| 27 dBm | 5 dBi | 34 dBm EIRP | ❌ Excede claramente |
| 24 dBm | 3 dBi | 29 dBm EIRP | ✅ Compliant |
| 22 dBm | 5 dBi | 29 dBm EIRP | ✅ Compliant |

> ⚠️ **Regla práctica:** si usas antenas con ganancia superior a 0 dBi (la mayoría de antenas comerciales tienen 3–5 dBi), **reduce la potencia de salida del radio** para mantener el EIRP total dentro del límite legal.

La mayoría del hardware Meshtastic comercial viene con TX power configurado a ≤ 22 dBm por defecto, dejando margen para antenas hasta 5 dBi. Pero si subes manualmente a 27 dBm en hardware capaz (T-Beam, RAK 4631 con PA), **verifica la antena**.

### 2.7. Otras regiones

| Región | Norma | Banda principal | Notas |
|---|---|---|---|
| **UK post-Brexit** | Mantiene ETSI EN 300 220 | 868 MHz | Compatible con UE |
| **Suiza, Noruega** | ETSI | 868 MHz | Compatibles |
| **EEUU / Canadá** | FCC Part 15 | 902–928 MHz | Incompatible con UE |
| **México** | IFETEL (varía) | 902–928 MHz típico | Verificar local |
| **Argentina, Chile** | varía por país | 902–928 MHz típico | Verificar local |
| **Japón** | ARIB STD-T108 | 920–928 MHz | Incompatible |
| **China** | SRRC | 470–510 MHz / 779–787 MHz | Incompatible |
| **Australia / NZ** | ACMA | 915–928 MHz | Incompatible |

**Foco de este manual:** Portugal y UE (incluyendo UK por compatibilidad post-Brexit). Para otras regiones, consultar la documentación Meshtastic oficial y la regulación local.

### 2.8. Consecuencias del no-compliance

- **Autoridad nacional (ANACOM/SETID/etc.):** puede aplicar multas significativas (hasta decenas de miles de euros para uso no autorizado de espectro). En la práctica, casos relacionados con SRDs y LoRa son raros, pero existen.
- **Interferencias:** transmitir fuera de la sub-banda permitida o por encima de potencia puede interferir con servicios críticos (alarmas médicas, telemetría industrial, radioastronomía).
- **Mesh degradado:** otros nodes pueden ignorar tus paquetes o banearte informalmente de la red comunitaria.
- **Equipo:** transmitir sin antena o con mismatch de impedancia puede dañar el estágio de RF del radio (irreversible).

---

## 3. Hardware

### 3.1. Categorías de dispositivos

Meshtastic soporta una matriz amplia de hardware. Los criterios esenciales son:

- **MCU:** ESP32 / ESP32-S3 (Espressif), NRF52 (Nordic), RP2040 (Raspberry Pi)
- **Radio LoRa:** Semtech SX1262 (recomendado), SX1276 (legacy), LR1110 (avanzado, con GNSS)
- **Banda:** específica para 868 MHz (variantes 433 o 915 son incompatibles en territorio CEPT)

### 3.2. Hardware probado y usado en Portugal

> Lista alineada con la referencia de la [comunidad Meshtastic PT](https://meshtastic.pt/hardware) y el [comparador](https://meshtastic.pt/comparator).

| Dispositivo | MCU | Radio | Display | GPS | Batería | Notas |
|---|---|---|---|---|---|---|
| **Heltec WiFi LoRa 32 V3** | ESP32-S3 | SX1262 | OLED 0.96" | ❌ | externa | Económico, popular para nodes fijos |
| **Heltec Wireless Stick Lite V3** | ESP32-S3 | SX1262 | ❌ | ❌ | externa | Headless, ideal para repeaters |
| **LilyGO T-Deck Plus** | ESP32-S3 | SX1262 | TFT 2.8" táctil | L76K | 18650 | Standalone completo (teclado QWERTY) — ver [T-Deck Complete Guide](https://github.com/noduscypher/tdeck-guides) |
| **LilyGO T-Beam V1.2** | ESP32 | SX1276/SX1262 | OLED opcional | L76K | 18650 | Holder batería integrado, popular |
| **LilyGO T3-S3** | ESP32-S3 | SX1262 | OLED | ❌ | LiPo | Compacto |
| **RAK WisBlock 4631** | NRF52840 | SX1262 | módulo | módulo | LiPo | Modular profesional |
| **SenseCAP Card Tracker T1000-E** | nRF52840 | LR1110 | E-Ink | sí | interna | Form-factor tarjeta de crédito |
| **Seeed XIAO nRF52840** | NRF52840 | externo | ❌ | ❌ | externa | Para construcciones custom |

Para detalles específicos del T-Deck Plus (instalación, touch fix, gestión de batería), consultar:

- [T-Deck Plus Complete Guide](https://github.com/noduscypher/tdeck-guides) — instalación, configuración inicial
- [T-Deck Plus Battery Guide](https://github.com/noduscypher/tdeck-guides) — optimización energética
- [T-Deck Plus Touch Fix Guide](https://github.com/noduscypher/tdeck-guides) — corrección de problemas táctiles

### 3.3. Antenas

**Frecuencia correcta:**

- Antena 868 MHz (preferiblemente "EU 863–870 MHz") para EU_868
- Antena 433 MHz para EU_433
- ❌ Nunca usar antena 915 MHz (US) o 923 MHz (JP) en 868 — el tuning RF es diferente y el VSWR resultante puede dañar el estágio de RF

**Conector — regla de oro:**

| Convención | Pin central macho | Dónde se ve |
|---|---|---|
| **SMA estándar** | Pin metálico | LoRa, GPS, RF profesional |
| **RP-SMA (Reverse Polarity)** | Hueco | WiFi (routers), 2.4 GHz |

**La mayoría de los radios Meshtastic usan SMA estándar.** No confundir con RP-SMA del mundo WiFi — no encajan.

**Ganancia típica:**

- 2–3 dBi: antenas pequeñas (rubber duck) — bueno para uso urbano y movilidad
- 5 dBi: antenas mayores fijas — bueno para nodes ROUTER en azotea
- 7+ dBi: antenas Yagi o colineales grandes — para deployments profesionales o point-to-point

**Importante:** la ganancia contribuye al EIRP. Ver sección 2.6.

> ⚠️ **NUNCA operar el radio sin antena conectada.** La energía RF no tiene a dónde ir y puede destruir el estágio final del transceiver de forma irreversible. Esta es la primera regla ANTES de encender cualquier dispositivo Meshtastic. No hay aviso de software que evite esto — es responsabilidad del usuario.

### 3.4. Energía

**Baterías:**

- **18650 Li-Ion** (3.7 V nominal, 2 000–3 500 mAh): estándar en T-Beam, T-Deck. Comprar de marcas verificadas (Samsung, LG, Sony) — los clones suelen estar sobreclasificados.
- **LiPo packs** (1S, 3.7 V): estándar en RAK WisBlock, algunos Heltec. Atención a protección contra overcharge/discharge.
- **LiFePO4** (3.2 V nominal): no soportado nativamente por los chargers Meshtastic, pero viable con BMS externo. Ventaja: 4 000+ ciclos vs 500 de las LiPo/Li-Ion.

**Solar (off-grid nodes):**

- Panel 10–30 W (Iberia, 4–6 h sol pico/día)
- Charge controller MPPT 10 A (PWM pierde ~15–20% de eficiencia)
- Batería 12 V LiFePO4 (o 18650 con BMS) con capacidad para 3 días autonomía
- Conversor DC-DC 12 V → 5 V/3 A

Consumo típico de un node Meshtastic ESP32 + LoRa:

- Idle (RX): ~50–80 mA
- TX a 27 dBm: ~250–350 mA (durante la transmisión)
- Deep sleep (CLIENT_MUTE): ~1–5 mA

En una 18650 de 3 000 mAh, autonomía aproximada:

- CLIENT (uso activo): 1–3 días
- CLIENT_MUTE: 1–2 semanas
- ROUTER (alimentación continua): N/A

Para optimización detallada, consultar [T-Deck Plus Battery Guide](https://github.com/noduscypher/tdeck-guides).

---

## 4. Configuración detallada

### 4.1. Instalación de firmware

**Métodos principales:**

#### 4.1.1. Web Flasher (recomendado)

1. Navegador Chromium-based (Chrome, Edge, Brave) — Firefox no soporta WebSerial
2. Acceder a <https://flasher.meshtastic.org>
3. Conectar el dispositivo via cable USB-C de **datos** (no solo carga)
4. Click "Connect" → seleccionar puerto serial
5. Escoger firmware:
   - **Beta** (recomendado para uso comunitario en PT — fixes recientes incluidos)
   - **Stable** para deployments conservadores
   - ❌ **Alpha** solo para test de features nuevas (puede tener bugs)
6. **Full Erase** solo en el primer flash o en caso de problemas graves; en las actualizaciones usa flash normal para preservar configuraciones

#### 4.1.2. CLI (`esptool` + `meshtastic-cli`)

```bash
# Instalar Meshtastic CLI Python (incluye esptool como dependencia)
pip3 install --upgrade "meshtastic[cli]"

# Verificar versión
meshtastic --version

# Verificar dispositivo conectado
meshtastic --info
```

#### 4.1.3. Beta vs Stable — cuándo escoger cada uno

| Criterio | Beta | Stable |
|---|---|---|
| Comunidad PT | ✅ Recomendado | OK |
| MQTT meshtastic.pt | ✅ Funciona mejor | OK |
| Bugs recientes corregidos | ✅ Sí | ❌ Retrasado |
| Features nuevas | ✅ Sí | ❌ |
| Riesgo de regresiones | ⚠️ Pequeño | ✅ Estable |

La comunidad meshtastic.pt usa mayoritariamente Beta para evitar incompatibilidades temporales.

### 4.2. Región y LoRa Settings

**Configuración via app, web client (<https://client.meshtastic.org>) o CLI:**

```bash
# Definir región
meshtastic --set lora.region EU_868

# Verificar configuración LoRa actual
meshtastic --get lora
```

**Settings esenciales:**

| Setting | EU_868 (recomendado PT) | EU_433 |
|---|---|---|
| `lora.region` | `EU_868` | `EU_433` |
| `lora.modem_preset` | `LONG_FAST` (default) | `LONG_FAST` |
| `lora.channel_num` | 1 (default LongFast) | 4 (default LongFast) |
| `lora.hop_limit` | 3 (default) o 4 (convención PT) | 3 o 4 |
| `lora.tx_power` | Auto o ≤ 22 dBm con antena 5 dBi | Auto |
| `lora.use_preset` | true | true |

> ⚠️ **El frequency slot afecta la sub-banda → afecta el compliance.** Slot 1 (LongFast EU_868) cae en G3 (500 mW, 10% OK). Otros slots pueden caer en G1 o G2 (25 mW, 1% o 0.1%).

### 4.3. Modem Presets

Tabla comparativa de los presets estándar:

| Preset | Bandwidth | SF | Coding Rate | Range típico | Airtime (typical pkt) | Throughput | Uso recomendado |
|---|---|---|---|---|---|---|---|
| **LONG_SLOW** | 125 kHz | 12 | 4/8 | ~10–15 km | ~8 s | ~37 bps | Range máximo, bajísimo tráfico |
| **LONG_MODERATE** | 125 kHz | 11 | 4/5 | ~7–10 km | ~4 s | ~134 bps | Equilibrio range/throughput |
| **LONG_FAST** (default) | 250 kHz | 11 | 4/5 | ~5–8 km | ~2 s | ~268 bps | Default global |
| **MEDIUM_SLOW** | 250 kHz | 10 | 4/5 | ~3–5 km | ~1.5 s | ~537 bps | Redes mayores, buen equilibrio |
| **MEDIUM_FAST** | 250 kHz | 9 | 4/5 | ~3–5 km | ~1 s | ~1 074 bps | Urbano, redes densas |
| **SHORT_SLOW** | 250 kHz | 8 | 4/5 | ~1–3 km | ~500 ms | ~2 148 bps | Eventos, alta densidad local |
| **SHORT_FAST** | 250 kHz | 7 | 4/5 | ~1–2 km | ~200 ms | ~4 297 bps | Hub local, máximo throughput |
| **SHORT_TURBO** | 500 kHz | 7 | 4/5 | <1 km | ~100 ms | ~8 594 bps | ⚠️ No legal en algunas regiones (BW 500 kHz) |

> ✓ Valores aproximados, basados en documentación oficial Meshtastic y fuentes técnicas (haraldkreuzer.net, openelab.io).

> ⚠️ **SHORT_TURBO usa 500 kHz de bandwidth — puede no ser legal en todas las sub-bandas EU_868.** Verificar antes de usar.

> 💡 La comunidad meshtastic.pt y la mayoría de redes europeas usa **LONG_FAST** como estándar. Considera cambiar a MEDIUM_SLOW o MEDIUM_FAST si tu red local tiene más de 60 nodes o está congestionada.

### 4.4. Hop Limit

| Configuración | Valor | Origen |
|---|---|---|
| Default Meshtastic global | **3** | Documentación oficial Meshtastic |
| Máximo absoluto | **7** | Límite firmware |
| Convención comunidad PT | **4** en modo Client | [meshtastic.pt](https://meshtastic.pt/getstarted) |

> ⚠️ **Hop limit no está regulado por ANACOM.** La recomendación de 4 en PT es convención de buena vecindad de la comunidad local — equilibra cobertura vs congestión.

Cada hop adicional **multiplica el airtime** consumido por el mensaje en la red. Hop limit alto en redes densas degrada throughput de todos.

### 4.5. TX Power

**Configuración via CLI:**

```bash
# Definir potencia (en dBm)
meshtastic --set lora.tx_power 20

# Auto (firmware decide en base a la región)
meshtastic --set lora.tx_power 0
```

**Recomendaciones prácticas para EU_868 LongFast (G3):**

| Antena | TX Power recomendado | EIRP resultante | Compliance |
|---|---|---|---|
| 0 dBi (isotrópica teórica) | 27 dBm | 29 dBm EIRP | ✅ |
| 2 dBi (rubber duck) | 25 dBm | 29 dBm EIRP | ✅ |
| 3 dBi (whip) | 24 dBm | 29 dBm EIRP | ✅ |
| 5 dBi (whip larga) | 22 dBm | 29 dBm EIRP | ✅ |
| 7 dBi (Yagi pequeña) | 20 dBm | 29 dBm EIRP | ✅ |

### 4.6. Node Configuration — User Settings

```bash
# Long name (hasta 39 chars)
meshtastic --set-owner "Duarte-8"

# Short name (hasta 4 chars)
meshtastic --set-owner-short "DUA8"
```

**Convención Meshtastic Portugal (opcional, no obligatoria):**

Añadir sufijo numérico al nombre del node indica la banda usada:

- `-8` para 868 MHz
- `-4` para 433 MHz
- `-24` para 2.4 GHz (futuras integraciones)

Ejemplos: `Duarte-8`, `João-4`, `Maria-24`.

> ✓ Verificado contra [meshtastic.pt/getstarted](https://meshtastic.pt/getstarted): "Se usa la frecuencia como sufijo: -8 (868), -4 (433), -24 (2.4 GHz)".

> ⚠️ **NO es regla Meshtastic global** — solo convención de la comunidad portuguesa. Útil en mallas mixtas pero opcional.

### 4.7. Device Roles

| Role | Descripción | Routing | Power | Uso típico |
|---|---|---|---|---|
| **CLIENT** | Mobile node estándar | Reenvía paquetes | Medio | Móvil + node personal |
| **CLIENT_MUTE** | Recibe pero no reenvía | No | Bajo | Múltiples nodes próximos (evita feedback) |
| **CLIENT_HIDDEN** | CLIENT pero oculto en la red | Sí | Medio | Casos de privacidad |
| **TRACKER** | Foco en GPS, low-power | Limitado | Muy bajo | Tracking outdoor |
| **LOST_AND_FOUND** | Beacon de emergencia | Limitado | Muy bajo | Caso de emergencia |
| **TAK** | Compatibilidad ATAK/CivTAK | Sí | Medio | Operaciones coordinadas |
| **SENSOR** | Telemetría periódica | Limitado | Muy bajo | Sensores ambientales |
| **ROUTER** | Repetidor fijo dedicado | Sí, prioritario | Alto (alimentación continua) | Backbone fijo |
| **ROUTER_LATE** | Router con delay (last-resort) | Sí | Alto | Cobertura redundante |
| **REPEATER** | Solo reenvía (sin app) | Sí | Alto | Repetidor puro |

**Regla práctica:**

- Tú (mobile) → CLIENT
- Otros nodes próximos tuyos que no quieres que congestionen → CLIENT_MUTE
- Node fijo en azotea con alimentación continua → ROUTER

> ⚠️ **No configures todos los nodes como ROUTER** — sobrecarga la malla con retransmisiones redundantes. ROUTER es para nodes **fijos con alimentación continua y antena bien posicionada**.

### 4.8. Channels

**Estructura:**

- 1 canal **PRIMARY** (siempre activo, índice 0)
- Hasta 7 canales **SECONDARY** (índices 1–7)

**Default LongFast (canal público mundial):**

- Nombre: `LongFast`
- PSK: `AQ==` (clave pública conocida — encripción efectivamente nula)

**Canales privados:**

```bash
# Generar PSK aleatoria (256-bit)
meshtastic --ch-set name "MiCanal" --ch-set psk random --ch-index 1
```

**Encripción:**

- **AES-256-CTR** si PSK ≥ 32 bytes
- **AES-128-CTR** si PSK 16 bytes
- **AES-CTR (PSK pública AQ==)** = encripción simbólica, no proporciona confidencialidad

**Mensajes directos (DM):** usan **PKI E2EE** (ECC X25519 + AES-256), independiente del canal. Las claves públicas se intercambian cuando dos nodes "se ven" por primera vez.

> ⚠️ La confidencialidad de DMs depende de que tu node no haya sido comprometido. No hay autenticación fuerte contra spoofing — Meshtastic no fue diseñado para uso adversarial serio. Para amenazas de modelo elevado, considera SimpleX, Signal o herramientas más robustas.

### 4.9. MQTT — meshtastic.pt

**Configuraciones verificadas en 2026-05-07 contra [meshtastic.pt/getstarted](https://meshtastic.pt/getstarted):**

| Parámetro | Valor |
|---|---|
| Address | `mqtt.meshtastic.pt` |
| Username | `meshdev` |
| Password | `large4cats` |
| Root Topic | `msh` |
| Encryption | ON |
| TLS | OFF |
| Map Reporting (CLIENT) | 7 200 s (2 h) |
| Map Reporting (gateway/ROUTER) | 14 400 s (4 h) |
| Proxy to Client | ON si Bluetooth permanente |

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

> ✓ Credenciales públicas, parte del tutorial oficial meshtastic.pt.

> ⚠️ **WiFi y Bluetooth no funcionan en simultáneo en los radios ESP32 Meshtastic** — el stack RF es compartido. Al activar uno, el otro se desactiva.

### 4.10. GPS & Position

**Settings principales:**

```bash
# GPS habilitado
meshtastic --set position.gps_enabled true

# Smart position broadcast (envía posición cuando cambia significativamente)
meshtastic --set position.position_broadcast_smart_enabled true

# Intervalo máximo de broadcast (incluso sin movimiento)
meshtastic --set position.position_broadcast_secs 900

# Precisión (privacidad — reducir bits)
meshtastic --set position.position_precision 14  # ~1.5 km de imprecisión
```

**Privacidad:**

- `position_precision = 32`: precisión máxima (decimales completos)
- `position_precision = 14`: ~1.5 km de error a propósito
- `position_precision = 0`: sin broadcast de posición

Para nodes ROUTER fijos: `position.fixed_position = true` + coordenadas manuales. Ahorra GPS power y evita telemetría innecesaria.

### 4.11. Network Settings

| Setting | Notas |
|---|---|
| `network.wifi_enabled` | True para acceso directo via web client. ⚠️ Conflicta con Bluetooth. |
| `bluetooth.enabled` | True para conectar a la app móvil (default) |
| `serial.enabled` | True para CLI/debug via USB. No conflicta con BT/WiFi. |

---

## 5. Modos de operación

### 5.1. CLIENT

- **Default para uso personal mobile**
- Routing on, hop limit normal (3 o 4), MQTT uplink si se desea
- Buena elección para 1 node por persona

### 5.2. CLIENT_MUTE

- No reenvía mensajes (saves airtime)
- Útil cuando tienes **múltiples nodes próximos** (ej: móvil + T-Deck en el coche) — evita feedback loops
- Recibe normalmente, solo no retransmite

### 5.3. ROUTER / ROUTER_LATE

- **Solo para nodes fijos con alimentación continua y antena bien posicionada**
- Reenvía paquetes con prioridad
- Alta autonomía RF no importa (alimentación continua)
- Posicionamiento: azotea, balcón alto, con line-of-sight
- ROUTER_LATE: variante con pequeño delay artificial — usado cuando hay múltiples routers y quieres priorizar unos sobre otros

### 5.4. TRACKER

- GPS ON, broadcast posición frecuente
- Routing limitado para ahorrar batería
- Bueno para uso outdoor, senderismo, animales, flota

### 5.5. SENSOR

- Telemetría periódica (temperatura, humedad, etc.)
- Sin participación activa en chat
- Ideal para sensores ambientales long-life

---

## 6. Buenas prácticas

### 6.1. Airtime management

**Meshtastic NO es WhatsApp.** El bandwidth total disponible es del orden de cientos de bps. Comportamientos a evitar:

- ❌ Conversaciones largas en canales públicos (LongFast tiene ~268 bps efectivos — 5 conversaciones simultáneas saturan)
- ❌ Mensajes > 200 caracteres (el paquete LoRa se fragmenta, multiplica airtime)
- ❌ Imágenes, audio, ficheros (no soportado nativamente, y aunque lo fuera, sería absurdo a 268 bps)
- ❌ Bots automáticos con broadcast frecuente en canal público

**Buenas prácticas:**

- ✅ Mensajes cortos, telegráficos
- ✅ Conversaciones largas → canal privado dedicado
- ✅ Telemetría → canal separado, no en el canal de chat
- ✅ Saludos sociales → sentido común (no es IRC)

### 6.2. Duty cycle awareness

Cómo ver duty cycle actual:

```bash
meshtastic --info | grep -i duty
```

En la app: `Settings → Network → LoRa → Stats`

Síntomas de duty cycle exceeded:

- Mensajes enviados pero "Send pending" persistente
- Logs del firmware muestran "Duty cycle exceeded, deferring TX"

**Mitigación:** reducir broadcast intervals, usar preset con menor airtime (MEDIUM en vez de LONG), separar chat y telemetría en canales diferentes.

### 6.3. Comportamientos prohibidos / problemáticos

| Comportamiento | Razón |
|---|---|
| ❌ Range Test mode en canal público | Spam automático, satura mesh entera |
| ❌ HAM mode en territorio CEPT | Desactiva encripción — viola privacidad de otros + status legal ambiguo |
| ❌ TX Power por encima del límite legal G3 (27 dBm + ganancia antena) | Multa, interferencias |
| ❌ Broadcast de telemetría a < 60 s en canal público | Saturación |
| ❌ Hop limit > 4 sin coordinación previa con la comunidad | Multiplica retransmisiones |
| ❌ Múltiples canales públicos activos con PSK pública (AQ==) | No tiene ganancia práctica |

### 6.4. Etiqueta mesh

- **Nombre único e identificable.** Evita "Node1", "Test", "Meshtastic User" — ayuda a otros a identificar quién está en la mesh.
- **Short-name distintivo.** El short name (4 chars) es lo que aparece en listados — escoger algo legible.
- **Respetar canal LongFast público.** Para conversaciones privadas, crear canal dedicado.
- **Educar nuevos users.** Direccionar a [meshtastic.pt](https://meshtastic.pt) y este manual.
- **Reportar problemas con calma.** Errores técnicos vs malos comportamientos tienen respuestas diferentes.

---

## 7. Troubleshooting

### 7.1. Hardware

**Device no aparece en el ordenador:**

- ¿Cable USB de datos (no solo carga)? Probar con otro cable.
- Drivers (Windows): CH340/CP210x según el chip USB-Serial.
- ¿LED de power encendido? Si no, problema de PSU o switch.

**Calentamiento excesivo:**

- ¿TX Power demasiado alto sin disipación?
- Antena con VSWR alto (mismatch) — puede estar reflejando energía hacia el radio.
- Batería mal contactada generando voltaje inestable.

**Pantalla negra:**

- ¿Brillo a cero (modo ahorro)? Tocar un botón.
- ¿Boot stuck? Leer logs via serial (`meshtastic --noproto`).
- Ver guías específicas de hardware (T-Deck Plus → [Touch Fix Guide](https://github.com/noduscypher/tdeck-guides)).

### 7.2. RF / mesh

**Mensajes no entregados:**

- ¿Hop limit insuficiente para la distancia?
- ¿Otros nodes en range están retransmitiendo (configurados como CLIENT, no CLIENT_MUTE)?
- Duty cycle exceeded — el mensaje queda pendiente.

**Range bajo:**

- ¿Antena correcta para la banda? (868 vs 915)
- ¿Conector limpio, bien apretado?
- ¿Posicionamiento (interior vs exterior, altura, line-of-sight)?

**Duty cycle exceeded:**

- Reducir broadcast frequency (`position.position_broadcast_secs`, `telemetry.*_update_interval`)
- Cambiar a preset con menor airtime (LONG_FAST → MEDIUM_FAST)
- Separar canales

### 7.3. Software

**Firmware brick / no arranca:**

```bash
# Modo DFU + Full erase
meshtastic --noproto
# Luego flashear firmware via web flasher con Full Erase
```

**Configuraciones no se guardan:**

- Encadenar comandos CLI en una línea:

  ```bash
  meshtastic --set lora.region EU_868 --set lora.hop_limit 3 --reboot
  ```

- Cada comando individual reinicia el device, perdiendo estado intermedio.

**MQTT desconecta:**

- Verificar credenciales (`meshdev / large4cats` para meshtastic.pt)
- ¿WiFi estable? ¿Internet accesible?
- ¿Firewall bloqueando port 1883 (MQTT default)?

**No GPS fix:**

- Salir al exterior, vista de cielo abierto.
- Cold start: 5–15 minutos primera vez.
- Antena GPS conectada (algunos modelos tienen cerámica integrada, otros necesitan externa).

### 7.4. Diagnóstico CLI

**Comandos esenciales:**

```bash
meshtastic --info                   # Estado general del node
meshtastic --nodes                  # Lista nodes en la mesh
meshtastic --get lora               # Config LoRa actual
meshtastic --get mqtt               # Config MQTT actual
meshtastic --noproto                # Modo debug (logs en texto)

# Mensajes via CLI
meshtastic --sendtext "hola desde la CLI"
meshtastic --dest '!abcd1234' --sendtext "DM via CLI"

# Reset si todo falla
meshtastic --factory-reset
```

---

## 8. Temas avanzados

### 8.1. CLI completa — workflows comunes

**Setup inicial completo en un comando:**

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

**Backup y restore de configuraciones:**

```bash
# Export configuración actual
meshtastic --export-config > my-config.yaml

# Restore
meshtastic --configure my-config.yaml
```

### 8.2. Custom firmware

- Repositorio principal: <https://github.com/meshtastic/firmware>
- Build local: PlatformIO + VS Code
- Targets pre-definidos para hardware común (`heltec_v3`, `tbeam`, `t-deck`)
- Custom plugins: API Lua existe para extensiones simples

Build típico:

```bash
git clone https://github.com/meshtastic/firmware
cd firmware
pio run -e heltec-v3
# Genera firmware.bin en .pio/build/heltec-v3/
```

### 8.3. Network architecture

**Principios para deployment de mesh comunitaria:**

1. **Routers en altura, con line-of-sight inter-router** — backbone primero
2. **Clients distribuidos** — cobertura por densidad
3. **Hop limit conservador** (3) — multiplica airtime
4. **Múltiples canales privados** para grupos — preserva el canal público para descubrimiento
5. **MQTT gateway dedicado** — 1 o 2 routers hacen uplink, no todos
6. **Documentar** topología, frecuencias, presets, contactos de los node operators

Para Portugal, el [mapa nodes meshtastic.pt](https://meshtastic.pt/map) y el [Malha PT](https://malha.meshtastic.pt) ayudan a visualizar deployment existente antes de añadir.

### 8.4. Integración externa

- **MQTT clients externos:** Mosquitto, MQTT Explorer — útil para dashboards y logging
- **API Python:** el package `meshtastic` permite scripts complejos
- **Home Assistant:** integración via MQTT bridge — sensores LoRa en dashboard HA
- **Grafana + InfluxDB:** logging telemetría long-term
- **Webhooks:** firmware soporta envío de eventos a URLs externas

---

## 9. Apéndices

### Apéndice A — Region codes Meshtastic completos

| Código | Región | Banda | Notas |
|---|---|---|---|
| `EU_868` | Europa (recomendado PT/ES) | 869.4–869.65 MHz | G3, 500 mW, 10% |
| `EU_433` | Europa (alternativa) | 433.05–434.79 MHz | Límites varían por sub-banda |
| `US` | EEUU / Canadá | 902–928 MHz | FCC Part 15 |
| `JP` | Japón | 920–928 MHz | ARIB |
| `CN` | China | 470–510 MHz | SRRC |
| `KR` | Corea del Sur | 920–923 MHz | — |
| `TW` | Taiwán | 920–925 MHz | — |
| `RU` | Rusia | 868.7–869.2 MHz | — |
| `IN` | India | 865–867 MHz | — |
| `NZ_865` | Nueva Zelanda | 864–868 MHz | — |
| `TH` | Tailandia | 920–925 MHz | — |
| `LORA_24` | Global 2.4 GHz (futuro) | 2 400–2 483.5 MHz | Hardware específico |
| `UA_433` | Ucrania 433 | 433–434 MHz | — |
| `UA_868` | Ucrania 868 | 868.0–868.6 MHz | — |
| `MY_433` | Malasia 433 | 433–435 MHz | — |
| `MY_919` | Malasia 919 | 919–924 MHz | — |
| `SG_923` | Singapur | 917–925 MHz | — |
| `PH_433` / `PH_868` / `PH_915` | Filipinas | varias | — |

⚠️ En territorio CEPT (Portugal, España, etc.) usar **solo** `EU_868` (recomendado) o `EU_433`. Otras = ilegal.

### Apéndice B — Modem presets (resumen técnico)

Ver tabla completa en la [sección 4.3](#43-modem-presets).

### Apéndice C — EU_868 frequency slots vs sub-bandas ETSI

Ver mapeo en la [sección 2.3](#23-mapeo-meshtastic-eu_868--sub-bandas).

Para presets diferentes de LongFast, calcular slot via:

- [Calculadora Meshtastic PT](https://meshtastic.pt/tools/powercalc)
- [Frequency Slot Calculator (heypete/GitHub)](https://github.com/heypete/meshtastic_frequency_slot_calculator)

### Apéndice D — Recursos de la comunidad Meshtastic Portugal

- **Website:** <https://meshtastic.pt>
- **Primeros pasos (PT):** <https://meshtastic.pt/getstarted>
- **FAQ:** <https://meshtastic.pt/faq>
- **Hardware compatible:** <https://meshtastic.pt/hardware>
- **Comparador hardware:** <https://meshtastic.pt/comparator>
- **Mapa de nodes:** <https://meshtastic.pt/map>
- **Malha PT (network status):** <https://malha.meshtastic.pt>
- **Calculadora de potencias:** <https://meshtastic.pt/tools/powercalc>
- **Canales comunitarios:** <https://meshtastic.pt/channels>
- **Desbloqueo de node:** <https://meshtastic.pt/unblock>
- **Servidor IoT:** <https://meshtastic.pt/getstarted/iot>
- **Telegram:** <https://t.me/comunidademeshtasticpt>
- **Matrix:** <https://matrix.to/#/!BLCckQWdGZvhvgYrMW:matrix.org>
- **GitHub:** <https://github.com/meshtastic-pt>

> 💡 Aunque la comunidad está basada en Portugal, el sitio meshtastic.pt soporta interfaz en español y la mayoría de la información técnica es directamente aplicable a cualquier país CEPT (incluyendo España).

### Apéndice E — Glosario

| Término | Significado |
|---|---|
| **AES-CTR** | Advanced Encryption Standard, modo Counter — encripción simétrica usada en canales Meshtastic |
| **ALOHA** | Protocolo MAC simple (transmisión sin coordinación previa, retransmisión en caso de colisión) |
| **ANACOM** | Autoridade Nacional de Comunicações (Portugal) — regulador RF |
| **BW** | Bandwidth — anchura espectral usada para la transmisión (kHz) |
| **CR** | Coding Rate — proporción de redundancia FEC (4/5, 4/6, 4/7, 4/8) |
| **CSS** | Chirp Spread Spectrum — modulación base de LoRa |
| **Duty cycle** | Porcentaje máximo de tiempo permitido en transmisión (regulado por ETSI) |
| **EIRP** | Equivalent Isotropic Radiated Power — potencia radiada referenciada a antena isotrópica |
| **ERP** | Effective Radiated Power — potencia radiada referenciada a antena dipolo (EIRP – 2.15 dB) |
| **ETSI** | European Telecommunications Standards Institute |
| **G1/G2/G3/G4** | Sub-bandas ETSI dentro de la banda 863–870 MHz |
| **Hop / Hop limit** | Número de retransmisiones permitidas para que un mensaje se propague en la mesh |
| **LBT / AFA** | Listen Before Talk / Adaptive Frequency Agility — alternativas a duty cycle |
| **LoRa** | Long Range — tecnología RF Semtech propietaria basada en CSS |
| **LXMF** | Lightweight Extensible Message Format (no Meshtastic — protocolo Reticulum) |
| **MQTT** | Message Queuing Telemetry Transport — protocolo pub/sub usado para uplink/downlink internet |
| **MUI** | Meshtastic UI — interface fancy en los dispositivos con display |
| **PKI** | Public Key Infrastructure — usado para encripción punto-a-punto (DMs) |
| **PSK** | Pre-Shared Key — clave simétrica compartida por canal |
| **RED** | Radio Equipment Directive 2014/53/UE |
| **RSSI** | Received Signal Strength Indicator (dBm) |
| **RX / TX** | Recepción / Transmisión |
| **SF** | Spreading Factor (7 a 12) — cantidad de "espaciado" temporal (más SF = más range, menos throughput) |
| **SNR** | Signal-to-Noise Ratio (dB) |
| **SRD** | Short Range Device — dispositivo de baja potencia regulado por ETSI EN 300 220 |
| **SX1262 / SX1276 / LR1110** | Familias de transceivers LoRa Semtech |
| **TOFU** | Trust On First Use — modelo de confianza usado en SimpleX (no Meshtastic) |
| **VSWR** | Voltage Standing Wave Ratio — medida de mismatch entre radio y antena (idealmente <2:1) |

### Apéndice F — Enlaces oficiales (re-verificar antes de publicar)

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

- **v1.0 (2026-05-07):** versión inicial pública. Compliance verificado contra ETSI EN 300 220 (V3.2.1 / V3.3.1) y meshtastic.pt.

---

## Notas finales

Este manual es un documento vivo. La regulación evoluciona (las revisiones de ETSI EN 300 220 salen regularmente; ANACOM y otras autoridades nacionales actualizan recomendaciones), el ecosistema Meshtastic evoluciona (nuevos presets, nuevo hardware, nuevas features), y la práctica de la comunidad portuguesa sigue madurando.

Contribuciones, correcciones y mejoras son bienvenidas a través de la [comunidad Meshtastic PT](https://meshtastic.pt) o directamente en el repositorio.

> **Última verificación técnica:** 2026-05-07
>
> **Próxima revisión sugerida:** después del release ETSI EN 300 220-2 v3.4.x (estimado 2026-Q4) o release Meshtastic firmware 3.0+ (estimado 2026).

---

☥ walk quietly, ₿ut keep the signal alive ⚡

*noduscypher-rawmesh node*
*Licencia: CC BY-SA 4.0 — <https://creativecommons.org/licenses/by-sa/4.0/>*
