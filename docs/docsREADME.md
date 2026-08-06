# 📚 Documentação — SAP Integration Suite Learning

Índice da documentação técnica do projeto, organizado por bloco de cenários. Cada documento descreve objetivo, arquitetura, passo a passo, troubleshooting e evidências do laboratório correspondente.

---

## 🧩 Base conceitual

| Doc | Conteúdo |
|---|---|
| [01 — Ambiente SAP BTP](./01-ambiente-btp.md) | Estrutura do ambiente, capabilities e autenticação |
| [02 — Cloud Integration (CPI) Básico](./02-cloud-integration-basics.md) | Conceitos fundamentais de iFlows, adapters e monitoramento |

---

## 🅰️ Bloco A — CPI Fundamentos

| Cenário | Doc | Descrição |
|---|---|---|
| A1 | [03 — HTTP → Webhook](./03-a1-http-to-webhook.md) | Receber, ajustar e encaminhar mensagem |
| A2 | [04 — Timer → API pública](./04-a2-timer-to-api.md) | Consumir API externa via Request Reply |
| A3 | [05 — Message Mapping (JSON → XML)](./05-a3-message-mapping.md) | Transformação de estrutura e formato |
| A4 | [06 — Groovy Script](./06-a4-groovy-script.md) | Lógica de negócio e enriquecimento |

---

## 🅱️ Bloco B — Padrões de Integração

| Cenário | Doc | Descrição |
|---|---|---|
| B1 | [07 — Content-Based Router](./07-b1-content-based-router.md) | Roteamento por conteúdo (XPath) |
| B2 | [08 — Content Enricher](./08-b2-content-enricher.md) | Enriquecimento via lookup OData V4 |
| B3 | [09 — Splitter](./09-b3-splitter.md) | Divisão de lote em itens individuais |
| B4 | [10 — Aggregator](./10-b4-aggregator.md) | Consolidação de mensagens (CamelSplitComplete) |
| B5 | [11 — Multicast](./11-b5-multicast.md) | Distribuição simultânea (MES/PLM/ERP) |

---

## 🇨 Bloco C — Resiliência e Erros

| Cenário | Doc | Descrição |
|---|---|---|
| C1 | [12 — Exception Subprocess](./12-c1-exception-subprocess.md) | Tratamento de erros (try/catch visual) |
| C2 | [13 — Retry](./13-c2-retry-timeout.md) | Reenvio automático em falhas temporárias |
| C3 | [14 — Dead Letter (JMS)](./14-c3-dead-letter.md) | Guaranteed delivery, retry assíncrono, dead letter |
| C4 | [15 — Data Store & Idempotência](./15-c4-data-store.md) | Deduplicação (Data Store + Idempotent Process Call) |

---

## 🇩 Bloco D — Conectividade / Adapters

| Cenário | Doc | Descrição |
|---|---|---|
| D1 | [16 — OData Adapter](./16-d1-odata-adapter.md) | Query dinâmica ao OData V4 (Northwind) |
| D2 | _em breve_ | SOAP Adapter |
| D3 | _em breve_ | SFTP Adapter |
| D4 | _em breve_ | ProcessDirect |

---

## 🔜 Próximos blocos

| Bloco | Tema |
|---|---|
| E | API Management (Proxy, Policies, OAuth, Developer Portal) |
| F | Segurança (CSRF real, Client Certificate, OAuth 2.0, Keystore) |
| G | Cenários SAP MM / PP / QM |
| H | Event-Driven e End-to-End |

---

> 📌 Voltar para o [README principal do projeto](../README.md)
