# 🧪 LAB14 — C4: Data Store & Idempotência (Deduplicação de Pedidos)

> **Bloco C — Resiliência e Erros** | Camada 1 (trilha oficial) 🥇
> Último cenário do Bloco C: garantir que a **mesma mensagem não seja processada duas vezes**, aplicando o conceito de **Idempotência** por meio de duas abordagens — **Data Store manual** (Caminho A) e **Idempotent Process Call** (Caminho B, best practice SAP).

---

## 🎯 Objetivo

Se o C3 (Dead Letter) garante que **nenhuma mensagem se perca**, o C4 garante o oposto complementar: que **nenhuma mensagem seja duplicada**. Juntos, cobrem os dois maiores desafios de resiliência em integração.

Os objetivos são:

- Compreender e aplicar o conceito de **Idempotência**
- Implementar **deduplicação** de pedidos usando o **Data Store** (Caminho A)
- Implementar a mesma deduplicação com o **Idempotent Process Call** (Caminho B)
- Comparar as duas abordagens e entender **quando usar cada uma**

---

## 🧠 O conceito — O que é Idempotência?

Em integração, uma operação é **idempotente** quando executá-la **uma vez** ou **várias vezes** produz **o mesmo resultado**. Ou seja, mesmo que a mensagem chegue repetida, o efeito no sistema é como se tivesse chegado apenas uma vez.

### 🤔 Por que isso é um problema real?

No mundo real, a **mesma mensagem chega duplicada** com frequência. Alguns exemplos:

| Situação | O que acontece |
|---|---|
| 🖱️ **Usuário impaciente** | Clica "Enviar" várias vezes achando que não funcionou |
| 🔁 **Retry automático** | O sistema não recebe o ACK e reenvia a mensagem |
| 📶 **Falha de rede** | A resposta se perde e o remetente reenvia por precaução |
| ⚙️ **Erro de integração** | Um reprocessamento dispara a mensagem novamente |

Sem tratamento, cada duplicata vira um **novo pedido**: estoque baixado em dobro, cobrança duplicada, ordem de produção repetida. Um desastre operacional. 💥

### ✅ A solução: detectar e ignorar duplicados

```text
Chega o pedido PED-100
      ↓
Já processei PED-100 antes?
      ├─ NÃO → processa + registra a chave  ✅
      └─ SIM → rejeita (duplicado)          ⛔
```

O segredo é ter uma **chave única** (no nosso caso, o número do pedido — `ebeln`) e um **repositório** que "lembra" o que já foi processado. É exatamente isso que o Data Store e o Idempotent Process Call fazem.

---

## 🏭 O cenário — Pedido de Compra (SAP MM)

Usamos uma estrutura de **Purchase Order** no padrão SAP MM, com campos genéricos:

```json
{
  "purchaseOrder": {
    "header": {
      "ebeln": "7800000085",
      "bsart": "ZLAB",
      "lifnr": "FORN-0001",
      "bukrs": "9000",
      "waers": "BRL",
      "bedat": "2026-08-06",
      "messageFunction": "009"
    },
    "items": [
      { "ebelp": "00010", "matnr": "MAT-GEN-001", "txz01": "Balanca Industrial XPTO", "menge": 100, "meins": "PC", "netpr": 250.00 },
      { "ebelp": "00020", "matnr": "MAT-GEN-002", "txz01": "Suporte Generico XPTO", "menge": 50, "meins": "PC", "netpr": 480.00 }
    ]
  }
}
```

| Campo | SAP | Papel no cenário |
|---|---|---|
| **ebeln** | Purchasing Document | **A chave** da deduplicação |
| **messageFunction** | Message Function (IDoc) | `009` = criação, `004` = atualização |

> 💡 O `messageFunction` é o campo real do SAP que diferencia uma mensagem **nova** (`009`) de uma **atualização** (`004`) — usado nos IDocs. Isso enriquece a lógica: o mesmo `ebeln` pode ser rejeitado (duplicado) ou aceito (update), dependendo desse campo.

---

# 🅰️ PARTE 1 — Caminho A: Data Store Manual

## 🏗️ Arquitetura

```mermaid
flowchart LR
    A["Pedido (Postman)"] --> B["Groovy - Extract Key"]
    B --> C["Data Store - Get<br/>(verifica se existe)"]
    C --> D["Groovy - Check Existence<br/>(SAP_DataStoreEntryFound)"]
    D --> E{"Router - Decisão"}
    E -->|novo| F["Write + Resposta 201"]
    E -->|duplicado| G["Resposta 409"]
    E -->|update| H["Write Overwrite + Resposta 200"]
```

## 🔧 Componentes

| Componente | Função |
|---|---|
| **Groovy - Extract Key** | Extrai `ebeln` e `messageFunction` para properties |
| **Data Store - Get** | Consulta se a chave já existe (Throw Exception on Missing: desmarcado) |
| **Groovy - Check Existence** | Lê o header `SAP_DataStoreEntryFound` e define a `decisao` |
| **Router - Dedup Decision** | Roteia: NEW / DUPLICATE / UPDATE |
| **Write 1** (New) | Grava a chave (sem overwrite) |
| **Write 2** (Update) | Sobrescreve a chave (com overwrite) |
| **Groovys de resposta** | Montam as respostas PROCESSED / REJECTED / UPDATED |

## 🔑 A detecção correta

A chave de tudo é o header que o **Data Store Get** preenche ao consultar:

```text
SAP_DataStoreEntryFound = 'true'   → a chave JÁ existe
SAP_DataStoreEntryFound = 'false'  → a chave NÃO existe
```

O Groovy de verificação usa esse header para decidir:

```groovy
def encontrado = message.getHeaders().get("SAP_DataStoreEntryFound")
def existe = (encontrado?.toString()?.equalsIgnoreCase("true")) ? "SIM" : "NAO"

def messageFunction = message.getProperty("messageFunction")?.toString() ?: "009"
def decisao
if (messageFunction == "004") {
    decisao = (existe == "SIM") ? "UPDATE" : "NEW"
} else {
    decisao = (existe == "SIM") ? "DUPLICATE" : "NEW"
}
message.setProperty("decisao", decisao)
```

> 🐛 **Nota de troubleshooting (resumida):** a detecção de existência exigiu iteração. Métodos baseados no corpo da mensagem e em headers específicos de versão (`SapDataStoreEntryId`, `SapDataStoreState`) apresentaram instabilidade. A solução **robusta e confiável** foi o header oficial **`SAP_DataStoreEntryFound`**, que retorna `true`/`false` de forma consistente.

## 🧩 Os 3 comportamentos (com dados reais dos testes)

### ✅ CREATE — pedido novo (messageFunction 009)

**Resposta — 201 Created:**
```json
{
  "status": "PROCESSED",
  "codigo": "DEDUP-201",
  "pedido": "7800000085",
  "mensagem": "Pedido novo processado e registrado com sucesso",
  "acao": "GRAVADO_NO_DATA_STORE"
}
```

### ⛔ DUPLICATE — mesmo pedido reenviado (009)

**Resposta — 409 Conflict:**
```json
{
  "status": "REJECTED",
  "codigo": "DEDUP-409",
  "pedido": "7800000085",
  "mensagem": "Pedido duplicado - ja foi processado anteriormente",
  "acao": "IGNORADO_POR_IDEMPOTENCIA"
}
```

### 🔄 UPDATE — atualização do pedido (messageFunction 004)

**Request (quantidades e preços alterados):**
```json
{
  "purchaseOrder": {
    "header": { "ebeln": "7800000085", "messageFunction": "004", "...": "..." },
    "items": [
      { "ebelp": "00010", "matnr": "MAT-GEN-001", "txz01": "Balanca Industrial XPTO", "menge": 150, "meins": "PC", "netpr": 400.00 },
      { "ebelp": "00020", "matnr": "MAT-GEN-002", "txz01": "Suporte Generico XPTO", "menge": 33, "meins": "PC", "netpr": 550.00 }
    ]
  }
}
```

**Resposta — 200 OK:**
```json
{
  "status": "UPDATED",
  "codigo": "DEDUP-200",
  "pedido": "7800000085",
  "mensagem": "Pedido existente atualizado com sucesso",
  "acao": "SOBRESCRITO_NO_DATA_STORE",
  "atualizadoEm": "2026-08-06T16:29:57"
}
```

> 💡 **Destaque do Caminho A:** ele suporta o **UPDATE com sobrescrita real** — o Write 2 (com Overwrite) substitui os dados antigos pelos novos no Data Store. Essa flexibilidade é o grande diferencial desta abordagem.

## 📊 Códigos HTTP profissionais

Cada resposta usa o status HTTP correto para o cenário:

| Cenário | HTTP | Significado |
|---|---|---|
| Create | **201** Created | Recurso criado |
| Duplicate | **409** Conflict | Conflito (recurso já existe) — padrão da indústria para duplicidade |
| Update | **200** OK | Recurso atualizado |

---

# 🅱️ PARTE 2 — Caminho B: Idempotent Process Call (Best Practice SAP)

## 🎯 A abordagem nativa da SAP

O **Idempotent Process Call** é um componente **projetado especificamente** para deduplicação. Ele mantém um **repositório idempotente interno** e detecta duplicados **automaticamente** — sem Data Store manual, sem Get, sem verificação de header.

```text
Idempotent Process Call verifica o Message ID:
   ├─ ID novo      → CamelDuplicateMessage = false
   └─ ID já visto  → CamelDuplicateMessage = true
```

## 🏗️ Arquitetura

```mermaid
flowchart LR
    A["Pedido (Postman)"] --> B["Groovy - Extract Key"]
    B --> C["Idempotent Process Call"]
    C --> D["Local Integration Process"]
    D --> E{"Router<br/>CamelDuplicateMessage?"}
    E -->|false| F["Resposta 201 PROCESSED"]
    E -->|true| G["Resposta 409 REJECTED"]
```

## 🔧 A estrutura em dois níveis

O padrão usa um **Local Integration Process** (subprocesso) que o Idempotent Process Call chama:

```text
┌─ Integration Process (principal) ─────────────────────┐
│  Start → Groovy Extract Key → Idempotent Call → End   │
│                                    ↓ chama              │
│  ┌─ Local Integration Process (Process_Dedup) ──────┐  │
│  │  Start → Router - Duplicate Decision            │  │
│  │            ├─ CamelDuplicateMessage=true →       │  │
│  │            │     Groovy Response Duplicate → End  │  │
│  │            └─ default →                           │  │
│  │                  Groovy Response Processed → End  │  │
│  └───────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

## 🔑 Configuração do Idempotent Process Call

| Parâmetro | Valor |
|---|---|
| **Local Integration Process** | Process_Dedup |
| **Message ID** | `${property.ebeln}` |
| **Skip Process Call for Duplicates** | **DESMARCADO** |

> 💡 **Por que "Skip" desmarcado?** Se marcado, o componente simplesmente **pula** o subprocesso nos duplicados (não executa nada). Desmarcado, ele **executa o subprocesso mesmo em duplicados**, mas com `CamelDuplicateMessage = true` — permitindo montar uma **resposta customizada** de rejeição.

## 🔀 A decisão no Router

O Router dentro do subprocesso usa a flag automática:

```text
Route_Duplicate:  ${property.CamelDuplicateMessage} = 'true'
Route_New:        Default Route
```

## 🧩 Os 2 comportamentos (com dados reais)

### ✅ CREATE — pedido novo

**Resposta — 201 Created:**
```json
{
  "status": "PROCESSED",
  "codigo": "IDEM-201",
  "pedido": "7800000090",
  "mensagem": "Pedido novo processado (exactly-once garantido)",
  "acao": "REGISTRADO_NO_IDEMPOTENT_REPOSITORY",
  "processadoEm": "2026-08-06T17:19:09"
}
```

### ⛔ DUPLICATE — mesmo pedido reenviado

**Resposta — 409 Conflict:**
```json
{
  "status": "REJECTED",
  "codigo": "IDEM-409",
  "pedido": "7800000090",
  "mensagem": "Pedido duplicado detectado pelo Idempotent Process Call",
  "acao": "IGNORADO_EXACTLY_ONCE",
  "rejeitadoEm": "2026-08-06T17:20:46"
}
```

> 💡 **Destaque do Caminho B:** a detecção é **automática e nativa**. O componente cuida de armazenar e comparar o Message ID sozinho — código e estrutura muito mais enxutos. É o padrão **exactly-once** recomendado pela SAP.

---

## ⚖️ Comparação: Caminho A vs Caminho B

| Aspecto | 🅰️ Data Store manual | 🅱️ Idempotent Process Call |
|---|---|---|
| **Detecção** | Manual (Get + Groovy + header) | Automática (nativa) |
| **Componentes** | Muitos (Get, Groovy, Router, 2 Writes) | Poucos (Idempotent Call + subprocesso) |
| **Suporta UPDATE** | ✅ Sim (sobrescrita real) | ❌ Não (foco em exactly-once) |
| **Complexidade** | Maior (mais controle) | Menor (mais limpo) |
| **Best practice SAP** | Válido | ⭐ Recomendado para exactly-once |
| **Flexibilidade** | Alta (lógica customizável) | Focada (novo vs duplicado) |
| **Repositório** | Data Store (visível, gerenciável) | Repositório idempotente interno |

### 🎯 Quando usar cada um?

- **Use o Caminho A (Data Store)** quando precisar de **controle e flexibilidade**: suportar atualização/sobrescrita, aplicar lógica de negócio complexa, ou gerenciar/inspecionar as chaves armazenadas.
- **Use o Caminho B (Idempotent Process Call)** quando o objetivo for **exactly-once puro** (novo vs. duplicado) com o mínimo de código — a forma mais limpa e recomendada pela SAP.

> 💡 **Decisão de arquitetura:** as duas abordagens não competem — cada uma tem seu ponto forte. Saber escolher a ferramenta certa para cada requisito é o que diferencia um integrador experiente.

---

## 💡 Melhoria futura identificada

Durante o desenvolvimento, identificou-se um **caso de borda**: uma mensagem de **atualização** (`messageFunction 004`) para um pedido que **não existe** no repositório. No cenário atual, ela é tratada como criação (comportamento *upsert* tolerante).

Em um ambiente produtivo com ERP, o correto seria **rejeitar** essa mensagem com um erro de negócio (*"pedido não encontrado para atualização"*), pois **não se pode atualizar um registro inexistente**. Essa validação foi identificada e documentada como evolução futura — reconhecendo a regra de negócio correta mesmo sem implementá-la neste laboratório de estudo.

---

## 📸 Evidências

### 🅰️ Caminho A — Data Store Manual

**Construção**

**1. iFlow completo + configuração do Data Store**
![iFlow Data Store](../evidences/lab13/01-iflow-datastore-config.png)

**2. Router — decisão de deduplicação**
![Router Dedup](../evidences/lab13/02-router-dedup-config.png)

**3. Write 1 — gravação (caminho New)**
![Write 1](../evidences/lab13/03-datastore-write1-new.png)

**4. Write 2 — gravação com Overwrite (caminho Update)**
![Write 2](../evidences/lab13/04-datastore-write2-update.png)

**✅ Fluxo CREATE**

**5. Postman — envio create (201)**
![Postman Create](../evidences/lab13/05-postman-create-201.png)

**6. Integration Flow — caminho até End_New**
![Flow New](../evidences/lab13/06-flow-path-new.png)

**7. Message Content — resposta PROCESSED**
![Message Processed](../evidences/lab13/07-message-content-processed.png)

**⛔ Fluxo DUPLICATE**

**8. Postman — reenvio (409)**
![Postman Duplicate](../evidences/lab13/08-postman-duplicate-409.png)

**9. Integration Flow — caminho até End_Duplicate**
![Flow Duplicate](../evidences/lab13/09-flow-path-duplicate.png)

**10. Message Content — resposta REJECTED**
![Message Rejected](../evidences/lab13/10-message-content-rejected.png)

**🔄 Fluxo UPDATE**

**11. Postman — envio update (200)**
![Postman Update](../evidences/lab13/11-postman-update-200.png)

**12. Integration Flow — caminho até End_Update**
![Flow Update](../evidences/lab13/12-flow-path-update.png)

**13. Message Content — resposta UPDATED**
![Message Updated](../evidences/lab13/13-message-content-updated.png)

---

### 🅱️ Caminho B — Idempotent Process Call

**Construção**

**14. iFlow B + configuração do Idempotent Process Call**
![iFlow B Idempotent](../evidences/lab13/14-iflowB-idempotent-config.png)

**15. Router — decisão por CamelDuplicateMessage**
![Router B](../evidences/lab13/15-routerB-duplicate-decision.png)

**✅ Fluxo CREATE**

**16. Postman — envio create (201, IDEM-201)**
![Postman B Create](../evidences/lab13/16-postmanB-create-201.png)

**17. Integration Flow — trilha até End_New**
![Flow B New](../evidences/lab13/17-flowB-path-new.png)

**18. Message Content — resposta PROCESSED**
![Message B Processed](../evidences/lab13/18-messageB-content-processed.png)

**⛔ Fluxo DUPLICATE**

**19. Postman — reenvio (409, IDEM-409)**
![Postman B Duplicate](../evidences/lab13/19-postmanB-duplicate-409.png)

**20. Integration Flow — trilha até End_Duplicate**
![Flow B Duplicate](../evidences/lab13/20-flowB-path-duplicate.png)

**21. Message Content — resposta REJECTED**
![Message B Rejected](../evidences/lab13/21-messageB-content-rejected.png)

---

## ✅ Conclusão

O cenário C4 aplicou o conceito de **Idempotência** — garantir que a mesma mensagem, mesmo chegando repetidas vezes, produza o efeito de uma única execução. Foram implementadas **duas abordagens complementares**:

- O **Caminho A (Data Store manual)** oferece máximo controle, suportando inclusive **atualização com sobrescrita real** dos dados, além de create e rejeição de duplicados.
- O **Caminho B (Idempotent Process Call)** entrega a mesma deduplicação de forma **nativa, enxuta e recomendada pela SAP**, focada no padrão *exactly-once*.

Implementar as duas abordagens — e compreender **quando usar cada uma** — demonstra domínio não apenas técnico, mas de **critério arquitetural**. Combinado ao C3 (Dead Letter), o C4 fecha os dois pilares da resiliência de mensagens: **não perder** (C3) e **não duplicar** (C4).

Com este laboratório, o **Bloco C — Resiliência e Erros** é concluído (C1 a C4).

**Recursos praticados:** Data Store (Get/Write) · Idempotent Process Call · Local Integration Process · Router · Groovy · Header `SAP_DataStoreEntryFound` · flag `CamelDuplicateMessage` · Idempotência / Exactly-once · Códigos HTTP (201/409/200) · Monitoring/Trace

**Cenário anterior:** [C3 — Dead Letter com JMS](./14-c3-dead-letter.md)

**Próximo bloco:** Bloco D — Conectividade / Adapters (OData, SOAP, SFTP, ProcessDirect)
