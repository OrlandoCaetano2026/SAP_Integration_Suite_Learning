<div align="center">

# 📨 Índice de Payloads de Teste

### Mensagens de entrada e saída — SAP Integration Suite Learning

![Payloads](https://img.shields.io/badge/payloads-JSON%20%2F%20XML-0FAAFF?style=for-the-badge&logo=json&logoColor=white)
![Uso](https://img.shields.io/badge/uso-Postman%20%2F%20referência-success?style=for-the-badge)

</div>

---

> 📨 Mensagens utilizadas nos testes dos laboratórios. Podem ser importadas diretamente no Postman ou usadas como referência para reproduzir cada cenário. Cada payload é vinculado ao documento correspondente.

**🧭 Navegação:** [🏠 Principal](../README.md) · [📚 Documentação](../docs/docsREADME.md) · [📦 iFlows](../iflows/iflows_README.md) · [📮 Postman](../postman/postman_README.md)

---

## Ⓐ Bloco A — CPI Fundamentos

<table>
<tr><th>Arquivo</th><th>Cenário</th><th>Doc</th></tr>
<tr><td><code>a1-entrada.json</code></td><td>A1 · HTTP to Webhook</td><td><a href="../docs/03-a1-http-to-webhook.md">📄</a></td></tr>
<tr><td><code>a3-pedido.json</code></td><td>A3 · Message Mapping</td><td><a href="../docs/05-a3-message-mapping.md">📄</a></td></tr>
<tr><td><code>a4-pedido.json</code></td><td>A4 · Groovy Script</td><td><a href="../docs/06-a4-groovy-script.md">📄</a></td></tr>
</table>

> 💡 O **A2** (Timer → API) não possui payload: é disparado por Timer, sem trigger HTTP.

## Ⓑ Bloco B — Padrões de Integração

<table>
<tr><th>Arquivo</th><th>Cenário</th><th>Doc</th></tr>
<tr><td><code>b1-alto.json</code> · <code>b1-medio.json</code> · <code>b1-baixo.json</code></td><td>B1 · Router (3 rotas)</td><td><a href="../docs/07-b1-content-based-router.md">📄</a></td></tr>
<tr><td><code>b2-pedido.json</code></td><td>B2 · Content Enricher</td><td><a href="../docs/08-b2-content-enricher.md">📄</a></td></tr>
<tr><td><code>b3-lote-itens.json</code></td><td>B3 · Splitter (3 itens)</td><td><a href="../docs/09-b3-splitter.md">📄</a></td></tr>
<tr><td><code>b4-lote-8itens.json</code></td><td>B4 · Aggregator (8 itens)</td><td><a href="../docs/10-b4-aggregator.md">📄</a></td></tr>
<tr><td><code>b5-ordem-producao.json</code></td><td>B5 · Multicast</td><td><a href="../docs/11-b5-multicast.md">📄</a></td></tr>
</table>

## Ⓒ Bloco C — Resiliência e Erros

<table>
<tr><th>Arquivo</th><th>Cenário</th><th>Doc</th></tr>
<tr><td><code>c1-ordem-valida.json</code> · <code>c1-ordem-invalida.json</code></td><td>C1 · Exception (200 / 422)</td><td><a href="../docs/12-c1-exception-subprocess.md">📄</a></td></tr>
<tr><td><code>c2-confirmacao-producao.json</code></td><td>C2 · Retry</td><td><a href="../docs/13-c2-retry-timeout.md">📄</a></td></tr>
<tr><td><code>c3-confirmacao-producao.json</code></td><td>C3 · Dead Letter (MES → ERP)</td><td><a href="../docs/14-c3-dead-letter.md">📄</a></td></tr>
<tr><td><code>c4-teste1-create.json</code> · <code>c4-teste2-duplicado.json.json</code> · <code>c4-teste3-update.json</code></td><td>C4 · Data Store (201 / 409 / 200)</td><td><a href="../docs/15-c4-data-store.md">📄</a></td></tr>
</table>

## Ⓓ Bloco D — Conectividade / Adapters

<table>
<tr><th>Arquivo</th><th>Cenário</th><th>Doc</th></tr>
<tr><td><code>d1-consulta-germany.json</code> · <code>d1-consulta-france.json</code> · <code>d1-consulta-nome.json</code></td><td>D1 · OData (3 consultas)</td><td><a href="../docs/16-d1-odata-adapter.md">📄</a></td></tr>
<tr><td><code>d2-nota-fiscal.json</code></td><td>D2 · SOAP (Split/Gather)</td><td><a href="../docs/17-d2-soap-adapter.md">📄</a></td></tr>
<tr><td><code>d3-ordem-producao.json</code></td><td>D3 · SFTP (SAP → MES)</td><td><a href="../docs/18-d3-sftp-adapter.md">📄</a></td></tr>
<tr><td><code>d4-teste1-bloqueio-compras.json</code> · <code>d4-teste2-bloqueio-qualidade.json</code> · <code>d4-teste3-pedido-liberado.json</code></td><td>D4 · ProcessDirect (3 casos)</td><td><a href="../docs/19-d4-processdirect.md">📄</a></td></tr>
</table>

## Ⓔ / Ⓕ Blocos E e F — API Management e Segurança

<table>
<tr><th>Arquivo</th><th>Cenário</th><th>Doc</th></tr>
<tr><td><code>e3-vendor-override.json</code></td><td>E3 · Override (escrita via API OAuth)</td><td><a href="../docs/22-e3-oauth-scopes.md">📄</a></td></tr>
<tr><td><code>e6-e7-mes-order-status-write.json</code></td><td>E6+E7 · Status MES</td><td><a href="../docs/24-e6-e7-mes-order-status-backend.md">📄</a></td></tr>
<tr><td><code>f4-b2b-asn.json</code></td><td>F4 · ASN B2B via mTLS</td><td><a href="../docs/26-f4-b2b-client-certificate-mtls.md">📄</a></td></tr>
<tr><td><code>f5-mm-purchase-order-change.json</code></td><td>F5 · Alteração de pedido com CSRF</td><td><a href="../docs/27-f5-csrf-token-validation.md">📄</a></td></tr>
<tr><td><code>f6-mm-supplier-confirmation-valid.json</code></td><td>F6 · Confirmação válida</td><td><a href="../docs/28-f6-api-threat-protection.md">📄</a></td></tr>
</table>

## Ⓗ Bloco H — Event-Driven (Event Mesh)

> Os eventos do Bloco H são publicados via **AMQP 1.0** (Solace PubSub+), usando o **Try-Me** do broker e um **OMS Simulator em Node.js** (biblioteca `rhea`). Por serem gerados dinamicamente com identificadores e correlação próprios, os payloads de referência estão nos próprios documentos.

<table>
<tr><th>Evento</th><th>Domínio</th><th>Doc</th></tr>
<tr><td><code>PurchaseOrderCreated</code></td><td>SAP MM</td><td><a href="../docs/32-h1-solace-pubsub-event-mesh-foundation.md">H1</a> · <a href="../docs/33-h2-sap-cloud-integration-publisher-solace-amqp.md">H2</a></td></tr>
<tr><td><code>ProductionOrderConfirmed</code></td><td>SAP PP / MES</td><td><a href="../docs/34-h3-sap-cloud-integration-subscriber-solace-amqp.md">H3</a></td></tr>
<tr><td><code>WarehousePickingRequested</code></td><td>SAP WM</td><td><a href="../docs/35-h4-solace-competing-consumers-scaling.md">H4</a></td></tr>
</table>

<div align="center">

📌 [Voltar ao README principal](../README.md)

</div>
