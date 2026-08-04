# 📨 Payloads

Mensagens de entrada (JSON) utilizadas nos testes dos laboratórios. Podem ser importadas diretamente no Postman ou usadas como referência.

## 🅰️ Bloco A — CPI Fundamentos
| Arquivo | Cenário |
|---|---|
| [a1-entrada.json](./a1-entrada.json) | A1 — HTTP to Webhook |
| [a3-pedido.json](./a3-pedido.json) | A3 — Message Mapping |
| [a4-pedido.json](./a4-pedido.json) | A4 — Groovy Script |

> 💡 O A2 (Timer → API) não possui payload de entrada: é disparado por Timer.

## 🅱️ Bloco B — Padrões de Integração
| Arquivo | Cenário |
|---|---|
| [b1-alto.json](./b1-alto.json) | B1 — Router (rota ALTO) |
| [b1-medio.json](./b1-medio.json) | B1 — Router (rota MÉDIO) |
| [b1-baixo.json](./b1-baixo.json) | B1 — Router (rota BAIXO) |
| [b2-pedido.json](./b2-pedido.json) | B2 — Content Enricher |
| [b3-lote-itens.json](./b3-lote-itens.json) | B3 — Splitter (lote com 3 itens) |
| [b4-lote-8itens.json](./b4-lote-8itens.json) | B4 — Aggregator (lote com 8 itens) |
| [b5-ordem-producao.json](./b5-ordem-producao.json) | B5 — Multicast (Ordem de Produção) |

## 🇨 Bloco C — Resiliência e Erros
| Arquivo | Cenário |
|---|---|
| [c1-ordem-valida.json](./c1-ordem-valida.json) | C1 — Exception Subprocess (ordem válida → 200) |
| [c1-ordem-invalida.json](./c1-ordem-invalida.json) | C1 — Exception Subprocess (ordem inválida → 422) |
| [c2-confirmacao-producao.json](./c2-confirmacao-producao.json) | C2 — Retry (confirmação de produção) |
