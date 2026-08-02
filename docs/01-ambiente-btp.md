# 🌐 Ambiente SAP BTP

> Documento base do projeto: descreve o ambiente utilizado para o desenvolvimento dos laboratórios práticos com SAP Integration Suite.

---

## 🎯 Objetivo

Registrar a configuração do ambiente **SAP Business Technology Platform (BTP)** onde os Integration Flows (iFlows) deste portfólio são desenvolvidos, testados e monitorados.

---

## 🧩 O que é o SAP BTP

O **SAP Business Technology Platform (BTP)** é a plataforma de negócios e tecnologia da SAP na nuvem. Ela reúne, em um único ambiente, capacidades de:

- **Integração** (SAP Integration Suite)
- **Extensão de aplicações** (desenvolvimento low-code e pro-code)
- **Dados e analytics**
- **Inteligência artificial**

No contexto deste projeto, o BTP é o "guarda-chuva" que hospeda o **SAP Integration Suite**.

---

## 🏗️ Estrutura do ambiente

| Elemento | Descrição |
|---|---|
| **Global Account** | Conta principal que agrupa todas as assinaturas e entitlements |
| **Subaccount** | Ambiente isolado onde o Integration Suite é habilitado |
| **Space (Cloud Foundry)** | Espaço lógico onde ficam as instâncias de serviço (ex.: `dev`) |
| **SAP Integration Suite** | Serviço de integração (iPaaS) usado nos laboratórios |
| **Process Integration Runtime** | Serviço que expõe os endpoints dos iFlows e gerencia a autenticação |

---

## 🛠️ Capabilities habilitadas

Para os laboratórios deste projeto, as seguintes capabilities do Integration Suite são utilizadas:

- ✅ **Cloud Integration (CPI)** — desenvolvimento dos Integration Flows
- ✅ **API Management** — criação, segurança e governança de APIs
- ✅ **Event Mesh** *(cenários complementares — Camada 2)*

---

## 🔐 Autenticação de aplicações (Service Key)

As chamadas externas aos iFlows (por exemplo, via Postman) **não utilizam** o login do portal SAP. É necessária uma credencial técnica gerada a partir do serviço **Process Integration Runtime**:

1. **BTP Cockpit** → *Instances and Subscriptions* → **Create**
2. Service: **Process Integration Runtime** | Plan: **integration-flow**
3. Role: **ESBMessaging.send** | Grant-type: **Client Credentials**
4. Criar a instância e a **Service Key**
5. Utilizar `clientid` (username) e `clientsecret` (password) no cliente HTTP

> 🔒 **Segurança:** `clientid` e `clientsecret` são credenciais sensíveis e **nunca** devem ser versionados no repositório ou expostos em capturas de tela.

---

## ✅ Checklist do ambiente

- [x] Acesso ao SAP BTP confirmado
- [x] Integration Suite ativado
- [x] Capability Cloud Integration disponível
- [x] Capability API Management disponível
- [x] Space Cloud Foundry criado
- [x] Process Integration Runtime configurado
- [x] Service Key criada para autenticação de senders
- [x] Acesso aos menus Design e Monitor confirmado

---

## 📚 Referências

- [SAP Integration Suite — SAP Learning](https://learning.sap.com/products/business-technology-platform/integration-suite)
- [Developing with SAP Integration Suite](https://learning.sap.com/learning-journeys/developing-with-sap-integration-suite)

**Próximo documento:** [02 — Cloud Integration (CPI): conceitos básicos](./02-cloud-integration-basics.md)
