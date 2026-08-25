<div align="center">

# 🔗 SAP Integration Suite Learning

### Hands-on certification lab & enterprise integration engineering portfolio

**🌐 Language / Idioma:** [🇧🇷 Português](README.md) · 🇺🇸 **English**

<br/>

![SAP Integration Suite](https://img.shields.io/badge/SAP-Integration%20Suite-0FAAFF?style=for-the-badge&logo=sap&logoColor=white)
![SAP BTP](https://img.shields.io/badge/SAP-BTP-1A6CB3?style=for-the-badge&logo=sap&logoColor=white)
![Cloud Integration](https://img.shields.io/badge/Cloud%20Integration-CPI-005A9E?style=for-the-badge&logo=sap&logoColor=white)
![API Management](https://img.shields.io/badge/API-Management-6C2EB9?style=for-the-badge&logo=sap&logoColor=white)
![Event Mesh](https://img.shields.io/badge/Advanced-Event%20Mesh-00C1D4?style=for-the-badge&logo=sap&logoColor=white)

<br/>

![Status](https://img.shields.io/badge/status-in%20development-yellow?style=flat-square)
![Blocks](https://img.shields.io/badge/blocks-8-blue?style=flat-square)
![Documents](https://img.shields.io/badge/technical%20documents-35%2B-success?style=flat-square)
![Labs](https://img.shields.io/badge/labs-33-success?style=flat-square)
![iFlows](https://img.shields.io/badge/iFlows-40%2B-success?style=flat-square)
![Evidences](https://img.shields.io/badge/evidences-300%2B-success?style=flat-square)
![Tools](https://img.shields.io/badge/tools-15%2B-informational?style=flat-square)

<br/>

![Last Commit](https://img.shields.io/github/last-commit/OrlandoCaetano2026/SAP_Integration_Suite_Learning?style=flat-square&color=informational)
![Repo Size](https://img.shields.io/github/repo-size/OrlandoCaetano2026/SAP_Integration_Suite_Learning?style=flat-square)
![Commit Activity](https://img.shields.io/github/commit-activity/m/OrlandoCaetano2026/SAP_Integration_Suite_Learning?style=flat-square)
![Top Language](https://img.shields.io/github/languages/top/OrlandoCaetano2026/SAP_Integration_Suite_Learning?style=flat-square)

</div>

---

> 💡 A hands-on project for studying, building, and **preparing for the SAP Integration Suite certification**. It follows the official [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite) journey **and goes beyond it**, adding market-relevant scenarios with advanced security, messaging, and event-driven architecture.
>
> The goal goes beyond theory: to **build real end-to-end integration scenarios**, document every step with technical storytelling, generate traceable evidences, and build a consistent engineering portfolio.

---

### 🧭 Quick navigation

| 📚 [Documentation](docs/docsREADME.md) | 🎓 [Certification](certification/certification_README.md) | 📸 [Evidences](evidences/evidencesREADME.md) | 📦 [iFlows](iflows/iflows_README.md) | 📨 [Payloads](payloads/payloads_README.md) | 📮 [Postman](postman/postman_README.md) |
|:---:|:---:|:---:|:---:|:---:|:---:|

---

## 🎓 Certification preparation

<div align="center">

![Certification](https://img.shields.io/badge/SAP%20Certified%20Associate-Integration%20Developer-0FAAFF?style=for-the-badge&logo=sap&logoColor=white)

</div>

This lab is structured to cover the **SAP Certified Associate – Integration Developer** exam domains, mapping each area to concrete blocks and documents.

<table>
<tr><th>Exam domain (focus)</th><th>Where it is practiced</th><th>Coverage</th></tr>
<tr><td>Cloud Integration — iFlow modeling</td><td>Blocks A, B, C</td><td>✅ High</td></tr>
<tr><td>Mappings and message transformation</td><td>A3, B2, E6+E7</td><td>✅ High</td></tr>
<tr><td>Connectivity and adapters</td><td>Block D (OData, SOAP, SFTP, JDBC, ProcessDirect)</td><td>✅ High</td></tr>
<tr><td>Error handling and resilience</td><td>Block C</td><td>✅ High</td></tr>
<tr><td>API Management</td><td>Block E</td><td>✅ High</td></tr>
<tr><td>Security (Basic, API Key, OAuth, mTLS, CSRF, PGP, SAML)</td><td>Block F</td><td>✅ High</td></tr>
<tr><td>Event-driven architecture (Event Mesh)</td><td>Block H</td><td>🔄 In progress</td></tr>
<tr><td>Monitoring and operations</td><td>Cross-cutting</td><td>✅ High</td></tr>
</table>

---

## ⚙️ Engineering methodology

```mermaid
flowchart LR
    A[Design\narchitecture and contract] --> B[Build\niFlow / policy / broker]
    B --> C[Test\nPostman · Node.js · Monitor]
    C --> D[Evidence\ntraceable captures]
    D --> E[Document\ntechnical storytelling]
    E --> F[Version\nGit · GitHub]
    F --> A

    classDef step fill:#174a7e,color:#fff,stroke:#65a8e5;
    class A,B,C,D,E,F step;
```

**Quality standards applied to every document:** executive vision and technical storytelling, general and detailed Mermaid architecture, complete self-contained code, contextualized evidences, root-cause troubleshooting, SAP best practices with official links, production recommendations, and clickable navigation between scenarios.

---

## 🛠️ Technology stack

<div align="center">

![Integration Suite](https://img.shields.io/badge/SAP-Integration%20Suite-0FAAFF?style=flat-square&logo=sap&logoColor=white)
![Cloud Integration](https://img.shields.io/badge/Cloud-Integration-005A9E?style=flat-square&logo=sap&logoColor=white)
![API Management](https://img.shields.io/badge/API-Management-6C2EB9?style=flat-square&logo=sap&logoColor=white)
![BTP](https://img.shields.io/badge/SAP-BTP-1A6CB3?style=flat-square&logo=sap&logoColor=white)
![OData](https://img.shields.io/badge/OData-V4-0088CC?style=flat-square)
![SOAP](https://img.shields.io/badge/SOAP-Web%20Service-8E44AD?style=flat-square)
![SFTP](https://img.shields.io/badge/SFTP-File-27AE60?style=flat-square)
![JDBC](https://img.shields.io/badge/JDBC-PostgreSQL-336791?style=flat-square&logo=postgresql&logoColor=white)
![AMQP](https://img.shields.io/badge/AMQP-1.0-FF6600?style=flat-square)
![Solace](https://img.shields.io/badge/Solace-PubSub%2B-00C895?style=flat-square&logo=solace&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-Producer-339933?style=flat-square&logo=node.js&logoColor=white)
![OAuth2](https://img.shields.io/badge/OAuth-2.0-EB5424?style=flat-square&logo=auth0&logoColor=white)
![SAML](https://img.shields.io/badge/SAML-Bearer-1F6FEB?style=flat-square)
![mTLS](https://img.shields.io/badge/mTLS-X.509-C0392B?style=flat-square)
![PGP](https://img.shields.io/badge/PGP-Message%20Level-4B0082?style=flat-square)
![Groovy](https://img.shields.io/badge/Groovy-Script-4298B8?style=flat-square&logo=apachegroovy&logoColor=white)
![Postman](https://img.shields.io/badge/Postman-Testing-FF6C37?style=flat-square&logo=postman&logoColor=white)
![Git](https://img.shields.io/badge/Git-Version%20Control-F05032?style=flat-square&logo=git&logoColor=white)

</div>

---

## 🏛️ Simulated landscape architecture

```mermaid
flowchart TB
    subgraph EXT[External Systems and Tools]
        OMS[OMS / MES Simulators]
        BACK[Webhook.site · RequestBin · Beeceptor · Mockoon]
        DB[(Neon PostgreSQL)]
        SFTP[(SFTP Server)]
    end
    subgraph BTP[SAP Business Technology Platform]
        CPI[Cloud Integration\niFlows]
        APIM[API Management\nProxies · Policies]
    end
    subgraph EDA[Event-Driven Layer]
        SOL[Solace PubSub+\nAMQP 1.0]
    end
    OMS -->|HTTPS / AMQP| CPI
    CPI <-->|OData · SOAP · SFTP · JDBC| EXT
    CPI <-->|AMQP publish/subscribe| SOL
    SOL -->|Competing Consumers| CPI
    APIM -->|governance and security| CPI
    CPI -->|HTTPS| BACK
    CPI <-->|JDBC| DB
    CPI <-->|files| SFTP

    classDef sap fill:#174a7e,color:#fff,stroke:#65a8e5;
    classDef eda fill:#00695c,color:#fff,stroke:#4db6ac;
    classDef ext fill:#5d4037,color:#fff,stroke:#a1887f;
    class CPI,APIM sap;
    class SOL eda;
    class OMS,BACK,DB,SFTP ext;
```

---

## 📊 Project metrics

<div align="center">

| 📦 Blocks | 📚 Documents | 🧪 Labs | 🔀 iFlows | 📸 Evidences | 🧰 Tools |
|:---:|:---:|:---:|:---:|:---:|:---:|
| **8** | **35+** | **33** | **40+** | **300+** | **15+** |

</div>

---

## 🗺️ Roadmap by blocks

<table>
<tr>
<th>Block</th>
<th>Focus</th>
<th>Scenarios</th>
<th>Status</th>
</tr>
<tr>
<td>Ⓐ</td>
<td>CPI Fundamentals</td>
<td>A1–A4</td>
<td>https://img.shields.io/badge/-completed-success?style=flat-square/></td>
</tr>
<tr>
<td>Ⓑ</td>
<td>Integration Patterns</td>
<td>B1–B5</td>
<td>https://img.shields.io/badge/-completed-success?style=flat-square</td>
</tr>
<tr>
<td>Ⓒ</td>
<td>Resilience and Errors</td>
<td>C1–C4</td>
<td>https://img.shields.io/badge/-completed-success?style=flat-square</td>
</tr>
<tr>
<td>Ⓓ</td>
<td>Connectivity / Adapters</td>
<td>D1–D4</td>
<td>https://img.shields.io/badge/-completed-success?style=flat-square</td>
</tr>
<tr>
<td>Ⓔ</td>
<td>API Management</td>
<td>E0–E12</td>
<td>https://img.shields.io/badge/-completed-success?style=flat-square</td>
</tr>
<tr>
<td>Ⓕ</td>
<td>Security</td>
<td>F4–F8</td>
<td>https://img.shields.io/badge/-completed-success?style=flat-square</td>
</tr>
<tr>
<td>Ⓖ</td>
<td>SAP MM / PP / QM</td>
<td>G1–G3</td>
<td>https://img.shields.io/badge/-planned-lightgrey?style=flat-square</td>
</tr>
<tr>
<td>Ⓗ</td>
<td>Event-Driven (Event Mesh)</td>
<td>H1–H4</td>
<td>https://img.shields.io/badge/-in%20progress-yellow?style=flat-square</td>
</tr>
</table>

---

## 🧱 Blocks and scenarios

### Ⓐ Block A — CPI Fundamentals 🥇
<table>
<tr><th>#</th><th>Scenario</th><th>Doc</th></tr>
<tr><td>A1</td><td>HTTP → Content Modifier → Webhook.site</td><td><a href="docs/03-a1-http-to-webhook.md">📄</a></td></tr>
<tr><td>A2</td><td>Timer → Request Reply → Public API</td><td><a href="docs/04-a2-timer-to-api.md">📄</a></td></tr>
<tr><td>A3</td><td>Message Mapping (JSON → XML)</td><td><a href="docs/05-a3-message-mapping.md">📄</a></td></tr>
<tr><td>A4</td><td>Groovy Script</td><td><a href="docs/06-a4-groovy-script.md">📄</a></td></tr>
</table>

### Ⓑ Block B — Integration Patterns 🥇
<table>
<tr><th>#</th><th>Scenario</th><th>Doc</th></tr>
<tr><td>B1</td><td>Content-Based Router</td><td><a href="docs/07-b1-content-based-router.md">📄</a></td></tr>
<tr><td>B2</td><td>Content Enricher</td><td><a href="docs/08-b2-content-enricher.md">📄</a></td></tr>
<tr><td>B3</td><td>Splitter</td><td><a href="docs/09-b3-splitter.md">📄</a></td></tr>
<tr><td>B4</td><td>Aggregator / Gather</td><td><a href="docs/10-b4-aggregator.md">📄</a></td></tr>
<tr><td>B5</td><td>Multicast</td><td><a href="docs/11-b5-multicast.md">📄</a></td></tr>
</table>

### Ⓒ Block C — Resilience and Errors 🥇
<table>
<tr><th>#</th><th>Scenario</th><th>Doc</th></tr>
<tr><td>C1</td><td>Exception Subprocess</td><td><a href="docs/12-c1-exception-subprocess.md">📄</a></td></tr>
<tr><td>C2</td><td>Retry and timeout</td><td><a href="docs/13-c2-retry-timeout.md">📄</a></td></tr>
<tr><td>C3</td><td>Dead Letter (JMS)</td><td><a href="docs/14-c3-dead-letter.md">📄</a></td></tr>
<tr><td>C4</td><td>Data Store & Idempotency</td><td><a href="docs/15-c4-data-store.md">📄</a></td></tr>
</table>

### Ⓓ Block D — Connectivity / Adapters 🥇
<table>
<tr><th>#</th><th>Scenario</th><th>Doc</th></tr>
<tr><td>D1</td><td>OData Adapter</td><td><a href="docs/16-d1-odata-adapter.md">📄</a></td></tr>
<tr><td>D2</td><td>SOAP Adapter</td><td><a href="docs/17-d2-soap-adapter.md">📄</a></td></tr>
<tr><td>D3</td><td>SFTP Adapter</td><td><a href="docs/18-d3-sftp-adapter.md">📄</a></td></tr>
<tr><td>D4</td><td>ProcessDirect + JDBC</td><td><a href="docs/19-d4-processdirect.md">📄</a></td></tr>
</table>

### Ⓔ Block E — API Management 🥇
<table>
<tr><th>#</th><th>Scenario</th><th>Doc</th></tr>
<tr><td>E0/E1/E12</td><td>API Proxy + Basic Authentication</td><td><a href="docs/20-e-api-management-proxy-basic-auth.md">📄</a></td></tr>
<tr><td>E2</td><td>Verify API Key</td><td><a href="docs/21-e2-verify-api-key.md">📄</a></td></tr>
<tr><td>E3</td><td>OAuth 2.0 and Scopes</td><td><a href="docs/22-e3-oauth-scopes.md">📄</a></td></tr>
<tr><td>E4+E5</td><td>Quota and Spike Arrest</td><td><a href="docs/23-e4-e5-quota-spike-arrest.md">📄</a></td></tr>
<tr><td>E6+E7</td><td>JSON → XML and Assign Message</td><td><a href="docs/24-e6-e7-mes-order-status-backend.md">📄</a></td></tr>
<tr><td>E10</td><td>API Analytics</td><td><a href="docs/25-e10-api-analytics.md">📄</a></td></tr>
</table>

### Ⓕ Block F — Security 🥇
<table>
<tr><th>#</th><th>Scenario</th><th>Doc</th></tr>
<tr><td>F4</td><td>Keystore, Client Certificate and mTLS</td><td><a href="docs/26-f4-b2b-client-certificate-mtls.md">📄</a></td></tr>
<tr><td>F5</td><td>Real CSRF</td><td><a href="docs/27-f5-csrf-token-validation.md">📄</a></td></tr>
<tr><td>F6</td><td>API Threat Protection</td><td><a href="docs/28-f6-api-threat-protection.md">📄</a></td></tr>
<tr><td>F7</td><td>PGP Message-Level Security</td><td><a href="docs/29-f7-pgp-message-level-security.md">📄</a></td></tr>
<tr><td>F8A–F8B</td><td>Authentication Context and Technical User SAML Bearer</td><td><a href="docs/30-f8-authentication-context-technical-user-saml-bearer.md">📄</a></td></tr>
<tr><td>F8E</td><td>End-User SAML Bearer Group-Based Authorization</td><td><a href="docs/31-f8e-end-user-saml-bearer-group-based-authorization.md">📄</a></td></tr>
</table>

### Ⓗ Block H — Event-Driven Integration (Event Mesh) 🥈
<table>
<tr><th>#</th><th>Scenario</th><th>Doc</th></tr>
<tr><td>H1</td><td>Solace PubSub+ Event Mesh Foundation</td><td><a href="docs/32-h1-solace-pubsub-event-mesh-foundation.md">📄</a></td></tr>
<tr><td>H2</td><td>CPI Publisher to Solace via AMQP 1.0</td><td><a href="docs/33-h2-sap-cloud-integration-publisher-solace-amqp.md">📄</a></td></tr>
<tr><td>H3</td><td>CPI Subscriber from Solace via AMQP (SAP PP/MES)</td><td><a href="docs/34-h3-sap-cloud-integration-subscriber-solace-amqp.md">📄</a></td></tr>
<tr><td>H4</td><td>Competing Consumers and Horizontal Scaling (SAP WM)</td><td><a href="docs/35-h4-solace-competing-consumers-scaling.md">📄</a></td></tr>
</table>

---

## 📁 Repository structure

```
SAP_Integration_Suite_Learning/
├── docs/            # 📚 Technical documentation (35+ documents)
├── iflows/          # 📦 Exported Integration Flows (.zip)
├── payloads/        # 📨 Input/output messages (JSON/XML)
├── postman/         # 📮 Test collections per block
├── evidences/       # 📸 Execution evidences (labXX)
├── certification/   # 🎓 Certification preparation dashboard
├── README.md        # 🇧🇷 Portuguese version
└── README.en.md     # 🇺🇸 This file
```

---

## 📚 Official SAP references

- 🥇 Main journey: [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite)
- 🥈 Event-Driven: [Discovering Event-Driven Integration with SAP Integration Suite, advanced event mesh](https://learning.sap.com/courses/discovering-event-driven-integration-with-sap-integration-suite-advanved-event-mesh)
- 🥈 AEM Tutorials: [Get Started with SAP Integration Suite, advanced event mesh](https://developers.sap.com/mission.advanced-event-mesh-get-started.html)
- 📖 Overview: [SAP Integration Suite — SAP Learning](https://learning.sap.com/products/business-technology-platform/integration-suite)

---

## 👤 Author / 📇 Contact

<div align="center">

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Orlando%20Caetano-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/orlando-caetano/)
[![GitHub](https://img.shields.io/badge/GitHub-OrlandoCaetano2026-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/OrlandoCaetano2026)

**Orlando Caetano**  
SAP Specialist • Integration • Artificial Intelligence  
SAP MM Consultant with know-how in PP, QM and WM

![SAP MM](https://img.shields.io/badge/SAP-MM-0FAAFF?style=flat-square&logo=sap&logoColor=white)
![SAP PP](https://img.shields.io/badge/SAP-PP-2ECC71?style=flat-square&logo=sap&logoColor=white)
![SAP QM](https://img.shields.io/badge/SAP-QM-E67E22?style=flat-square&logo=sap&logoColor=white)
![SAP WM](https://img.shields.io/badge/SAP-WM-E74C3C?style=flat-square&logo=sap&logoColor=white)

</div>

> 📌 Study and portfolio project. The SAP MM, PP, QM, WM, MES and Event-Driven scenarios are educational simulations for architecture and integration practice.
