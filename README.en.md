
### 🔗 SAP Integration Suite Learning
  
**🌐 Language / Idioma:** [🇧🇷 Português](README.md)  🇺🇸 **English**  
[SAP](https://img.shields.io/badge/SAP-Integration%20Suite-0FAAFF?logo=sap&logoColor=white) [BTP](https://img.shields.io/badge/SAP-BTP-2570B8?logo=sap&logoColor=white) [Postman](https://img.shields.io/badge/Testing-Postman-FF6C37?logo=postman&logoColor=white) [Status](https://img.shields.io/badge/status-in%20development-yellow)  
Hands-on project for studying, developing, and preparing for the **SAP Integration Suite** certification. The project follows the official [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite) learning journey **and goes beyond it**, adding complementary scenarios that are highly valued in the market.  
The goal goes beyond theory: to build **real end-to-end integration scenarios**, document each step, and generate execution evidences, forming a solid technical portfolio.

#### 📑 Table of Contents
- [What is SAP Integration Suite](#-what-is-sap-integration-suite)
- [Main capabilities](#-main-capabilities)
- [Cloud Integration (CPI)](#-cloud-integration-cpi)
- [API and API Management](#-api-and-api-management)
- [Two-layer approach](#-two-layer-approach)
- [Project goal](#-project-goal)
- [Market-standard patterns covered](#-market-standard-patterns-covered)
- [Repository structure](#-repository-structure)
- [Practice blocks and scenarios](#-practice-blocks-and-scenarios)
- [Tools used](#-tools-used)
- [Workflow](#-workflow)
- [Official SAP references](#-official-sap-references)
- [Author](#-author--contact)

#### 🧩 What is SAP Integration Suite
  
**SAP Integration Suite** is SAP's integration platform as a service (**iPaaS – Integration Platform as a Service**), running on **SAP Business Technology Platform (BTP)**. It connects applications, processes, data, and events across **cloud, on-premise, and hybrid** landscapes, enabling SAP and non-SAP systems to communicate in a standardized, secure, and scalable way.  
It is the evolution of the former **SAP Cloud Platform Integration (CPI)** and brings together, in a single environment, several integration capabilities, along with intelligence features (AI assistance) and acceleration through pre-built content.

#### 🛠️ Main capabilities

<table>
<tr>
<th>Capability</th>
<th>Description</th>
</tr>
<tr>
<td>**Cloud Integration (CPI)**</td>
<td>Development of integration flows (iFlows) between systems</td>
</tr>
<tr>
<td>**API Management**</td>
<td>Creation, publishing, security, governance, and monitoring of APIs</td>
</tr>
<tr>
<td>**Event Mesh / Advanced Event Mesh**</td>
<td>Event-driven integration</td>
</tr>
<tr>
<td>**Open Connectors**</td>
<td>Ready-to-use connectors for third-party (SaaS) applications</td>
</tr>
<tr>
<td>**Integration Advisor**</td>
<td>Acceleration of B2B/EDI integrations with AI support</td>
</tr>
<tr>
<td>**Trading Partner Management**</td>
<td>Management of business partners in B2B scenarios</td>
</tr>
<tr>
<td>**Graph**</td>
<td>Unified data model for API access</td>
</tr>
</table>


#### 🔄 Cloud Integration (CPI)
  
**Cloud Integration**, historically known as **CPI**, is the heart of the platform. It is where **Integration Flows (iFlows)** are built — flows that receive, transform, route, and deliver messages between systems.  
Key features practiced in this project:
- **Adapters** (HTTP, HTTPS, SOAP, OData, SFTP, JDBC, ProcessDirect, JMS, AMQP, etc.)
- **Content Modifier** (handling headers, properties, and body)
- **Message Mapping** (JSON ↔ XML transformation)
- **Groovy Script** (custom logic)
- **Router / Splitter / Aggregator / Gather / Multicast** (integration patterns)
- **Exception Subprocess** (error handling)
- **Data Store** (temporary persistence)
- **Security Material** (User Credentials, SSH Known Hosts, JDBC Data Sources, keys and certificates)
- **Monitoring** (Message Processing, logs, and payloads)

#### 🌐 API and API Management
  
An **API (Application Programming Interface)** is the contract that allows systems to exchange data in a standardized way. In modern integrations, the **API-First** model is the standard — especially in **S/4HANA**, which exposes most of its functions via **OData and REST APIs**.  
**API Management** is the capability responsible for **exposing, protecting, and governing** these APIs. With it, you can:
- Create **API Proxies** that abstract the real backend
- Apply security and control **Policies**: Verify API Key, OAuth, Quota (call limit), Spike Arrest (traffic spike protection), JSON ↔ XML, Assign Message (request/response manipulation)
- Group APIs into **API Products** and consumption plans
- Publish to the **Developer Portal**
- Monitor usage and performance via **Analytics**

#### 🧭 Two-layer approach
  
This project is organized into **two complementary layers**. The idea is to master the core required for the certification and, at the same time, go beyond it with scenarios that make a difference in the real market.

##### 🥇 Layer 1 — Official SAP journey (certification core)
  
Follows the official repertoire of the [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite) journey, focusing on:
- Cloud Integration (CPI) and Integration Flows
- API Management
- Mappings and message transformation
- Monitoring and operations

##### 🥈 Layer 2 — Complementary scenarios (market differentiator)
  
Goes **beyond the official journey**, covering topics highly valued in real projects that are **not deeply covered** in the certification's official repertoire:
- **Event-Driven Integration** with Event Mesh / Advanced Event Mesh
- **B2B / EDI** (purchase order, invoice, ASN)
- **OData / API-First** (S/4HANA standard)
- **Hybrid integration** (cloud + on-premise via Cloud Connector)
- **External database connectivity** (JDBC) and **internal logic reuse** (ProcessDirect)  
⚠️ **Note:** Layer 2 scenarios are studied from **topic-specific** official SAP content (outside the main journey). For example, Event Mesh has its own learning journey: [Discovering Event-Driven Integration with SAP Integration Suite, advanced event mesh](https://learning.sap.com/courses/discovering-event-driven-integration-with-sap-integration-suite-advanved-event-mesh). In other words, the project intentionally goes **beyond the exam scope**, adding extra processes and practices.

#### 🎯 Project goal
- Master **Cloud Integration (CPI)** and **API Management** in practice
- Apply **enterprise integration patterns** (EIP)
- Implement **security, error handling, and resilience**
- Explore **event-driven architecture** (Event Mesh)
- Simulate **SAP MM, PP, QM, and WM processes** in realistic scenarios
- Build a **documented technical portfolio** with evidences
- Prepare for the **official SAP Integration Suite certification**

#### 🌍 Market-standard patterns covered
- **Event-Driven Integration** (SAP Event Mesh / Advanced Event Mesh) — event-driven architecture, a strong market trend.
- **API-First / OData** — the S/4HANA integration standard.
- **B2B / EDI** (purchase order, invoice, ASN) — widely used in the industrial sector.
- **Hybrid integration** (cloud + on-premise via Cloud Connector) — an essential and recurring concept in exams and projects.
- **External database connectivity (JDBC)** and **internal logic reuse (ProcessDirect)** — common patterns in medium/large enterprise architectures.

#### 📁 Repository structure

<table>
<tr>
<th>Folder</th>
<th>Description</th>
</tr>
<tr>
<td>docs/</td>
<td>Technical documentation for each scenario (goal, architecture, step-by-step, and learnings)</td>
</tr>
<tr>
<td>iflows/</td>
<td>Integration Flows exported from Integration Suite (.zip artifacts)</td>
</tr>
<tr>
<td>payloads/</td>
<td>Input and output messages used in tests (JSON/XML)</td>
</tr>
<tr>
<td>postman/</td>
<td>Postman test collections for sending messages</td>
</tr>
<tr>
<td>evidences/</td>
<td>Execution evidences: monitoring screenshots, logs, and processed payloads</td>
</tr>
<tr>
<td>certification/</td>
<td>Certification preparation status and progress</td>
</tr>
</table>


#### 🧱 Practice blocks and scenarios
  
🥇 = Layer 1 (official journey)  🥈 = Layer 2 (complementary / beyond the journey)

##### Ⓐ Block A — CPI Fundamentals 🥇

<table>
<tr>
<th>\#</th>
<th>Scenario</th>
<th>Goal</th>
<th>Doc</th>
</tr>
<tr>
<td>A1</td>
<td>HTTP → Content Modifier → Webhook.site</td>
<td>First iFlow: receive, adjust, and forward a message</td>
<td>[view](docs/03-a1-http-to-webhook.md)</td>
</tr>
<tr>
<td>A2</td>
<td>Timer → Request Reply → Public API</td>
<td>Consume an external API on a schedule</td>
<td>[view](docs/04-a2-timer-to-api.md)</td>
</tr>
<tr>
<td>A3</td>
<td>Message Mapping (JSON → JSON / JSON → XML)</td>
<td>Message transformation</td>
<td>[view](docs/05-a3-message-mapping.md)</td>
</tr>
<tr>
<td>A4</td>
<td>Groovy Script for payload manipulation</td>
<td>Custom logic in the flow</td>
<td>[view](docs/06-a4-groovy-script.md)</td>
</tr>
</table>


##### Ⓑ Block B — CPI Integration Patterns 🥇

<table>
<tr>
<th>\#</th>
<th>Scenario</th>
<th>Goal</th>
<th>Doc</th>
</tr>
<tr>
<td>B1</td>
<td>Content-Based Router</td>
<td>Route messages by condition</td>
<td>[view](docs/07-b1-content-based-router.md)</td>
</tr>
<tr>
<td>B2</td>
<td>Content Enricher (Request Reply)</td>
<td>Enrich data from another source</td>
<td>[view](docs/08-b2-content-enricher.md)</td>
</tr>
<tr>
<td>B3</td>
<td>Splitter</td>
<td>Split a batch of items into individual messages</td>
<td>[view](docs/09-b3-splitter.md)</td>
</tr>
<tr>
<td>B4</td>
<td>Aggregator / Gather</td>
<td>Consolidate responses</td>
<td>[view](docs/10-b4-aggregator.md)</td>
</tr>
<tr>
<td>B5</td>
<td>Multicast</td>
<td>Send to multiple destinations</td>
<td>[view](docs/11-b5-multicast.md)</td>
</tr>
</table>


##### Ⓒ Block C — CPI Resilience and Errors 🥇

<table>
<tr>
<th>\#</th>
<th>Scenario</th>
<th>Goal</th>
<th>Doc</th>
</tr>
<tr>
<td>C1</td>
<td>Exception Subprocess</td>
<td>Standardized error handling</td>
<td>[view](docs/12-c1-exception-subprocess.md)</td>
</tr>
<tr>
<td>C2</td>
<td>Retry and timeout handling</td>
<td>Resilience for temporary failures</td>
<td>[view](docs/13-c2-retry-timeout.md)</td>
</tr>
<tr>
<td>C3</td>
<td>Dead Letter / reprocessing (JMS)</td>
<td>Recovery of failed messages</td>
<td>[view](docs/14-c3-dead-letter.md)</td>
</tr>
<tr>
<td>C4</td>
<td>Data Store & Idempotency (2 approaches)</td>
<td>Temporary persistence and message deduplication</td>
<td>[view](docs/15-c4-data-store.md)</td>
</tr>
</table>


##### Ⓓ Block D — CPI Connectivity / Adapters 🥇

<table>
<tr>
<th>\#</th>
<th>Scenario</th>
<th>Goal</th>
<th>Doc</th>
</tr>
<tr>
<td>D1</td>
<td>OData Adapter</td>
<td>Integration in the SAP S/4HANA standard</td>
<td>[view](docs/16-d1-odata-adapter.md)</td>
</tr>
<tr>
<td>D2</td>
<td>SOAP Adapter</td>
<td>Integration with external SOAP services (Split/Gather)</td>
<td>[view](docs/17-d2-soap-adapter.md)</td>
</tr>
<tr>
<td>D3</td>
<td>SFTP Adapter</td>
<td>File-based integration (hot folder Producer/Consumer)</td>
<td>[view](docs/18-d3-sftp-adapter.md)</td>
</tr>
<tr>
<td>D4</td>
<td>ProcessDirect + JDBC</td>
<td>Call an iFlow from another + database connectivity</td>
<td>[view](docs/19-d4-processdirect.md)</td>
</tr>
</table>

  
💡 The originally planned standalone **D5 — JDBC Adapter** scenario was merged into **D4**, which already covers ProcessDirect + JDBC in an integrated, realistic way.

##### Ⓔ Block E — API Management 🥇

<table>
<tr>
<th>\#</th>
<th>Scenario</th>
<th>Goal</th>
<th>Doc</th>
<th>Status</th>
</tr>
<tr>
<td>E0, E1, E12</td>
<td>Capability, API Proxy and Basic Authentication</td>
<td>Expose a backend with KVM-based technical authentication</td>
<td>[20](docs/20-e-api-management-proxy-basic-auth.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E2</td>
<td>Verify API Key</td>
<td>Control access with Consumer Keys</td>
<td>[21](docs/21-e2-verify-api-key.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E3</td>
<td>OAuth 2.0 and Scopes</td>
<td>Client Credentials, Products, Apps and scope authorization</td>
<td>[22](docs/22-e3-oauth-scopes.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E4+E5</td>
<td>Quota and Spike Arrest</td>
<td>Limit consumption and protect against traffic bursts</td>
<td>[23](docs/23-e4-e5-quota-spike-arrest.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E6+E7</td>
<td>JSON → XML and Assign Message</td>
<td>XML response and scope-based conditional visibility</td>
<td>[24](docs/24-e6-e7-mes-order-status-backend.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E8+E9</td>
<td>Products, Rate Plans, Apps and Developer Hub</td>
<td>Governed distribution and consumption</td>
<td>[21](docs/21-e2-verify-api-key.md), [22](docs/22-e3-oauth-scopes.md), [23](docs/23-e4-e5-quota-spike-arrest.md), [24](docs/24-e6-e7-mes-order-status-backend.md)</td>
<td>✅</td>
</tr>
<tr>
<td>E10</td>
<td>API Analytics</td>
<td>Overview, Health, Usage and Custom View</td>
<td>[25](docs/25-e10-api-analytics.md)</td>
<td>✅</td>
</tr>
</table>

  
✅ **Block E completed on August 16, 2026.**

##### Ⓕ Block F — Security (cross-cutting) 🥇

<table>
<tr>
<th>\#</th>
<th>Scenario</th>
<th>Goal</th>
<th>Doc</th>
<th>Status</th>
</tr>
<tr>
<td>F1</td>
<td>Basic Authentication</td>
<td>Already practiced in Block E; improvements postponed until after certification</td>
<td>[20](docs/20-e-api-management-proxy-basic-auth.md), [24](docs/24-e6-e7-mes-order-status-backend.md)</td>
<td>✅ Covered</td>
</tr>
<tr>
<td>F2</td>
<td>API Key</td>
<td>Already practiced in E2</td>
<td>[21](docs/21-e2-verify-api-key.md)</td>
<td>✅ Covered</td>
</tr>
<tr>
<td>F3</td>
<td>OAuth 2.0</td>
<td>Already practiced in E3 and E6+E7</td>
<td>[22](docs/22-e3-oauth-scopes.md), [24](docs/24-e6-e7-mes-order-status-backend.md)</td>
<td>✅ Covered</td>
</tr>
<tr>
<td>F4</td>
<td>Keystore, Client Certificate and mTLS</td>
<td>Inbound B2B authentication with X.509</td>
<td>[26](docs/26-f4-b2b-client-certificate-mtls.md)</td>
<td>✅</td>
</tr>
<tr>
<td>F5</td>
<td>Real CSRF</td>
<td>Session-bound token and cookies for an SAP MM purchase order change</td>
<td>[27](docs/27-f5-csrf-token-validation.md)</td>
<td>✅</td>
</tr>
<tr>
<td>F6</td>
<td>API Threat Protection</td>
<td>JSON, XML and Regular Expression Protection</td>
<td>[28](docs/28-f6-api-threat-protection.md)</td>
<td>✅</td>
</tr>
<tr>
<td>F7</td>
<td>PGP Message-Level Security</td>
<td>Message encryption, signing, verification and negative tests</td>
<td>[29](docs/29-f7-pgp-message-level-security.md)</td>
<td>✅</td>
</tr>
<tr>
<td>F8A–F8B</td>
<td>Authentication Context and Technical User SAML Bearer</td>
<td>Inbound context, anti-spoofing, RFC 7522, introspection and authorization</td>
<td>[30](docs/30-f8-authentication-context-technical-user-saml-bearer.md)</td>
<td>✅</td>
</tr>
<tr>
<td>F8E</td>
<td>End-User SAML Bearer Group-Based Authorization</td>
<td>Group-based authorization with real Buyer/Manager users in WSO2</td>
<td>[31](docs/31-f8e-end-user-saml-bearer-group-based-authorization.md)</td>
<td>✅</td>
</tr>
</table>

  
🔒 Future post-certification hardening: combine **mTLS + CSRF** and deepen Basic Auth, API Key and OAuth negative tests.

##### Ⓖ Block G — SAP MM / PP / QM Scenarios 🥈

<table>
<tr>
<th>\#</th>
<th>Scenario</th>
<th>Goal</th>
<th>Doc</th>
</tr>
<tr>
<td>G1</td>
<td>SAP MM — Material validation</td>
<td>Validate stock movement</td>
<td>—</td>
</tr>
<tr>
<td>G2</td>
<td>SAP PP — Production order</td>
<td>Process production confirmation</td>
<td>—</td>
</tr>
<tr>
<td>G3</td>
<td>SAP QM — Quality inspection</td>
<td>Handle inspection results</td>
<td>—</td>
</tr>
</table>


##### Ⓗ Block H — Event-Driven Integration (Event Mesh) 🥈

<table>
<tr>
<th>\#</th>
<th>Scenario</th>
<th>Goal</th>
<th>Doc</th>
<th>Status</th>
</tr>
<tr>
<td>H1</td>
<td>Solace PubSub+ Event Mesh Foundation</td>
<td>Broker, topic, durable queue, Direct and Guaranteed Messaging (SAP MM)</td>
<td>[32](docs/32-h1-solace-pubsub-event-mesh-foundation.md)</td>
<td>✅</td>
</tr>
<tr>
<td>H2</td>
<td>CPI Publisher to Solace via AMQP 1.0</td>
<td>Publish business events with TLS/SASL and end-to-end correlation</td>
<td>[33](docs/33-h2-sap-cloud-integration-publisher-solace-amqp.md)</td>
<td>✅</td>
</tr>
<tr>
<td>H3</td>
<td>CPI Subscriber from Solace via AMQP (SAP PP/MES)</td>
<td>Consume a production confirmation backlog and deliver to an external backend</td>
<td>[34](docs/34-h3-sap-cloud-integration-subscriber-solace-amqp.md)</td>
<td>✅</td>
</tr>
<tr>
<td>H4</td>
<td>Competing Consumers and Horizontal Scaling (SAP WM)</td>
<td>Non-Exclusive Queue, multiple workers, failure and dynamic recovery</td>
<td>[35](docs/35-h4-solace-competing-consumers-scaling.md)</td>
<td>✅</td>
</tr>
</table>

  
🔄 Block H in progress: additional scenarios for topic-based routing, resilience (DLQ/replay) and hybrid integration (MQTT/Kafka) planned as a continuation.

#### 🧰 Tools used
- **SAP BTP** (Business Technology Platform)
- **SAP Integration Suite** (Cloud Integration + API Management + Event Mesh)
- **Solace PubSub+ Cloud** (AMQP 1.0/MQTT/REST event broker used in Block H)
- **Node.js + rhea** (OMS Simulator AMQP 1.0 for event publishing)
- **Postman** (message sending and testing)
- **Webhook.site, RequestBin and Beeceptor** (reception validation and external backends)
- **Public APIs** (e.g., JSONPlaceholder, dataaccess.com NumberConversion, Northwind OData V4) to simulate backends
- **SFTPCloud** (free test SFTP server, used for file-based integration)
- **Neon** (free serverless PostgreSQL database, used for JDBC connectivity)
- **Mockoon + ngrok** (local simulation of external systems, such as an ERP)
- **SAP Developer Hub** (portal for managing Applications and Subscriptions in API Management)
- **VS Code + Git** (versioning and documentation)
- **GitHub** (portfolio and version control)

#### 🔁 Workflow
- Develop the iFlow in SAP Integration Suite (browser)
- Test and capture evidences (monitoring screenshots)
- Export the iFlow (.zip) from Integration Suite
- Add the artifact to iflows/ and evidences to evidences/
- Document the scenario in docs/
- Commit and push via VS Code (Source Control)

#### 📚 Official SAP references
- 🥇 Main journey: [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite)
- 🥈 Event-Driven: [Discovering Event-Driven Integration with SAP Integration Suite, advanced event mesh](https://learning.sap.com/courses/discovering-event-driven-integration-with-sap-integration-suite-advanved-event-mesh)
- 🥈 AEM Tutorials: [Get Started with SAP Integration Suite, advanced event mesh](https://developers.sap.com/mission.advanced-event-mesh-get-started.html)
- 📖 Overview: [SAP Integration Suite — SAP Learning](https://learning.sap.com/products/business-technology-platform/integration-suite)

#### 👤 Author / 📬 Contact
  
[LinkedIn](https://www.linkedin.com/in/orlando-caetano/)[GitHub](https://github.com/OrlandoCaetano2026)  
**Orlando Caetano**SAP Specialist • Integration • Artificial Intelligence
SAP MM Consultant with know-how in PP, QM and WM  
[SAP MM](https://img.shields.io/badge/SAP-MM-0FAAFF?logo=sap&logoColor=white) [SAP PP](https://img.shields.io/badge/SAP-PP-0FAAFF?logo=sap&logoColor=white) [SAP QM](https://img.shields.io/badge/SAP-QM-0FAAFF?logo=sap&logoColor=white) [SAP WM](https://img.shields.io/badge/SAP-WM-0FAAFF?logo=sap&logoColor=white)  
📌 Study and portfolio project. The SAP MM, PP, QM, WM, MES and Event-Driven scenarios are educational simulations for integration practice.
