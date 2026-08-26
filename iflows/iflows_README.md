<div align="center">

# 📦 Catálogo de Integration Flows

### Artefatos executáveis — SAP Integration Suite Learning

![iFlows](https://img.shields.io/badge/iFlows-40%2B-0FAAFF?style=for-the-badge&logo=sap&logoColor=white)
![Formato](https://img.shields.io/badge/formato-.zip%20exportável-success?style=for-the-badge)
![Reutilizável](https://img.shields.io/badge/importável-em%20qualquer%20tenant-6C2EB9?style=for-the-badge)

</div>

---

> 📦 Esta pasta contém os **Integration Flows exportados** do SAP Integration Suite (`.zip`). Cada artefato é o iFlow real, **importável e executável** em um tenant do Integration Suite, permitindo reproduzir integralmente os cenários documentados.

**🧭 Navegação:** [🏠 Principal](../README.md) · [📚 Documentação](../docs/docsREADME.md) · [📸 Evidências](../evidences/evidencesREADME.md) · [📨 Payloads](../payloads/payloads_README.md)

---

## 🔧 Como exportar e importar

<table>
<tr><th>📥 Exportar (para adicionar aqui)</th><th>📤 Importar (para reutilizar)</th></tr>
<tr>
<td valign="top">
Design → Integrations and APIs → abrir o pacote → selecionar o iFlow → <b>Download</b> (Merged Configured and Default Values) → gera o <code>.zip</code>.
</td>
<td valign="top">
Design → pacote de destino → <b>Add → Integration Flow → Upload</b> → selecionar o <code>.zip</code>.
</td>
</tr>
</table>

---

## Ⓐ Bloco A — CPI Fundamentos

<table>
<tr><th>Artefato</th><th>Papel na arquitetura</th><th>Doc</th></tr>
<tr><td><code>A1_HTTP_To_Webhook.zip</code></td><td>HTTPS Sender → Content Modifier → Receiver externo</td><td><a href="../docs/03-a1-http-to-webhook.md">📄</a></td></tr>
<tr><td><code>A2_Timer_To_API.zip</code></td><td>Timer → Request Reply para API pública</td><td><a href="../docs/04-a2-timer-to-api.md">📄</a></td></tr>
<tr><td><code>A3_Message_Mapping.zip</code></td><td>Message Mapping JSON → XML</td><td><a href="../docs/05-a3-message-mapping.md">📄</a></td></tr>
<tr><td><code>A4_Groovy_Script.zip</code></td><td>Groovy Script de enriquecimento</td><td><a href="../docs/06-a4-groovy-script.md">📄</a></td></tr>
</table>

## Ⓑ Bloco B — Padrões de Integração

<table>
<tr><th>Artefato</th><th>Papel na arquitetura</th><th>Doc</th></tr>
<tr><td><code>B1_Content_Based_Router.zip</code></td><td>Router por conteúdo (XPath)</td><td><a href="../docs/07-b1-content-based-router.md">📄</a></td></tr>
<tr><td><code>B2_Content_Enricher.zip</code></td><td>Enricher via lookup OData V4</td><td><a href="../docs/08-b2-content-enricher.md">📄</a></td></tr>
<tr><td><code>B3_Splitter.zip</code></td><td>Splitter de lote</td><td><a href="../docs/09-b3-splitter.md">📄</a></td></tr>
<tr><td><code>B4_Aggregator.zip</code></td><td>Aggregator (CamelSplitComplete)</td><td><a href="../docs/10-b4-aggregator.md">📄</a></td></tr>
<tr><td><code>B5_Multicast.zip</code></td><td>Multicast (MES/PLM/ERP)</td><td><a href="../docs/11-b5-multicast.md">📄</a></td></tr>
</table>

## Ⓒ Bloco C — Resiliência e Erros

<table>
<tr><th>Artefato</th><th>Papel na arquitetura</th><th>Doc</th></tr>
<tr><td><code>C1_Exception_Subprocess.zip</code></td><td>Exception Subprocess (try/catch visual)</td><td><a href="../docs/12-c1-exception-subprocess.md">📄</a></td></tr>
<tr><td><code>C2_Retry.zip</code></td><td>Retry via HTTP Adapter</td><td><a href="../docs/13-c2-retry-timeout.md">📄</a></td></tr>
<tr><td><code>C3_Producer.zip</code></td><td>Dead Letter — Producer (grava na fila JMS)</td><td><a href="../docs/14-c3-dead-letter.md">📄</a></td></tr>
<tr><td><code>C3_Consumer.zip</code></td><td>Dead Letter — Consumer (consome + retry + dead letter)</td><td><a href="../docs/14-c3-dead-letter.md">📄</a></td></tr>
<tr><td><code>C4_Data_Store.zip</code></td><td>Deduplicação — Caminho A (Data Store manual)</td><td><a href="../docs/15-c4-data-store.md">📄</a></td></tr>
<tr><td><code>C4B_Idempotent_Process_Call.zip</code></td><td>Deduplicação — Caminho B (Idempotent Process Call)</td><td><a href="../docs/15-c4-data-store.md">📄</a></td></tr>
</table>

## Ⓓ Bloco D — Conectividade / Adapters

<table>
<tr><th>Artefato</th><th>Papel na arquitetura</th><th>Doc</th></tr>
<tr><td><code>D1_OData_Adapter.zip</code></td><td>OData Adapter (query dinâmica)</td><td><a href="../docs/16-d1-odata-adapter.md">📄</a></td></tr>
<tr><td><code>D2_SOAP_Adapter.zip</code></td><td>SOAP Adapter (Split/Gather)</td><td><a href="../docs/17-d2-soap-adapter.md">📄</a></td></tr>
<tr><td><code>D3_SFTP_Producer.zip</code></td><td>SFTP Producer (grava no hot folder)</td><td><a href="../docs/18-d3-sftp-adapter.md">📄</a></td></tr>
<tr><td><code>D3_SFTP_Consumer.zip</code></td><td>SFTP Consumer (polling + processamento)</td><td><a href="../docs/18-d3-sftp-adapter.md">📄</a></td></tr>
<tr><td><code>D4_ProcessDirect_Main.zip</code></td><td>ProcessDirect Main (HTTP + Router + JDBC)</td><td><a href="../docs/19-d4-processdirect.md">📄</a></td></tr>
<tr><td><code>D4_ProcessDirect_VendorValidation.zip</code></td><td>Utility JDBC reutilizável</td><td><a href="../docs/19-d4-processdirect.md">📄</a></td></tr>
</table>

## Ⓔ Bloco E — API Management

<table>
<tr><th>Artefato</th><th>Papel na arquitetura</th><th>Doc</th></tr>
<tr><td><code>E3_VendorOverride.zip</code></td><td>Escrita via API usada nos testes OAuth</td><td><a href="../docs/22-e3-oauth-scopes.md">📄</a></td></tr>
<tr><td><code>E6_E7_MES_OrderStatus_ProcessDirect.zip</code></td><td>Backend MES Order Status</td><td><a href="../docs/24-e6-e7-mes-order-status-backend.md">📄</a></td></tr>
</table>

> 💡 Policies e API Proxies do API Management não são exportados como iFlow `.zip`.

## Ⓕ Bloco F — Segurança

<table>
<tr><th>Artefato</th><th>Papel na arquitetura</th><th>Doc</th></tr>
<tr><td><code>F4_B2B_ClientCertificate_Receiver.zip</code></td><td>Client Certificate Authentication e mTLS</td><td><a href="../docs/26-f4-b2b-client-certificate-mtls.md">📄</a></td></tr>
<tr><td><code>F5_MM_PurchaseOrder_CSRF_Protected_API.zip</code></td><td>CSRF real em alteração de pedido SAP MM</td><td><a href="../docs/27-f5-csrf-token-validation.md">📄</a></td></tr>
<tr><td><code>F6_MM_SupplierConfirmation_Backend.zip</code></td><td>Backend para API Threat Protection</td><td><a href="../docs/28-f6-api-threat-protection.md">📄</a></td></tr>
<tr><td><code>F7_MM_PGP_SupplierConfirmation_Producer.zip</code></td><td>PGP — criptografia + assinatura</td><td><a href="../docs/29-f7-pgp-message-level-security.md">📄</a></td></tr>
<tr><td><code>F7_MM_PGP_SupplierConfirmation_Consumer.zip</code></td><td>PGP — descriptografia + verificação</td><td><a href="../docs/29-f7-pgp-message-level-security.md">📄</a></td></tr>
<tr><td><code>F7_MM_PGP_Unauthorized_Signer_Test_Producer.zip</code></td><td>PGP — teste negativo (assinante não autorizado)</td><td><a href="../docs/29-f7-pgp-message-level-security.md">📄</a></td></tr>
<tr><td><code>F7_MM_PGP_Unsigned_Message_Test_Producer.zip</code></td><td>PGP — teste negativo (mensagem sem assinatura)</td><td><a href="../docs/29-f7-pgp-message-level-security.md">📄</a></td></tr>
<tr><td><code>F8A_MM_Authenticated_Principal_Capture.zip</code></td><td>Authentication Context Capture e anti-spoofing</td><td><a href="../docs/30-f8-authentication-context-technical-user-saml-bearer.md">📄</a></td></tr>
<tr><td><code>F8B_MM_SAML_Bearer_Technical_User_Token.zip</code></td><td>Technical User SAML Bearer, introspection e autorização</td><td><a href="../docs/30-f8-authentication-context-technical-user-saml-bearer.md">📄</a></td></tr>
<tr><td><code>F8C_MM_EndUser_Principal_Propagation.zip</code></td><td>Exploração de End-User Principal Propagation (IAS/XSUAA/Approuter)</td><td><a href="../docs/30-f8-authentication-context-technical-user-saml-bearer.md">📄</a></td></tr>
<tr><td><code>F8E_MM_SAML_Bearer_End_User_Authorization.zip</code></td><td>Autorização por grupos com usuários reais no WSO2</td><td><a href="../docs/31-f8e-end-user-saml-bearer-group-based-authorization.md">📄</a></td></tr>
</table>

## Ⓗ Bloco H — Event-Driven Integration (Event Mesh)

<table>
<tr><th>Artefato</th><th>Papel na arquitetura</th><th>Doc</th></tr>
<tr><td><code>H2 SAP Cloud Integration to Solace AMQP Publisher.zip</code></td><td>CPI como publisher AMQP 1.0 (SAP MM)</td><td><a href="../docs/33-h2-sap-cloud-integration-publisher-solace-amqp.md">📄</a></td></tr>
<tr><td><code>H3 SAP PP MES Production Confirmation AMQP Subscriber.zip</code></td><td>CPI como consumer AMQP (SAP PP/MES)</td><td><a href="../docs/34-h3-sap-cloud-integration-subscriber-solace-amqp.md">📄</a></td></tr>
<tr><td><code>H4 WM Picking Worker A.zip</code></td><td>Competing Consumer A (SAP WM → Webhook.site)</td><td><a href="../docs/35-h4-solace-competing-consumers-scaling.md">📄</a></td></tr>
<tr><td><code>H4 WM Picking Worker B.zip</code></td><td>Competing Consumer B (SAP WM → RequestBin)</td><td><a href="../docs/35-h4-solace-competing-consumers-scaling.md">📄</a></td></tr>
<tr><td><code>H4 WM Picking Worker C.zip</code></td><td>Competing Consumer C (SAP WM → Beeceptor)</td><td><a href="../docs/35-h4-solace-competing-consumers-scaling.md">📄</a></td></tr>
<tr><td><code>H5 QM Critical Alert Consumer.zip</code></td><td>Consumer SAP QM com roteamento seletivo por topic hierarchy</td><td><a href="../docs/36-h5-solace-topic-hierarchy-wildcards-fanout.md">📄</a></td></tr>
<tr><td><code>H6 MM Supplier Confirmation Consumer.zip</code></td><td>Consumer SAP MM com retry, DMQ e recuperação operacional</td><td><a href="../docs/37-h6-solace-dead-letter-retry-replay.md">📄</a></td></tr>
<tr><td><code>H7 PM Industrial Telemetry Consumer.zip</code></td><td>Consumer SAP PM para telemetria MQTT, ordem de manutenção e alerta SMTP</td><td><a href="../docs/38-h7-solace-mqtt-industrial-telemetry.md">📄</a></td></tr>
</table>

> 💡 O **H1** (Event Mesh Foundation) é executado diretamente no broker Solace PubSub+ (topic, durable queue e Try-Me), sem um iFlow exportável próprio.

---

> 🧩 **Convenção:** o nome do `.zip` deve corresponder ao ID técnico do artefato no Integration Suite.

<div align="center">

📌 [Voltar ao README principal](../README.md)

</div>
