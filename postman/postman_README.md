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
| `{{access_token_manual}}` | Access Token OAuth gerado manualmente, usado no request E3 (Vendor Override) |

**Passo 3 — Selecionar o Environment**
Ative o Environment criado no seletor superior direito do Postman antes de enviar qualquer request.

> 🔒 **Importante:** `{{clientid}}`, `{{clientsecret}}`, `{{consumer_api_key}}` e `{{access_token_manual}}` **nunca** devem ser preenchidos com valores reais diretamente no arquivo da collection nem versionados no GitHub — mantenha-os apenas no seu Environment local do Postman (não sincronizado publicamente). A collection já vem configurada para usar exclusivamente variáveis, nunca valores fixos.

**Autenticação:** Basic Auth (`{{clientid}}` / `{{clientsecret}}`) configurada no nível da **coleção** — todos os requests dos Blocos A-D herdam essa autenticação automaticamente. Os requests
