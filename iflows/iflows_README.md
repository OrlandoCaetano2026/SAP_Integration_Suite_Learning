
### 📦 Integration Flows (iFlows)
  
Esta pasta contém os **Integration Flows exportados** do SAP Integration Suite, no formato .zip. Cada arquivo é o artefato real do iFlow, podendo ser **importado e executado** em um tenant do Integration Suite.

#### 📥 Como exportar um iFlow (para adicionar aqui)
  
SAP Integration Suite → Design → Integrations and APIs
→ abrir o pacote → selecionar o Integration Flow
→ botão "Download" (Merged Configured and Default Values) → gera o arquivo .zip

#### 📤 Como importar um iFlow (para reutilizar)
  
SAP Integration Suite → Design → pacote de destino
→ Add → Integration Flow → Upload → selecionar o .zip

#### 📋 iFlows do projeto

##### Ⓐ Bloco A — CPI Fundamentos

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Doc</th>
</tr>
<tr>
<td>A1\_HTTP\_To\_Webhook.zip</td>
<td>HTTP → Content Modifier → Webhook</td>
<td>[doc](../docs/03-a1-http-to-webhook.md)</td>
</tr>
<tr>
<td>A2\_Timer\_To\_API.zip</td>
<td>Timer → Request Reply → API pública</td>
<td>[doc](../docs/04-a2-timer-to-api.md)</td>
</tr>
<tr>
<td>A3\_Message\_Mapping.zip</td>
<td>Message Mapping (JSON → XML)</td>
<td>[doc](../docs/05-a3-message-mapping.md)</td>
</tr>
<tr>
<td>A4\_Groovy\_Script.zip</td>
<td>Groovy Script (enriquecimento)</td>
<td>[doc](../docs/06-a4-groovy-script.md)</td>
</tr>
</table>


##### Ⓑ Bloco B — Padrões de Integração

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Doc</th>
</tr>
<tr>
<td>B1\_Content\_Based\_Router.zip</td>
<td>Content-Based Router</td>
<td>[doc](../docs/07-b1-content-based-router.md)</td>
</tr>
<tr>
<td>B2\_Content\_Enricher.zip</td>
<td>Content Enricher (OData V4)</td>
<td>[doc](../docs/08-b2-content-enricher.md)</td>
</tr>
<tr>
<td>B3\_Splitter.zip</td>
<td>Splitter</td>
<td>[doc](../docs/09-b3-splitter.md)</td>
</tr>
<tr>
<td>B4\_Aggregator.zip</td>
<td>Aggregator (CamelSplitComplete)</td>
<td>[doc](../docs/10-b4-aggregator.md)</td>
</tr>
<tr>
<td>B5\_Multicast.zip</td>
<td>Multicast (MES/PLM/ERP)</td>
<td>[doc](../docs/11-b5-multicast.md)</td>
</tr>
</table>


##### Ⓒ Bloco C — Resiliência e Erros

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Doc</th>
</tr>
<tr>
<td>C1\_Exception\_Subprocess.zip</td>
<td>Exception Subprocess</td>
<td>[doc](../docs/12-c1-exception-subprocess.md)</td>
</tr>
<tr>
<td>C2\_Retry.zip</td>
<td>Retry (HTTP Adapter)</td>
<td>[doc](../docs/13-c2-retry-timeout.md)</td>
</tr>
<tr>
<td>C3\_Producer.zip</td>
<td>Dead Letter — Producer (grava na fila JMS)</td>
<td>[doc](../docs/14-c3-dead-letter.md)</td>
</tr>
<tr>
<td>C3\_Consumer.zip</td>
<td>Dead Letter — Consumer (consome + retry + dead letter)</td>
<td>[doc](../docs/14-c3-dead-letter.md)</td>
</tr>
<tr>
<td>C4\_Data\_Store.zip</td>
<td>Deduplicação — Caminho A (Data Store manual)</td>
<td>[doc](../docs/15-c4-data-store.md)</td>
</tr>
<tr>
<td>C4B\_Idempotent\_Process\_Call.zip</td>
<td>Deduplicação — Caminho B (Idempotent Process Call)</td>
<td>[doc](../docs/15-c4-data-store.md)</td>
</tr>
</table>


##### Ⓓ Bloco D — Conectividade / Adapters

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Doc</th>
</tr>
<tr>
<td>D1\_OData\_Adapter.zip</td>
<td>OData Adapter (query dinâmica)</td>
<td>[doc](../docs/16-d1-odata-adapter.md)</td>
</tr>
<tr>
<td>D2\_SOAP\_Adapter.zip</td>
<td>SOAP Adapter (Split/Gather + Web Service externo)</td>
<td>[doc](../docs/17-d2-soap-adapter.md)</td>
</tr>
<tr>
<td>D3\_SFTP\_Producer.zip</td>
<td>SFTP Producer (grava arquivo no hot folder)</td>
<td>[doc](../docs/18-d3-sftp-adapter.md)</td>
</tr>
<tr>
<td>D3\_SFTP\_Consumer.zip</td>
<td>SFTP Consumer (polling + processamento)</td>
<td>[doc](../docs/18-d3-sftp-adapter.md)</td>
</tr>
<tr>
<td>D4\_ProcessDirect\_Main.zip</td>
<td>ProcessDirect Main (HTTP + Router + JDBC)</td>
<td>[doc](../docs/19-d4-processdirect.md)</td>
</tr>
<tr>
<td>D4\_ProcessDirect\_VendorValidation.zip</td>
<td>ProcessDirect Utility (consulta JDBC reutilizável)</td>
<td>[doc](../docs/19-d4-processdirect.md)</td>
</tr>
</table>


##### Ⓔ Bloco E — API Management

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Doc</th>
</tr>
<tr>
<td>E3\_VendorOverride.zip</td>
<td>Escrita via API usada nos testes OAuth</td>
<td>[doc](../docs/22-e3-oauth-scopes.md)</td>
</tr>
<tr>
<td>E6\_E7\_MES\_OrderStatus\_ProcessDirect.zip</td>
<td>Backend MES Order Status</td>
<td>[doc](../docs/24-e6-e7-mes-order-status-backend.md)</td>
</tr>
</table>


##### Ⓕ Bloco F — Segurança

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Doc</th>
</tr>
<tr>
<td>F4\_B2B\_ClientCertificate\_Receiver.zip</td>
<td>Client Certificate Authentication e mTLS</td>
<td>[doc](../docs/26-f4-b2b-client-certificate-mtls.md)</td>
</tr>
<tr>
<td>F5\_MM\_PurchaseOrder\_CSRF\_Protected\_API.zip</td>
<td>CSRF real em alteração de pedido SAP MM</td>
<td>[doc](../docs/27-f5-csrf-token-validation.md)</td>
</tr>
<tr>
<td>F6\_MM\_SupplierConfirmation\_Backend.zip</td>
<td>API Threat Protection</td>
<td>[doc](../docs/28-f6-api-threat-protection.md)</td>
</tr>
<tr>
<td>F7\_MM\_PGP\_SupplierConfirmation\_Producer.zip</td>
<td>PGP — Producer (criptografia + assinatura)</td>
<td>[doc](../docs/29-f7-pgp-message-level-security.md)</td>
</tr>
<tr>
<td>F7\_MM\_PGP\_SupplierConfirmation\_Consumer.zip</td>
<td>PGP — Consumer (descriptografia + verificação de assinatura)</td>
<td>[doc](../docs/29-f7-pgp-message-level-security.md)</td>
</tr>
<tr>
<td>F7\_MM\_PGP\_Unauthorized\_Signer\_Test\_Producer.zip</td>
<td>PGP — teste negativo (assinante não autorizado)</td>
<td>[doc](../docs/29-f7-pgp-message-level-security.md)</td>
</tr>
<tr>
<td>F7\_MM\_PGP\_Unsigned\_Message\_Test\_Producer.zip</td>
<td>PGP — teste negativo (mensagem sem assinatura)</td>
<td>[doc](../docs/29-f7-pgp-message-level-security.md)</td>
</tr>
<tr>
<td>F8A\_MM\_Authenticated\_Principal\_Capture.zip</td>
<td>Authentication Context Capture e anti-spoofing</td>
<td>[doc](../docs/30-f8-authentication-context-technical-user-saml-bearer.md)</td>
</tr>
<tr>
<td>F8B\_MM\_SAML\_Bearer\_Technical\_User\_Token.zip</td>
<td>Technical User SAML Bearer, introspection e autorização</td>
<td>[doc](../docs/30-f8-authentication-context-technical-user-saml-bearer.md)</td>
</tr>
<tr>
<td>F8C\_MM\_EndUser\_Principal\_Propagation.zip</td>
<td>Exploração de End-User Principal Propagation (IAS/XSUAA/Approuter)</td>
<td>[doc](../docs/30-f8-authentication-context-technical-user-saml-bearer.md)</td>
</tr>
<tr>
<td>F8E\_MM\_SAML\_Bearer\_End\_User\_Authorization.zip</td>
<td>Autorização por grupos com usuários reais no WSO2</td>
<td>[doc](../docs/31-f8e-end-user-saml-bearer-group-based-authorization.md)</td>
</tr>
</table>


##### Ⓗ Bloco H — Event-Driven Integration (Event Mesh)

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Doc</th>
</tr>
<tr>
<td>H2 SAP Cloud Integration to Solace AMQP Publisher.zip</td>
<td>CPI como publisher AMQP 1.0 para Solace PubSub+</td>
<td>[doc](../docs/33-h2-sap-cloud-integration-publisher-solace-amqp.md)</td>
</tr>
<tr>
<td>H3 SAP PP MES Production Confirmation AMQP Subscriber.zip</td>
<td>CPI como consumer AMQP de confirmações de produção (SAP PP/MES)</td>
<td>[doc](../docs/34-h3-sap-cloud-integration-subscriber-solace-amqp.md)</td>
</tr>
<tr>
<td>H4 WM Picking Worker A.zip</td>
<td>Competing Consumer A (SAP WM → Webhook.site)</td>
<td>[doc](../docs/35-h4-solace-competing-consumers-scaling.md)</td>
</tr>
<tr>
<td>H4 WM Picking Worker B.zip</td>
<td>Competing Consumer B (SAP WM → RequestBin)</td>
<td>[doc](../docs/35-h4-solace-competing-consumers-scaling.md)</td>
</tr>
<tr>
<td>H4 WM Picking Worker C.zip</td>
<td>Competing Consumer C (SAP WM → Beeceptor)</td>
<td>[doc](../docs/35-h4-solace-competing-consumers-scaling.md)</td>
</tr>
</table>

  
💡 O H1 (Event Mesh Foundation) é executado diretamente no broker Solace PubSub+ (topic, durable queue e Try-Me), sem um iFlow exportável próprio.  
💡 Policies e API Proxies do API Management não são exportados como iFlow .zip.  
💡 Mantenha o nome do .zip igual ao ID técnico do artefato no Integration Suite.  
📌 Voltar para o [README principal do projeto](../README.md)
