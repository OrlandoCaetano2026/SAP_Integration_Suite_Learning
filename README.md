## 🔗 SAP Integration Suite Learning

**🌐 Idioma / Language:** 🇧🇷 **Português** | [🇺🇸 English](README.en.md)

![SAP](https://img.shields.io/badge/SAP-Integration%20Suite-0FAAFF?logo=sap&logoColor=white) ![BTP](https://img.shields.io/badge/SAP-BTP-2570B8?logo=sap&logoColor=white) ![Postman](https://img.shields.io/badge/Testes-Postman-FF6C37?logo=postman&logoColor=white) ![Status](https://img.shields.io/badge/status-em%20desenvolvimento-yellow)

Projeto prático de estudo, desenvolvimento e preparação para a certificação **SAP Integration Suite**. O projeto acompanha a trilha oficial [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite) **e vai além dela**, incluindo cenários complementares muito valorizados no mercado.

O objetivo é ir além da teoria: construir **cenários reais de integração** de ponta a ponta, documentar cada etapa e gerar evidências de execução, formando um portfólio técnico consistente.

### 📑 Índice
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

---

### 🧩 O que é SAP Integration Suite

O **SAP Integration Suite** é a plataforma de integração como serviço (**iPaaS – Integration Platform as a Service**) da SAP, executada no **SAP Business Technology Platform (BTP)**. Ela conecta aplicações, processos, dados e eventos em ambientes **cloud, on-premise e híbridos**, permitindo que sistemas SAP e não-SAP se comuniquem de forma padronizada, segura e escalável.

É a evolução do antigo **SAP Cloud Platform Integration (CPI)** e reúne, em um único ambiente, diversas capabilities de integração, além de recursos de inteligência (assistência por IA) e de aceleração por meio de conteúdo pré-construído.

---

### 🛠️ Principais capabilities

| Capability | Descrição |
|---|---|
| **Cloud Integration (CPI)** | Desenvolvimento de fluxos de integração (Integration Flows / iFlows) entre sistemas |
| **API Management** | Criação, publicação, segurança, governança e monitoramento de APIs |
| **Event Mesh / Advanced Event Mesh** | Integração orientada a eventos (event-driven) |
| **Open Connectors** | Conectores prontos para aplicações de terceiros (SaaS) |
| **Integration Advisor** | Aceleração de integrações B2B/EDI com apoio de IA |
| **Trading Partner Management** | Gestão de parceiros comerciais em cenários B2B |
| **Graph** | Modelo de dados unificado para acesso via API |

---

### 🔄 Cloud Integration (CPI)

O **Cloud Integration**, historicamente chamado de **CPI**, é o coração da plataforma. Nele são desenvolvidos os **Integration Flows (iFlows)** — fluxos que recebem, transformam, roteiam e entregam mensagens entre sistemas.

Principais recursos praticados neste projeto:
- **Adapters** (HTTP, HTTPS, SOAP, OData, SFTP, JDBC, ProcessDirect, JMS, etc.)
- **Content Modifier** (manipulação de headers, properties e body)
- **Message Mapping** (transformação JSON ↔ XML)
- **Groovy Script** (lógica customizada)
- **Router / Splitter / Aggregator / Gather / Multicast** (padrões de integração)
- **Exception Subprocess** (tratamento de erros)
- **Data Store** (persistência temporária)
- **Security Material** (User Credentials, SSH Known Hosts, JDBC Data Sources)
- **Monitoramento** (Message Processing, logs e payloads)

---

### 🌐 API e API Management

Uma **API (Application Programming Interface)** é o contrato que permite que sistemas troquem dados de forma padronizada. Em integrações modernas, o modelo **API-First** é padrão — especialmente no **S/4HANA**, que expõe grande parte de suas funções via **APIs OData e REST**.

O **API Management** é a capability responsável por **expor, proteger e governar** essas APIs. Com ele é possível:
- Criar **API Proxies** que abstraem o backend real
- Aplicar **Policies** de segurança e controle: Verify API Key, OAuth, Quota (limite de chamadas), Spike Arrest (proteção contra picos de tráfego), JSON ↔ XML, Assign Message (manipulação de request/response)
- Agrupar APIs em **API Products** e planos de consumo
- Publicar no **Developer Portal**
- Monitorar uso e performance via **Analytics**

---

### 🧭 Abordagem em duas camadas

Este projeto é organizado em **duas camadas complementares**. A ideia é dominar o núcleo exigido na certificação e, ao mesmo tempo, ir além com cenários que fazem diferença no mercado real.

#### 🥇 Camada 1 — Trilha oficial SAP (núcleo da certificação)

Segue o repertório oficial da trilha [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite), com foco em:
- Cloud Integration (CPI) e Integration Flows
- API Management
- Mapeamentos e transformação de mensagens
- Monitoramento e operações

#### 🥈 Camada 2 — Cenários complementares (diferencial de mercado)

Vai **além da trilha oficial**, cobrindo temas muito valorizados em projetos reais que **não são aprofundados** no repertório oficial da certificação:
- **Event-Driven Integration** com Event Mesh / Advanced Event Mesh
- **B2B / EDI** (pedido de compra, nota fiscal, ASN)
- **OData / API-First** (padrão S/4HANA)
- **Integração híbrida** (cloud + on-premise via Cloud Connector)
- **Conectividade com bancos de dados externos** (JDBC) e **reuso interno de lógica** (ProcessDirect)

> ⚠️ **Importante:** os cenários da Camada 2 são estudados a partir de conteúdos oficiais SAP **específicos de cada tema** (fora da trilha principal). Por exemplo, o Event Mesh tem sua própria jornada de aprendizagem: [Discovering Event-Driven Integration with SAP Integration Suite, advanced event mesh](https://learning.sap.com/courses/discovering-event-driven-integration-with-sap-integration-suite-advanved-event-mesh). Ou seja, o projeto vai propositalmente **além do escopo da prova**, agregando processos e práticas extras.

---

### 🎯 Objetivo do projeto
- Dominar **Cloud Integration (CPI)** e **API Management** na prática
- Aplicar **padrões corporativos de integração** (EIP)
- Implementar **segurança, tratamento de erros e resiliência**
- Explorar **arquitetura orientada a eventos** (Event Mesh)
- Simular **processos SAP MM, PP e QM** em cenários realistas
- Construir um **portfólio técnico documentado** com evidências
- Preparar para a **certificação oficial SAP Integration Suite**

---

### 🌍 Padrões de mercado abordados
- **Event-Driven Integration** (SAP Event Mesh / Advanced Event Mesh) — arquitetura orientada a eventos, tendência forte no mercado.
- **API-First / OData** — padrão de integração do S/4HANA.
- **B2B / EDI** (pedido de compra, nota fiscal, ASN) — muito usado no setor industrial.
- **Integração híbrida** (cloud + on-premise via Cloud Connector) — conceito essencial e recorrente em provas e projetos.
- **Conectividade a banco de dados externo (JDBC)** e **reuso interno de lógica (ProcessDirect)** — padrões comuns em arquiteturas corporativas de médio/grande porte.

---

### 📁 Estrutura do repositório

| Pasta | Descrição |
|---|---|
| `docs/` | Documentação técnica de cada cenário (objetivo, arquitetura, passo a passo e aprendizados) |
| `iflows/` | Integration Flows exportados do Integration Suite (artefatos .zip) |
| `payloads/` | Mensagens de entrada e saída utilizadas nos testes (JSON/XML) |
| `postman/` | Coleções de testes do Postman para envio de mensagens |
| `evidences/` | Evidências de execução: prints do monitoramento, logs e payloads processados |
| `certification/` | Status de preparação e progresso rumo à certificação |
| `simulados/` | Simulados de preparação para a certificação, com correção comentada |

---

### 🧱 Blocos e cenários de prática

🥇 = Camada 1 (trilha oficial) &nbsp;&nbsp; 🥈 = Camada 2 (complementar / além da trilha)

#### Ⓐ Bloco A — CPI Fundamentos 🥇

| # | Cenário | Objetivo | Doc |
|---|---|---|---|
| A1 | HTTP → Content Modifier → Webhook.site | Primeiro iFlow: receber, ajustar e encaminhar mensagem | [ver](docs/03-a1-http-to-webhook.md) |
| A2 | Timer → Request Reply → API pública | Consumir API externa de forma agendada | [ver](docs/04-a2-timer-to-api.md) |
| A3 | Message Mapping (JSON → JSON / JSON → XML) | Transformação de mensagens | [ver](docs/05-a3-message-mapping.md) |
| A4 | Groovy Script para manipulação de payload | Lógica customizada no fluxo | [ver](docs/06-a4-groovy-script.md) |

#### Ⓑ Bloco B — CPI Padrões de Integração 🥇

| # | Cenário | Objetivo | Doc |
|---|---|---|---|
| B1 | Content-Based Router | Rotear mensagens por condição | [ver](docs/07-b1-content-based-router.md) |
| B2 | Content Enricher (Request Reply) | Enriquecer dados a partir de outra fonte | [ver](docs/08-b2-content-enricher.md) |
| B3 | Splitter | Quebrar lote de itens em mensagens individuais | [ver](docs/09-b3-splitter.md) |
| B4 | Aggregator / Gather | Consolidar respostas | [ver](docs/10-b4-aggregator.md) |
| B5 | Multicast | Enviar para múltiplos destinos | [ver](docs/11-b5-multicast.md) |

#### Ⓒ Bloco C — CPI Resiliência e Erros 🥇

| # | Cenário | Objetivo | Doc |
|---|---|---|---|
| C1 | Exception Subprocess | Tratamento padronizado de erros | [ver](docs/12-c1-exception-subprocess.md) |
| C2 | Retry e tratamento de timeout | Resiliência em falhas temporárias | [ver](docs/13-c2-retry-timeout.md) |
| C3 | Dead Letter / reprocessamento (JMS) | Recuperação de mensagens com falha | [ver](docs/14-c3-dead-letter.md) |
| C4 | Data Store & Idempotência (2 abordagens) | Persistência temporária e deduplicação de mensagens | [ver](docs/15-c4-data-store.md) |

#### Ⓓ Bloco D — CPI Conectividade / Adapters 🥇

| # | Cenário | Objetivo | Doc |
|---|---|---|---|
| D1 | OData Adapter | Integração no padrão SAP S/4HANA | [ver](docs/16-d1-odata-adapter.md) |
| D2 | SOAP Adapter | Integração com serviços SOAP externos (Split/Gather) | [ver](docs/17-d2-soap-adapter.md) |
| D3 | SFTP Adapter | Integração de arquivos (hot folder Producer/Consumer) | [ver](docs/18-d3-sftp-adapter.md) |
| D4 | ProcessDirect + JDBC | Chamar um iFlow a partir de outro + conectividade a banco de dados | [ver](docs/19-d4-processdirect.md) |

> 💡 O cenário **D5 — JDBC Adapter**, originalmente planejado separadamente, foi incorporado ao **D4**, que já cobre ProcessDirect + JDBC de forma integrada e realista.

#### Ⓔ Bloco E — API Management 🥇

| # | Cenário | Objetivo | Doc |
|---|---|---|---|
| E0 | Ativação da capability + API Provider | Habilitar API Management no tenant e conectar ao backend real | — |
| E1 | API Proxy apontando para backend | Expor API controlada | — |
| E2 | Policy: Verify API Key | Autenticação básica por chave | — |
| E3 | Policy: OAuth | Autenticação segura | — |
| E4 | Policy: Quota | Limitar número de chamadas | — |
| E5 | Policy: Spike Arrest | Proteção contra picos de tráfego | — |
| E6 | Policy: JSON ↔ XML | Conversão de formatos | — |
| E7 | Policy: Assign Message | Manipular request/response | — |
| E8 | API Products e planos | Agrupar e distribuir APIs | — |
| E9 | Developer Portal | Publicação de API | — |
| E10 | Analytics de API | Monitoramento de uso | — |
| E11 | Policy: Access Control | Controle de acesso por IP (whitelist/blacklist) | — |
| E12 | Policy: Basic Authentication | Autenticação do Proxy junto ao backend real | — |

#### Ⓕ Bloco F — Segurança (transversal) 🥇

| # | Cenário | Objetivo | Doc |
|---|---|---|---|
| F1 | Basic Authentication | Autenticação simples | — |
| F2 | API Key | Controle de acesso por chave | — |
| F3 | OAuth 2.0 | Autorização segura | — |
| F4 | Certificados / Keystore | Segurança baseada em certificados | — |
| F5 | CSRF real | Proteção contra Cross-Site Request Forgery | — |
| F6 | Client Certificate Authentication (mTLS) | Autenticação mútua via certificado | — |

#### Ⓖ Bloco G — Cenários SAP MM / PP / QM 🥈

| # | Cenário | Objetivo | Doc |
|---|---|---|---|
| G1 | SAP MM — Validação de material | Validar movimentação de estoque | — |
| G2 | SAP PP — Ordem de produção | Processar confirmação de produção | — |
| G3 | SAP QM — Inspeção de qualidade | Tratar resultado de inspeção | — |

#### Ⓗ Bloco H — Event-Driven e Cenário final End-to-End 🥈

| # | Cenário | Objetivo | Doc |
|---|---|---|---|
| H1 | Event Mesh — publish/subscribe | Integração orientada a eventos (event-driven) | — |
| H2 | CPI consumindo/publicando eventos no Event Mesh | Conectar Cloud Integration à arquitetura de eventos | — |
| H3 | MES → Integration Suite → Validação MM/PP/QM → Destino → Monitoramento | Integração completa de ponta a ponta | — |

---

### 🧰 Ferramentas utilizadas
- **SAP BTP** (Business Technology Platform)
- **SAP Integration Suite** (Cloud Integration + API Management + Event Mesh)
- **Postman** (envio e teste de mensagens)
- **Webhook.site** (validação de recebimento de mensagens)
- **APIs públicas** (ex.: JSONPlaceholder, dataaccess.com NumberConversion, Northwind OData V4) para simulação de backends
- **SFTPCloud** (servidor SFTP gratuito de teste, usado na integração de arquivos)
- **Neon** (banco de dados PostgreSQL serverless gratuito, usado na conectividade JDBC)
- **Mockoon + ngrok** (simulação local de sistemas externos, como ERP)
- **VS Code + Git** (versionamento e documentação)
- **GitHub** (portfólio e controle de versão)

---

### 🔁 Fluxo de trabalho
1. Desenvolver o iFlow no SAP Integration Suite (navegador)
2. Testar e capturar evidências (prints do monitoramento)
3. Exportar o iFlow (.zip) do Integration Suite
4. Adicionar o artefato em `iflows/` e as evidências em `evidences/`
5. Documentar o cenário em `docs/`
6. Commit e push via VS Code (Source Control)

---

### 📚 Referências oficiais SAP
- 🥇 Trilha principal: [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite)
- 🥈 Event-Driven: [Discovering Event-Driven Integration with SAP Integration Suite, advanced event mesh](https://learning.sap.com/courses/discovering-event-driven-integration-with-sap-integration-suite-advanved-event-mesh)
- 🥈 Tutoriais AEM: [Get Started with SAP Integration Suite, advanced event mesh](https://developers.sap.com/mission.advanced-event-mesh-get-started.html)
- 📖 Visão geral: [SAP Integration Suite — SAP Learning](https://learning.sap.com/products/business-technology-platform/integration-suite)

---

### 👤 Autor / 📬 Contato

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Orlando%20Caetano-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/orlando-caetano/)
[![GitHub](https://img.shields.io/badge/GitHub-OrlandoCaetano2026-181717?logo=github&logoColor=white)](https://github.com/OrlandoCaetano2026)

**Orlando Caetano**
Especialista SAP • Integração • Inteligência Artificial
Consultor SAP MM com know-how em PP, QM e WM

![SAP MM](https://img.shields.io/badge/SAP-MM-0FAAFF?logo=sap&logoColor=white) ![SAP PP](https://img.shields.io/badge/SAP-PP-0FAAFF?logo=sap&logoColor=white) ![SAP QM](https://img.shields.io/badge/SAP-QM-0FAAFF?logo=sap&logoColor=white) ![SAP WM](https://img.shields.io/badge/SAP-WM-0FAAFF?logo=sap&logoColor=white)

> 📌 Projeto de estudo e portfólio. Os cenários SAP MM, PP e QM são simulações educativas para prática de integração.
