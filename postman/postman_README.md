## 📮 Postman — SAP Integration Suite Learning

Coleção de testes dos laboratórios do projeto, cobrindo os Blocos A, B, C e D. Organizada por pastas, cada request corresponde a um cenário documentado em [docs/](../docs/).

### ⚙️ Como importar e configurar

**Passo 1 — Importar a collection**
No Postman: **File → Import** → selecione `Postman_collection.json`.

**Passo 2 — Criar um Environment**
Crie um novo Environment (ícone de engrenagem no canto superior direito) com as seguintes variáveis:

| Variável | Descrição |
|---|---|
| `{{base_url}}` | URL base do runtime do seu tenant CPI (ex.: `https://SEU-TENANT.it-cpitrial06-rt.cfapps.us10-001.hana.ondemand.com`) |
| `{{clientid}}` | Client ID da Service Key gerada no seu tenant SAP BTP |
| `{{clientsecret}}` | Client Secret da Service Key gerada no seu tenant SAP BTP |

**Passo 3 — Selecionar o Environment**
Ative o Environment criado no seletor superior direito do Postman antes de enviar qualquer request.

> 🔒 **Importante:** `{{clientid}}` e `{{clientsecret}}` **nunca** devem ser preenchidos com valores reais diretamente no arquivo da collection nem versionados no GitHub — mantenha-os apenas no seu Environment local do Postman (não sincronizado publicamente). A collection já vem configurada para usar exclusivamente variáveis, nunca valores fixos.

**Autenticação:** Basic Auth (`{{clientid}}` / `{{clientsecret}}`) configurada no nível da **coleção** — todos os requests herdam essa autenticação automaticamente.

---

### 📋 Requests disponíveis, por bloco

#### Ⓐ Bloco A — CPI Fundamentos

| Request | Payload | Descrição |
|---|---|---|
| A1 - HTTP to Webhook | [a1-entrada.json](../payloads/a1-entrada.json) | Primeiro iFlow: HTTP → Content Modifier → Webhook.site |
| A3 - Message Mapping (JSON to XML) | [a3-pedido.json](../payloads/a3-pedido.json) | Transformação JSON → XML |
| A4 - Groovy Script | [a4-pedido.json](../payloads/a4-pedido.json) | Enriquecimento via script customizado |

> 💡 O **A2** (Timer → API) não possui request na collection: é disparado automaticamente por Timer, sem gatilho HTTP.

#### Ⓑ Bloco B — Padrões de Integração

| Request | Payload | Descrição |
|---|---|---|
| B1 - Router (ALTO valor) | [b1-alto.json](../payloads/b1-alto.json) | Content-Based Router — rota de alto valor |
| B1 - Router (MEDIO valor) | [b1-medio.json](../payloads/b1-medio.json) | Content-Based Router — rota de médio valor |
| B1 - Router (BAIXO valor) | [b1-baixo.json](../payloads/b1-baixo.json) | Content-Based Router — rota de baixo valor |
| B2 - Content Enricher | [b2-pedido.json](../payloads/b2-pedido.json) | Enriquecimento via lookup OData V4 |
| B3 - Splitter | [b3-lote-itens.json](../payloads/b3-lote-itens.json) | Divisão de lote em itens individuais |
| B4 - Aggregator | [b4-lote-8itens.json](../payloads/b4-lote-8itens.json) | Consolidação de mensagens (CamelSplitComplete) |
| B5 - Multicast (Ordem de Producao) | [b5-ordem-producao.json](../payloads/b5-ordem-producao.json) | Distribuição simultânea (MES/PLM/ERP) |

#### Ⓒ Bloco C — Resiliência e Erros

| Request | Payload | Descrição |
|---|---|---|
| C1 - Exception Subprocess (ordem VALIDA) | [c1-ordem-valida.json](../payloads/c1-ordem-valida.json) | Fluxo sem erro → 200 |
| C1 - Exception Subprocess (ordem INVALIDA) | [c1-ordem-invalida.json](../payloads/c1-ordem-invalida.json) | Fluxo com erro tratado → 422 |
| C2 - Retry (Confirmacao de Producao) | [c2-confirmacao-producao.json](../payloads/c2-confirmacao-producao.json) | Retry automático em falhas temporárias (depende do **Mockoon**, simulando `/ok` ou `/falha`) |
| C3 - Dead Letter (Confirmacao MES -> ERP) | [c3-confirmacao-producao.json](../payloads/c3-confirmacao-producao.json) | Producer grava na fila JMS; Consumer entrega ao ERP simulado via **Mockoon + ngrok**. Depende desses serviços externos ativos para reproduzir o dead letter |
| C4 - Data Store (CREATE) | [c4-teste1-create.json](../payloads/c4-teste1-create.json) | Primeiro envio → 201 (registro criado) |
| C4 - Data Store (DUPLICATE) | [c4-teste2-duplicado.json.json](../payloads/c4-teste2-duplicado.json.json) | Reenvio do mesmo pedido → 409 (duplicidade detectada) |
| C4 - Data Store (UPDATE) | [c4-teste3-update.json](../payloads/c4-teste3-update.json) | Envio com `messageFunction` de atualização → 200 |
| C4B - Idempotent Process Call (CREATE/DUPLICATE) | reutiliza [c4-teste1-create.json](../payloads/c4-teste1-create.json) | Caminho B (best practice SAP) — 1º envio 201, reenvio 409 via `CamelDuplicateMessage` |

#### Ⓓ Bloco D — Conectividade / Adapters

| Request | Payload | Descrição |
|---|---|---|
| D1 - OData (Germany + Sales Rep) | [d1-consulta-germany.json](../payloads/d1-consulta-germany.json) | Query OData dinâmica: filtra clientes alemães que são Sales Representative (**Northwind OData V4**, API pública) |
| D1 - OData (France) | [d1-consulta-france.json](../payloads/d1-consulta-france.json) | Filtro simples por país, ordenado por cidade |
| D1 - OData (nome contém Market) | [d1-consulta-nome.json](../payloads/d1-consulta-nome.json) | Busca por nome parcial usando `contains()` |
| D2 - SOAP Adapter (Nota Fiscal) | [d2-nota-fiscal.json](../payloads/d2-nota-fiscal.json) | Split/Gather com chamada a Web Service SOAP externo (**dataaccess.com NumberConversion**, API pública) |
| D3 - SFTP Producer (Ordem de Producao) | [d3-ordem-producao.json](../payloads/d3-ordem-producao.json) | Grava arquivo no hot folder (depende do **SFTPCloud**, servidor de teste ativo) |
| D4 - ProcessDirect (Bloqueio Compras) | [d4-teste1-bloqueio-compras.json](../payloads/d4-teste1-bloqueio-compras.json) | Fornecedor bloqueado a nível de Organização de Compras (depende do **Neon**, banco PostgreSQL ativo) |
| D4 - ProcessDirect (Bloqueio Qualidade) | [d4-teste2-bloqueio-qualidade.json](../payloads/d4-teste2-bloqueio-qualidade.json) | Fornecedor bloqueado por Quality Info Record — QIR (depende do **Neon**) |
| D4 - ProcessDirect (Pedido Liberado) | [d4-teste3-pedido-liberado.json](../payloads/d4-teste3-pedido-liberado.json) | Caminho feliz: nenhum bloqueio, pedido criado (depende do **Neon**) |

> 💡 O **D3 — Consumer** (SFTP Sender com polling) não possui request HTTP na collection: ele é disparado automaticamente pelo próprio SAP Integration Suite ao detectar um arquivo novo na pasta monitorada do SFTPCloud, sem gatilho HTTP externo.

---

### 🌐 Dependências de serviços externos

Alguns cenários exigem que serviços externos estejam **ativos** no momento do teste, pois não fazem parte do tenant SAP:

| Serviço | Usado em | Observação |
|---|---|---|
| **Webhook.site** | A1 | Endpoint de recebimento gerado por sessão; se expirar, gere um novo e atualize o Content Modifier do iFlow |
| **APIs públicas** (JSONPlaceholder, Northwind OData V4, dataaccess.com) | A2, B2, D1, D2 | Serviços públicos, normalmente estáveis, sem necessidade de configuração adicional |
| **Mockoon + ngrok** | C2, C3 | Simulação local de ERP; o Mockoon precisa estar rodando e o túnel ngrok ativo para reproduzir sucesso/falha |
| **SFTPCloud** | D3 | Servidor SFTP de teste gratuito (validade de 7 dias por instância); se expirado, é necessário criar uma nova instância e atualizar as credenciais no Security Material do CPI |
| **Neon** | D4 | Banco de dados PostgreSQL serverless gratuito; a tabela `vendor_block_status` precisa existir e estar populada conforme documentado em [19-d4-processdirect.md](../docs/19-d4-processdirect.md) |

---

📌 Voltar para o [README principal do projeto](../README.md)
