<div align="center">

# 🎓 Painel de Preparação para a Certificação

### SAP Certified Associate — Integration Developer

![Certification](https://img.shields.io/badge/SAP%20Certified%20Associate-Integration%20Developer-0FAAFF?style=for-the-badge&logo=sap&logoColor=white)
![Readiness](https://img.shields.io/badge/preparação-avançada-success?style=for-the-badge)
![Blocos](https://img.shields.io/badge/blocos%20cobertos-6%2F8-blue?style=for-the-badge)

</div>

---

> 🎯 Este painel acompanha a **preparação prática** para a certificação, mapeando cada domínio do exame a laboratórios concretos, documentos e evidências deste repositório. A filosofia é simples: **provar competência construindo, não apenas estudando**.

**🧭 Navegação:** [🏠 Principal](../README.md) · [📚 Documentação](../docs/docsREADME.md) · [📸 Evidências](../evidences/evidencesREADME.md) · [📦 iFlows](../iflows/iflows_README.md)

---

## 📊 Progresso geral por bloco

<table>
<tr><th>Bloco</th><th>Período</th><th>Status</th><th>Escopo</th></tr>
<tr><td>Ⓐ CPI Fundamentos</td><td>01–03/08/2026</td><td>![](https://img.shields.io/badge/-concluído-success)</td><td>A1–A4</td></tr>
<tr><td>Ⓑ Padrões de Integração</td><td>03–04/08/2026</td><td>![](https://img.shields.io/badge/-concluído-success)</td><td>B1–B5</td></tr>
<tr><td>Ⓒ Resiliência e Erros</td><td>04–06/08/2026</td><td>![](https://img.shields.io/badge/-concluído-success)</td><td>C1–C4</td></tr>
<tr><td>Ⓓ Conectividade / Adapters</td><td>06–10/08/2026</td><td>![](https://img.shields.io/badge/-concluído-success)</td><td>D1–D4 (JDBC no D4)</td></tr>
<tr><td>Ⓔ API Management</td><td>11–16/08/2026</td><td>![](https://img.shields.io/badge/-concluído-success)</td><td>E0–E12</td></tr>
<tr><td>Ⓕ Segurança</td><td>16–24/08/2026</td><td>![](https://img.shields.io/badge/-concluído-success)</td><td>F4–F8</td></tr>
<tr><td>Ⓖ SAP MM / PP / QM</td><td>—</td><td>![](https://img.shields.io/badge/-planejado-lightgrey)</td><td>G1–G3</td></tr>
<tr><td>Ⓗ Event-Driven e E2E</td><td>24/08/2026</td><td>![](https://img.shields.io/badge/-em%20andamento-yellow)</td><td>H1–H4</td></tr>
</table>

---

## 🗺️ Blueprint do exame → cobertura no projeto

> Mapa entre os grandes temas cobrados na certificação e onde cada um é praticado, com o nível de preparo estimado.

<table>
<tr><th>Tema do exame</th><th>Documentos / Blocos</th><th>Preparo</th></tr>
<tr>
<td><b>Modelagem de Integration Flows</b></td>
<td>Docs 03–15 (Blocos A, B, C)</td>
<td>![](https://img.shields.io/badge/-alto-success)</td>
</tr>
<tr>
<td><b>Mapeamento e transformação</b></td>
<td>Docs 05 (A3), 08 (B2), 24 (E6+E7)</td>
<td>![](https://img.shields.io/badge/-alto-success)</td>
</tr>
<tr>
<td><b>Conectividade e adapters</b></td>
<td>Docs 16–19 (OData, SOAP, SFTP, JDBC, ProcessDirect)</td>
<td>![](https://img.shields.io/badge/-alto-success)</td>
</tr>
<tr>
<td><b>Tratamento de erros e resiliência</b></td>
<td>Docs 12–15 (Exception, Retry, Dead Letter, Idempotência)</td>
<td>![](https://img.shields.io/badge/-alto-success)</td>
</tr>
<tr>
<td><b>API Management</b></td>
<td>Docs 20–25 (Proxy, API Key, OAuth, Quota, Analytics)</td>
<td>![](https://img.shields.io/badge/-alto-success)</td>
</tr>
<tr>
<td><b>Segurança e autenticação</b></td>
<td>Docs 26–31 (mTLS, CSRF, Threat, PGP, SAML Bearer)</td>
<td>![](https://img.shields.io/badge/-alto-success)</td>
</tr>
<tr>
<td><b>Arquitetura orientada a eventos</b></td>
<td>Docs 32–35 (Event Mesh, AMQP, Competing Consumers)</td>
<td>![](https://img.shields.io/badge/-em%20evolução-yellow)</td>
</tr>
<tr>
<td><b>Monitoramento e operações</b></td>
<td>Transversal (todos os blocos)</td>
<td>![](https://img.shields.io/badge/-alto-success)</td>
</tr>
</table>

---

## 🧩 Matriz de competências técnicas

> Skills exercitadas ao longo do projeto e onde foram efetivamente praticadas.

<table>
<tr><th>Competência</th><th>Onde foi praticada</th><th>Evidência</th></tr>
<tr><td>Content Modifier / Router / Splitter / Aggregator</td><td>Docs 03, 07, 09, 10</td><td><a href="../evidences/evidencesREADME.md">lab01–lab09</a></td></tr>
<tr><td>Message Mapping (JSON ↔ XML)</td><td>Docs 05, 24</td><td><a href="../evidences/evidencesREADME.md">lab03, lab22</a></td></tr>
<tr><td>Groovy Script</td><td>Docs 06, 32–35</td><td><a href="../evidences/evidencesREADME.md">lab04, lab30–lab33</a></td></tr>
<tr><td>Exception / Retry / Dead Letter / Idempotência</td><td>Docs 12–15</td><td><a href="../evidences/evidencesREADME.md">lab10–lab13</a></td></tr>
<tr><td>OData / SOAP / SFTP / JDBC / ProcessDirect</td><td>Docs 16–19</td><td><a href="../evidences/evidencesREADME.md">lab14–lab17</a></td></tr>
<tr><td>API Proxy / Policies / Products / OAuth</td><td>Docs 20–25</td><td><a href="../evidences/evidencesREADME.md">lab18–lab23</a></td></tr>
<tr><td>mTLS / CSRF / Threat Protection / PGP / SAML</td><td>Docs 26–31</td><td><a href="../evidences/evidencesREADME.md">lab24–lab29</a></td></tr>
<tr><td>Event Mesh / AMQP / Guaranteed Messaging / Competing Consumers</td><td>Docs 32–35</td><td><a href="../evidences/evidencesREADME.md">lab30–lab33</a></td></tr>
</table>

---

## Ⓕ Bloco F — Detalhamento de segurança

<table>
<tr><th>Cenário</th><th>Tema</th><th>Status</th><th>Doc</th></tr>
<tr><td>F1</td><td>Basic Authentication</td><td>✅ Coberto</td><td><a href="../docs/20-e-api-management-proxy-basic-auth.md">20</a> · <a href="../docs/24-e6-e7-mes-order-status-backend.md">24</a></td></tr>
<tr><td>F2</td><td>API Key</td><td>✅ Coberto</td><td><a href="../docs/21-e2-verify-api-key.md">21</a></td></tr>
<tr><td>F3</td><td>OAuth 2.0</td><td>✅ Coberto</td><td><a href="../docs/22-e3-oauth-scopes.md">22</a> · <a href="../docs/24-e6-e7-mes-order-status-backend.md">24</a></td></tr>
<tr><td>F4</td><td>Keystore, Client Certificate e mTLS</td><td>✅</td><td><a href="../docs/26-f4-b2b-client-certificate-mtls.md">26</a></td></tr>
<tr><td>F5</td><td>CSRF Token Validation</td><td>✅</td><td><a href="../docs/27-f5-csrf-token-validation.md">27</a></td></tr>
<tr><td>F6</td><td>API Threat Protection</td><td>✅</td><td><a href="../docs/28-f6-api-threat-protection.md">28</a></td></tr>
<tr><td>F7</td><td>PGP Message-Level Security</td><td>✅</td><td><a href="../docs/29-f7-pgp-message-level-security.md">29</a></td></tr>
<tr><td>F8A–F8B</td><td>Auth Context e Technical User SAML Bearer</td><td>✅</td><td><a href="../docs/30-f8-authentication-context-technical-user-saml-bearer.md">30</a></td></tr>
<tr><td>F8E</td><td>End-User SAML Bearer Group-Based Authorization</td><td>✅</td><td><a href="../docs/31-f8e-end-user-saml-bearer-group-based-authorization.md">31</a></td></tr>
</table>

> 🔒 Combinação **mTLS + CSRF** e aprofundamentos de Basic Auth, API Key e OAuth ficam como hardening pós-certificação.

---

## Ⓗ Bloco H — Event-Driven (Event Mesh)

<table>
<tr><th>Cenário</th><th>Tema</th><th>Status</th><th>Doc</th></tr>
<tr><td>H1</td><td>Event Mesh Foundation (Direct e Guaranteed Messaging)</td><td>✅</td><td><a href="../docs/32-h1-solace-pubsub-event-mesh-foundation.md">32</a></td></tr>
<tr><td>H2</td><td>CPI Publisher para Solace via AMQP 1.0</td><td>✅</td><td><a href="../docs/33-h2-sap-cloud-integration-publisher-solace-amqp.md">33</a></td></tr>
<tr><td>H3</td><td>CPI Subscriber do Solace via AMQP (SAP PP/MES)</td><td>✅</td><td><a href="../docs/34-h3-sap-cloud-integration-subscriber-solace-amqp.md">34</a></td></tr>
<tr><td>H4</td><td>Competing Consumers e Escala Horizontal (SAP WM)</td><td>✅</td><td><a href="../docs/35-h4-solace-competing-consumers-scaling.md">35</a></td></tr>
</table>

---

## ✅ Checklist de readiness

- [x] Modelagem de iFlows e padrões EIP
- [x] Mapeamento e transformação de mensagens
- [x] Conectividade multi-protocolo (OData, SOAP, SFTP, JDBC)
- [x] Resiliência (exception, retry, dead letter, idempotência)
- [x] API Management (proxy, policies, products, analytics)
- [x] Segurança (mTLS, CSRF, threat protection, PGP, SAML Bearer)
- [x] Event-Driven (Event Mesh, AMQP, guaranteed messaging, competing consumers)
- [x] Monitoramento e troubleshooting documentado

---

## 📎 Certificado

O certificado oficial será anexado nesta pasta após a aprovação no exame.

<div align="center">

📌 [Voltar ao README principal](../README.md)

</div>
