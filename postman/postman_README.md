
#### 📮 Postman — SAP Integration Suite Learning
  
As collections foram divididas por bloco para reduzir tamanho, facilitar manutenção e permitir importar apenas o conteúdo necessário.

##### 📦 Collections

<table>
<tr>
<th>Arquivo</th>
<th>Conteúdo</th>
</tr>
<tr>
<td>[Postman\_Block\_A\_collection.json](./Postman_Block_A_collection.json)</td>
<td>CPI Fundamentos</td>
</tr>
<tr>
<td>[Postman\_Block\_B\_collection.json](./Postman_Block_B_collection.json)</td>
<td>Padrões de Integração</td>
</tr>
<tr>
<td>[Postman\_Block\_C\_collection.json](./Postman_Block_C_collection.json)</td>
<td>Resiliência e Erros</td>
</tr>
<tr>
<td>[Postman\_Block\_D\_collection.json](./Postman_Block_D_collection.json)</td>
<td>Conectividade e Adapters</td>
</tr>
<tr>
<td>[Postman\_Block\_E\_collection.json](./Postman_Block_E_collection.json)</td>
<td>API Management</td>
</tr>
<tr>
<td>[Postman\_Block\_F\_collection.json](./Postman_Block_F_collection.json)</td>
<td>Segurança, mTLS e CSRF</td>
</tr>
</table>

  
O arquivo [Postman\_collection.json](./Postman_collection.json) foi mantido apenas como índice de compatibilidade e não concentra mais todos os requests.

##### ⚙️ Variáveis principais

<table>
<tr>
<th>Variável</th>
<th>Uso</th>
</tr>
<tr>
<td>{{base\_url}}</td>
<td>Runtime Cloud Integration</td>
</tr>
<tr>
<td>{{clientid}}</td>
<td>Client ID da Service Key</td>
</tr>
<tr>
<td>{{clientsecret}}</td>
<td>Client Secret da Service Key</td>
</tr>
<tr>
<td>{{apim\_base\_url}}</td>
<td>Base URL do API Management</td>
</tr>
<tr>
<td>{{consumer\_api\_key}}</td>
<td>Consumer Key do Developer App</td>
</tr>
<tr>
<td>{{access\_token\_manual}}</td>
<td>Token OAuth manual, quando aplicável</td>
</tr>
<tr>
<td>{{mes\_legacy\_access\_token}}</td>
<td>Token do App legado E6+E7</td>
</tr>
<tr>
<td>{{mes\_ops\_access\_token}}</td>
<td>Token do App interno E6+E7</td>
</tr>
<tr>
<td>{{f5\_csrf\_token}}</td>
<td>Token salvo automaticamente pelo Fetch do F5</td>
</tr>
</table>


##### 🔒 Segurança
- Nunca versione Client Secret, tokens, cookies, PFX, PEM ou chaves privadas.
- O certificado do F4 é configurado localmente em Postman Settings → Certificates.
- O F5 depende do Cookie Jar para preservar JSESSIONID e cookies da sessão.
- Os valores reais devem permanecer somente no Environment local.

##### 🧩 Bloco H — Event-Driven (Event Mesh)
- Os cenários do Bloco H utilizam o **AMQP 1.0** do Solace PubSub+, e não requests REST/Postman para publicação/consumo de eventos.
- A publicação de eventos foi feita via **Try-Me** do broker e via um **OMS Simulator em Node.js** (biblioteca `rhea`, AMQP 1.0/TLS/SASL).
- O consumo é feito pelos iFlows do Cloud Integration (AMQP Sender Adapter), com entrega a backends externos (Webhook.site, RequestBin e Beeceptor).
- Os payloads de exemplo de cada cenário estão documentados nos próprios documentos 32 a 35.

##### 📍 Status
- Collections A até D consolidadas.
- Collection E atualizada com E6+E7 e E10.
- Collection F com F4 e F5; testes de F6 e F7 documentados nos respectivos docs.  
📌 Voltar para o [README principal do projeto](../README.md)
