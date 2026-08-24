
### 🔗 SAP Integration Suite Learning
  
**🌐 Idioma / Language:** 🇧🇷 **Português**  [🇺🇸 English](README.en.md)  
[SAP](https://img.shields.io/badge/SAP-Integration%20Suite-0FAAFF?logo=sap&logoColor=white) [BTP](https://img.shields.io/badge/SAP-BTP-2570B8?logo=sap&logoColor=white) [Postman](https://img.shields.io/badge/Testes-Postman-FF6C37?logo=postman&logoColor=white) [Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)  
Projeto prático de estudo, desenvolvimento e preparação para a certificação **SAP Integration Suite**. O projeto acompanha a trilha oficial [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite) **e vai além dela**, incluindo cenários complementares muito valorizados no mercado.  
O objetivo é ir além da teoria: construir **cenários reais de integração** de ponta a ponta, documentar cada etapa e gerar evidências de execução, formando um portfólio técnico consistente.

#### 📑 Índice
- [O que é SAP Integration Suite](#-o-que-é-sap-integration-suite)
- [Principais capabilities](#-principais-capabilities)
- [Cloud Integration (CPI)](#-cloud-integration-cpi)
- [API e API Management](#-api-e-api-management)
- [Abordagem em duas camadas](#-abordagem-em-duas-camadas)
- [Objetivo do projeto](#-objetivo-do-projeto)
- [Padrões de mercado abordados](#-padrões-de-mercado-abordados)
- [Estrutura do repositório](#-estrutura-do-repositório)
- [Blocos e cenários de prática](#-blocos-e-cenários-de-prática)
- [Ferramentas utilizadas](#-ferramentas-utilizadas)
- [Fluxo de trabalho](#-fluxo-de-trabalho)
- [Referências oficiais SAP](#-referências-oficiais-sap)
- [Autor](#-autor--contato)

#### 🧩 O que é SAP Integration Suite
  
O **SAP Integration Suite** é a plataforma de integração como serviço (**iPaaS – Integration Platform as a Service**) da SAP, executada no **SAP Business Technology Platform (BTP)**. Ela conecta aplicações, processos, dados e eventos em ambientes **cloud, on-premise e híbridos**, permitindo que sistemas SAP e não-SAP se comuniquem de forma padronizada, segura e escalável.  
É a evolução do antigo **SAP Cloud Platform Integration (CPI)** e reúne, em um único ambiente, diversas capabilities de integração, além de recursos de inteligência (assistência por IA) e de aceleração por meio de conteúdo pré-construído.

#### 🛠️ Principais capabilities

<table>
<tr>
<th>Capability</th>
<th>Descrição</th>
</tr>
<tr>
<td>**Cloud Integration (CPI)**</td>
<td>Desenvolvimento de fluxos de integração (Integration Flows / iFlows) entre sistemas</td>
</tr>
<tr>
<td>**API Management**</td>
<td>Criação, publicação, segurança, governança e monitoramento de APIs</td>
</tr>
<tr>
<td>**Event Mesh / Advanced Event Mesh**</td>
<td>Integração orientada a eventos (event-driven)</td>
</tr>
<tr>
<td>**Open Connectors**</td>
<td>Conectores prontos para aplicações de terceiros (SaaS)</td>
</tr>
<tr>
<td>**Integration Advisor**</td>
<td>Aceleração de integrações B2B/EDI com apoio de IA</td>
</tr>
<tr>
<td>**Trading Partner Management**</td>
<td>Gestão de parceiros comerciais em cenários B2B</td>
</tr>
<tr>
<td>**Graph**</td>
<td>Modelo de dados unificado para acesso via API</td>
</tr>
</table>


#### 🔄 Cloud Integration (CPI)
  
O **Cloud Integration**, historicamente chamado de **CPI**, é o coração da plataforma. Nele são desenvolvidos os **Integration Flows (iFlows)** — fluxos que recebem, transformam, roteiam e entregam mensagens entre sistemas.  
Principais recursos praticados neste projeto:
- **Adapters** (HTTP, HTTPS, SOAP, OData, SFTP, JDBC, ProcessDirect, JMS, AMQP, etc.)
- **Content Modifier** (manipulação de headers, properties e body)
- **Message Mapping** (transformação JSON ↔ XML)
- **Groovy Script** (lógica customizada)
- **Router / Splitter / Aggregator / Gather / Multicast** (padrões de integração)
- **Exception Subprocess** (tratamento de erros)
- **Data Store** (persistência temporária)
- **Security Material** (User Credentials, SSH Known Hosts, JDBC Data Sources, chaves e certificados)
- **Monitoramento** (Message Processing, logs e payloads)

#### 🌐 API e API Management
  
Uma **API (Application Programming Interface)** é o contrato que permite que sistemas troquem dados de forma padronizada. Em integrações modernas, o modelo **API-First** é padrão — especialmente no **S/4HANA**, que expõe grande parte de suas funções via **APIs OData e REST**.  
O **API Management** é a capability responsável por **expor, proteger e governar** essas APIs. Com ele é possível:
- Criar **API Proxies** que abstraem o backend real
- Aplicar **Policies** de segurança e controle: Verify API Key, OAuth, Quota (limite de chamadas), Spike Arrest (proteção contra picos de tráfego), JSON ↔ XML, Assign Message (manipulação de request/response)
- Agrupar APIs em **API Products** e planos de consumo
- Publicar no **Developer Portal**
- Monitorar uso e performance via **Analytics**

#### 🧭 Abordagem em duas camadas
  
Este projeto é organizado em **duas camadas complementares**. A ideia é dominar o núcleo exigido na certificação e, ao mesmo tempo, ir além com cenários que fazem diferença no mercado real.

##### 🥇 Camada 1 — Trilha oficial SAP (núcleo da certificação)
  
Segue o repertório oficial da trilha [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite), com foco em:
- Cloud Integration (CPI) e Integration Flows
- API Management
- Mapeamentos e transformação de mensagens
- Monitoramento e operações

##### 🥈 Camada 2 — Cenários complementares (diferencial de mercado)
  
Vai **além da trilha oficial**, cobrindo temas muito valorizados em projetos reais que **não são aprofundados** no repertório oficial da certificação:
- **Event-Driven Integration** com Event Mesh / Advanced Event Mesh
- **B2B / EDI** (pedido de compra, nota fiscal, ASN)
- **OData / API-First** (padrão S/4HANA)
- **Integração híbrida** (cloud + on-premise via Cloud Connector)
- **Conectividade com bancos de dados externos** (JDBC) e **reuso interno de lógica** (ProcessDirect)  
⚠️ **Importante:** os cenários da Camada 2 são estudados a partir de conteúdos oficiais SAP **específicos de cada tema** (fora da trilha principal). Por exemplo, o Event Mesh tem sua própria jornada de aprendizagem: [Discovering Event-Driven Integration with SAP Integration Suite, advanced event mesh](https://learning.sap.com/courses/discovering-event-driven-integration-with-sap-integration-suite-advanved-event-mesh). Ou seja, o projeto vai propositalmente **além do escopo da prova**, agregando processos e práticas extras.

#### 🎯 Objetivo do projeto
- Dominar **Cloud Integration (CPI)** e **API Management** na prática
- Aplicar **padrões corporativos de integração** (EIP)
- Implementar **segurança, tratamento de erros e resiliência**
- Explorar **arquitetura orientada a eventos** (Event Mesh)
- Simular **processos SAP MM, PP, QM e WM** em cenários realistas
- Construir um **portfólio técnico documentado** com evidências
- Preparar para a **certificação oficial SAP Integration Suite**

#### 🌍 Padrões de mercado abordados
- **Event-Driven Integration** (SAP Event Mesh / Advanced Event Mesh) — arquitetura orientada a eventos, tendência forte no mercado.
- **API-First / OData** — padrão de integração do S/4HANA.
- **B2B / EDI** (pedido de compra, nota fiscal, ASN) — muito usado no setor industrial.
- **Integração híbrida** (cloud + on-premise via Cloud Connector) — conceito essencial e recorrente em provas e projetos.
- **Conectividade a banco de dados externo (JDBC)** e **reuso interno de lógica (ProcessDirect)** — padrões comuns em arquiteturas corporativas de médio/grande porte.

#### 📁 Estrutura do repositório

<table>
<tr>
<th>Pasta</th>
<th>Descrição</th>
</tr>
<tr>
<td>docs/</td>
<td>Documentação técnica de cada cenário (objetivo, arquitetura, passo a passo e aprendizados)</td>
</tr>
<tr>
<td>iflows/</td>
<td>Integration Flows exportados do Integration Suite (artefatos .zip)</td>
</tr>
<tr>
<td>payloads/</td>
<td>Mensagens de entrada e saída utilizadas nos testes (JSON/XML)</td>
</tr>
<tr>
<td>postman/</td>
<td>Coleções de testes do Postman para envio de mensagens</td>
</tr>
<tr>
<td>evidences/</td>
<td>Evidências de execução: prints do monitoramento, logs e payloads processados</td>
</tr>
<tr>
<td>certification/</td>
<td>Status de preparação e progresso rumo à certificação</td>
</tr>
</table>


#### 🧱 Blocos e cenários de prática
  
🥇 = Camada 1 (trilha oficial)  🥈 = Camada 2 (complementar / além da trilha)

##### Ⓐ Bloco A — CPI Fundamentos 🥇

<table>
<tr>
<th>\#</th>
<th>Cenário</th>
<th>Objetivo</th>
<th>Doc</th>
</tr>
<tr>
<td>A1</td>
<td>HTTP → Content Modifier → Webhook.site</td>
<td>Primeiro iFlow: receber, ajustar e encaminhar mensagem</td>
<td>[ver](docs/03-a1-http-to-webhook.md)</td>
</tr>
<tr>
<td>A2</td>
<td>Timer → Request Reply → API pública</td>
<td>Consumir API externa de forma agendada</td>
<td>[ver](docs/04-a2-timer-to-api.md)</td>
</tr>
<tr>
<td>A3</td>
<td>Message Mapping (JSON → JSON / JSON → XML)</td>
<td>Transformação de mensagens</td>
<td>[ver](docs/05-a3-message-mapping.md)</td>
</tr>
<tr>
<td>A4</td>
<td>Groovy Script para manipulação de payload</td>
<td>Lógica customizada no fluxo</td>
<td>[ver](docs/06-a4-groovy-script.md)</td>
</tr>
</table>


##### Ⓑ Bloco B — CPI Padrões de Integração 🥇

<table>
<tr>
<th>\#</th>
<th>Cenário</th>
<th>Objetivo</th>
<th>Doc</th>
</tr>
<tr>
<td>B1</td>
<td>Content-Based Router</td>
<td>Rotear mensagens por condição</td>
<td>[ver](docs/07-b1-content-based-router.md)</td>
</tr>
<tr>
<td>B2</td>
<td>Content Enricher (Request Reply)</td>
<td>Enriquecer dados a partir de outra fonte</td>
<td>[ver](docs/08-b2-content-enricher.md)</td>
</tr>
<tr>
<td>B3</td>
<td>Splitter</td>
<td>Quebrar lote de itens em mensagens individuais</td>
<td>[ver](docs/09-b3-splitter.md)</td>
</tr>
<tr>
<td>B4</td>
<td>Aggregator / Gather</td>
<td>Consolidar respostas</td>
<td>[ver](docs/10-b4-aggregator.md)</td>
</tr>
<tr>
<td>B5</td>
<td>Multicast</td>
<td>Enviar para múltiplos destinos</td>
<td>[ver](docs/11-b5-multicast.md)</td>
</tr>
</table>


##### Ⓒ Bloco C — CPI Resiliência e Erros 🥇

<table>
<tr>
<th>\#</th>
<th>Cenário</th>
<th>Objetivo</th>
<th>Doc</th>
</tr>
<tr>
<td>C1</td>
<td>Exception Subprocess</td>
<td>Tratamento padronizado de erros</td>
<td>[ver](docs/12-c1-exception-subprocess.md)</td>
</tr>
<tr>
<td>C2</td>
<td>Retry e tratamento de timeout</td>
<td>Resiliência em falhas temporárias</td>
<td>[ver](docs/13-c2-retry-timeout.md)</td>
</tr>
<tr>
<td>C3</td>
<td>Dead Letter / reprocessamento (JMS)</td>
<td>Recuperação de mensagens com falha</td>
<td>[ver](docs/14-c3-dead-letter.md)</td>
</tr>
<tr>
<td>C4</td>
<td>Data Store & Idempotência (2 abordagens)</td>
<td>Persistência temporária e deduplicação de mensagens</td>
<td>[ver](docs/15-c4-data-store.md)</td>
</tr>
</table>


##### Ⓓ Bloco D — CPI Conectividade / Adapters 🥇

<table>
<tr>
<th>\#</th>
<th>Cenário</th>
<th>Objetivo</th>
<th>Doc</th>
</tr>
<tr>
<td>D1</td>
<td>OData Adapter</td>
<td>Integração no padrão SAP S/4HANA</td>
<td>[ver](docs/16-d1-odata-adapter.md)</td>
</tr>
<tr>
<td>D2</td>
<td>SOAP Adapter</td>
<td>Integração com serviços SOAP externos (Split/Gather)</td>
<td>[ver](docs/17-d2-soap-adapter.md)</td>
</tr>
<tr>
<td>D3</td>
<td>SFTP Adapter</td>
<td>Integração de arquivos (hot folder Producer/Consumer)</td>
<td>[ver](docs/18-d3-sftp-adapter.md)</td>
</tr>
<tr>
<td>D4</td>
<td>ProcessDirect + JDBC</td>
<td>Chamar um iFlow a partir de outro + conectividade a banco de dados</td>
<td>[ver](docs/19-d4-processdirect.md)</td>
</tr>
</table>

  
💡 O cenário **D5 — JDBC Adapter**, originalmente planejado separadamente, foi incorporado ao **D4**, que já cobre ProcessDirect + JDBC de forma integrada e realista.

##### Ⓔ Bloco E — API Management 🥇

<table>
<tr>
<th>\#</th>
<th>Cenário</th>
<th>Objetivo</th>
<th>Doc</th>
<th>Status</th>
</tr>
<tr>
<td>E0, E1, E12</td>
<td>Capability, API Proxy e Basic Authentication</td>
<td>Expor backend com autenticação técnica via KVM</td>
<td>[20](docs/20-e-api-management-proxy-basic-auth.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E2</td>
<td>Verify API Key</td>
<td>Controlar acesso por Consumer Key</td>
<td>[21](docs/21-e2-verify-api-key.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E3</td>
<td>OAuth 2.0 e Scopes</td>
<td>Client Credentials, Products, Apps e autorização por escopo</td>
<td>[22](docs/22-e3-oauth-scopes.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E4+E5</td>
<td>Quota e Spike Arrest</td>
<td>Limitar consumo e proteger contra rajadas</td>
<td>[23](docs/23-e4-e5-quota-spike-arrest.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E6+E7</td>
<td>JSON → XML e Assign Message</td>
<td>Resposta XML e visibilidade condicional por scope</td>
<td>[24](docs/24-e6-e7-mes-order-status-backend.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E8+E9</td>
<td>Products, Rate Plans, Apps e Developer Hub</td>
<td>Distribuição e consumo governado</td>
<td>[21](docs/21-e2-verify-api-key.md), [22](docs/22-e3-oauth-scopes.md), [23](docs/23-e4-e5-quota-spike-arrest.md), [24](docs/24-e6-e7-mes-order-status-backend.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E10</td>
<td>API Analytics</td>
<td>Overview, Health, Usage e Custom View</td>
<td>[25](docs/25-e10-api-analytics.md)</td>
<td>✅</td>
</tr>
</table>

  
✅ **Bloco E concluído em 16/08/2026.**

##### Ⓕ Bloco F — Segurança (transversal) 🥇

<table>
<tr>
<th>\#</th>
<th>Cenário</th>
<th>Objetivo</th>
<th>Doc</th>
<th>Status</th>
</tr>
<tr>
<td>F1</td>
<td>Basic Authentication</td>
<td>Já praticado no Bloco E; melhorias ficam para pós-certificação</td>
<td>[20](docs/20-e-api-management-proxy-basic-auth.md), [24](docs/24-e6-e7-mes-order-status-backend.md)</td>
<td>✅ Coberto</td>
</tr>
<tr>
<td>F2</td>
<td>API Key</td>
<td>Já praticado no E2</td>
<td>[21](docs/21-e2-verify-api-key.md)</td>
<td>✅ Coberto</td>
</tr>
<tr>
<td>F3</td>
<td>OAuth 2.0</td>
<td>Já praticado no E3 e E6+E7</td>
<td>[22](docs/22-e3-oauth-scopes.md), [24](docs/24-e6-e7-mes-order-status-backend.md)</td>
<td>✅ Coberto</td>
</tr>
<tr>
<td>F4</td>
<td>Keystore, Client Certificate e mTLS</td>
<td>Autenticação B2B inbound com X.509</td>
<td>[26](docs/26-f4-b2b-client-certificate-mtls.md)</td>
<td>✅</td>
</tr>
<tr>
<td>F5</td>
<td>CSRF real</td>
<td>Token e cookies de sessão em alteração de pedido SAP MM</td>
<td>[27](docs/27-f5-csrf-token-validation.md)</td>
<td>✅</td>
</tr>
<tr>
<td>F6</td>
<td>API Threat Protection</td>
<td>JSON, XML e Regular Expression Protection</td>
<td>[28](docs/28-f6-api-threat-protection.md)</td>
<td>✅</td>
</tr>
<tr>
<td>F7</td>
<td>PGP Message-Level Security</td>
<td>Criptografia, assinatura, verificação e testes negativos</td>
<td>[29](docs/29-f7-pgp-message-level-security.md)</td>
<td>✅</td>
</tr>
<tr>
<td>F8A–F8B</td>
<td>Authentication Context e Technical User SAML Bearer</td>
<td>Contexto inbound, anti-spoofing, RFC 7522, introspecção e autorização</td>
<td>[30](docs/30-f8-authentication-context-technical-user-saml-bearer.md)</td>
<td>✅</td>
</tr>
<tr>
<td>F8E</td>
<td>End-User SAML Bearer Group-Based Authorization</td>
<td>Autorização por grupos com usuários reais (Buyer/Manager) no WSO2</td>
<td>[31](docs/31-f8e-end-user-saml-bearer-group-based-authorization.md)</td>
<td>✅</td>
</tr>
</table>

  
🔒 Hardening futuro pós-certificação: combinar **mTLS + CSRF** e aprofundar testes já cobertos de Basic Auth, API Key e OAuth.

##### Ⓖ Bloco G — Cenários SAP MM / PP / QM 🥈

<table>
<tr>
<th>\#</th>
<th>Cenário</th>
<th>Objetivo</th>
<th>Doc</th>
</tr>
<tr>
<td>G1</td>
<td>SAP MM — Validação de material</td>
<td>Validar movimentação de estoque</td>
<td>—</td>
</tr>
<tr>
<td>G2</td>
<td>SAP PP — Ordem de produção</td>
<td>Processar confirmação de produção</td>
<td>—</td>
</tr>
<tr>
<td>G3</td>
<td>SAP QM — Inspeção de qualidade</td>
<td>Tratar resultado de inspeção</td>
<td>—</td>
</tr>
</table>


##### Ⓗ Bloco H — Event-Driven Integration (Event Mesh) 🥈

<table>
<tr>
<th>\#</th>
<th>Cenário</th>
<th>Objetivo</th>
<th>Doc</th>
<th>Status</th>
</tr>
<tr>
<td>H1</td>
<td>Solace PubSub+ Event Mesh Foundation</td>
<td>Broker, topic, durable queue, Direct e Guaranteed Messaging (SAP MM)</td>
<td>[32](docs/32-h1-solace-pubsub-event-mesh-foundation.md)</td>
<td>✅</td>
</tr>
<tr>
<td>H2</td>
<td>CPI Publisher para Solace via AMQP 1.0</td>
<td>Publicar eventos de negócio com TLS/SASL e correlação ponta a ponta</td>
<td>[33](docs/33-h2-sap-cloud-integration-publisher-solace-amqp.md)</td>
<td>✅</td>
</tr>
<tr>
<td>H3</td>
<td>CPI Subscriber do Solace via AMQP (SAP PP/MES)</td>
<td>Consumir backlog de confirmações de produção e entregar a backend externo</td>
<td>[34](docs/34-h3-sap-cloud-integration-subscriber-solace-amqp.md)</td>
<td>✅</td>
</tr>
<tr>
<td>H4</td>
<td>Competing Consumers e Escala Horizontal (SAP WM)</td>
<td>Non-Exclusive Queue, múltiplos workers, falha e recuperação dinâmica</td>
<td>[35](docs/35-h4-solace-competing-consumers-scaling.md)</td>
<td>✅</td>
</tr>
</table>

  
🔄 Bloco H em andamento: cenários adicionais de roteamento por tópico, resiliência (DLQ/replay) e integração híbrida (MQTT/Kafka) planejados como continuidade.

#### 🧰 Ferramentas utilizadas
- **SAP BTP** (Business Technology Platform)
- **SAP Integration Suite** (Cloud Integration + API Management + Event Mesh)
- **Solace PubSub+ Cloud** (event broker AMQP 1.0/MQTT/REST usado no Bloco H)
- **Node.js + rhea** (OMS Simulator AMQP 1.0 para publicação de eventos)
- **Postman** (envio e teste de mensagens)
- **Webhook.site, RequestBin e Beeceptor** (validação de recebimento e backends externos)
- **APIs públicas** (ex.: JSONPlaceholder, dataaccess.com NumberConversion, Northwind OData V4) para simulação de backends
- **SFTPCloud** (servidor SFTP gratuito de teste, usado na integração de arquivos)
- **Neon** (banco de dados PostgreSQL serverless gratuito, usado na conectividade JDBC)
- **Mockoon + ngrok** (simulação local de sistemas externos, como ERP)
- **SAP Developer Hub** (portal de gestão de Applications e Subscriptions do API Management)
- **VS Code + Git** (versionamento e documentação)
- **GitHub** (portfólio e controle de versão)

#### 🔁 Fluxo de trabalho
- Desenvolver o iFlow no SAP Integration Suite (navegador)
- Testar e capturar evidências (prints do monitoramento)
- Exportar o iFlow (.zip) do Integration Suite
- Adicionar o artefato em iflows/ e as evidências em evidences/
- Documentar o cenário em docs/
- Commit e push via VS Code (Source Control)

#### 📚 Referências oficiais SAP
- 🥇 Trilha principal: [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite)
- 🥈 Event-Driven: [Discovering Event-Driven Integration with SAP Integration Suite, advanced event mesh](https://learning.sap.com/courses/discovering-event-driven-integration-with-sap-integration-suite-advanved-event-mesh)
- 🥈 Tutoriais AEM: [Get Started with SAP Integration Suite, advanced event mesh](https://developers.sap.com/mission.advanced-event-mesh-get-started.html)
- 📖 Visão geral: [SAP Integration Suite — SAP Learning](https://learning.sap.com/products/business-technology-platform/integration-suite)

#### 👤 Autor / 📬 Contato
  
[LinkedIn](https://www.linkedin.com/in/orlando-caetano/)[GitHub](https://github.com/OrlandoCaetano2026)  
**Orlando Caetano**Especialista SAP • Integração • Inteligência Artificial
Consultor SAP MM com know-how em PP, QM e WM  
[SAP MM](https://img.shields.io/badge/SAP-MM-0FAAFF?logo=sap&logoColor=white) [SAP PP](https://img.shields.io/badge/SAP-PP-0FAAFF?logo=sap&logoColor=white) [SAP QM](https://img.shields.io/badge/SAP-QM-0FAAFF?logo=sap&logoColor=white) [SAP WM](https://img.shields.io/badge/SAP-WM-0FAAFF?logo=sap&logoColor=white)  
📌 Projeto de estudo e portfólio. Os cenários SAP MM, PP, QM, WM, MES e Event-Driven são simulações educativas para prática de integração.
