## 📸 Evidências — SAP Integration Suite Learning

Índice das evidências de execução (prints de monitoramento, payloads, Postman, destinos e vídeos), organizado por bloco. Cada pasta labXX corresponde a um cenário documentado em [docs/](../docs/).

### Ⓐ Bloco A — CPI Fundamentos

| Lab | Cenário | Evidências | Doc |
|---|---|---|---|
| lab01 | A1 — HTTP → Webhook | [pasta](./lab01) | [doc](../docs/03-a1-http-to-webhook.md) |
| lab02 | A2 — Timer → API pública | [pasta](./lab02) | [doc](../docs/04-a2-timer-to-api.md) |
| lab03 | A3 — Message Mapping | [pasta](./lab03) | [doc](../docs/05-a3-message-mapping.md) |
| lab04 | A4 — Groovy Script | [pasta](./lab04) | [doc](../docs/06-a4-groovy-script.md) |

### Ⓑ Bloco B — Padrões de Integração

| Lab | Cenário | Evidências | Doc |
|---|---|---|---|
| lab05 | B1 — Content-Based Router | [pasta](./lab05) | [doc](../docs/07-b1-content-based-router.md) |
| lab06 | B2 — Content Enricher | [pasta](./lab06) | [doc](../docs/08-b2-content-enricher.md) |
| lab07 | B3 — Splitter | [pasta](./lab07) | [doc](../docs/09-b3-splitter.md) |
| lab08 | B4 — Aggregator | [pasta](./lab08) | [doc](../docs/10-b4-aggregator.md) |
| lab09 | B5 — Multicast | [pasta](./lab09) | [doc](../docs/11-b5-multicast.md) |

### Ⓒ Bloco C — Resiliência e Erros

| Lab | Cenário | Evidências | Doc |
|---|---|---|---|
| lab10 | C1 — Exception Subprocess | [pasta](./lab10) | [doc](../docs/12-c1-exception-subprocess.md) |
| lab11 | C2 — Retry | [pasta](./lab11) | [doc](../docs/13-c2-retry-timeout.md) |
| lab12 | C3 — Dead Letter (JMS) | [pasta](./lab12) | [doc](../docs/14-c3-dead-letter.md) |
| lab13 | C4 — Data Store & Idempotência | [pasta](./lab13) | [doc](../docs/15-c4-data-store.md) |

### Ⓓ Bloco D — Conectividade / Adapters

| Lab | Cenário | Evidências | Doc |
|---|---|---|---|
| lab14 | D1 — OData Adapter | [pasta](./lab14) | [doc](../docs/16-d1-odata-adapter.md) |
| lab15 | D2 — SOAP Adapter | [pasta](./lab15) | [doc](../docs/17-d2-soap-adapter.md) |
| lab16 | D3 — SFTP Adapter (Producer + Consumer) | [pasta](./lab16) | [doc](../docs/18-d3-sftp-adapter.md) |
| lab17 | D4 — ProcessDirect + JDBC | [pasta](./lab17) | [doc](../docs/19-d4-processdirect.md) |

### Ⓔ Bloco E — API Management

| Lab | Cenário | Evidências | Doc |
|---|---|---|---|
| lab18 | E0, E1, E12 — API Proxy + Basic Authentication (KVM) | [pasta](./lab18) | [doc](../docs/20-e-api-management-proxy-basic-auth.md) |
| lab19 | E2 — Verify API Key | [pasta](./lab19) | [doc](../docs/21-e2-verify-api-key.md) |
| lab20 | E3 — OAuth 2.0 e Scopes (Fornecedor vs. Compliance) | [pasta](./lab20) | [doc](../docs/22-e3-oauth-scopes.md) |
| lab21 | E4, E5 — Quota Dinâmica e Spike Arrest | [pasta](./lab21) | [doc](../docs/23-e4-e5-quota-spike-arrest.md) |

💡 **Convenção de nomes:** os arquivos seguem a ordem narrativa do cenário (ex.: 01-iflow, 02-postman, 03-...), com dados sensíveis (URLs de tenant, credenciais, headers de rastreamento) borrados antes do commit.

📌 Voltar para o [README principal do projeto](../README.md)
