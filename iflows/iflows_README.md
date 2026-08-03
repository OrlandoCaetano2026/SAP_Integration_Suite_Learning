# 📦 Integration Flows (iFlows)

Esta pasta contém os **Integration Flows exportados** do SAP Integration Suite, no formato `.zip`. Cada arquivo é o artefato real do iFlow, podendo ser **importado e executado** em um tenant do Integration Suite.

---

## 📥 Como exportar um iFlow (para adicionar aqui)

```text
SAP Integration Suite → Design → Integrations and APIs
→ abrir o pacote → selecionar o Integration Flow
→ botão "Download" → gera o arquivo .zip
```

## 📤 Como importar um iFlow (para reutilizar)

```text
SAP Integration Suite → Design → pacote de destino
→ Add → Integration Flow → Upload → selecionar o .zip
```

---

## 📋 iFlows do projeto

| Arquivo | Cenário | Doc |
|---|---|---|
| `A1_HTTP_To_Webhook.zip` | HTTP → Content Modifier → Webhook | [doc](../docs/03-a1-http-to-webhook.md) |
| `A2_Timer_To_API.zip` | Timer → Request Reply → API pública | [doc](../docs/04-a2-timer-to-api.md) |
| `A3_Message_Mapping.zip` | Message Mapping (JSON → XML) | [doc](../docs/05-a3-message-mapping.md) |
| `A4_Groovy_Script.zip` | Groovy Script (enriquecimento) | [doc](../docs/06-a4-groovy-script.md) |
| `B1_Content_Based_Router.zip` | Content-Based Router | [doc](../docs/07-b1-content-based-router.md) |
| `B2_Content_Enricher.zip` | Content Enricher (OData V4) | [doc](../docs/08-b2-content-enricher.md) |
| `B3_Splitter.zip` | Splitter | [doc](../docs/09-b3-splitter.md) |

> 💡 Ao adicionar novos iFlows, mantenha o nome do `.zip` igual ao nome do artefato no Integration Suite.
