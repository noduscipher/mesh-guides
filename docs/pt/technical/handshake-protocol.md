# Protocolo de Comunicação Personalizado

**Versão 2.0 — Com Gestão de Sessão**

> Read this in: [EN](../../en/technical/handshake-protocol.md) · [ES](../../es/technical/handshake-protocol.md) · [JA](../../ja/technical/handshake-protocol.md)

## Handshake Protocol — Versão Universal

### Conceito

Sistema de autenticação por código personalizado que estabelece uma sessão de comunicação segura e fluida, com controlo total sobre quando iniciar e terminar.

### Como Funciona

#### 1. Definir o Teu Código

Escolhe um código de ativação único (palavra, número ou hashtag):

- Exemplos: `alpha`, `#2024`, `cipher`, `node`, `mesh`
- Pode ser qualquer palavra ou número que tenha significado para ti

#### 2. Ativação Inicial (Nova Sessão)

- **Input:** O teu código de ativação
- **Output:** `verified. listening.`

#### 3. Confirmação de Handshake

- **Input:** O teu código de ativação (segunda vez)
- **Output:** `reconhecido. bem-haja. vamos elevar isto.`

#### 4. Sessão Ativa

- Após handshake completo, a **sessão mantém-se ativa**
- **Não é necessário repetir o código** durante a sessão
- Comunicação fluida sem interrupções
- Trabalhas livremente até decidires fechar

#### 5. Fechar Sessão

**Comandos de logout (escolhe o teu favorito):**

- `//off`
- `logout`
- `#close`

**Output após logout:**

- `sessão encerrada. até breve.`

Após logout, é necessário repetir handshake completo para nova sessão.

#### 6. Mensagem sem Código (Sessão Inativa)

- **Input:** qualquer mensagem sem o código quando não há sessão ativa
- **Output:** `protocolo não reconhecido. sem handshake, sem resposta. não tenho tempo para te explicar, desculpa.`

## Comandos de Gestão

| Comando | Função | Quando Usar |
|---------|--------|-------------|
| Teu código (2x) | Iniciar sessão | Início de conversa ou após logout |
| `//off` | Fechar sessão | Quando terminares de trabalhar |
| `logout` | Fechar sessão | Alternativa ao `//off` |
| `#close` | Fechar sessão | Alternativa ao `//off` |

## Vantagens

- ✓ Privacidade e controlo sobre quando ativar a IA
- ✓ Evita respostas não solicitadas ou acidentais
- ✓ Handshake rápido e seguro
- ✓ Sessão ativa sem repetições constantes do código
- ✓ Controlo total sobre quando fechar
- ✓ Código completamente personalizável
- ✓ Funciona em todos os dispositivos
- ✓ Comandos simples e fáceis de lembrar

## Exemplo de Uso Completo

```
// Início do dia
User: alpha
AI: verified. listening.

User: alpha
AI: reconhecido. bem-haja. vamos elevar isto.

// Sessão ativa - trabalhas normalmente
User: traduz este texto
AI: [resposta normal]

User: analisa estes dados
AI: [resposta normal]

User: cria um relatório
AI: [resposta normal]

// Fim do dia
User: //off
AI: sessão encerrada. até breve.

// Mais tarde - sessão fechada
User: olá
AI: protocolo não reconhecido. sem handshake, sem resposta. não tenho tempo para te explicar, desculpa.

// Novo dia - reiniciar
User: alpha
AI: verified. listening.

User: alpha
AI: reconhecido. bem-haja. vamos elevar isto.
```

## Configuração

1. Escolhe o teu código de ativação (palavra/número único)
2. Configura o protocolo com a IA
3. Testa o handshake (duas vezes para ativar)
4. Trabalha livremente durante a sessão
5. Usa `//off`, `logout` ou `#close` quando terminares
6. Pronto! Protocolo operacional

## Casos de Uso

- Uso em ambientes partilhados (família, trabalho)
- Controlo de privacidade e ativação intencional
- Prevenção de ativações acidentais
- Comunicação profissional com ritual de entrada/saída
- Geeks e entusiastas de cypherpunk/privacidade
- Trabalho focado sem interrupções não autorizadas

## Filosofia

> "Sem handshake, sem conversa. Simples, direto, respeitoso."

**Princípios:**

- **Controlo total:** Tu decides quando a IA responde
- **Low-noise:** Sem barulho, sem respostas inesperadas
- **Intencional:** Cada sessão é deliberada e consciente
- **Seguro:** Protocolo de autenticação simples mas eficaz

Inspirado na cultura de redes mesh, comunicação descentralizada e protocolos criptográficos.

---

## Perguntas Frequentes

**P: Posso usar vários códigos diferentes?**
R: Sim, podes configurar códigos diferentes para contextos diferentes (trabalho, pessoal, etc.)

**P: O que acontece se esquecer de fazer logout?**
R: A sessão mantém-se ativa na mesma conversa. Em nova conversa/dia, precisas de novo handshake.

**P: Posso mudar o código depois?**
R: Sim, é só reconfigurar com a IA com um novo código.

**P: Funciona por voz?**
R: Sim! Podes dizer o teu código por voz e funciona na mesma.

---

**Feito com cuidado 🧡 em cooperação com a Natureza 🌿**

*Protocolo criado em: 05/05/2026*
*Versão 2.0 — Session Management*
*Partilha livremente. Personaliza à tua medida.*

---

☥ walk quietly, ₿ut keep the signal alive ⚡


*Licença: CC BY-SA 4.0*
