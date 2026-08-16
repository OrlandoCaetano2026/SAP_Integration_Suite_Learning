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
| E0, E1, E12 | [20 — API Proxy + Basic Authentication](./20-e-api-management-proxy-basic-auth.md) | API Provider, Proxy e KVM com Basic Authentication |
| E2 | [21 — Verify API Key](./21-e2-verify-api-key.md) | API Product, Developer App e Consumer Key |
| E3 | [22 — OAuth 2.0 e Scopes](./22-e3-oauth-scopes.md) | Client Credentials, autorização por scopes e escrita via API |
| E4+E5 | [23 — Quota Dinâmica e Spike Arrest](./23-e4-e5-quota-spike-arrest.md) | Controle de consumo e proteção contra rajadas |
| E6+E7 | [24 — MES Order Status](./24-e6-e7-mes-order-status-backend.md) | Backend, Products, Apps, Assign Message, mascaramento condicional e JSON → XML |
| E8+E9 | [Docs 21 a 24](./21-e2-verify-api-key.md) | Products, Rate Plans, Apps e Developer Hub praticados de forma integrada |
| E10 | [25 — API Analytics](./25-e10-api-analytics.md) | Overview, Health, Usage e Custom View operacional |

> ✅ Bloco E concluído em 16/08/2026.

### Ⓕ Bloco F — Segurança

| Cenário | Doc | Descrição |
|---|---|---|
| F4 | [26 — B2B Client Certificate e mTLS](./26-f4-b2b-client-certificate-mtls.md) | Certificate Service Key, PFX e testes sem certificado, válido e não confiável |
| F5 | [27 — CSRF Token Validation](./27-f5-csrf-token-validation.md) | Alteração de pedido SAP MM com token e cookies vinculados à sessão |
| F6 | [28 — API Threat Protection](./28-f6-api-threat-protection.md) | JSON, XML e Regular Expression Protection, próximo cenário |

### 📍 Status atual

- Blocos A, B, C, D e E concluídos.
- F1, F2 e F3 cobertos nos cenários anteriores.
- F4 e F5 concluídos em 16/08/2026.
- Próximo cenário: F6 — API Threat Protection.

📌 Voltar para o [README principal do projeto](../README.md)
