# 📨 Payloads

Mensagens de entrada (JSON) utilizadas nos testes dos laboratórios. Podem ser importadas diretamente no Postman ou usadas como referência.

## 🅰️ Bloco A — CPI Fundamentos
| Arquivo | Cenário | Abrir |
|---|---|---|
| `a1-entrada.json` | A1 — HTTP to Webhook | [ver](./a1-entrada.json) |
| `a3-pedido.json` | A3 — Message Mapping | [ver](./a3-pedido.json) |
| `a4-pedido.json` | A4 — Groovy Script | [ver](./a4-pedido.json) |

> 💡 O A2 (Timer → API) não possui payload de entrada: é disparado por Timer.

## 🅱️ Bloco B — Padrões de Integração
| Arquivo | Cenário | Abrir |
|---|---|---|
| `b1-alto.json` | B1 — Router (rota ALTO) | [ver](./b1-alto.json) |
| `b1-medio.json` | B1 — Router (rota MÉDIO) | [ver](./b1-medio.json) |
| `b1-baixo.json` | B1 — Router (rota BAIXO) | [ver](./b1-baixo.json) |
| `b2-pedido.json` | B2 — Content Enricher | [ver](./b2-pedido.json) |
| `b3-lote-itens.json` | B3 — Splitter (lote com 3 itens) | [ver](./b3-lote-itens.json) |
| `b4-lote-8itens.json` | B4 — Aggregator (lote com 8 itens) | [ver](./b4-lote-8itens.json) |
| `b5-ordem-producao.json` | B5 — Multicast (Ordem de Produção) | [ver](./b5-ordem-producao.json) |

## 🇨 Bloco C — Resiliência e Erros
| Arquivo | Cenário | Abrir |
|---|---|---|
| `c1-ordem-valida.json` | C1 — Exception (ordem válida → 200) | [ver](./c1-ordem-valida.json) |
| `c1-ordem-invalida.json` | C1 — Exception (ordem inválida → 422) | [ver](./c1-ordem-invalida.json) |
| `c2-confirmacao-producao.json` | C2 — Retry (confirmação de produção) | [ver](./c2-confirmacao-producao.json) |
| `c3-confirmacao-producao.json` | C3 — Dead Letter (confirmação MES → ERP) | [ver](./c3-confirmacao-producao.json) |
| `c4-teste1-create.json` | C4 — Data Store (create → 201) | [ver](./c4-teste1-create.json) |
| `c4-teste2-duplicado.json` | C4 — Data Store (duplicado → 409) | [ver](./c4-teste2-duplicado.json) |
| `c4-teste3-update.json` | C4 — Data Store (update → 200) | [ver](./c4-teste3-update.json) |

## 🇩 Bloco D — Conectividade / Adapters
| Arquivo | Cenário | Abrir |
|---|---|---|
| `d1-consulta-germany.json` | D1 — OData (Germany + Sales Rep) | [ver](./d1-consulta-germany.json) |
| `d1-consulta-france.json` | D1 — OData (France) | [ver](./d1-consulta-france.json) |
| `d1-consulta-nome.json` | D1 — OData (nome contém "Market") | [ver](./d1-consulta-nome.json) |
