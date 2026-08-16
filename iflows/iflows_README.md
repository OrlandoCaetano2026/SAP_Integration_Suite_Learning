## 📦 Integration Flows (iFlows)

Esta pasta contém os **Integration Flows exportados** do SAP Integration Suite, no formato .zip. Cada arquivo é o artefato real do iFlow, podendo ser **importado e executado** em um tenant do Integration Suite.

### 📥 Como exportar um iFlow (para adicionar aqui)

SAP Integration Suite → Design → Integrations and APIs
→ abrir o pacote → selecionar o Integration Flow
→ botão "Download" (Merged Configured and Default Values) → gera o arquivo .zip

### 📤 Como importar um iFlow (para reutilizar)

SAP Integration Suite → Design → pacote de destino
→ Add → Integration Flow → Upload → selecionar o .zip

### 📋 iFlows do projeto

#### Ⓐ Bloco A — CPI Fundamentos

| Arquivo | Cenário | Doc |
|---|---|---|
| A1_HTTP_To_Webhook.zip | HTTP → Content Modifier → Webhook | [doc](../docs/03-a1-http-to-webhook.md) |
| A2_Timer_To_API.zip | Timer → Request Reply → API pública | [doc](../docs/04-a2-timer-to-api.md) |
| A3_Message_Mapping.zip | Message Mapping (JSON → XML) | [doc](../docs/05-a3-message-mapping.md) |
| A4_Groovy_Script.zip | Groovy Script (enriquecimento) | [doc](../docs/06-a4-groovy-script.md) |

#### Ⓑ Bloco B — Padrões de Integração

| Arquivo | Cenário | Doc |
|---|---|---|
| B1_Content_Based_Router.zip | Content-Based Router | [doc](../docs/07-b1-content-based-router.md) |
| B2_Content_Enricher.zip | Content Enricher (OData V4) | [doc](../docs/08-b2-content-enricher.md) |
| B3_Splitter.zip | Splitter | [doc](../docs/09-b3-splitter.md) |
| B4_Aggregator.zip | Aggregator (CamelSplitComplete) | [doc](../docs/10-b4-aggregator.md) |
| B5_Multicast.zip | Multicast (MES/PLM/ERP) | [doc](../docs/11-b5-multicast.md) |

#### Ⓒ Bloco C — Resiliência e Erros

| Arquivo | Cenário | Doc |
|---|---|---|
| C1_Exception_Subprocess.zip | Exception Subprocess | [doc](../docs/12-c1-exception-subprocess.md) |
| C2_Retry.zip | Retry (HTTP Adapter) | [doc](../docs/13-c2-retry-timeout.md) |
| C3_Producer.zip | Dead Letter — Producer (grava na fila JMS) | [doc](../docs/14-c3-dead-letter.md) |
| C3_Consumer.zip | Dead Letter — Consumer (consome + retry + dead letter) | [doc](../docs/14-c3-dead-letter.md) |
| C4_Data_Store.zip | Deduplicação — Caminho A (Data Store manual) | [doc](../docs/15-c4-data-store.md) |
| C4B_Idempotent_Process_Call.zip | Deduplicação — Caminho B (Idempotent Process Call) | [doc](../docs/15-c4-data-store.md) |

#### Ⓓ Bloco D — Conectividade / Adapters

| Arquivo | Cenário | Doc |
|---|---|---|
| D1_OData_Adapter.zip | OData Adapter (query dinâmica) | [doc](../docs/16-d1-odata-adapter.md) |
| D2_SOAP_Adapter.zip | SOAP Adapter (Split/Gather + Web Service externo) | [doc](../docs/17-d2-soap-adapter.md) |
| D3_SFTP_Producer.zip | SFTP Producer (grava arquivo no hot folder) | [doc](../docs/18-d3-sftp-adapter.md) |
| D3_SFTP_Consumer.zip | SFTP Consumer (polling + processamento) | [doc](../docs/18-d3-sftp-adapter.md) |
| D4_ProcessDirect_Main.zip | ProcessDirect Main (HTTP + Router + JDBC) | [doc](../docs/19-d4-processdirect.md) |
| D4_ProcessDirect_VendorValidation.zip | ProcessDirect Utility (consulta JDBC reutilizável) | [doc](../docs/19-d4-processdirect.md) |

#### Ⓔ Bloco E — API Management

| Arquivo | Cenário | Doc |
|---|---|---|
| E3_VendorOverride.zip | Escrita via API usada nos testes OAuth | [doc](../docs/22-e3-oauth-scopes.md) |
| E6_E7_MES_OrderStatus_ProcessDirect.zip | Backend MES Order Status | [doc](../docs/24-e6-e7-mes-order-status-backend.md) |

#### Ⓕ Bloco F — Segurança

| Arquivo | Cenário | Doc |
|---|---|---|
| F4_B2B_ClientCertificate_Receiver.zip | Client Certificate Authentication e mTLS | [doc](../docs/26-f4-b2b-client-certificate-mtls.md) |
| F5_MM_PurchaseOrder_CSRF_Protected_API.zip | CSRF real em alteração de pedido SAP MM | [doc](../docs/27-f5-csrf-token-validation.md) |
| F6_MM_SupplierConfirmation_Backend.zip | API Threat Protection | [doc planejado](../docs/28-f6-api-threat-protection.md) |

💡 Policies e API Proxies do API Management não são exportados como iFlow `.zip`.

💡 Mantenha o nome do `.zip` igual ao ID técnico do artefato no Integration Suite.

📌 Voltar para o [README principal do projeto](../README.md)
