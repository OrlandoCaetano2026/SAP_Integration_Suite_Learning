<div align="center">

# 📚 Índice de Documentação Técnica

### Biblioteca de cenários — SAP Integration Suite Learning

![Documentos](https://img.shields.io/badge/documentos-38%2B-0FAAFF?style=for-the-badge&logo=sap&logoColor=white)
![Storytelling](https://img.shields.io/badge/padrão-storytelling%20técnico-6C2EB9?style=for-the-badge)
![Arquitetura](https://img.shields.io/badge/diagramas-Mermaid-00C1D4?style=for-the-badge)

</div>

---

> 📖 Cada documento deste índice descreve **um cenário completo de integração**: contexto de negócio, arquitetura (geral e detalhada em Mermaid), configuração passo a passo, códigos completos, evidências contextualizadas, troubleshooting com causa raiz, boas práticas SAP e recomendações de produção.

**🧭 Navegação:** [🏠 Principal](../README.md) · [🎓 Certificação](../certification/certification_README.md) · [📸 Evidências](../evidences/evidencesREADME.md) · [📦 iFlows](../iflows/iflows_README.md) · [📨 Payloads](../payloads/payloads_README.md) · [📮 Postman](../postman/postman_README.md)

**Legenda de status:** ✅ Concluído · 🔄 Em andamento · ⏳ Planejado

---

## 🧱 Base conceitual

<table>
<tr><th>Doc</th><th>Documento</th><th>O que aborda</th></tr>
<tr>
<td><img src="https://img.shields.io/badge/01-informational?style=flat-square"/></td>
<td><a href="./01-ambiente-btp.md"><b>Ambiente SAP BTP</b></a></td>
<td>Estrutura do ambiente, capabilities do Integration Suite e modelo de autenticação. É a fundação sobre a qual todos os cenários seguintes são construídos.</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/02-informational?style=flat-square"/></td>
<td><a href="./02-cloud-integration-basics.md"><b>Cloud Integration (CPI) Básico</b></a></td>
<td>Conceitos fundamentais de Integration Flows, adapters, Content Modifier e monitoramento — o vocabulário essencial usado em todo o projeto.</td>
</tr>
</table>

---

## Ⓐ Bloco A — CPI Fundamentos

> A jornada começa aqui: os primeiros iFlows, aprendendo a **receber, transformar, encaminhar e customizar** mensagens. É onde se domina o ciclo básico de um fluxo de integração.

<table>
<tr><th>Doc</th><th>Cenário</th><th>Storytelling técnico</th><th>Status</th></tr>
<tr>
<td><img src="https://img.shields.io/badge/03-blue?style=flat-square"/></td>
<td><a href="./03-a1-http-to-webhook.md"><b>A1 · HTTP → Webhook</b></a></td>
<td>O primeiro iFlow do projeto: recebe uma requisição HTTP, ajusta headers e body com Content Modifier e encaminha para um endpoint externo. Introduz o modelo Sender → Process → Receiver.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/04-blue?style=flat-square"/></td>
<td><a href="./04-a2-timer-to-api.md"><b>A2 · Timer → API pública</b></a></td>
<td>Dispara um fluxo por agendamento (Timer) e consome uma API externa via Request Reply, demonstrando integração ativa sem trigger externo.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/05-blue?style=flat-square"/></td>
<td><a href="./05-a3-message-mapping.md"><b>A3 · Message Mapping</b></a></td>
<td>Transforma estrutura e formato de mensagens (JSON → XML) com o editor gráfico de mapeamento — competência central da certificação.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/06-blue?style=flat-square"/></td>
<td><a href="./06-a4-groovy-script.md"><b>A4 · Groovy Script</b></a></td>
<td>Adiciona lógica de negócio e enriquecimento programático ao fluxo com Groovy, abrindo espaço para regras que o mapeamento visual não cobre.</td>
<td>✅</td>
</tr>
</table>

---

## Ⓑ Bloco B — Padrões de Integração

> Aqui entram os **Enterprise Integration Patterns**: rotear, enriquecer, dividir, agregar e distribuir mensagens. São os padrões que aparecem em praticamente todo projeto real.

<table>
<tr><th>Doc</th><th>Cenário</th><th>Storytelling técnico</th><th>Status</th></tr>
<tr>
<td><img src="https://img.shields.io/badge/07-blue?style=flat-square"/></td>
<td><a href="./07-b1-content-based-router.md"><b>B1 · Content-Based Router</b></a></td>
<td>Roteia mensagens para caminhos diferentes conforme o conteúdo (XPath), simulando priorização de pedidos por valor.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/08-blue?style=flat-square"/></td>
<td><a href="./08-b2-content-enricher.md"><b>B2 · Content Enricher</b></a></td>
<td>Enriquece a mensagem com dados de uma fonte externa via lookup OData V4, combinando informação de dois sistemas.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/09-blue?style=flat-square"/></td>
<td><a href="./09-b3-splitter.md"><b>B3 · Splitter</b></a></td>
<td>Divide um lote de itens em mensagens individuais, permitindo processamento item a item.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/10-blue?style=flat-square"/></td>
<td><a href="./10-b4-aggregator.md"><b>B4 · Aggregator</b></a></td>
<td>Consolida múltiplas mensagens em uma só, usando CamelSplitComplete para saber quando o lote terminou.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/11-blue?style=flat-square"/></td>
<td><a href="./11-b5-multicast.md"><b>B5 · Multicast</b></a></td>
<td>Distribui a mesma mensagem simultaneamente para múltiplos destinos (MES, PLM, ERP), padrão de fan-out.</td>
<td>✅</td>
</tr>
</table>

---

## Ⓒ Bloco C — Resiliência e Erros

> Integração de verdade falha, e este bloco ensina a **falhar bem**: tratar exceções, reprocessar, garantir entrega e evitar duplicidade. É o que separa um fluxo de laboratório de um fluxo produtivo.

<table>
<tr><th>Doc</th><th>Cenário</th><th>Storytelling técnico</th><th>Status</th></tr>
<tr>
<td><img src="https://img.shields.io/badge/12-blue?style=flat-square"/></td>
<td><a href="./12-c1-exception-subprocess.md"><b>C1 · Exception Subprocess</b></a></td>
<td>Implementa um try/catch visual, padronizando o tratamento de erros e retornando respostas HTTP coerentes (200 vs 422).</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/13-blue?style=flat-square"/></td>
<td><a href="./13-c2-retry-timeout.md"><b>C2 · Retry</b></a></td>
<td>Reenvia automaticamente em falhas temporárias, testado com endpoints que alternam sucesso e erro.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/14-blue?style=flat-square"/></td>
<td><a href="./14-c3-dead-letter.md"><b>C3 · Dead Letter (JMS)</b></a></td>
<td>Usa filas JMS para guaranteed delivery, retry assíncrono e dead letter, com Producer e Consumer desacoplados.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/15-blue?style=flat-square"/></td>
<td><a href="./15-c4-data-store.md"><b>C4 · Data Store & Idempotência</b></a></td>
<td>Duas abordagens de deduplicação: Data Store manual e Idempotent Process Call, evitando processar o mesmo pedido duas vezes.</td>
<td>✅</td>
</tr>
</table>

---

## Ⓓ Bloco D — Conectividade / Adapters

> Sistemas reais falam protocolos diferentes. Este bloco conecta o CPI a **OData, SOAP, SFTP e bancos de dados**, além de reuso interno de lógica com ProcessDirect.

<table>
<tr><th>Doc</th><th>Cenário</th><th>Storytelling técnico</th><th>Status</th></tr>
<tr>
<td><img src="https://img.shields.io/badge/16-blue?style=flat-square"/></td>
<td><a href="./16-d1-odata-adapter.md"><b>D1 · OData Adapter</b></a></td>
<td>Consulta dinâmica a um serviço OData V4, o padrão de integração do S/4HANA.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/17-blue?style=flat-square"/></td>
<td><a href="./17-d2-soap-adapter.md"><b>D2 · SOAP Adapter</b></a></td>
<td>Integra com um Web Service SOAP externo usando Split/Gather para processar múltiplos itens de uma nota fiscal.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/18-blue?style=flat-square"/></td>
<td><a href="./18-d3-sftp-adapter.md"><b>D3 · SFTP Adapter</b></a></td>
<td>Integração de arquivos via hot folder, com Producer gravando e Consumer fazendo polling e processamento.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/19-blue?style=flat-square"/></td>
<td><a href="./19-d4-processdirect.md"><b>D4 · ProcessDirect + JDBC</b></a></td>
<td>Comunicação interna entre iFlows e validação de fornecedor consultando um banco PostgreSQL via JDBC. O cenário D5 (JDBC) foi incorporado aqui.</td>
<td>✅</td>
</tr>
</table>

---

## Ⓔ Bloco E — API Management

> O foco muda de fluxos para **APIs governadas**: proxies, políticas de segurança, produtos, planos de consumo e analytics. É a camada que expõe e protege as integrações.

<table>
<tr><th>Doc</th><th>Cenário</th><th>Storytelling técnico</th><th>Status</th></tr>
<tr>
<td><img src="https://img.shields.io/badge/20-blueviolet?style=flat-square"/></td>
<td><a href="./20-e-api-management-proxy-basic-auth.md"><b>E0/E1/E12 · API Proxy + Basic Auth</b></a></td>
<td>Cria um API Provider e Proxy, protegendo o backend com Basic Authentication armazenada em Key Value Map.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/21-blueviolet?style=flat-square"/></td>
<td><a href="./21-e2-verify-api-key.md"><b>E2 · Verify API Key</b></a></td>
<td>Controla acesso por Consumer Key, com API Product e Developer App.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/22-blueviolet?style=flat-square"/></td>
<td><a href="./22-e3-oauth-scopes.md"><b>E3 · OAuth 2.0 e Scopes</b></a></td>
<td>Client Credentials e autorização por escopo, incluindo escrita via API com scope dedicado.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/23-blueviolet?style=flat-square"/></td>
<td><a href="./23-e4-e5-quota-spike-arrest.md"><b>E4+E5 · Quota e Spike Arrest</b></a></td>
<td>Limita o consumo por período e protege o backend contra rajadas de tráfego.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/24-blueviolet?style=flat-square"/></td>
<td><a href="./24-e6-e7-mes-order-status-backend.md"><b>E6+E7 · MES Order Status</b></a></td>
<td>Backend com Assign Message, mascaramento condicional por scope e resposta JSON → XML, cobrindo também E8/E9 na prática.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/25-blueviolet?style=flat-square"/></td>
<td><a href="./25-e10-api-analytics.md"><b>E10 · API Analytics</b></a></td>
<td>Overview, Health, Usage e Custom View, transformando dados de tráfego em visão operacional.</td>
<td>✅</td>
</tr>
</table>

---

## Ⓕ Bloco F — Segurança

> O bloco mais denso: **certificados e mTLS, CSRF real, proteção contra ameaças, criptografia PGP e propagação de identidade via SAML Bearer**. Aqui a integração ganha maturidade corporativa.

<table>
<tr><th>Doc</th><th>Cenário</th><th>Storytelling técnico</th><th>Status</th></tr>
<tr>
<td><img src="https://img.shields.io/badge/26-critical?style=flat-square"/></td>
<td><a href="./26-f4-b2b-client-certificate-mtls.md"><b>F4 · B2B Client Certificate e mTLS</b></a></td>
<td>Autenticação de cliente via X.509 e mTLS num cenário B2B realista, com Certificate Service Key e keystore.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/27-critical?style=flat-square"/></td>
<td><a href="./27-f5-csrf-token-validation.md"><b>F5 · CSRF Token Validation</b></a></td>
<td>Token CSRF e cookies vinculados à sessão em uma alteração de pedido SAP MM, com fluxo Fetch + Post.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/28-critical?style=flat-square"/></td>
<td><a href="./28-f6-api-threat-protection.md"><b>F6 · API Threat Protection</b></a></td>
<td>Proteção contra ameaças em JSON, XML e Regular Expression, defendendo a API de payloads maliciosos.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/29-critical?style=flat-square"/></td>
<td><a href="./29-f7-pgp-message-level-security.md"><b>F7 · PGP Message-Level Security</b></a></td>
<td>Criptografia, assinatura e verificação PGP, incluindo testes negativos (assinante não autorizado e mensagem sem assinatura).</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/30-critical?style=flat-square"/></td>
<td><a href="./30-f8-authentication-context-technical-user-saml-bearer.md"><b>F8A–F8B · Auth Context e Technical User SAML Bearer</b></a></td>
<td>Captura de contexto inbound, anti-spoofing, RFC 7522, token introspection e autorização READ/APPROVE. Inclui a exploração arquitetural do F8C (login humano via IAS/XSUAA/Approuter).</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/31-critical?style=flat-square"/></td>
<td><a href="./31-f8e-end-user-saml-bearer-group-based-authorization.md"><b>F8E · End-User Group-Based Authorization</b></a></td>
<td>Autorização por grupos com usuários reais (Buyer e Manager) no WSO2, mapeando privilégios a operações.</td>
<td>✅</td>
</tr>
</table>

---

## Ⓗ Bloco H — Event-Driven Integration (Event Mesh)

> A fronteira do projeto: **arquitetura orientada a eventos** com Solace PubSub+ e AMQP 1.0. Publicar, consumir, escalar e recuperar — os padrões que sustentam integrações modernas e desacopladas.

<table>
<tr><th>Doc</th><th>Cenário</th><th>Storytelling técnico</th><th>Status</th></tr>
<tr>
<td><img src="https://img.shields.io/badge/32-00C1D4?style=flat-square"/></td>
<td><a href="./32-h1-solace-pubsub-event-mesh-foundation.md"><b>H1 · Event Mesh Foundation</b></a></td>
<td>Constrói o event broker do zero: topic hierárquico, durable queue e a diferença prática entre Direct e Guaranteed Messaging, usando eventos de Purchase Order (SAP MM).</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/33-00C1D4?style=flat-square"/></td>
<td><a href="./33-h2-sap-cloud-integration-publisher-solace-amqp.md"><b>H2 · CPI Publisher via AMQP</b></a></td>
<td>Transforma o CPI em produtor AMQP 1.0 (TLS/SASL), construindo um event envelope e provando a correlação ponta a ponta entre CPI e broker.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/34-00C1D4?style=flat-square"/></td>
<td><a href="./34-h3-sap-cloud-integration-subscriber-solace-amqp.md"><b>H3 · CPI Subscriber via AMQP</b></a></td>
<td>O CPI vira consumidor: drena um backlog de confirmações de produção (SAP PP/MES), valida campos técnicos e entrega a um backend externo, provando desacoplamento temporal.</td>
<td>✅</td>
</tr>
<tr>
<td><img src="https://img.shields.io/badge/35-00C1D4?style=flat-square"/></td>
<td><a href="./35-h4-solace-competing-consumers-scaling.md"><b>H4 · Competing Consumers e Escala</b></a></td>
<td>Non-Exclusive Queue com três workers SAP WM e três backends distintos, demonstrando escala horizontal, falha controlada e recuperação dinâmica de capacidade.</td>
<td>✅</td>
</tr>
<tr><td></td><td><a href="./36-h5-solace-topic-hierarchy-wildcards-fanout.md"><b>H5 · Topic Hierarchy, Wildcards e Fan-out</b></a></td><td>Roteamento seletivo e fan-out no Solace para eventos SAP QM.</td><td>✅</td></tr>
<tr><td></td><td><a href="./37-h6-solace-dead-letter-retry-replay.md"><b>H6 · Retry, DMQ, Recovery e Message Replay</b></a></td><td>Retry interno, poison message, DMQ dedicada, recuperação e Message Replay para SAP MM.</td><td>✅</td></tr>
<tr><td></td><td><a href="./38-h7-solace-mqtt-industrial-telemetry.md"><b>H7 · MQTT Industrial Telemetry</b></a></td><td>Publisher .NET/MQTT, consumo AMQP, classificação, ordem SAP PM-like e alerta SMTP.</td><td>✅</td></tr>
</table>

---

## 🔗 Rastreabilidade: Doc ↔ Lab ↔ iFlow

<table>
<tr><th>Bloco</th><th>Documentos</th><th>Evidências</th><th>iFlows</th></tr>
<tr><td>A–C</td><td>03–15</td><td><a href="../evidences/evidencesREADME.md">lab01–lab13</a></td><td><a href="../iflows/iflows_README.md">A1–C4B</a></td></tr>
<tr><td>D–E</td><td>16–25</td><td><a href="../evidences/evidencesREADME.md">lab14–lab23</a></td><td><a href="../iflows/iflows_README.md">D1–E6/E7</a></td></tr>
<tr><td>F</td><td>26–31</td><td><a href="../evidences/evidencesREADME.md">lab24–lab29</a></td><td><a href="../iflows/iflows_README.md">F4–F8E</a></td></tr>
<tr><td>H</td><td>32-38</td><td><a href="../evidences/evidencesREADME.md">lab30-lab36</a></td><td><a href="../iflows/iflows_README.md">H2-H7</a></td></tr>
</table>

---

## 📍 Status atual

- ✅ Blocos **A, B, C, D e E** concluídos.
- ✅ **F1, F2 e F3** cobertos nos cenários de API Management; **F4, F5, F6 e F7** concluídos.
- ✅ **F8A/F8B** no Documento 30 (com a exploração arquitetural do F8C) e **F8E** no Documento 31.
- ✅ Bloco **H (Event-Driven / Event Mesh)** com **H1 a H7 concluídos** nos Documentos 32 a 38; expansão pós-certificação planejada.

<div align="center">

📌 [Voltar ao README principal](../README.md)

</div>
