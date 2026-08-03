# 📮 Postman

Coleção de testes do projeto, com as requisições dos laboratórios que possuem sender HTTP.

## 📥 Como importar

```text
Postman → Import → selecione o arquivo:
SAP_Integration_Suite_Learning.postman_collection.json
```

## 🔐 Configuração (variáveis)

Para não expor credenciais, a coleção usa variáveis. Configure-as em um **Environment** do Postman:

| Variável | Valor |
|---|---|
| `base_url` | URL base do runtime (ex.: `https://SEU-TENANT.it-cpitrial06-rt.cfapps.us10-001.hana.ondemand.com`) |
| `clientid` | clientid da Service Key (Process Integration Runtime / plano `integration-flow`) |
| `clientsecret` | clientsecret da Service Key |

> 🔒 **Segurança:** nunca versione `clientid`/`clientsecret` no repositório. Mantenha-os apenas no Environment local do Postman.

## 📋 Requisições incluídas

- **Bloco A:** A1 (HTTP to Webhook), A3 (Message Mapping), A4 (Groovy Script)
- **Bloco B:** B1 (Router: alto/médio/baixo), B2 (Content Enricher), B3 (Splitter)

> 💡 O A2 (Timer → API) não tem requisição: é disparado por Timer, sem sender HTTP.
