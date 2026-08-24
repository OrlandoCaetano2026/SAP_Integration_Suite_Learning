
### 📚 Documentação — SAP Integration Suite Learning
  
Índice da documentação técnica do projeto, organizado por bloco de cenários. Cada documento descreve objetivo, arquitetura, passo a passo, troubleshooting e evidências do laboratório correspondente.

#### 🧩 Base conceitual

<table>
<tr>
<th>  
Doc
</th>
<th>  
Conteúdo
</th>
</tr>
<tr>
<td>  
[01 — Ambiente SAP BTP](./01-ambiente-btp.md)
</td>
<td>  
Estrutura do ambiente, capabilities e autenticação
</td>
</tr>
<tr>
<td>  
[02 — Cloud Integration (CPI) Básico](./02-cloud-integration-basics.md)
</td>
<td>  
Conceitos fundamentais de iFlows, adapters e monitoramento
</td>
</tr>
</table>


#### Ⓐ Bloco A — CPI Fundamentos

<table>
<tr>
<th>  
Cenário
</th>
<th>  
Doc
</th>
<th>  
Descrição
</th>
</tr>
<tr>
<td>  
A1
</td>
<td>  
[03 — HTTP → Webhook](./03-a1-http-to-webhook.md)
</td>
<td>  
Receber, ajustar e encaminhar mensagem
</td>
</tr>
<tr>
<td>  
A2
</td>
<td>  
[04 — Timer → API pública](./04-a2-timer-to-api.md)
</td>
<td>  
Consumir API externa via Request Reply
</td>
</tr>
<tr>
<td>  
A3
</td>
<td>  
[05 — Message Mapping (JSON → XML)](./05-a3-message-mapping.md)
</td>
<td>  
Transformação de estrutura e formato
</td>
</tr>
<tr>
<td>  
A4
</td>
<td>  
[06 — Groovy Script](./06-a4-groovy-script.md)
</td>
<td>  
Lógica de negócio e enriquecimento
</td>
</tr>
</table>


#### Ⓑ Bloco B — Padrões de Integração

<table>
<tr>
<th>  
Cenário
</th>
<th>  
Doc
</th>
<th>  
Descrição
</th>
</tr>
<tr>
<td>  
B1
</td>
<td>  
[07 — Content-Based Router](./07-b1-content-based-router.md)
</td>
<td>  
Roteamento por conteúdo (XPath)
</td>
</tr>
<tr>
<td>  
B2
</td>
<td>  
[08 — Content Enricher](./08-b2-content-enricher.md)
</td>
<td>  
Enriquecimento via lookup OData V4
</td>
</tr>
<tr>
<td>  
B3
</td>
<td>  
[09 — Splitter](./09-b3-splitter.md)
</td>
<td>  
Divisão de lote em itens individuais
</td>
</tr>
<tr>
<td>  
B4
</td>
<td>  
[10 — Aggregator](./10-b4-aggregator.md)
</td>
<td>  
Consolidação de mensagens (CamelSplitComplete)
</td>
</tr>
<tr>
<td>  
B5
</td>
<td>  
[11 — Multicast](./11-b5-multicast.md)
</td>
<td>  
Distribuição simultânea (MES/PLM/ERP)
</td>
</tr>
</table>


#### Ⓒ Bloco C — Resiliência e Erros

<table>
<tr>
<th>  
Cenário
</th>
<th>  
Doc
</th>
<th>  
Descrição
</th>
</tr>
<tr>
<td>  
C1
</td>
<td>  
[12 — Exception Subprocess](./12-c1-exception-subprocess.md)
</td>
<td>  
Tratamento de erros (try/catch visual)
</td>
</tr>
<tr>
<td>  
C2
</td>
<td>  
[13 — Retry](./13-c2-retry-timeout.md)
</td>
<td>  
Reenvio automático em falhas temporárias
</td>
</tr>
<tr>
<td>  
C3
</td>
<td>  
[14 — Dead Letter (JMS)](./14-c3-dead-letter.md)
</td>
<td>  
Guaranteed delivery, retry assíncrono, dead letter
</td>
</tr>
<tr>
<td>  
C4
</td>
<td>  
[15 — Data Store & Idempotência](./15-c4-data-store.md)
</td>
<td>  
Deduplicação (Data Store + Idempotent Process Call)
</td>
</tr>
</table>


#### Ⓓ Bloco D — Conectividade / Adapters

<table>
<tr>
<th>  
Cenário
</th>
<th>  
Doc
</th>
<th>  
Descrição
</th>
</tr>
<tr>
<td>  
D1
</td>
<td>  
[16 — OData Adapter](./16-d1-odata-adapter.md)
</td>
<td>  
Query dinâmica ao OData V4 (Northwind)
</td>
</tr>
<tr>
<td>  
D2
</td>
<td>  
[17 — SOAP Adapter](./17-d2-soap-adapter.md)
</td>
<td>  
Integração com Web Service SOAP externo (Split/Gather)
</td>
</tr>
<tr>
<td>  
D3
</td>
<td>  
[18 — SFTP Adapter](./18-d3-sftp-adapter.md)
</td>
<td>  
Integração de arquivos via hot folder (Producer/Consumer)
</td>
</tr>
<tr>
<td>  
D4
</td>
<td>  
[19 — ProcessDirect + JDBC](./19-d4-processdirect.md)
</td>
<td>  
Comunicação interna entre iFlows + validação de fornecedor via banco de dados
</td>
</tr>
</table>

  
💡 O cenário **D5 — JDBC Adapter**, originalmente planejado separadamente, foi incorporado ao **D4**, que já cobre ProcessDirect + JDBC de forma integrada e realista.

#### Ⓔ Bloco E — API Management

<table>
<tr>
<th>  
Cenário
</th>
<th>  
Doc
</th>
<th>  
Descrição
</th>
</tr>
<tr>
<td>  
E0, E1, E12
</td>
<td>  
[20 — API Proxy + Basic Authentication](./20-e-api-management-proxy-basic-auth.md)
</td>
<td>  
API Provider, Proxy e KVM com Basic Authentication
</td>
</tr>
<tr>
<td>  
E2
</td>
<td>  
[21 — Verify API Key](./21-e2-verify-api-key.md)
</td>
<td>  
API Product, Developer App e Consumer Key
</td>
</tr>
<tr>
<td>  
E3
</td>
<td>  
[22 — OAuth 2.0 e Scopes](./22-e3-oauth-scopes.md)
</td>
<td>  
Client Credentials, autorização por scopes e escrita via API
</td>
</tr>
<tr>
<td>  
E4+E5
</td>
<td>  
[23 — Quota Dinâmica e Spike Arrest](./23-e4-e5-quota-spike-arrest.md)
</td>
<td>  
Controle de consumo e proteção contra rajadas
</td>
</tr>
<tr>
<td>  
E6+E7
</td>
<td>  
[24 — MES Order Status](./24-e6-e7-mes-order-status-backend.md)
</td>
<td>  
Backend, Products, Apps, Assign Message, mascaramento condicional e JSON → XML
</td>
</tr>
<tr>
<td>  
E8+E9
</td>
<td>  
[Docs 21 a 24](./21-e2-verify-api-key.md)
</td>
<td>  
Products, Rate Plans, Apps e Developer Hub praticados de forma integrada
</td>
</tr>
<tr>
<td>  
E10
</td>
<td>  
[25 — API Analytics](./25-e10-api-analytics.md)
</td>
<td>  
Overview, Health, Usage e Custom View operacional
</td>
</tr>
</table>

  
✅ Bloco E concluído em 16/08/2026.

#### Ⓕ Bloco F — Segurança

<table>
<tr>
<th>  
Cenário
</th>
<th>  
Doc
</th>
<th>  
Descrição
</th>
<th>  
Status
</th>
</tr>
<tr>
<td>  
F4
</td>
<td>  
[26 — B2B Client Certificate e mTLS](./26-f4-b2b-client-certificate-mtls.md)
</td>
<td>  
Certificate Service Key, X.509, autenticação de cliente e mTLS B2B
</td>
<td>  
✅ Concluído
</td>
</tr>
<tr>
<td>  
F5
</td>
<td>  
[27 — CSRF Token Validation](./27-f5-csrf-token-validation.md)
</td>
<td>  
Token CSRF e cookies vinculados à sessão em alteração SAP MM
</td>
<td>  
✅ Concluído
</td>
</tr>
<tr>
<td>  
F6
</td>
<td>  
[28 — API Threat Protection](./28-f6-api-threat-protection.md)
</td>
<td>  
JSON, XML e Regular Expression Threat Protection
</td>
<td>  
✅ Concluído
</td>
</tr>
<tr>
<td>  
F7
</td>
<td>  
[29 — PGP Message-Level Security](./29-f7-pgp-message-level-security.md)
</td>
<td>  
Criptografia, assinatura, verificação e testes negativos PGP
</td>
<td>  
✅ Concluído
</td>
</tr>
<tr>
<td>  
F8A–F8B
</td>
<td>  
[30 — Authentication Context and Technical User SAML Bearer](./30-f8-authentication-context-technical-user-saml-bearer.md)
</td>
<td>  
Contexto inbound, anti-spoofing, RFC 7522, introspecção e autorização READ/APPROVE; inclui a exploração arquitetural do F8C (login humano via IAS/XSUAA/Approuter)
</td>
<td>  
✅ Concluído
</td>
</tr>
<tr>
<td>  
F8E
</td>
<td>  
[31 — End-User SAML Bearer Group-Based Authorization](./31-f8e-end-user-saml-bearer-group-based-authorization.md)
</td>
<td>  
Autorização por grupos com usuários reais (Buyer/Manager) no WSO2
</td>
<td>  
✅ Concluído
</td>
</tr>
</table>


#### Ⓗ Bloco H — Event-Driven Integration (Event Mesh)

<table>
<tr>
<th>  
Cenário
</th>
<th>  
Doc
</th>
<th>  
Descrição
</th>
<th>  
Status
</th>
</tr>
<tr>
<td>  
H1
</td>
<td>  
[32 — Solace PubSub+ Event Mesh Foundation](./32-h1-solace-pubsub-event-mesh-foundation.md)
</td>
<td>  
Event broker, topic, durable queue, Direct e Guaranteed Messaging (SAP MM)
</td>
<td>  
✅ Concluído
</td>
</tr>
<tr>
<td>  
H2
</td>
<td>  
[33 — SAP Cloud Integration Publisher to Solace via AMQP](./33-h2-sap-cloud-integration-publisher-solace-amqp.md)
</td>
<td>  
CPI como publisher AMQP 1.0 (TLS/SASL), event envelope e correlação ponta a ponta
</td>
<td>  
✅ Concluído
</td>
</tr>
<tr>
<td>  
H3
</td>
<td>  
[34 — SAP Cloud Integration Subscriber from Solace via AMQP](./34-h3-sap-cloud-integration-subscriber-solace-amqp.md)
</td>
<td>  
CPI como consumer AMQP, backlog SAP PP/MES e entrega a backend externo
</td>
<td>  
✅ Concluído
</td>
</tr>
<tr>
<td>  
H4
</td>
<td>  
[35 — Competing Consumers e Escala Horizontal](./35-h4-solace-competing-consumers-scaling.md)
</td>
<td>  
Non-Exclusive Queue, três workers SAP WM, três backends, falha e recuperação
</td>
<td>  
✅ Concluído
</td>
</tr>
</table>


#### 📍 Status atual
- Blocos A, B, C, D e E concluídos.
- F1, F2 e F3 cobertos nos cenários de API Management.
- F4, F5, F6 e F7 concluídos.
- F8A/F8B concluídos no Documento 30 (com a exploração arquitetural do F8C); F8E concluído no Documento 31.
- Bloco H (Event-Driven / Event Mesh) em andamento: H1 a H4 concluídos nos Documentos 32 a 35.
- O Bloco F foi dividido em documentos modulares para preservar profundidade técnica e navegação.
📌 Voltar para o [README principal do projeto](../README.md)
