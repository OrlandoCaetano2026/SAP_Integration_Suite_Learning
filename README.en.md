# 🔗 SAP Integration Suite Learning

**🌐 Language / Idioma:** [🇧🇷 Português](README.md) | 🇺🇸 **English**

![SAP](https://img.shields.io/badge/SAP-Integration%20Suite-0FAAFF?logo=sap&logoColor=white)
![BTP](https://img.shields.io/badge/SAP-BTP-2570B8?logo=sap&logoColor=white)
![Postman](https://img.shields.io/badge/Testing-Postman-FF6C37?logo=postman&logoColor=white)
![Status](https://img.shields.io/badge/status-in%20development-yellow)

Hands-on project for studying, developing, and preparing for the **SAP Integration Suite** certification. The project follows the official [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite) learning journey **and goes beyond it**, adding complementary scenarios that are highly valued in the market.

The goal goes beyond theory: to build **real end-to-end integration scenarios**, document each step, and generate execution evidences, forming a solid technical portfolio.

---

## 📑 Table of Contents

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
- [Author](#-author)

---

## 🧩 What is SAP Integration Suite

**SAP Integration Suite** is SAP's integration platform as a service (**iPaaS – Integration Platform as a Service**), running on **SAP Business Technology Platform (BTP)**. It connects applications, processes, data, and events across **cloud, on-premise, and hybrid** landscapes, enabling SAP and non-SAP systems to communicate in a standardized, secure, and scalable way.

It is the evolution of the former **SAP Cloud Platform Integration (CPI)** and brings together, in a single environment, several integration capabilities, along with intelligence features (AI assistance) and acceleration through pre-built content.

---

## 🛠️ Main capabilities

| Capability | Description |
|---|---|
| **Cloud Integration (CPI)** | Development of integration flows (iFlows) between systems |
| **API Management** | Creation, publishing, security, governance, and monitoring of APIs |
| **Event Mesh / Advanced Event Mesh** | Event-driven integration |
| **Open Connectors** | Ready-to-use connectors for third-party (SaaS) applications |
| **Integration Advisor** | Acceleration of B2B/EDI integrations with AI support |
| **Trading Partner Management** | Management of business partners in B2B scenarios |
| **Graph** | Unified data model for API access |

---

## 🔄 Cloud Integration (CPI)

**Cloud Integration**, historically known as **CPI**, is the heart of the platform. It is where **Integration Flows (iFlows)** are built — flows that receive, transform, route, and deliver messages between systems.

Key features practiced in this project:

- **Adapters** (HTTP, HTTPS, SOAP, OData, SFTP, ProcessDirect, etc.)
- **Content Modifier** (handling headers, properties, and body)
- **Message Mapping** (JSON ↔ XML transformation)
- **Groovy Script** (custom logic)
- **Router / Splitter / Aggregator / Multicast** (integration patterns)
- **Exception Subprocess** (error handling)
- **Data Store** (temporary persistence)
- **Monitoring** (Message Processing, logs, and payloads)

---

## 🌐 API and API Management

An **API (Application Programming Interface)** is the contract that allows systems to exchange data in a standardized way. In modern integrations, the **API-First** model is the standard — especially in **S/4HANA**, which exposes most of its functions via **OData and REST APIs**.

**API Management** is the capability responsible for **exposing, protecting, and governing** these APIs. With it, you can:

- Create **API Proxies** that abstract the real backend
- Apply security and control **Policies**:
  - Verify API Key
  - OAuth
  - Quota (call limit)
  - Spike Arrest (traffic spike protection)
  - JSON ↔ XML
  - Assign Message (request/response manipulation)
- Group APIs into **API Products** and consumption plans
- Publish to the **Developer Portal**
- Monitor usage and performance via **Analytics**

---

## 🧭 Two-layer approach

This project is organized into **two complementary layers**. The idea is to master the core required for the certification and, at the same time, go beyond it with scenarios that make a difference in the real market.

### 🥇 Layer 1 — Official SAP journey (certification core)

Follows the official repertoire of the [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite) journey, focusing on:

- Cloud Integration (CPI) and Integration Flows
- API Management
- Mappings and message transformation
- Monitoring and operations

### 🥈 Layer 2 — Complementary scenarios (market differentiator)

Goes **beyond the official journey**, covering topics highly valued in real projects that are **not deeply covered** in the certification's official repertoire:

- **Event-Driven Integration** with Event Mesh / Advanced Event Mesh
- **B2B / EDI** (purchase order, invoice, ASN)
- **OData / API-First** (S/4HANA standard)
- **Hybrid integration** (cloud + on-premise via Cloud Connector)

> ⚠️ **Note:** Layer 2 scenarios are studied from **topic-specific** official SAP content (outside the main journey). For example, Event Mesh has its own learning journey: [Discovering Event-Driven Integration with SAP Integration Suite, advanced event mesh](https://learning.sap.com/courses/discovering-event-driven-integration-with-sap-integration-suite-advanved-event-mesh). In other words, the project intentionally goes **beyond the exam scope**, adding extra processes and practices.

---

## 🎯 Project goal

- Master **Cloud Integration (CPI)** and **API Management** in practice
- Apply **enterprise integration patterns** (EIP)
- Implement **security, error handling, and resilience**
- Explore **event-driven architecture** (Event Mesh)
- Simulate **SAP MM, PP, and QM processes** in realistic scenarios
- Build a **documented technical portfolio** with evidences
- Prepare for the **official SAP Integration Suite certification**

---

## 🌍 Market-standard patterns covered

- **Event-Driven Integration** (SAP Event Mesh / Advanced Event Mesh) — event-driven architecture, a strong market trend.
- **API-First / OData** — the S/4HANA integration standard.
- **B2B / EDI** (purchase order, invoice, ASN) — widely used in the industrial sector.
- **Hybrid integration** (cloud + on-premise via Cloud Connector) — an essential and recurring concept in exams and projects.

---

## 📁 Repository structure

| Folder | Description |
|---|---|
| `docs/` | Technical documentation for each scenario (goal, architecture, step-by-step, and learnings) |
| `iflows/` | Integration Flows exported from Integration Suite (`.zip` artifacts) |
| `payloads/` | Input and output messages used in tests (JSON/XML) |
| `postman/` | Postman test collections for sending messages |
| `evidences/` | Execution evidences: monitoring screenshots, logs, and processed payloads |
| `simulados/` | Certification preparation practice exams, with commented answers |

---

## 🧱 Practice blocks and scenarios

> 🥇 = Layer 1 (official journey) &nbsp;|&nbsp; 🥈 = Layer 2 (complementary / beyond the journey)

### 🅰️ Block A — CPI Fundamentals 🥇
| # | Scenario | Goal |
|---|---|---|
| A1 | HTTP → Content Modifier → Webhook.site | First iFlow: receive, adjust, and forward a message |
| A2 | Timer → Request Reply → Public API | Consume an external API on a schedule |
| A3 | Message Mapping (JSON → JSON / JSON → XML) | Message transformation |
| A4 | Groovy Script for payload manipulation | Custom logic in the flow |

### 🅱️ Block B — CPI Integration Patterns 🥇
| # | Scenario | Goal |
|---|---|---|
| B1 | Content-Based Router | Route messages by condition |
| B2 | Content Enricher (Request Reply) | Enrich data from another source |
| B3 | Splitter | Split a batch of items into individual messages |
| B4 | Aggregator / Gather | Consolidate responses |
| B5 | Multicast | Send to multiple destinations |

### 🇨 Block C — CPI Resilience and Errors 🥇
| # | Scenario | Goal |
|---|---|---|
| C1 | Exception Subprocess | Standardized error handling |
| C2 | Retry and timeout handling | Resilience for temporary failures |
| C3 | Dead Letter / reprocessing | Recovery of failed messages |
| C4 | Data Store | Temporary message persistence |

### 🇩 Block D — CPI Connectivity / Adapters 🥇
| # | Scenario | Goal |
|---|---|---|
| D1 | OData Adapter | Integration in the SAP S/4HANA standard |
| D2 | SOAP Adapter | Integration with SOAP services |
| D3 | SFTP Adapter | File-based integration |
| D4 | ProcessDirect | Call an iFlow from another iFlow |

### 🇪 Block E — API Management 🥇
| # | Scenario | Goal |
|---|---|---|
| E1 | API Proxy pointing to backend | Expose a controlled API |
| E2 | Policy: Verify API Key | Basic key-based authentication |
| E3 | Policy: OAuth | Secure authentication |
| E4 | Policy: Quota | Limit the number of calls |
| E5 | Policy: Spike Arrest | Protection against traffic spikes |
| E6 | Policy: JSON ↔ XML | Format conversion |
| E7 | Policy: Assign Message | Manipulate request/response |
| E8 | API Products and plans | Group and distribute APIs |
| E9 | Developer Portal | API publishing |
| E10 | API Analytics | Usage monitoring |

### 🇫 Block F — Security (cross-cutting) 🥇
| # | Scenario | Goal |
|---|---|---|
| F1 | Basic Authentication | Simple authentication |
| F2 | API Key | Key-based access control |
| F3 | OAuth 2.0 | Secure authorization |
| F4 | Certificates / Keystore | Certificate-based security |

### 🇬 Block G — SAP MM / PP / QM Scenarios 🥈
| # | Scenario | Goal |
|---|---|---|
| G1 | SAP MM — Material validation | Validate stock movement |
| G2 | SAP PP — Production order | Process production confirmation |
| G3 | SAP QM — Quality inspection | Handle inspection results |

### 🇭 Block H — Event-Driven and Final End-to-End Scenario 🥈
| # | Scenario | Goal |
|---|---|---|
| H1 | Event Mesh — publish/subscribe | Event-driven integration |
| H2 | CPI consuming/publishing events on Event Mesh | Connect Cloud Integration to the event architecture |
| H3 | MES → Integration Suite → MM/PP/QM validation → Destination → Monitoring | Complete end-to-end integration |

---

## 🧰 Tools used

- **SAP BTP** (Business Technology Platform)
- **SAP Integration Suite** (Cloud Integration + API Management + Event Mesh)
- **Postman** (message sending and testing)
- **Webhook.site** (message reception validation)
- **Public APIs** (e.g., JSONPlaceholder) to simulate backends
- **VS Code + Git** (versioning and documentation)
- **GitHub** (portfolio and version control)

---

## 🔁 Workflow

```text
1. Develop the iFlow in SAP Integration Suite (browser)
2. Test and capture evidences (monitoring screenshots)
3. Export the iFlow (.zip) from Integration Suite
4. Add the artifact to iflows/ and evidences to evidences/
5. Document the scenario in docs/
6. Commit and push via VS Code (Source Control)
```

---

## 📚 Official SAP references

- 🥇 Main journey: [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite)
- 🥈 Event-Driven: [Discovering Event-Driven Integration with SAP Integration Suite, advanced event mesh](https://learning.sap.com/courses/discovering-event-driven-integration-with-sap-integration-suite-advanved-event-mesh)
- 🥈 AEM Tutorials: [Get Started with SAP Integration Suite, advanced event mesh](https://developers.sap.com/mission.advanced-event-mesh-get-started.html)
- 📖 Overview: [SAP Integration Suite — SAP Learning](https://learning.sap.com/products/business-technology-platform/integration-suite)

---

## 👤 Author

**Orlando Caetano**
SAP Specialist • Integration • AI

[![GitHub](https://img.shields.io/badge/GitHub-OrlandoCaetano2026-181717?logo=github)](https://github.com/OrlandoCaetano2026)

---

> 📌 Study and portfolio project. The SAP MM, PP, and QM scenarios are educational simulations for integration practice.
