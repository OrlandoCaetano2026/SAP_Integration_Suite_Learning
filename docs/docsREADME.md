## 📚 Documentação — SAP Integration Suite Learning

Índice da documentação técnica do projeto, organizado por bloco de cenários. Cada documento descreve objetivo, arquitetura, passo a passo, troubleshooting e evidências do laboratório correspondente.

### 🧩 Base conceitual

| Doc | Conteúdo |
|---|---|
| [01 — Ambiente SAP BTP](./01-ambiente-btp.md) | Estrutura do ambiente, capabilities e autenticação |
| [02 — Cloud Integration (CPI) Básico](./02-cloud-integration-basics.md) | Conceitos fundamentais de iFlows, adapters e monitoramento |

### Ⓐ Bloco A — CPI Fundamentos

| Cenário | Doc | Descrição |
|---|---|---|
| A1 | [03 — HTTP → Webhook](./03-a1-http-to-webhook.md) | Receber, ajustar e encaminhar mensagem |
| A2 | [04 — Timer → API pública](./04-a2-timer-to-api.md) | Consumir API externa via Request Reply |
| A3 | [05 — Message Mapping (JSON → XML)](./05-a3-message-mapping.md) | Transformação de estrutura e formato |
| A4 | [06 — Groovy Script](./06-a4-groovy-script.md) | Lógica de negócio e enriquecimento |

### Ⓑ Bloco B — Padrões de Integração

| Cenário | Doc | Descrição |
|---|---|---|
| B1 | [07 — Content-Based Router](./07-b1-content-based-router.md) | Roteamento por conteúdo (XPath) |
| B2 | [08 — Content Enricher](./08-b2-content-enricher.md) | Enriquecimento via lookup OData V4 |
| B3 | [09 — Splitter](./09-b3-splitter.md) | Divisão de lote em itens individuais |
| B4 | [10 — Aggregator](./10-b4-aggregator.md) | Consolidação de mensagens (CamelSplitComplete) |
| B5 | [11 — Multicast](./11-b5-multicast.md) | Distribuição simultânea (MES/PLM/ERP) |

### Ⓒ Bloco C — Resiliência e Erros

| Cenário | Doc | Descrição |
|---|---|---|
| C1 | [12 — Exception Subprocess](./12-c1-exception-subprocess.md) | Tratamento de erros (try/catch visual) |
| C2 | [13 — Retry](./13-c2-retry-timeout.md) | Reenvio automático em falhas temporárias |
| C3 | [14 — Dead Letter (JMS)](./14-c3-dead-letter.md) | Guaranteed delivery, retry assíncrono, dead letter |
| C4 | [15 — Data Store & Idempotência](./15-c4-data-store.md) | Deduplicação (Data Store + Idempotent Process Call) |

### Ⓓ Bloco D — Conectividade / Adapters

| Cenário | Doc | Descrição |
|---|---|---|
| D1 | [16 — OData Adapter](./16-d1-odata-adapter.md) | Query dinâmica ao OData V4 (Northwind) |
| D2 | [17 — SOAP Adapter](./17-d2-soap-adapter.md) | Integração com Web Service SOAP externo (Split/Gather) |
| D3 | [18 — SFTP Adapter](./18-d3-sftp-adapter.md) | Integração de arquivos via hot folder (Producer/Consumer) |
| D4 | [19 — ProcessDirect + JDBC](./19-d4-processdirect.md) | Comunicação interna entre iFlows + validação de fornecedor via banco de dados |

> 💡 O cenário **D5 — JDBC Adapter**, originalmente planejado separadamente, foi incorporado ao **D4**, que já cobre ProcessDirect + JDBC de forma integrada e realista.

### Ⓔ Bloco E — API Management

| Cenário | Doc | Descrição |
|---|---|---|
| E0, E1, E12 | [20 — API Proxy + Basic Authentication](./20-e-api-management-proxy-basic-auth.md) | Ativação da capability, API Provider, primeiro API Proxy e autenticação do Proxy junto ao backend via Key Value Map criptografado |
| E2 | [21 — Verify API Key](./21-e2-verify-api-key.md) | Proteção do Proxy contra consumidores externos via API Key, com fluxo completo de API Product e Developer Hub |
| E3 | [22 — OAuth 2.0 e Scopes](./22-e3-oauth-scopes.md) | Servidor de tokens OAuth Client Credentials, primeira operação de escrita via API, e permissões diferenciadas por Scope (Fornecedor vs. Compliance) |
| E4, E5 | [23 — Quota Dinâmica e Spike Arrest](./23-e4-e5-quota-spike-arrest.md) | Planos comerciais (Free/Premium) com limite de chamadas dinâmico, e proteção contra rajadas instantâneas de tráfego |

> 💡 Os cenários **E8 (API Products e planos)** e **E9 (Developer Portal)** já foram amplamente explorados na prática dentro dos docs 21, 22 e 23 (criação de Products, Rate Plans e Developer Apps), embora ainda não possuam um documento dedicado e isolado.

### 🔜 Próximos blocos

| Bloco | Tema |
|---|---|
| Ⓔ E | Policies restantes: JSON ↔ XML, Assign Message, Access Control, API Analytics |
| Ⓕ F | Segurança (CSRF real, Client Certificate, OAuth 2.0, Keystore) |
| Ⓖ G | Cenários SAP MM / PP / QM |
| Ⓗ H | Event-Driven e End-to-End |

📌 Voltar para o [README principal do projeto](../README.md)
