### 📮 Postman — SAP Integration Suite Learning

As collections foram divididas por bloco para reduzir tamanho, facilitar manutenção e permitir importar apenas o conteúdo necessário.

#### 📦 Collections

| Arquivo | Conteúdo |
|---|---|
| [Postman_Block_A_collection.json](./Postman_Block_A_collection.json) | CPI Fundamentos |
| [Postman_Block_B_collection.json](./Postman_Block_B_collection.json) | Padrões de Integração |
| [Postman_Block_C_collection.json](./Postman_Block_C_collection.json) | Resiliência e Erros |
| [Postman_Block_D_collection.json](./Postman_Block_D_collection.json) | Conectividade e Adapters |
| [Postman_Block_E_collection.json](./Postman_Block_E_collection.json) | API Management |
| [Postman_Block_F_collection.json](./Postman_Block_F_collection.json) | Segurança, mTLS e CSRF |

O arquivo [Postman_collection.json](./Postman_collection.json) foi mantido apenas como índice de compatibilidade e não concentra mais todos os requests.

#### ⚙️ Variáveis principais

| Variável | Uso |
|---|---|
| `{{base_url}}` | Runtime Cloud Integration |
| `{{clientid}}` | Client ID da Service Key |
| `{{clientsecret}}` | Client Secret da Service Key |
| `{{apim_base_url}}` | Base URL do API Management |
| `{{consumer_api_key}}` | Consumer Key do Developer App |
| `{{access_token_manual}}` | Token OAuth manual, quando aplicável |
| `{{mes_legacy_access_token}}` | Token do App legado E6+E7 |
| `{{mes_ops_access_token}}` | Token do App interno E6+E7 |
| `{{f5_csrf_token}}` | Token salvo automaticamente pelo Fetch do F5 |

#### 🔒 Segurança

- Nunca versione Client Secret, tokens, cookies, PFX, PEM ou chaves privadas.
- O certificado do F4 é configurado localmente em Postman Settings → Certificates.
- O F5 depende do Cookie Jar para preservar `JSESSIONID` e cookies da sessão.
- Os valores reais devem permanecer somente no Environment local.

#### 📍 Status

- Collections A até D consolidadas.
- Collection E atualizada com E6+E7 e E10.
- Collection F criada com F4 e F5.
- F6 será acrescentado após a execução do laboratório.

📌 Voltar para o [README principal do projeto](../README.md)
