
#### 🎓 Certificação — SAP Integration Suite
  
Esta pasta acompanha a preparação para **SAP Certified Associate — Integration Developer**.

##### 📊 Status por bloco

<table>
<tr>
<th>  
Bloco
</th>
<th>  
Período
</th>
<th>  
Status
</th>
<th>  
Observação
</th>
</tr>
<tr>
<td>  
A — CPI Fundamentos
</td>
<td>  
01-03/08/2026
</td>
<td>  
✅ Concluído
</td>
<td>  
A1-A4
</td>
</tr>
<tr>
<td>  
B — Padrões de Integração
</td>
<td>  
03-04/08/2026
</td>
<td>  
✅ Concluído
</td>
<td>  
B1-B5
</td>
</tr>
<tr>
<td>  
C — Resiliência e Erros
</td>
<td>  
04-06/08/2026
</td>
<td>  
✅ Concluído
</td>
<td>  
C1-C4
</td>
</tr>
<tr>
<td>  
D — Conectividade / Adapters
</td>
<td>  
06-10/08/2026
</td>
<td>  
✅ Concluído
</td>
<td>  
D1-D4; JDBC incorporado ao D4
</td>
</tr>
<tr>
<td>  
E — API Management
</td>
<td>  
11-16/08/2026
</td>
<td>  
✅ Concluído
</td>
<td>  
E0-E10, E12 e recursos E8/E9 cobertos nos laboratórios
</td>
</tr>
<tr>
<td>  
F — Segurança
</td>
<td>  
16-24/08/2026
</td>
<td>  
✅ Concluído
</td>
<td>  
F4 a F7 concluídos; F8A/F8B no Doc 30 e F8E no Doc 31
</td>
</tr>
<tr>
<td>  
G — SAP MM / PP / QM
</td>
<td>  
—
</td>
<td>  
⏳ Planejado
</td>
<td>  
Cenários complementares
</td>
</tr>
<tr>
<td>  
H — Event-Driven e E2E
</td>
<td>  
24/08/2026
</td>
<td>  
🔄 Em andamento
</td>
<td>  
Event Mesh: H1 a H4 concluídos (Docs 32 a 35)
</td>
</tr>
</table>


##### Ⓕ Bloco F — Roadmap aprovado

<table>
<tr>
<th>  
Cenário
</th>
<th>  
Tema
</th>
<th>  
Status
</th>
</tr>
<tr>
<td>  
F1
</td>
<td>  
Basic Authentication
</td>
<td>  
✅ Coberto nos docs 20 e 24
</td>
</tr>
<tr>
<td>  
F2
</td>
<td>  
API Key
</td>
<td>  
✅ Coberto no doc 21
</td>
</tr>
<tr>
<td>  
F3
</td>
<td>  
OAuth 2.0
</td>
<td>  
✅ Coberto nos docs 22 e 24
</td>
</tr>
<tr>
<td>  
F4
</td>
<td>  
Keystore, Client Certificate e mTLS
</td>
<td>  
✅ [Doc 26](../docs/26-f4-b2b-client-certificate-mtls.md)
</td>
</tr>
<tr>
<td>  
F5
</td>
<td>  
CSRF Token Validation
</td>
<td>  
✅ [Doc 27](../docs/27-f5-csrf-token-validation.md)
</td>
</tr>
<tr>
<td>  
F6
</td>
<td>  
API Threat Protection
</td>
<td>  
✅ [Doc 28](../docs/28-f6-api-threat-protection.md)
</td>
</tr>
<tr>
<td>  
F7
</td>
<td>  
PGP Message-Level Security
</td>
<td>  
✅ [Doc 29](../docs/29-f7-pgp-message-level-security.md)
</td>
</tr>
<tr>
<td>  
F8A–F8B
</td>
<td>  
Authentication Context e Technical User SAML Bearer
</td>
<td>  
✅ [Doc 30](../docs/30-f8-authentication-context-technical-user-saml-bearer.md)
</td>
</tr>
<tr>
<td>  
F8E
</td>
<td>  
End-User SAML Bearer Group-Based Authorization
</td>
<td>  
✅ [Doc 31](../docs/31-f8e-end-user-saml-bearer-group-based-authorization.md)
</td>
</tr>
</table>

  
Melhorias adicionais de Basic Auth, API Key e OAuth e o cenário combinado mTLS + CSRF ficam para hardening pós-certificação.

##### Ⓗ Bloco H — Event-Driven (Event Mesh)

<table>
<tr>
<th>  
Cenário
</th>
<th>  
Tema
</th>
<th>  
Status
</th>
</tr>
<tr>
<td>  
H1
</td>
<td>  
Solace PubSub+ Event Mesh Foundation (Direct e Guaranteed Messaging)
</td>
<td>  
✅ [Doc 32](../docs/32-h1-solace-pubsub-event-mesh-foundation.md)
</td>
</tr>
<tr>
<td>  
H2
</td>
<td>  
CPI Publisher para Solace via AMQP 1.0
</td>
<td>  
✅ [Doc 33](../docs/33-h2-sap-cloud-integration-publisher-solace-amqp.md)
</td>
</tr>
<tr>
<td>  
H3
</td>
<td>  
CPI Subscriber do Solace via AMQP (SAP PP/MES)
</td>
<td>  
✅ [Doc 34](../docs/34-h3-sap-cloud-integration-subscriber-solace-amqp.md)
</td>
</tr>
<tr>
<td>  
H4
</td>
<td>  
Competing Consumers e Escala Horizontal (SAP WM)
</td>
<td>  
✅ [Doc 35](../docs/35-h4-solace-competing-consumers-scaling.md)
</td>
</tr>
</table>


##### 🎯 Progresso geral
- **Blocos concluídos:** A, B, C, D, E e F.
- **Bloco atual:** H — Event-Driven Integration (Event Mesh).
- **Laboratórios de segurança concluídos:** F4, F5, F6, F7, F8A, F8B e F8E.
- **Laboratórios de Event Mesh concluídos:** H1, H2, H3 e H4.

##### 📎 Certificado
  
O certificado oficial será anexado após a aprovação no exame.  
📌 Voltar para o [README principal do projeto](../README.md)
