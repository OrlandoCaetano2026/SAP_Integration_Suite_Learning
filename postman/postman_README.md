## 📮 Postman — SAP Integration Suite Learning

Coleção de testes dos laboratórios do projeto, cobrindo os Blocos A, B, C, D e E. Organizada por pastas, cada request corresponde a um cenário documentado em ../docs/.

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
| `{{apim_base_url}}` | URL base do API Management (ex.: `https://SEU-TENANT-apim.SEU-DATACENTER.hana.ondemand.com/SEU-TENANT`), usada pelos requests do Bloco E |
| `{{consumer_api_key}}` | Consumer Key gerada para um Developer App no Developer Hub, usada no request E2 (Verify API Key) |

**Passo 3 — Selecionar o Environment**
Ative o Environment criado no seletor superior direito do Postman antes de enviar qualquer request.

> 🔒 **Importante:** `{{clientid}}`, `{{clientsecret}}` e `{{consumer_api_key}}` **nunca** devem ser preenchidos com valores reais diretamente no arquivo da collection nem versionados no GitHub — mantenha-os apenas no seu Environment local do Postman (não sincronizado publicamente). A collection já vem configurada para usar exclusivamente variáveis, nunca valores fixos.

**Autenticação:** Basic Auth (`{{clientid}}` / `{{clientsecret}}`) configurada no nível da **coleção** — todos os requests dos Blocos A-D herdam essa autenticação automaticamente. Os requests do Bloco E usam autenticação própria por request (No Auth no E1, header `apikey` no E2), conforme a Policy de segurança configurada em cada Proxy.

### 📋 Requests disponíveis, por bloco

#### Ⓐ Bloco A — CPI Fundamentos

| Request | Payload | Descrição |
|---|---|---|
| A1 - HTTP to Webhook | ../payloads/a1-entrada.json | Primeiro iFlow: HTTP → Content Modifier → Webhook.site |
| A3 - Message Mapping (JSON to XML) | ../payloads/a3-pedido.json | Transformação JSON → XML |
| A4 - Groovy Script | ../payloads/a4-pedido.json | Enriquecimento via script customizado |

💡 O **A2** (Timer → API) não possui request na collection: é disparado automaticamente por Timer, sem gatilho HTTP.

#### Ⓑ Bloco B — Padrões de Integração

| Request | Payload | Descrição |
|---|---|---|
| B1 - Router (ALTO valor) | ../payloads/b1-alto.json | Content-Based Router — rota de alto valor |
| B1 - Router (MEDIO valor) | b1-medio.json | Content-Based Router — rota de médio valor |
| B1 - Router (BAIXO valor) | ../payloads/b1-baixo.json | Content-Based Router — rota de baixo valor |
| B2 - Content Enricher | ../payloads/b2-pedido.json | Enriquecimento via lookup OData V4 |
| B3 - Splitter | b3-lote-itens.json | Divisão de lote em itens individuais |
| B4 - Aggregator | ../payloads/b4-lote-8itens.json | Consolidação de mensagens (CamelSplitComplete) |
| B5 - Multicast (Ordem de Producao) | ../payloads/b5-ordem-producao.json | Distribuição simultânea (MES/PLM/ERP) |

#### Ⓒ Bloco C — Resiliência e Erros

| Request | Payload | Descrição |
|---|---|---|
| C1 - Exception Subprocess (ordem VALIDA) | ../payloads/c1-ordem-valida.json | Fluxo sem erro → 200 |
| C1 - Exception Subprocess (ordem INVALIDA) | ../payloads/c1-ordem-invalida.json | Fluxo com erro tratado → 422 |
| C2 - Retry (Confirmacao de Producao) | ../payloads/c2-confirmacao-producao.json | Retry automático em falhas temporárias (depende do **Mockoon**, simulando `/ok` ou `/falha`) |
| C3 - Dead Letter (Confirmacao MES -> ERP) | ../payloads/c3-confirmacao-producao.json | Producer grava na fila JMS; Consumer entrega ao ERP simulado via **Mockoon + ngrok**. Depende desses serviços externos ativos para reproduzir o dead letter |
| C4 - Data Store (CREATE) | ../payloads/c4-teste1-create.json | Primeiro envio → 201 (registro criado) |
| C4 - Data Store (DUPLICATE) | ../payloads/c4-teste2-duplicado.json.json | Reenvio do mesmo pedido → 409 (duplicidade detectada) |
| C4 - Data Store (UPDATE) | ../payloads/c4-teste3-update.json | Envio com `messageFunction` de atualização → 200 |
| C4B - Idempotent Process Call (CREATE/DUPLICATE) | reutiliza ../payloads/c4-teste1-create.json | Caminho B (best practice SAP) — 1º envio 201, reenvio 409 via `CamelDuplicateMessage` |

#### Ⓓ Bloco D — Conectividade / Adapters

| Request | Payload | Descrição |
|---|---|---|
| D1 - OData (Germany + Sales Rep) | ../payloads/d1-consulta-germany.json | Query OData dinâmica: filtra clientes alemães que são Sales Representative (**Northwind OData V4**, API pública) |
| D1 - OData (France) | ../payloads/d1-consulta-france.json | Filtro simples por país, ordenado por cidade |
| D1 - OData (nome contém Market) | ../payloads/d1-consulta-nome.json | Busca por nome parcial usando `contains()` |
| D2 - SOAP Adapter (Nota Fiscal) | ../payloads/d2-nota-fiscal.json | Split/Gather com chamada a Web Service SOAP externo (**dataaccess.com NumberConversion**, API pública) |
| D3 - SFTP Producer (Ordem de Producao) | ../payloads/d3-ordem-producao.json | Grava arquivo no hot folder (depende do **SFTPCloud**, servidor de teste ativo) |
| D4 - ProcessDirect (Bloqueio Compras) | ../payloads/d4-teste1-bloqueio-compras.json | Fornecedor bloqueado a nível de Organização de Compras (depende do **Neon**, banco PostgreSQL ativo) |
| D4 - ProcessDirect (Bloqueio Qualidade) | ../payloads/d4-teste2-bloqueio-qualidade.json | Fornecedor bloqueado por Quality Info Record — QIR (depende do **Neon**) |
| D4 - ProcessDirect (Pedido Liberado) | ../payloads/d4-teste3-pedido-liberado.json | Caminho feliz: nenhum bloqueio, pedido criado (depende do **Neon**) |

💡 O **D3 — Consumer** (SFTP Sender com polling) não possui request HTTP na collection: ele é disparado automaticamente pelo próprio SAP Integration Suite ao detectar um arquivo novo na pasta monitorada do SFTPCloud, sem gatilho HTTP externo.

#### Ⓔ Bloco E — API Management

| Request | Payload | Descrição |
|---|---|---|
| E1 - API Proxy (D1 OData via API Management) | reutiliza ../payloads/d1-consulta-germany.json | Mesmo teste do D1, agora passando pelo API Proxy `D1_OData_Proxy` — sem autenticação do lado do consumidor, já que o Proxy se autentica sozinho no backend (KVM + Basic Authentication) |
| E2 - Verify API Key (D4 Vendor Validation via API Management) | reutiliza ../payloads/d4-teste2-bloqueio-qualidade.json | Mesmo teste do D4, agora protegido pela Policy Verify API Key — requer preencher `{{consumer_api_key}}` com uma Consumer Key gerada no Developer Hub |

⚠️ **Sobre E3 (OAuth), E4 (Quota) e E5 (Spike Arrest):** esses três cenários **não foram incluídos como requests prontos** na collection, propositalmente. Eles dependem da geração prévia de um **Access Token OAuth 2.0**, que expira automaticamente após ~30 minutos (`expires_in: 1799`) — um request salvo com um token fixo ficaria inutilizado rapidamente, exigindo sempre uma etapa manual de regeneração antes do teste.

Resumo de como esses 3 cenários funcionam e foram executados na prática:
- **E3 (OAuth):** o consumidor faz um POST ao Proxy `E3_OAuth_Token_Server_Proxy` (`?grant_type=client_credentials`, Basic Auth com Consumer Key/Secret) e recebe um `access_token` (Bearer Token) válido por ~30 minutos. Esse token é então usado no header `Authorization: Bearer <token>` para chamar o `D4_VendorValidation_Proxy` e o `E3_VendorOverride_Proxy` — cada um exigindo um Scope diferente (`vendor.read` ou `vendor.write`) conforme o Developer App que gerou o token.
- **E4 (Quota):** o mesmo token OAuth é usado para chamar o Proxy repetidamente; a Policy de Quota lê dinamicamente o limite de chamadas configurado no API Product do consumidor (ex.: 5/minuto no plano Free, 100/minuto no plano Premium) e bloqueia com erro `policies.ratelimit.QuotaViolation` ao ultrapassar o limite.
- **E5 (Spike Arrest):** com o mesmo token, disparando várias chamadas em rápida sucessão (via Postman Collection Runner), a Policy de Spike Arrest bloqueia chamadas que ocorram dentro da mesma janela de tempo mínima configurada, retornando erro `policies.ratelimit.SpikeArrestViolation` — independente do total acumulado de chamadas.

O funcionamento completo, passo a passo, com todos os XMLs de Policy e evidências de teste, está documentado em ../docs/22-e3-oauth-scopes.md e ../docs/23-e4-e5-quota-spike-arrest.md.

### 🌐 Dependências de serviços externos

Alguns cenários exigem que serviços externos estejam **ativos** no momento do teste, pois não fazem parte do tenant SAP:

| Serviço | Usado em | Observação |
|---|---|---|
| **Webhook.site** | A1 | Endpoint de recebimento gerado por sessão; se expirar, gere um novo e atualize o Content Modifier do iFlow |
| **APIs públicas** (JSONPlaceholder, Northwind OData V4, dataaccess.com) | A2, B2, D1, D2 | Serviços públicos, normalmente estáveis, sem necessidade de configuração adicional |
| **Mockoon + ngrok** | C2, C3 | Simulação local de ERP; o Mockoon precisa estar rodando e o túnel ngrok ativo para reproduzir sucesso/falha |
| **SFTPCloud** | D3 | Servidor SFTP de teste gratuito (validade de 7 dias por instância); se expirado, é necessário criar uma nova instância e atualizar as credenciais no Security Material do CPI |
| **Neon** | D4, E1-E5 | Banco de dados PostgreSQL serverless gratuito; a tabela `vendor_block_status` precisa existir e estar populada conforme documentado em ../docs/19-d4-processdirect.md |
| **SAP Developer Hub** | E2, E3, E4 | Portal separado do Integration Suite, necessário para criar Developer Apps e gerar Consumer Keys/Secrets; exige Role Collections específicas atribuídas no BTP Cockpit (ver troubleshooting em ../docs/21-e2-verify-api-key.md) |

📌 Voltar para o ../README.md