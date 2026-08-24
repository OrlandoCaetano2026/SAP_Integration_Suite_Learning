
### 📨 Payloads
  
Mensagens de entrada (JSON) utilizadas nos testes dos laboratórios. Podem ser importadas diretamente no Postman ou usadas como referência.

#### Ⓐ Bloco A — CPI Fundamentos

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Abrir</th>
</tr>
<tr>
<td>a1-entrada.json</td>
<td>A1 — HTTP to Webhook</td>
<td>[ver](./a1-entrada.json)</td>
</tr>
<tr>
<td>a3-pedido.json</td>
<td>A3 — Message Mapping</td>
<td>[ver](./a3-pedido.json)</td>
</tr>
<tr>
<td>a4-pedido.json</td>
<td>A4 — Groovy Script</td>
<td>[ver](./a4-pedido.json)</td>
</tr>
</table>

  
💡 O **A2** (Timer → API) não possui payload de entrada: é disparado por Timer, sem trigger HTTP externo.

#### Ⓑ Bloco B — Padrões de Integração

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Abrir</th>
</tr>
<tr>
<td>b1-alto.json</td>
<td>B1 — Router (rota ALTO)</td>
<td>[ver](./b1-alto.json)</td>
</tr>
<tr>
<td>b1-medio.json</td>
<td>B1 — Router (rota MÉDIO)</td>
<td>[ver](./b1-medio.json)</td>
</tr>
<tr>
<td>b1-baixo.json</td>
<td>B1 — Router (rota BAIXO)</td>
<td>[ver](./b1-baixo.json)</td>
</tr>
<tr>
<td>b2-pedido.json</td>
<td>B2 — Content Enricher</td>
<td>[ver](./b2-pedido.json)</td>
</tr>
<tr>
<td>b3-lote-itens.json</td>
<td>B3 — Splitter (lote com 3 itens)</td>
<td>[ver](./b3-lote-itens.json)</td>
</tr>
<tr>
<td>b4-lote-8itens.json</td>
<td>B4 — Aggregator (lote com 8 itens)</td>
<td>[ver](./b4-lote-8itens.json)</td>
</tr>
<tr>
<td>b5-ordem-producao.json</td>
<td>B5 — Multicast (Ordem de Produção)</td>
<td>[ver](./b5-ordem-producao.json)</td>
</tr>
</table>


#### Ⓒ Bloco C — Resiliência e Erros

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Abrir</th>
</tr>
<tr>
<td>c1-ordem-valida.json</td>
<td>C1 — Exception Subprocess (ordem válida → 200)</td>
<td>[ver](./c1-ordem-valida.json)</td>
</tr>
<tr>
<td>c1-ordem-invalida.json</td>
<td>C1 — Exception Subprocess (ordem inválida → 422)</td>
<td>[ver](./c1-ordem-invalida.json)</td>
</tr>
<tr>
<td>c2-confirmacao-producao.json</td>
<td>C2 — Retry (confirmação de produção)</td>
<td>[ver](./c2-confirmacao-producao.json)</td>
</tr>
<tr>
<td>c3-confirmacao-producao.json</td>
<td>C3 — Dead Letter (confirmação MES → ERP)</td>
<td>[ver](./c3-confirmacao-producao.json)</td>
</tr>
<tr>
<td>c4-teste1-create.json</td>
<td>C4 — Data Store (create → 201)</td>
<td>[ver](./c4-teste1-create.json)</td>
</tr>
<tr>
<td>c4-teste2-duplicado.json.json</td>
<td>C4 — Data Store (duplicado → 409)</td>
<td>[ver](./c4-teste2-duplicado.json.json)</td>
</tr>
<tr>
<td>c4-teste3-update.json</td>
<td>C4 — Data Store (update → 200)</td>
<td>[ver](./c4-teste3-update.json)</td>
</tr>
</table>

  
💡 O **C4B — Idempotent Process Call** (Caminho B da deduplicação) reutiliza a mesma estrutura de payload do c4-teste1-create.json (create) para o teste de duplicidade — não possui um arquivo de payload próprio e distinto no repositório.

#### Ⓓ Bloco D — Conectividade / Adapters

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Abrir</th>
</tr>
<tr>
<td>d1-consulta-germany.json</td>
<td>D1 — OData (Germany + Sales Rep)</td>
<td>[ver](./d1-consulta-germany.json)</td>
</tr>
<tr>
<td>d1-consulta-france.json</td>
<td>D1 — OData (France)</td>
<td>[ver](./d1-consulta-france.json)</td>
</tr>
<tr>
<td>d1-consulta-nome.json</td>
<td>D1 — OData (nome contém "Market")</td>
<td>[ver](./d1-consulta-nome.json)</td>
</tr>
<tr>
<td>d2-nota-fiscal.json</td>
<td>D2 — SOAP Adapter (Split/Gather — Nota Fiscal)</td>
<td>[ver](./d2-nota-fiscal.json)</td>
</tr>
<tr>
<td>d3-ordem-producao.json</td>
<td>D3 — SFTP Adapter (Ordem de Produção SAP → MES)</td>
<td>[ver](./d3-ordem-producao.json)</td>
</tr>
<tr>
<td>d4-teste1-bloqueio-compras.json</td>
<td>D4 — ProcessDirect (fornecedor bloqueado — Compras)</td>
<td>[ver](./d4-teste1-bloqueio-compras.json)</td>
</tr>
<tr>
<td>d4-teste2-bloqueio-qualidade.json</td>
<td>D4 — ProcessDirect (fornecedor bloqueado — Qualidade/QIR)</td>
<td>[ver](./d4-teste2-bloqueio-qualidade.json)</td>
</tr>
<tr>
<td>d4-teste3-pedido-liberado.json</td>
<td>D4 — ProcessDirect (pedido liberado)</td>
<td>[ver](./d4-teste3-pedido-liberado.json)</td>
</tr>
</table>

  
💡 O **D3 — Consumer** (SFTP Sender com polling) não possui payload de entrada próprio: ele lê automaticamente o arquivo gravado pelo Producer (d3-ordem-producao.json) diretamente do servidor SFTP, sem receber requisição HTTP.

#### Ⓔ Bloco E — API Management

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Abrir</th>
</tr>
<tr>
<td>e3-vendor-override.json</td>
<td>E3 — Override de bloqueio de fornecedor (escrita via API, requer token OAuth com scope vendor.write)</td>
<td>[ver](./e3-vendor-override.json)</td>
</tr>
</table>

  
💡 Os cenários E0, E1, E2, E4 e E5 reaproveitam os mesmos payloads já existentes do Bloco D (d1-consulta-\*.json e d4-teste\*.json), já que testam a camada de API Management (Proxy/Policies) sobre os mesmos backends, sem alterar a estrutura do payload de negócio em si.

##### Novos payloads dos Blocos E e F

<table>
<tr>
<th>Arquivo</th>
<th>Cenário</th>
<th>Abrir</th>
</tr>
<tr>
<td>e6-e7-mes-order-status-write.json</td>
<td>E6+E7 — gravação/atualização de status MES</td>
<td>[ver](./e6-e7-mes-order-status-write.json)</td>
</tr>
<tr>
<td>f4-b2b-asn.json</td>
<td>F4 — ASN B2B via mTLS</td>
<td>[ver](./f4-b2b-asn.json)</td>
</tr>
<tr>
<td>f5-mm-purchase-order-change.json</td>
<td>F5 — alteração de pedido SAP MM com CSRF</td>
<td>[ver](./f5-mm-purchase-order-change.json)</td>
</tr>
<tr>
<td>f6-mm-supplier-confirmation-valid.json</td>
<td>F6 — confirmação de fornecedor válida</td>
<td>[ver](./f6-mm-supplier-confirmation-valid.json)</td>
</tr>
</table>


#### Ⓗ Bloco H — Event-Driven (Event Mesh)

Os eventos do Bloco H são publicados via **AMQP 1.0** (Solace PubSub+), utilizando o **Try-Me** do broker e um **OMS Simulator em Node.js** (biblioteca `rhea`). Por serem gerados dinamicamente com identificadores e correlação próprios, os payloads de referência de cada cenário estão documentados diretamente nos respectivos documentos:

<table>
<tr>
<th>Evento</th>
<th>Domínio</th>
<th>Doc</th>
</tr>
<tr>
<td>PurchaseOrderCreated</td>
<td>SAP MM</td>
<td>[H1](../docs/32-h1-solace-pubsub-event-mesh-foundation.md) / [H2](../docs/33-h2-sap-cloud-integration-publisher-solace-amqp.md)</td>
</tr>
<tr>
<td>ProductionOrderConfirmed</td>
<td>SAP PP / MES</td>
<td>[H3](../docs/34-h3-sap-cloud-integration-subscriber-solace-amqp.md)</td>
</tr>
<tr>
<td>WarehousePickingRequested</td>
<td>SAP WM</td>
<td>[H4](../docs/35-h4-solace-competing-consumers-scaling.md)</td>
</tr>
</table>

  
📌 Voltar para o [README principal do projeto](../README.md)
