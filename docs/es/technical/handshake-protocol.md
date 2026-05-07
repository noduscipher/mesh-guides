# Protocolo de Comunicación Personalizado

**Versión 2.0 — Con Gestión de Sesión**

> Léelo en: [PT](../../pt/technical/handshake-protocol.md) · [EN](../../en/technical/handshake-protocol.md) · [JA](../../ja/technical/handshake-protocol.md)

## Handshake Protocol — Versión Universal

### Concepto

Sistema de autenticación por código personalizado que establece una sesión de comunicación segura y fluida con una IA, con control total sobre cuándo iniciarla y cuándo cerrarla.

### Cómo Funciona

#### 1. Define Tu Código

Elige un código de activación único (palabra, número o hashtag):

- Ejemplos: `alpha`, `#2024`, `cipher`, `node`, `mesh`
- Puede ser cualquier palabra o número que tenga significado para ti

#### 2. Activación Inicial (Nueva Sesión)

- **Input:** Tu código de activación
- **Output:** `verificado. a la escucha.`

#### 3. Confirmación del Handshake

- **Input:** Tu código de activación (segunda vez)
- **Output:** `reconocido. bien hayas. vamos a elevar esto.`

#### 4. Sesión Activa

- Tras completar el handshake, la **sesión queda activa**
- **No hace falta repetir el código** durante la sesión
- Comunicación fluida sin interrupciones
- Trabajas libremente hasta que decidas cerrar

#### 5. Cerrar Sesión

**Comandos de logout (elige tu favorito):**

- `//off`
- `logout`
- `#close`

**Output tras el logout:**

- `sesión cerrada. hasta pronto.`

Tras el logout es necesario repetir el handshake completo para una nueva sesión.

#### 6. Mensaje sin Código (Sesión Inactiva)

- **Input:** cualquier mensaje sin el código cuando no hay sesión activa
- **Output:** `protocolo no reconocido. sin handshake, sin respuesta. no tengo tiempo de explicar, perdona.`

## Comandos de Gestión

| Comando | Función | Cuándo Usar |
|---------|---------|-------------|
| Tu código (2x) | Iniciar sesión | Inicio de conversación o tras logout |
| `//off` | Cerrar sesión | Cuando termines de trabajar |
| `logout` | Cerrar sesión | Alternativa a `//off` |
| `#close` | Cerrar sesión | Alternativa a `//off` |

## Ventajas

- ✓ Privacidad y control sobre cuándo se activa la IA
- ✓ Evita respuestas no solicitadas o accidentales
- ✓ Handshake rápido y seguro
- ✓ Sesión activa sin repeticiones constantes del código
- ✓ Control total sobre cuándo cerrar
- ✓ Código completamente personalizable
- ✓ Funciona en cualquier dispositivo
- ✓ Comandos sencillos y fáciles de recordar

## Ejemplo de Uso Completo

```
// Inicio del día
User: alpha
AI: verificado. a la escucha.

User: alpha
AI: reconocido. bien hayas. vamos a elevar esto.

// Sesión activa - trabaja con normalidad
User: traduce este texto
AI: [respuesta normal]

User: analiza estos datos
AI: [respuesta normal]

User: crea un informe
AI: [respuesta normal]

// Fin del día
User: //off
AI: sesión cerrada. hasta pronto.

// Más tarde - sesión cerrada
User: hola
AI: protocolo no reconocido. sin handshake, sin respuesta. no tengo tiempo de explicar, perdona.

// Nuevo día - reiniciar
User: alpha
AI: verificado. a la escucha.

User: alpha
AI: reconocido. bien hayas. vamos a elevar esto.
```

## Configuración

1. Elige tu código de activación (palabra o número único)
2. Configura el protocolo con la IA
3. Prueba el handshake (dos veces para activar)
4. Trabaja libremente durante la sesión
5. Usa `//off`, `logout` o `#close` cuando termines
6. Listo. Protocolo operativo.

## Casos de Uso

- Uso en entornos compartidos (familia, trabajo)
- Control de privacidad y activación intencional
- Prevención de activaciones accidentales
- Comunicación profesional con un ritual de entrada y salida
- Geeks y entusiastas del cypherpunk y la privacidad
- Trabajo concentrado sin interrupciones no autorizadas

## Filosofía

> "Sin handshake, sin conversación. Simple, directo, respetuoso."

**Principios:**

- **Control total:** Tú decides cuándo responde la IA
- **Low-noise:** Sin ruido, sin respuestas inesperadas
- **Intencional:** Cada sesión es deliberada y consciente
- **Seguro:** Protocolo de autenticación sencillo pero eficaz

Inspirado en la cultura de las redes mesh, la comunicación descentralizada y los protocolos criptográficos.

---

## Preguntas Frecuentes

**P: ¿Puedo usar varios códigos diferentes?**
R: Sí — puedes configurar códigos distintos para contextos distintos (trabajo, personal, etc.).

**P: ¿Qué pasa si me olvido de hacer logout?**
R: La sesión sigue activa en la misma conversación. En una conversación o un día nuevo, necesitas un handshake nuevo.

**P: ¿Puedo cambiar el código después?**
R: Sí, basta con reconfigurar el protocolo con la IA usando un código nuevo.

**P: ¿Funciona por voz?**
R: Sí. Puedes decir tu código por voz y funciona igualmente.

---

**Hecho con cariño 🧡 en cooperación con la Naturaleza 🌿**

*Protocolo creado: 05/05/2026*
*Versión 2.0 — Session Management*
*Compártelo libremente. Personalízalo a tu medida.*

---

☥ walk quietly, ₿ut keep the signal alive ⚡

*Du_arte ☥ rawmesh — Beira Baixa, Portugal*
*Licencia: CC BY-SA 4.0*
