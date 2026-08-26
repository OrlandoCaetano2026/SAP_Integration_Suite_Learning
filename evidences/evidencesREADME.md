<div align="center">

# 📸 Índice de Evidências de Execução

### Rastreabilidade completa — SAP Integration Suite Learning

![Evidências](https://img.shields.io/badge/evidências-300%2B-0FAAFF?style=for-the-badge&logo=sap&logoColor=white)
![Labs](https://img.shields.io/badge/laboratórios-36-success?style=for-the-badge)
![Rastreável](https://img.shields.io/badge/100%25-rastreável-6C2EB9?style=for-the-badge)

</div>

---

> 📷 Cada pasta `labXX` reúne as **evidências reais de execução** de um cenário: prints de monitoramento, payloads processados, respostas Postman, telas dos brokers e destinos externos. As evidências são numeradas a partir de `01` dentro de cada laboratório e **dados sensíveis são mascarados** antes do commit.

**🧭 Navegação:** [🏠 Principal](../README.md) · [📚 Documentação](../docs/docsREADME.md) · [🎓 Certificação](../certification/certification_README.md) · [📦 iFlows](../iflows/iflows_README.md)

---

## 📊 Volume de evidências por bloco

<table>
<tr><th>Bloco</th><th>Labs</th><th>Foco das evidências</th></tr>
<tr><td>Ⓐ Fundamentos</td><td>lab01–lab04</td><td>Fluxos básicos, mapeamento e Groovy</td></tr>
<tr><td>Ⓑ Padrões</td><td>lab05–lab09</td><td>Roteamento, enriquecimento, split/aggregate</td></tr>
<tr><td>Ⓒ Resiliência</td><td>lab10–lab13</td><td>Exceções, retry, dead letter, deduplicação</td></tr>
<tr><td>Ⓓ Adapters</td><td>lab14–lab17</td><td>OData, SOAP, SFTP, JDBC</td></tr>
<tr><td>Ⓔ API Management</td><td>lab18–lab23</td><td>Proxies, policies, OAuth, analytics</td></tr>
<tr><td>Ⓕ Segurança</td><td>lab24–lab29</td><td>mTLS, CSRF, threat, PGP, SAML Bearer</td></tr>
<tr><td>Ⓗ Event Mesh</td><td>lab30-lab36</td><td>Broker, AMQP, competing consumers, wildcards, DMQ, replay e MQTT</td></tr>
</table>

---

## Ⓐ Bloco A — CPI Fundamentos

<table>
<tr><th>Lab</th><th>Cenário</th><th>O que comprova</th><th>Links</th></tr>
<tr><td>lab01</td><td>A1 · HTTP → Webhook</td><td>Recebimento HTTP, ajuste de payload e encaminhamento externo</td><td><a href="./lab01">📁 pasta</a> · <a href="../docs/03-a1-http-to-webhook.md">📄 doc</a></td></tr>
<tr><td>lab02</td><td>A2 · Timer → API pública</td><td>Disparo agendado e consumo de API externa</td><td><a href="./lab02">📁 pasta</a> · <a href="../docs/04-a2-timer-to-api.md">📄 doc</a></td></tr>
<tr><td>lab03</td><td>A3 · Message Mapping</td><td>Transformação JSON → XML com mapeamento gráfico</td><td><a href="./lab03">📁 pasta</a> · <a href="../docs/05-a3-message-mapping.md">📄 doc</a></td></tr>
<tr><td>lab04</td><td>A4 · Groovy Script</td><td>Enriquecimento programático do payload</td><td><a href="./lab04">📁 pasta</a> · <a href="../docs/06-a4-groovy-script.md">📄 doc</a></td></tr>
</table>

## Ⓑ Bloco B — Padrões de Integração

<table>
<tr><th>Lab</th><th>Cenário</th><th>O que comprova</th><th>Links</th></tr>
<tr><td>lab05</td><td>B1 · Content-Based Router</td><td>Roteamento por conteúdo em múltiplos caminhos</td><td><a href="./lab05">📁</a> · <a href="../docs/07-b1-content-based-router.md">📄</a></td></tr>
<tr><td>lab06</td><td>B2 · Content Enricher</td><td>Enriquecimento via lookup OData</td><td><a href="./lab06">📁</a> · <a href="../docs/08-b2-content-enricher.md">📄</a></td></tr>
<tr><td>lab07</td><td>B3 · Splitter</td><td>Divisão de lote em itens individuais</td><td><a href="./lab07">📁</a> · <a href="../docs/09-b3-splitter.md">📄</a></td></tr>
<tr><td>lab08</td><td>B4 · Aggregator</td><td>Consolidação com CamelSplitComplete</td><td><a href="./lab08">📁</a> · <a href="../docs/10-b4-aggregator.md">📄</a></td></tr>
<tr><td>lab09</td><td>B5 · Multicast</td><td>Distribuição simultânea a múltiplos destinos</td><td><a href="./lab09">📁</a> · <a href="../docs/11-b5-multicast.md">📄</a></td></tr>
</table>

## Ⓒ Bloco C — Resiliência e Erros

<table>
<tr><th>Lab</th><th>Cenário</th><th>O que comprova</th><th>Links</th></tr>
<tr><td>lab10</td><td>C1 · Exception Subprocess</td><td>Tratamento de erro com respostas 200/422</td><td><a href="./lab10">📁</a> · <a href="../docs/12-c1-exception-subprocess.md">📄</a></td></tr>
<tr><td>lab11</td><td>C2 · Retry</td><td>Reenvio automático em falhas temporárias</td><td><a href="./lab11">📁</a> · <a href="../docs/13-c2-retry-timeout.md">📄</a></td></tr>
<tr><td>lab12</td><td>C3 · Dead Letter (JMS)</td><td>Guaranteed delivery, retry assíncrono e dead letter</td><td><a href="./lab12">📁</a> · <a href="../docs/14-c3-dead-letter.md">📄</a></td></tr>
<tr><td>lab13</td><td>C4 · Data Store & Idempotência</td><td>Deduplicação: create 201, duplicate 409, update 200</td><td><a href="./lab13">📁</a> · <a href="../docs/15-c4-data-store.md">📄</a></td></tr>
</table>

## Ⓓ Bloco D — Conectividade / Adapters

<table>
<tr><th>Lab</th><th>Cenário</th><th>O que comprova</th><th>Links</th></tr>
<tr><td>lab14</td><td>D1 · OData Adapter</td><td>Consulta dinâmica a serviço OData V4</td><td><a href="./lab14">📁</a> · <a href="../docs/16-d1-odata-adapter.md">📄</a></td></tr>
<tr><td>lab15</td><td>D2 · SOAP Adapter</td><td>Split/Gather com Web Service SOAP externo</td><td><a href="./lab15">📁</a> · <a href="../docs/17-d2-soap-adapter.md">📄</a></td></tr>
<tr><td>lab16</td><td>D3 · SFTP Adapter</td><td>Producer grava e Consumer processa via polling</td><td><a href="./lab16">📁</a> · <a href="../docs/18-d3-sftp-adapter.md">📄</a></td></tr>
<tr><td>lab17</td><td>D4 · ProcessDirect + JDBC</td><td>Reuso interno e validação via banco PostgreSQL</td><td><a href="./lab17">📁</a> · <a href="../docs/19-d4-processdirect.md">📄</a></td></tr>
</table>

## Ⓔ Bloco E — API Management

<table>
<tr><th>Lab</th><th>Cenário</th><th>O que comprova</th><th>Links</th></tr>
<tr><td>lab18</td><td>E0/E1/E12 · API Proxy + Basic Auth</td><td>Proxy protegido com credenciais via KVM</td><td><a href="./lab18">📁</a> · <a href="../docs/20-e-api-management-proxy-basic-auth.md">📄</a></td></tr>
<tr><td>lab19</td><td>E2 · Verify API Key</td><td>Controle de acesso por Consumer Key</td><td><a href="./lab19">📁</a> · <a href="../docs/21-e2-verify-api-key.md">📄</a></td></tr>
<tr><td>lab20</td><td>E3 · OAuth 2.0 e Scopes</td><td>Client Credentials e autorização por escopo</td><td><a href="./lab20">📁</a> · <a href="../docs/22-e3-oauth-scopes.md">📄</a></td></tr>
<tr><td>lab21</td><td>E4+E5 · Quota e Spike Arrest</td><td>Controle de consumo e proteção contra rajadas</td><td><a href="./lab21">📁</a> · <a href="../docs/23-e4-e5-quota-spike-arrest.md">📄</a></td></tr>
<tr><td>lab22</td><td>E6+E7 · MES Order Status</td><td>Assign Message, mascaramento por scope e JSON → XML</td><td><a href="./lab22">📁</a> · <a href="../docs/24-e6-e7-mes-order-status-backend.md">📄</a></td></tr>
<tr><td>lab23</td><td>E10 · API Analytics</td><td>Overview, Health, Usage e Custom View</td><td><a href="./lab23">📁</a> · <a href="../docs/25-e10-api-analytics.md">📄</a></td></tr>
</table>

## Ⓕ Bloco F — Segurança

<table>
<tr><th>Lab</th><th>Cenário</th><th>O que comprova</th><th>Links</th></tr>
<tr><td>lab24</td><td>F4 · Client Certificate e mTLS</td><td>Autenticação X.509 e mTLS B2B</td><td><a href="./lab24">📁</a> · <a href="../docs/26-f4-b2b-client-certificate-mtls.md">📄</a></td></tr>
<tr><td>lab25</td><td>F5 · CSRF Token Validation</td><td>Token CSRF e cookies de sessão em alteração SAP MM</td><td><a href="./lab25">📁</a> · <a href="../docs/27-f5-csrf-token-validation.md">📄</a></td></tr>
<tr><td>lab26</td><td>F6 · API Threat Protection</td><td>Proteção JSON, XML e Regex</td><td><a href="./lab26">📁</a> · <a href="../docs/28-f6-api-threat-protection.md">📄</a></td></tr>
<tr><td>lab27</td><td>F7 · PGP Message-Level Security</td><td>Criptografia, assinatura, verificação e testes negativos</td><td><a href="./lab27">📁</a> · <a href="../docs/29-f7-pgp-message-level-security.md">📄</a></td></tr>
<tr><td>lab28</td><td>F8A–F8B · Auth Context e SAML Bearer</td><td>26 evidências: anti-spoofing, WSO2, RFC 7522, introspection e autorização</td><td><a href="./lab28">📁</a> · <a href="../docs/30-f8-authentication-context-technical-user-saml-bearer.md">📄</a></td></tr>
<tr><td>lab29</td><td>F8E · Group-Based Authorization</td><td>Autorização por grupos com usuários reais no WSO2</td><td><a href="./lab29">📁</a> · <a href="../docs/31-f8e-end-user-saml-bearer-group-based-authorization.md">📄</a></td></tr>
</table>

> 💡 O **Documento 30** também descreve a exploração arquitetural do **F8C** (login humano via IAS/XSUAA/Approuter), documentada sem evidências próprias.

## Ⓗ Bloco H — Event-Driven Integration (Event Mesh)

<table>
<tr><th>Lab</th><th>Cenário</th><th>O que comprova</th><th>Links</th></tr>
<tr><td>lab30</td><td>H1 · Event Mesh Foundation</td><td>Broker, durable queue, Direct e Guaranteed Messaging</td><td><a href="./lab30">📁</a> · <a href="../docs/32-h1-solace-pubsub-event-mesh-foundation.md">📄</a></td></tr>
<tr><td>Lab31</td><td>H2 · CPI Publisher via AMQP</td><td>Publicação AMQP 1.0 com envelope e correlação ponta a ponta</td><td><a href="./Lab31">📁</a> · <a href="../docs/33-h2-sap-cloud-integration-publisher-solace-amqp.md">📄</a></td></tr>
<tr><td>Lab32</td><td>H3 · CPI Subscriber via AMQP</td><td>Backlog SAP PP/MES consumido e entregue a backend externo</td><td><a href="./Lab32">📁</a> · <a href="../docs/34-h3-sap-cloud-integration-subscriber-solace-amqp.md">📄</a></td></tr>
<tr><td>lab33</td><td>H4 · Competing Consumers e Escala</td><td>31 evidências: 3 workers SAP WM, 3 backends, falha e recuperação</td><td><a href="./lab33">📁</a> · <a href="../docs/35-h4-solace-competing-consumers-scaling.md">📄</a></td></tr>
<tr><td>lab34</td><td>H5 · Topic Hierarchy, Wildcards e Fan-out</td><td>Roteamento seletivo, subscriptions e distribuição de eventos SAP QM</td><td><a href="./lab34">📁</a> · <a href="../docs/36-h5-solace-topic-hierarchy-wildcards-fanout.md">📄</a></td></tr>
<tr><td>lab35</td><td>H6 · Retry, DMQ, Recovery e Message Replay</td><td>47 evidências de resiliência, poison message, recuperação e replay</td><td><a href="./lab35">📁</a> · <a href="../docs/37-h6-solace-dead-letter-retry-replay.md">📄</a></td></tr>
<tr><td>lab36</td><td>H7 · MQTT Industrial Telemetry</td><td>24 evidências de MQTT/.NET, AMQP, roteamento, ordem de manutenção e e-mail</td><td><a href="./lab36">📁</a> · <a href="../docs/38-h7-solace-mqtt-industrial-telemetry.md">📄</a></td></tr>
</table>

---

> 🔐 **Política de evidências:** numeração a partir de `01` por laboratório, sem continuidade entre labs; dados sensíveis (tokens, secrets, senhas, e-mails) mascarados antes do commit; nomes técnicos descritivos e alinhados ao storytelling de cada documento.

<div align="center">

📌 [Voltar ao README principal](../README.md)

</div>
