# Meshtastic para Principiantes — Portugal/Europa

**Versión:** 1.0 · **Fecha:** 2026-05-07 · **Licencia:** CC BY-SA 4.0
**Para personas que nunca han oído hablar de Meshtastic y quieren empezar.**

> 📄 Léelo en: [PT](../../pt/user/meshtastic-beginner-guide.md) · [EN](../../en/user/meshtastic-beginner-guide.md) · ES

---

## ⚠️ Importante antes de empezar

**Esta guía es para Portugal y Europa (banda 868 MHz).**

Reglas simples:

- ✅ Configura siempre la región como **"EU_868"** (Europa 868 MHz)
- ❌ **Nunca** uses otras regiones (US, Japón, China, Australia) en territorio europeo
- ⚠️ Región errónea = ilegal en Europa y funciona mal

> 💡 **Para usuarios en España:** las normas técnicas son las mismas (ETSI EN 300 220 aplica en todo el espacio CEPT). La única diferencia es la autoridad nacional — en España es la **SETID** (Secretaría de Estado de Telecomunicaciones e Infraestructuras Digitales) en vez de ANACOM.

Si tienes dudas, pide ayuda a la [comunidad Meshtastic PT](https://meshtastic.pt) o únete al [grupo de Telegram](https://t.me/comunidademeshtasticpt).

---

## Índice

1. [Bienvenido a Meshtastic](#1-bienvenido-a-meshtastic)
2. [Primeros pasos — ¿qué necesito?](#2-primeros-pasos--qué-necesito)
3. [Instalación paso a paso](#3-instalación-paso-a-paso)
4. [Uso diario](#4-uso-diario)
5. [Configuraciones simples](#5-configuraciones-simples)
6. [Problemas comunes (FAQ)](#6-problemas-comunes-faq)
7. [Consejos útiles](#7-consejos-útiles)
8. [Únete a la comunidad](#8-únete-a-la-comunidad)
9. [FAQ rápido](#9-faq-rápido)

---

## 1. Bienvenido a Meshtastic

### ¿Qué es?

Imagina **walkie-talkies inteligentes** que se ayudan unos a otros para pasar mensajes. Cada dispositivo recibe un mensaje, y si no es para él, **lo pasa al siguiente** hasta llegar al destino.

Todo esto **sin internet, sin operadora, sin tarjeta SIM**.

### ¿Por qué usarlo?

- 📵 **Funciona sin red móvil** — útil en montaña, mar, eventos, emergencias
- 🔒 **Mensajes privados** — encriptados automáticamente
- 🌍 **Largo alcance** — 5 a 15 km típicos, más con elevación
- 💰 **Hardware barato** — entre 30 € y 100 € por dispositivo
- 🔋 **Bajo consumo** — la batería dura días
- 🤝 **Comunidad** — en Portugal ya hay cientos de usuarios; comunidad creciente en España

### ¿Cómo funciona (sin complicar)?

1. Tu dispositivo Meshtastic es una pequeña radio con batería
2. Se conecta a tu móvil via Bluetooth
3. Escribes mensajes en la app del móvil
4. El dispositivo envía por radio (banda 868 MHz, largo alcance)
5. Otros dispositivos cercanos reciben o **retransmiten** a otros más lejos
6. Cuando llega al destino, aparece en la app de esa persona

La "**mesh**" significa que **todos los dispositivos ayudan a todos los demás** — como una red de amigos donde cada uno pasa el recado.

---

## 2. Primeros pasos — ¿qué necesito?

### Lo esencial

| Item | Qué es | Coste aproximado |
|---|---|---|
| 1× Dispositivo Meshtastic | Pequeño hardware con radio LoRa | 30–80 € |
| 1× Antena 868 MHz | ¡Crítica! Viene incluida en la mayoría | (incluido) |
| 1× Smartphone | Android o iOS reciente | (ya lo tienes) |
| 1× Cable USB-C de datos | Para configurar la 1ª vez | 5 € |

### ¿Dónde comprar?

**Tiendas online con envío a la península:**

- [Tinytronics (NL)](https://www.tinytronics.nl) — buen precio, UE sin aduana
- [OpenELab (DE)](https://eu.openelab.io) — distribuidor europeo Heltec
- [PTRobotics](https://www.ptrobotics.com) — Portugal
- [Mauser](https://mauser.pt) — Portugal
- AliExpress (ojo: aduana posible para envíos > 150 €)

### ¿Qué dispositivo escoger?

**Para empezar (más barato):**
- **Heltec WiFi LoRa 32 V3** (~25–30 €) — pequeño, con display OLED
- **Heltec Wireless Stick Lite V3** (~20 €) — sin display, aún más barato

**Para uso completo (con teclado y pantalla):**
- **LilyGO T-Deck Plus** (~70 €) — standalone, teclado QWERTY, pantalla táctil
  - Para detalles específicos, ver [T-Deck Plus Complete Guide](https://github.com/noduscypher/tdeck-guides)

**Para mochila y GPS:**
- **LilyGO T-Beam V1.2** (~40 €) — con GPS integrado

> 💡 Para una comparación detallada, ver el [comparador hardware meshtastic.pt](https://meshtastic.pt/comparator).

### ¿Qué verificar antes de comprar?

- ✅ **Banda 868 MHz** (NO 915 MHz que es de EEUU, NO 433 MHz)
- ✅ Antena incluida (la mayoría incluye, pero confirma)
- ✅ Cable USB-C (algunos vienen sin él)

---

## 3. Instalación paso a paso

### ⚠️ REGLA #1 antes de todo

**Conecta SIEMPRE la antena ANTES de encender el dispositivo.**

Si lo enciendes sin antena, puedes **dañar permanentemente** la parte de radio. Sin aviso. Sin retorno. Atornilla la antena bien firme antes de conectar USB o batería.

### 3.1. Instalar firmware (el "alma" del dispositivo)

> 💡 "Firmware" es el programa que hace funcionar el dispositivo. Tienes que instalarlo la primera vez (y actualizarlo de vez en cuando).

1. **Ordenador con Google Chrome** (no Firefox — no funciona aquí)
2. Conecta el dispositivo al ordenador via cable USB-C **(de datos, no solo carga)**
3. Abre <https://flasher.meshtastic.org>
4. Click en **"Connect"** y selecciona tu dispositivo en la lista
5. Escoge la **última versión Beta** (recomendado — tiene correcciones recientes)
6. Marca **"Full Erase"** solo la primera vez
7. Click en **"Flash"** y espera (1–3 minutos)
8. **No desconectes** el cable hasta que termine
9. Cuando termine, apaga y vuelve a encender el dispositivo

### 3.2. Instalar la app en el móvil

**Android:**

- [Meshtastic en Play Store](https://play.google.com/store/apps/details?id=com.geeksville.mesh)

**iOS:**

- [Meshtastic en App Store](https://apps.apple.com/es/app/meshtastic/id1586432531)

Instala y abre.

### 3.3. Conectar el móvil al dispositivo

1. **En el dispositivo:** enciéndelo (botón de power)
2. **En el móvil:** ve a la app Meshtastic → "+" → "Connect"
3. Selecciona tu dispositivo (nombre tipo "Meshtastic_xxxx")
4. Acepta el pairing Bluetooth si lo pide

### 3.4. Configuración básica (CRÍTICA)

Esta es la parte más importante. Sin esto, tu node no funciona correctamente en territorio CEPT.

**En la app, ve a Settings (engranaje) → Radio Configuration → LoRa:**

| Configuración | Valor |
|---|---|
| **Region** | **Europe 868** ⚠️ obligatorio en UE |
| **Modem Preset** | LongFast (deja default) |
| **Hop Limit** | 3 (default) o 4 (convención PT) |

**Ve a User:**

- **Long Name:** tu nombre o apodo (ej: `Duarte-8`)
- **Short Name:** abreviatura de 4 caracteres (ej: `DUA8`)

> 💡 **Convención en Portugal:** muchos usuarios añaden `-8` al final del nombre para indicar que usan 868 MHz. Es opcional pero ayuda a identificar quién está en qué banda. No es obligatorio.

**Ve a MQTT** (para conectar a la red comunitaria portuguesa):

| Configuración | Valor |
|---|---|
| **Address** | `mqtt.meshtastic.pt` |
| **Username** | `meshdev` |
| **Password** | `large4cats` |
| **Root Topic** | `msh` |
| **Encryption Enabled** | ✅ ON |
| **TLS Enabled** | ❌ OFF |

> ✓ Estas son las credenciales públicas oficiales de la red meshtastic.pt, verificadas el 2026-05-07.

**Save / Apply.** El dispositivo se reinicia.

¡Listo! Estás en la red.

---

## 4. Uso diario

### 4.1. Enviar mensajes

1. En la app, ve a **"Channels"** (lista de canales)
2. El canal por defecto se llama **"LongFast"** — es el canal público, todos lo ven
3. Escribe el mensaje
4. Envía

> ⚠️ **Importante:** los mensajes en "LongFast" los ven **TODOS los usuarios Meshtastic** dentro del alcance y en la red MQTT. **No es privado.**

### 4.2. Ver otros nodes

- **Lista:** muestra todos los dispositivos que el tuyo ha detectado
- **Mapa:** los muestra geográficamente (si tienen GPS activo)
- Para cada node ves: distancia, señal (RSSI), última actividad

### 4.3. Mensajes privados (DM)

1. Toca un node específico en la lista
2. Toca "Direct Message"
3. Escribe — solo esa persona recibe

> 🔒 **Los mensajes directos están encriptados end-to-end automáticamente.** Aunque sean retransmitidos por otros dispositivos, nadie en el medio puede leerlos.

### 4.4. ¡Mensajes cortos, por favor!

Meshtastic **no es WhatsApp**. El ancho de banda es muy bajo.

- ✅ Mensajes cortos (hasta ~200 caracteres)
- ✅ Conversaciones espaciadas
- ❌ No envíes fotos (no soporta)
- ❌ No hagas chat continuo en el canal público

---

## 5. Configuraciones simples

### 5.1. Privacidad

**No quiero compartir localización:**

- Ve a **Position** → desactiva "GPS Enabled"

**Quiero compartir pero con menos precisión:**

- Position → "Position Precision" → reduce a 14 (~1.5 km de error)

### 5.2. Ahorrar batería

- **Modo CLIENT_MUTE:** si tienes varios nodes próximos tuyos, configura este para recibir pero no retransmitir → ahorra batería
- **Reduce broadcast intervals:** Position Broadcast a 900 s (15 min) en vez de 30 s

### 5.3. Mejorar alcance

- **Antena externa de mejor calidad** (5 dBi es buena elección)
- **Posición elevada** — balcón, ventana alta, azotea
- **Línea de vista** con otros nodes — sin edificios entre vosotros
- **No en sótano** — RF no atraviesa bien cemento + tierra

---

## 6. Problemas comunes (FAQ)

### "No veo mensajes / no recibo"

**Verificar:**

1. ¿La región está en **EU_868**?
2. ¿Antena conectada (incluso solo para recibir)?
3. ¿Hay otros nodes en tu zona? Ver el [mapa meshtastic.pt](https://meshtastic.pt/map)
4. ¿Dispositivo realmente encendido y a la escucha?

### "El dispositivo no conecta a la app"

**Verificar:**

1. ¿Bluetooth activado en el móvil?
2. ¿Dispositivo encendido y LED parpadeando?
3. Reiniciar la app
4. "Forget" del pairing antiguo en Bluetooth del móvil y rehacer

### "La batería se acaba muy rápido"

**Soluciones:**

1. Modo CLIENT_MUTE si múltiples nodes próximos
2. Desactivar GPS si no lo necesitas
3. Desactivar WiFi (solo usa Bluetooth)
4. Reducir TX Power a 17 dBm en vez de 27 dBm (el alcance baja proporcionalmente)

### "Alcance muy bajo"

**Verificar:**

1. ¿Antena correcta para 868 MHz (no 915, no 433)?
2. ¿Conector limpio y bien apretado?
3. ¿Posición (interior vs exterior, altura)?
4. ¿Otros nodes demasiado lejos? (mapa)

### "T-Deck Plus con problemas táctiles"

→ Ver [T-Deck Plus Touch Fix Guide](https://github.com/noduscypher/tdeck-guides) — documento dedicado.

---

## 7. Consejos útiles

### Buena ciudadanía mesh

- ✅ **Mensajes cortos y espaciados** — todos compartimos el canal
- ✅ **Conversaciones largas → canal privado dedicado**
- ✅ **Nombre único e identificable** — evita "Node1", "Test"
- ✅ **Ayuda a nuevos usuarios** — tú también fuiste novato
- ❌ **Sin spam, sin bots en canal público**
- ❌ **No pruebes Range Test mode en el canal público** (envía spam automático)

### Cuándo NO usar Meshtastic

- ❌ Para emergencias serias (usa 112)
- ❌ Para conversaciones largas (usa Signal/Telegram)
- ❌ Para enviar fotos o ficheros
- ❌ Para chat tipo WhatsApp

### Cuándo ES perfecto

- ✅ Coordinación en senderismo sin cobertura
- ✅ Status updates rápidos ("llegué", "estoy aquí")
- ✅ Eventos con mucha gente (festivales, raids)
- ✅ Comunidad técnica local
- ✅ Backup para cuando la red móvil falla

---

## 8. Únete a la comunidad

La comunidad Meshtastic Portugal es activa, acogedora y ayuda a quien empieza.

| Recurso | Enlace |
|---|---|
| **Website oficial PT** | <https://meshtastic.pt> |
| **Primeros pasos PT** | <https://meshtastic.pt/getstarted> |
| **FAQ PT** | <https://meshtastic.pt/faq> |
| **Mapa de nodes PT** | <https://meshtastic.pt/map> |
| **Estado de la red** | <https://malha.meshtastic.pt> |
| **Telegram comunidad** | <https://t.me/comunidademeshtasticpt> |
| **Matrix** | <https://matrix.to/#/!BLCckQWdGZvhvgYrMW:matrix.org> |
| **Instagram** | <https://www.instagram.com/meshtastic.pt> |
| **Facebook grupo** | <https://www.facebook.com/groups/www.meshtastic.pt/> |
| **YouTube** | <https://www.youtube.com/channel/UC3awqON_H724kV4prigjPSA> |

Eventos ocasionales (meetups, demos, hackathons) anunciados en el [calendario meshtastic.pt](https://meshtastic.pt/eventos).

> 💡 Si estás en España y quieres conectar con otros usuarios locales, consulta también los grupos de Telegram/Matrix sobre LoRa y mesh networking en español. La regulación es la misma; la comunidad es complementaria.

---

## 9. FAQ rápido

**¿Es legal usar Meshtastic en Portugal/España?**
Sí, sin necesidad de licencia, siempre que uses la región `EU_868` o `EU_433` y no alteres potencia/duty cycle por encima de los defaults. Las autoridades nacionales (ANACOM en PT, SETID en ES) permiten SRDs en estas bandas libremente para uso personal.

**¿Necesito licencia de radioaficionado?**
No. Meshtastic en modo estándar es Short Range Device (SRD) — no requiere licencia.

**¿Funciona sin internet?**
Sí, ese es el punto principal. Los dispositivos se comunican directamente por radio. Internet solo se necesita si quieres conectar a la red MQTT global (opcional).

**¿Cuál es el alcance?**
Depende mucho del terreno y las antenas:
- Ciudad densa: 1–3 km típico
- Suburbano: 3–8 km
- Rural con elevación: 10–20 km+
- Récords mundiales: 200+ km con elevación extrema (montaña-mar)

**¿Puedo usarlo en la ciudad?**
Sí, pero el alcance se reduce por edificios. En redes mesh densas (mucha gente con nodes), los mensajes llegan más lejos via retransmisión.

**¿Cuánto cuesta empezar?**
Mínimo: 30 € (Heltec V3 + antena, ya tienes cable y móvil). Setup cómodo: 50–100 €.

**¿Cuánto consume la batería?**
En modo CLIENT normal: 1–3 días con batería 18650. En CLIENT_MUTE: 1–2 semanas. Routers fijos: alimentación continua.

**¿Puedo usarlo de viaje en Europa?**
Sí, EU_868 está armonizado en toda la UE + UK + Suiza + Noruega. En España, Portugal, Francia, Alemania, etc. — funciona igual.

**¿Y fuera de Europa?**
**Tienes que cambiar la región** a la del país donde estés (`US`, `JP`, `AU`, etc.). Las antenas 868 MHz funcionan mal en otras frecuencias — puede que necesites antena nueva.

**¿Es seguro / privado?**
- Mensajes directos (DM): encriptados end-to-end (PKI), solo el destinatario lee
- Canales privados (con PSK fuerte): encriptados, solo quien tiene la clave lee
- Canal público "LongFast": cualquier persona en alcance o en la red MQTT lee

Para necesidades de privacidad seria (amenazas avanzadas), usa Signal o SimpleX. Meshtastic es bueno para uso casual pero no fue diseñado para uso adversarial.

---

## Próximos pasos

Cuando quieras profundizar:

- [Manual Técnico Meshtastic](../technical/meshtastic-technical-guide.md) — para entender compliance ETSI, modos avanzados, configuraciones detalladas
- [Guías T-Deck Plus](https://github.com/noduscypher/tdeck-guides) — específicas del hardware T-Deck
- [meshtastic.pt](https://meshtastic.pt) — recurso vivo de la comunidad

---

☥ walk quietly, ₿ut keep the signal alive ⚡

*noduscypher-rawmesh node*
*Licencia: CC BY-SA 4.0*
