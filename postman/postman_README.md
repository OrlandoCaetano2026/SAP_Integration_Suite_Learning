<div align="center">

# 📮 Collections Postman

### Testes de integração — SAP Integration Suite Learning

![Postman](https://img.shields.io/badge/Postman-Collections-FF6C37?style=for-the-badge&logo=postman&logoColor=white)
![Organização](https://img.shields.io/badge/organização-por%20bloco-success?style=for-the-badge)

</div>

---

> 📮 As collections são divididas por bloco para reduzir tamanho, facilitar manutenção e permitir importar apenas o necessário. Cada collection cobre os requests de teste dos cenários do bloco correspondente.

**🧭 Navegação:** [🏠 Principal](../README.md) · [📚 Documentação](../docs/docsREADME.md) · [📨 Payloads](../payloads/payloads_README.md)

---

## 📦 Collections disponíveis

<table>
<tr><th>Collection</th><th>Conteúdo</th></tr>
<tr><td><a href="./Postman_Block_A_collection.json"><code>Postman_Block_A_collection.json</code></a></td><td>CPI Fundamentos</td></tr>
<tr><td><a href="./Postman_Block_B_collection.json"><code>Postman_Block_B_collection.json</code></a></td><td>Padrões de Integração</td></tr>
<tr><td><a href="./Postman_Block_C_collection.json"><code>Postman_Block_C_collection.json</code></a></td><td>Resiliência e Erros</td></tr>
<tr><td><a href="./Postman_Block_D_collection.json"><code>Postman_Block_D_collection.json</code></a></td><td>Conectividade e Adapters</td></tr>
<tr><td><a href="./Postman_Block_E_collection.json"><code>Postman_Block_E_collection.json</code></a></td><td>API Management</td></tr>
<tr><td><a href="./Postman_Block_F_collection.json"><code>Postman_Block_F_collection.json</code></a></td><td>Segurança, mTLS e CSRF</td></tr>
</table>

> 💡 O arquivo <code>Postman_collection.json</code> foi mantido apenas como índice de compatibilidade.

---

## ⚙️ Variáveis principais

<table>
<tr><th>Variável</th><th>Uso</th></tr>
<tr><td><code>{{base_url}}</code></td><td>Runtime Cloud Integration</td></tr>
<tr><td><code>{{clientid}}</code> · <code>{{clientsecret}}</code></td><td>Credenciais da Service Key</td></tr>
<tr><td><code>{{apim_base_url}}</code></td><td>Base URL do API Management</td></tr>
<tr><td><code>{{consumer_api_key}}</code></td><td>Consumer Key do Developer App</td></tr>
<tr><td><code>{{access_token_manual}}</code></td><td>Token OAuth manual, quando aplicável</td></tr>
<tr><td><code>{{mes_legacy_access_token}}</code> · <code>{{mes_ops_access_token}}</code></td><td>Tokens dos Apps E6+E7</td></tr>
<tr><td><code>{{f5_csrf_token}}</code></td><td>Token salvo automaticamente pelo Fetch do F5</td></tr>
</table>

---

## 🔒 Segurança

- Nunca versione Client Secret, tokens, cookies, PFX, PEM ou chaves privadas.
- O certificado do **F4** é configurado localmente em Postman Settings → Certificates.
- O **F5** depende do Cookie Jar para preservar JSESSIONID e cookies da sessão.
- Valores reais devem permanecer somente no Environment local.

---

## 🧩 Bloco H — Event-Driven (Event Mesh)

Os cenários do Bloco H utilizam **AMQP 1.0** (Solace PubSub+), e não requests REST/Postman para publicação e consumo de eventos:

- Publicação via **Try-Me** do broker e via **OMS Simulator em Node.js** (`rhea`, AMQP 1.0/TLS/SASL).
- Consumo pelos iFlows do Cloud Integration (AMQP Sender Adapter), com entrega a backends externos (Webhook.site, RequestBin e Beeceptor).
- Payloads de exemplo documentados diretamente nos documentos **32 a 38**. Os cenários H5-H7 usam Solace, Node.js/.NET, Mockoon, RequestBin e Mailtrap; por isso não dependem de collection Postman dedicada.

<div align="center">

📌 [Voltar ao README principal](../README.md)

</div>
