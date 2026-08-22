
## F8A–F8C — Authentication Context, Technical User SAML Bearer and End-User Principal Propagation Investigation
  
**Bloco F — Segurança \| Documento 30Escopo:** F8A Authentication Context Capture, F8B Technical User SAML Bearer (token exchange, introspecção e autorização de recurso protegido) e F8C End-User Principal Propagation, este último documentado como investigação arquitetural e troubleshooting, com login humano real validado e propagação de identidade até o runtime gerenciado do CPI não concluída por limites de arquitetura no ambiente trial.  
[← Bloco anterior: F7 — PGP Message-Level Security](./29-f7-pgp-message-level-security.md)  [Próximo cenário: F8D — SAML Web SSO Federation](./32-f8-saml-web-sso-federation.md)

### 1. Visão executiva
  
Este laboratório implementa uma cadeia completa de segurança e autorização baseada em identidade técnica entre o SAP Integration Suite e o WSO2 Identity Server e, na sequência, investiga a propagação de identidade de um usuário humano real até o Cloud Integration. O cenário parte de uma constatação importante: uma chamada inbound autenticada por OAuth Client Credentials comprova a identidade da aplicação cliente, mas não disponibiliza automaticamente um usuário humano confiável ao Integration Flow.  
O laboratório foi dividido em três partes complementares:
- **F8A — Authentication Context Capture:** classifica o contexto inbound, comprova a ausência de um principal humano no fluxo Client Credentials e rejeita tentativas de identity spoofing por headers HTTP.
- **F8B — Technical User OAuth 2.0 SAML Bearer:** cria uma assertion SAML 2.0 de curta duração, assina o XML com uma private key protegida no Keystore do SAP Integration Suite, troca a assertion por um access token OAuth no WSO2, introspecta o token e aplica autorização funcional a um recurso protegido de SAP MM.
- **F8C — End-User Principal Propagation (Investigação):** provisiona SAP Cloud Identity Services (IAS), Application Router, XSUAA e Destination Service para autenticar um usuário humano real e tentar propagar essa identidade até o Cloud Integration. O login interativo foi validado de ponta a ponta, mas a propagação end-user até o runtime gerenciado do CPI encontrou um boundary arquitetural documentado neste mesmo documento, sem geração de evidências.  
O resultado final comprova quatro controles distintos em F8A/F8B e mapeia, em F8C, um limite arquitetural relevante:
- **Autenticação do caller:** o endpoint inbound foi acessado por uma aplicação autorizada.
- **Confiança federada:** o WSO2 confiou no certificado público associado à chave privada do SAP Integration Suite.
- **Token exchange:** a assertion SAML assinada foi aceita como authorization grant conforme o perfil OAuth 2.0 SAML Bearer.
- **Autorização funcional:** o principal técnico foi autorizado a consultar requisições de compra, impedido de aprová-las e bloqueado ao solicitar uma operação não suportada.
- **Propagação end-user (F8C):** o login humano real foi validado, porém a reutilização direta do JWT do usuário para o runtime gerenciado do CPI não foi aceita, evidenciando a necessidade de trust/cross-consumption dedicado entre o XSUAA controlado pela aplicação e o serviço gerenciado.

### 2. Perfil técnico do cenário

<table>
<tr>
<th>  
Item
</th>
<th>  
Valor
</th>
</tr>
<tr>
<td>  
Integration Flow F8A
</td>
<td>  
F8A\_MM\_Authenticated\_Principal\_Capture
</td>
</tr>
<tr>
<td>  
Integration Flow F8B
</td>
<td>  
F8B\_MM\_SAML\_Bearer\_Technical\_User\_Token
</td>
</tr>
<tr>
<td>  
Endpoint F8A
</td>
<td>  
/f8a/mm/authenticated-principal
</td>
</tr>
<tr>
<td>  
Endpoint F8B
</td>
<td>  
/f8b/mm/saml-bearer/technical-user/token
</td>
</tr>
<tr>
<td>  
Authorization Server
</td>
<td>  
WSO2 Identity Server 7.0.0
</td>
</tr>
<tr>
<td>  
Java Runtime
</td>
<td>  
Eclipse Temurin JDK 17
</td>
</tr>
<tr>
<td>  
OAuth Grant
</td>
<td>  
urn:ietf:params:oauth:grant-type:saml2-bearer
</td>
</tr>
<tr>
<td>  
Introspection
</td>
<td>  
OAuth 2.0 Token Introspection
</td>
</tr>
<tr>
<td>  
Technical Principal
</td>
<td>  
f8.technical.purchasing.user
</td>
</tr>
<tr>
<td>  
Buyers Group
</td>
<td>  
F8\_PURCHASING\_BUYERS
</td>
</tr>
<tr>
<td>  
Managers Group
</td>
<td>  
F8\_PURCHASING\_MANAGERS
</td>
</tr>
<tr>
<td>  
Key Pair Alias
</td>
<td>  
f8\_saml\_bearer\_signing
</td>
</tr>
<tr>
<td>  
Key Type
</td>
<td>  
RSA
</td>
</tr>
<tr>
<td>  
Key Size
</td>
<td>  
3072 bits
</td>
</tr>
<tr>
<td>  
Certificate Signature Algorithm
</td>
<td>  
SHA512withRSA
</td>
</tr>
<tr>
<td>  
XML Signature Algorithm
</td>
<td>  
SHA512/RSA
</td>
</tr>
<tr>
<td>  
XML Digest Algorithm
</td>
<td>  
SHA256
</td>
</tr>
<tr>
<td>  
Token Endpoint
</td>
<td>  
https://\<ngrok-domain\>/oauth2/token
</td>
</tr>
<tr>
<td>  
Introspection Endpoint
</td>
<td>  
https://\<ngrok-domain\>/oauth2/introspect
</td>
</tr>
<tr>
<td>  
Protected Resource
</td>
<td>  
SAP\_MM\_PURCHASE\_REQUISITIONS
</td>
</tr>
<tr>
<td>  
Supported Operations
</td>
<td>  
READ, APPROVE
</td>
</tr>
</table>

  
O domínio ngrok utilizado no laboratório é temporário. Em produção, deve ser substituído por endpoint estável, certificado público confiável, controle de rede e disponibilidade compatível com o SLA da integração.

#### 2.1 Perfil técnico complementar do F8C

<table>
<tr>
<th>  
Item
</th>
<th>  
Valor
</th>
</tr>
<tr>
<td>  
Identity Provider
</td>
<td>  
SAP Cloud Identity Services (IAS) — tenant trial
</td>
</tr>
<tr>
<td>  
BTP Subaccount
</td>
<td>  
Subaccount trial Cloud Foundry (região us10)
</td>
</tr>
<tr>
<td>  
Application Router
</td>
<td>  
f8c-approuter (@sap/approuter 22.x)
</td>
</tr>
<tr>
<td>  
XSUAA Service Instance
</td>
<td>  
f8c-xsuaa
</td>
</tr>
<tr>
<td>  
Destination Service Instance
</td>
<td>  
f8c-destination-service
</td>
</tr>
<tr>
<td>  
Destination lógica
</td>
<td>  
f8c-cpi-destination
</td>
</tr>
<tr>
<td>  
Route pública
</td>
<td>  
f8c-approuter.cfapps.\<região\>.hana.ondemand.com
</td>
</tr>
<tr>
<td>  
Node.js Runtime
</td>
<td>  
Node 22.x (engine ^22)
</td>
</tr>
<tr>
<td>  
Cloud Foundry Stack
</td>
<td>  
cflinuxfs4
</td>
</tr>
<tr>
<td>  
Grupos de negócio (IAS)
</td>
<td>  
F8\_Purchasing\_Buyers, F8\_Purchasing\_Managers
</td>
</tr>
<tr>
<td>  
Login humano interativo
</td>
<td>  
Validado (callback /login/callback 302, sessão criada)
</td>
</tr>
<tr>
<td>  
Propagação end-user até o CPI
</td>
<td>  
Não concluída (boundary arquitetural)
</td>
</tr>
</table>

  
Todos os identificadores concretos de tenant, subaccount, client IDs, client secrets, e-mails de usuários reais e domínios temporários usados durante a investigação do F8C são tratados como material sensível e não são reproduzidos neste documento público. Qualquer segredo exposto durante os testes deve ser considerado comprometido e rotacionado.

### 3. Arquitetura

#### 3.1 F8A — Contexto inbound
Postman
→ HTTPS Sender protegido por ESBMessaging.send
→ captura do contexto de autenticação
→ classificação do principal
→ detecção de headers declarados pelo caller
→ resposta de diagnóstico

#### 3.2 F8B — Technical User SAML Bearer
Postman autenticado
→ SAP Integration Suite
→ geração da assertion SAML 2.0
→ XML Digital Signer
→ OAuth token request RFC 7522
→ WSO2 /oauth2/token
→ Bearer access token
→ WSO2 /oauth2/introspect
→ subject técnico validado
→ política de autorização
→ recurso SAP MM protegido

#### 3.3 Cadeia de confiança
SAP Integration Suite Keystore
→ private key f8\_saml\_bearer\_signing
→ assina a assertion
WSO2 Identity Server
→ certificado público correspondente
→ valida a assinatura
→ associa Issuer e token endpoint Alias
→ emite e introspecta o access token

#### 3.4 F8C — Arquitetura pretendida de propagação end-user
Browser (usuário humano real)
→ Application Router (f8c-approuter)
→ XSUAA (f8c-xsuaa) para autenticação OAuth/OIDC
→ SAP Cloud Identity Services (IAS) como Identity Provider
→ Destination Service (f8c-destination-service)
→ Destination lógica (f8c-cpi-destination)
→ Cloud Integration (runtime gerenciado)
→ reutilização dos conceitos F8B (SAML/WSO2/autorização) após a chegada da identidade ao CPI

A intenção do F8C era substituir o principal técnico do F8B por um usuário humano real, mantendo o restante da cadeia de autorização. O ponto crítico da arquitetura está na fronteira entre o Application Router e o runtime gerenciado do Cloud Integration, onde a identidade precisa ser aceita de forma confiável.

### 4. Fundamentos de segurança

#### 4.1 Client Credentials não representa automaticamente um usuário humano
  
O grant Client Credentials autentica a aplicação cliente. No F8A, o runtime confirmou o acesso ao endpoint, mas não disponibilizou SapAuthenticatedUserName nem outro principal humano confiável. O contexto foi classificado como TECHNICAL\_CLIENT.

#### 4.2 Header declarado pelo caller não é identidade autenticada
  
Headers como X-Authenticated-User, X-Principal e X-User são dados controlados pelo consumidor. O laboratório permitiu esses headers apenas para detectar a tentativa, mas nunca os utilizou como fonte confiável de identidade.

#### 4.3 SAML Bearer não é SAML Web SSO

<table>
<tr>
<th>  
OAuth 2.0 SAML Bearer
</th>
<th>  
SAML Web SSO
</th>
</tr>
<tr>
<td>  
Troca uma assertion por access token
</td>
<td>  
Cria uma sessão de login federado
</td>
</tr>
<tr>
<td>  
Não exige navegador
</td>
<td>  
Normalmente envolve navegador
</td>
</tr>
<tr>
<td>  
Usa token endpoint
</td>
<td>  
Usa SSO URL e ACS URL
</td>
</tr>
<tr>
<td>  
Adequado a chamadas backend
</td>
<td>  
Adequado a autenticação interativa
</td>
</tr>
</table>


#### 4.4 Autenticação e autorização são controles diferentes

<table>
<tr>
<th>  
Controle
</th>
<th>  
Pergunta respondida
</th>
</tr>
<tr>
<td>  
Autenticação
</td>
<td>  
Quem ou qual aplicação apresentou a credencial?
</td>
</tr>
<tr>
<td>  
Introspecção
</td>
<td>  
O token está ativo e a qual contexto pertence?
</td>
</tr>
<tr>
<td>  
Autorização
</td>
<td>  
O principal pode executar a operação solicitada?
</td>
</tr>
</table>

  
Um token ativo não concede automaticamente todas as operações. O 403 Forbidden do teste de aprovação comprova que o principal estava autenticado, mas não possuía o grupo funcional exigido.

#### 4.5 Propagação de identidade humana exige trust dedicado
  
No F8C, o Application Router autentica o usuário humano contra o XSUAA e o IAS e cria uma sessão válida. Porém, o token resultante é emitido para o próprio XSUAA da aplicação. Para que o runtime gerenciado do Cloud Integration aceite essa identidade, é necessário um relacionamento de confiança explícito (trust/cross-consumption) entre o XSUAA controlado pela aplicação e o serviço gerenciado que expõe o endpoint do CPI. Sem esse relacionamento, encaminhar o JWT do usuário resulta em rejeição, mesmo com a sessão de login válida.

### 5. F8A — Authentication Context Capture

#### 5.1 Estrutura do iFlow
HTTPS Sender
→ Capture\_Authenticated\_Principal
→ Build\_Principal\_Diagnostic\_Response
→ End

#### 5.2 HTTPS Sender

<table>
<tr>
<th>  
Parâmetro
</th>
<th>  
Valor
</th>
</tr>
<tr>
<td>  
Address
</td>
<td>  
/f8a/mm/authenticated-principal
</td>
</tr>
<tr>
<td>  
Authorization
</td>
<td>  
User Role
</td>
</tr>
<tr>
<td>  
User Role
</td>
<td>  
ESBMessaging.send
</td>
</tr>
<tr>
<td>  
CSRF Protected
</td>
<td>  
Disabled
</td>
</tr>
</table>


#### 5.3 Runtime Configuration
X-Authenticated-User\|X-Principal\|X-User  
Esses headers foram liberados somente para o teste controlado de identity spoofing.

#### 5.4 Groovy CaptureAuthenticatedPrincipal.groovy
import com.sap.gateway.ip.core.customdev.util.Message
import java.security.MessageDigest
import java.time.Instant
def Message processData(Message message) {
    Map\<String, Object\> headers = message.getHeaders()
    String sapAuthenticatedUserName = readHeader(headers, "SapAuthenticatedUserName")
    String camelAuthenticatedUser = readHeader(headers, "CamelAuthenticatedUser")
    String servletRemoteUser = readServletRemoteUser(headers)
    String claimedPrincipal = getFirstAvailableHeader(
        headers,
        \["X-Authenticated-User", "X-Principal", "X-User"\]
    )
    String authenticatedPrincipal = firstNonEmptyValue(
        \[sapAuthenticatedUserName, camelAuthenticatedUser, servletRemoteUser\]
    )
    String identitySource
    String principalType
    String principalCaptureStatus
    if (authenticatedPrincipal) {
        identitySource = determineIdentitySource(
            sapAuthenticatedUserName,
            camelAuthenticatedUser,
            servletRemoteUser
        )
        principalType = classifyPrincipal(authenticatedPrincipal)
        principalCaptureStatus = "AUTHENTICATED\_PRINCIPAL\_AVAILABLE"
    } else {
        authenticatedPrincipal = "NOT\_EXPOSED\_BY\_RUNTIME"
        identitySource = "SERVICE\_INSTANCE\_AUTHENTICATION\_CONTEXT"
        principalType = "TECHNICAL\_CLIENT"
        principalCaptureStatus = "AUTHENTICATED\_CLIENT\_WITHOUT\_USER\_PRINCIPAL"
    }
    boolean claimedPrincipalProvided = claimedPrincipal != null && !claimedPrincipal.trim().isEmpty()
    String principalSha256 = authenticatedPrincipal == "NOT\_EXPOSED\_BY\_RUNTIME"
        ? "NOT\_CALCULATED"
        : calculateSha256(authenticatedPrincipal.toLowerCase())
    List\<String\> diagnosticHeaderNames = getDiagnosticHeaderNames(headers)
    message.setProperty("authenticatedPrincipal", authenticatedPrincipal)
    message.setProperty("principalType", principalType)
    message.setProperty("principalSha256", principalSha256)
    message.setProperty("identitySource", identitySource)
    message.setProperty("principalCaptureStatus", principalCaptureStatus)
    message.setProperty("spoofingAttemptDetected", claimedPrincipalProvided.toString())
    message.setProperty("claimedPrincipal", claimedPrincipalProvided ? claimedPrincipal : "NOT\_PROVIDED")
    message.setProperty("claimedPrincipalAccepted", "false")
    message.setProperty(
        "diagnosticHeaderNames",
        diagnosticHeaderNames.isEmpty() ? "NONE" : diagnosticHeaderNames.join(", ")
    )
    message.setProperty("propagationStage", "F8A\_PRINCIPAL\_CAPTURE\_BASELINE")
    message.setProperty("propagationMode", "NOT\_DETERMINED")
    message.setProperty("endToEndPropagation", "NOT\_EXECUTED")
    message.setProperty("capturedAt", Instant.now().toString())
    message.setHeader("Content-Type", "application/json")
    return message
}
def String readHeader(Map\<String, Object\> headers, String headerName) {
    Object headerValue = headers.get(headerName)
    if (headerValue == null) {
        return null
    }
    String normalizedValue = headerValue.toString().trim()
    return normalizedValue.isEmpty() ? null : normalizedValue
}
def String readServletRemoteUser(Map\<String, Object\> headers) {
    Object servletRequest = headers.get("CamelHttpServletRequest")
    if (servletRequest == null) {
        return null
    }
    try {
        String remoteUser = servletRequest.getRemoteUser()?.toString()?.trim()
        return remoteUser ? remoteUser : null
    } catch (Exception ignored) {
        return null
    }
}
def String getFirstAvailableHeader(Map\<String, Object\> headers, List\<String\> headerNames) {
    for (String headerName : headerNames) {
        String headerValue = readHeader(headers, headerName)
        if (headerValue) {
            return headerValue
        }
    }
    return null
}
def String firstNonEmptyValue(List\<String\> values) {
    return values.find { value -\> value != null && !value.trim().isEmpty() }
}
def String determineIdentitySource(
    String sapAuthenticatedUserName,
    String camelAuthenticatedUser,
    String servletRemoteUser
) {
    if (sapAuthenticatedUserName) {
        return "SAP\_AUTHENTICATED\_USER\_NAME"
    }
    if (camelAuthenticatedUser) {
        return "CAMEL\_AUTHENTICATED\_USER"
    }
    if (servletRemoteUser) {
        return "HTTPS\_SERVLET\_REMOTE\_USER"
    }
    return "UNKNOWN"
}
def String classifyPrincipal(String authenticatedPrincipal) {
    String normalizedPrincipal = authenticatedPrincipal.toLowerCase()
    List\<String\> technicalPatterns = \[
        "client", "service", "technical", "oauth", "runtime", "integration", "sb-"
    \]
    boolean technicalPatternDetected = technicalPatterns.any { pattern -\>
        normalizedPrincipal.contains(pattern)
    }
    if (technicalPatternDetected) {
        return "TECHNICAL\_CLIENT"
    }
    if (normalizedPrincipal.contains("@") && normalizedPrincipal.contains(".")) {
        return "HUMAN\_USER\_CANDIDATE"
    }
    return "AUTHENTICATED\_PRINCIPAL"
}
def List\<String\> getDiagnosticHeaderNames(Map\<String, Object\> headers) {
    List\<String\> allowedHeaderNames = \[
        "SapAuthenticatedUserName",
        "CamelAuthenticatedUser",
        "CamelHttpMethod",
        "CamelHttpPath",
        "CamelServletContextPath",
        "CamelHttpServletRequest"
    \]
    return allowedHeaderNames.findAll { headerName -\> headers.containsKey(headerName) }
}
def String calculateSha256(String content) {
    MessageDigest digest = MessageDigest.getInstance("SHA-256")
    byte\[\] hash = digest.digest(content.getBytes("UTF-8"))
    return hash.collect { byteValue -\> String.format("%02x", byteValue & 0xff) }.join()
}

#### 5.5 Testes do F8A

<table>
<tr>
<th>  
Teste
</th>
<th>  
Entrada
</th>
<th>  
Resultado
</th>
</tr>
<tr>
<td>  
Baseline
</td>
<td>  
Client Credentials, sem headers de identidade
</td>
<td>  
TECHNICAL\_CLIENT, sem principal humano
</td>
</tr>
<tr>
<td>  
Spoofing
</td>
<td>  
X-Authenticated-User e X-Principal
</td>
<td>  
Detectado, mas claimedPrincipalAccepted=false
</td>
</tr>
</table>


### 6. Preparação do WSO2 Identity Server

#### 6.1 Identidades e grupos

<table>
<tr>
<th>  
Identidade
</th>
<th>  
Buyers
</th>
<th>  
Managers
</th>
</tr>
<tr>
<td>  
buyer.user
</td>
<td>  
Sim
</td>
<td>  
Não
</td>
</tr>
<tr>
<td>  
purchasing.manager
</td>
<td>  
Sim
</td>
<td>  
Sim
</td>
</tr>
<tr>
<td>  
f8.technical.purchasing.user
</td>
<td>  
Sim
</td>
<td>  
Não
</td>
</tr>
</table>


#### 6.2 Aplicação OAuth

<table>
<tr>
<th>  
Parâmetro
</th>
<th>  
Configuração
</th>
</tr>
<tr>
<td>  
Application
</td>
<td>  
F8 SAP Integration Suite SAML Bearer Client
</td>
</tr>
<tr>
<td>  
Type
</td>
<td>  
Standard-Based Application
</td>
</tr>
<tr>
<td>  
Protocol
</td>
<td>  
OAuth 2.0/OpenID Connect
</td>
</tr>
<tr>
<td>  
Client Type
</td>
<td>  
Confidential
</td>
</tr>
<tr>
<td>  
Grants
</td>
<td>  
Client Credential, SAML2
</td>
</tr>
<tr>
<td>  
Password Grant
</td>
<td>  
Disabled
</td>
</tr>
<tr>
<td>  
Implicit Grant
</td>
<td>  
Disabled
</td>
</tr>
</table>


#### 6.3 Endpoint público temporário
https://\<ngrok-domain\>
Token endpoint: https://\<ngrok-domain\>/oauth2/token
Introspection endpoint: https://\<ngrok-domain\>/oauth2/introspect

#### 6.4 Key Pair de assinatura

<table>
<tr>
<th>  
Parâmetro
</th>
<th>  
Valor
</th>
</tr>
<tr>
<td>  
Alias
</td>
<td>  
f8\_saml\_bearer\_signing
</td>
</tr>
<tr>
<td>  
Common Name
</td>
<td>  
f8.technical.purchasing.user
</td>
</tr>
<tr>
<td>  
Key Type
</td>
<td>  
RSA
</td>
</tr>
<tr>
<td>  
Key Size
</td>
<td>  
3072 bits
</td>
</tr>
<tr>
<td>  
Signature Algorithm
</td>
<td>  
SHA512withRSA
</td>
</tr>
<tr>
<td>  
Private Key
</td>
<td>  
Mantida exclusivamente no Keystore do SAP Integration Suite
</td>
</tr>
<tr>
<td>  
Public Certificate
</td>
<td>  
Importado no truststore do WSO2
</td>
</tr>
</table>


#### 6.5 Trust lógico do Issuer

<table>
<tr>
<th>  
Campo
</th>
<th>  
Valor
</th>
</tr>
<tr>
<td>  
Connection
</td>
<td>  
F8 SAP Integration Suite SAML Assertion Issuer
</td>
</tr>
<tr>
<td>  
Issuer
</td>
<td>  
F8 SAP Integration Suite SAML Assertion Issuer
</td>
</tr>
<tr>
<td>  
Alias
</td>
<td>  
https://\<ngrok-domain\>/oauth2/token
</td>
</tr>
<tr>
<td>  
Trusted Certificate
</td>
<td>  
Certificado público do alias f8\_saml\_bearer\_signing
</td>
</tr>
</table>

  
O certificado público estabelece confiança criptográfica. O Issuer e o Alias estabelecem a associação lógica usada pelo processador SAML Bearer do WSO2.

### 7. F8B.1 — SAML Bearer Token Exchange

#### 7.1 Estrutura inicial
HTTPS Sender
→ Build\_SAML\_Bearer\_Assertion
→ Sign\_SAML\_Bearer\_Assertion
→ Prepare\_OAuth\_Token\_Request
→ Request Reply Token
→ WSO2 Token Endpoint
→ Validate\_OAuth\_Token\_Response

#### 7.2 Assertion SAML

<table>
<tr>
<th>  
Elemento
</th>
<th>  
Valor
</th>
</tr>
<tr>
<td>  
Version
</td>
<td>  
2.0
</td>
</tr>
<tr>
<td>  
Issuer
</td>
<td>  
F8 SAP Integration Suite SAML Assertion Issuer
</td>
</tr>
<tr>
<td>  
Subject/NameID
</td>
<td>  
f8.technical.purchasing.user
</td>
</tr>
<tr>
<td>  
Confirmation Method
</td>
<td>  
urn:oasis:names:tc:SAML:2.0:cm:bearer
</td>
</tr>
<tr>
<td>  
Audience
</td>
<td>  
Token endpoint público
</td>
</tr>
<tr>
<td>  
Recipient
</td>
<td>  
Token endpoint público
</td>
</tr>
<tr>
<td>  
NotBefore
</td>
<td>  
Horário atual menos 30 segundos
</td>
</tr>
<tr>
<td>  
NotOnOrAfter
</td>
<td>  
Horário atual mais 5 minutos
</td>
</tr>
<tr>
<td>  
Group Attribute
</td>
<td>  
F8\_PURCHASING\_BUYERS
</td>
</tr>
</table>


#### 7.3 Groovy BuildSAMLBearerAssertion.groovy
import com.sap.gateway.ip.core.customdev.util.Message
import java.time.Instant
import java.time.temporal.ChronoUnit
import java.util.UUID
def Message processData(Message message) {
    Instant now = Instant.now()
    Instant notBefore = now.minus(30, ChronoUnit.SECONDS)
    Instant notOnOrAfter = now.plus(5, ChronoUnit.MINUTES)
    String assertionId = "\_${UUID.randomUUID().toString()}"
    String issuer = "F8 SAP Integration Suite SAML Assertion Issuer"
    String subject = "f8.technical.purchasing.user"
    String tokenServiceUrl = "https://\<ngrok-domain\>/oauth2/token"
    String issueInstant = now.toString()
    String notBeforeValue = notBefore.toString()
    String notOnOrAfterValue = notOnOrAfter.toString()
    String assertion = """\<saml2:Assertion xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" ID="${assertionId}" IssueInstant="${issueInstant}" Version="2.0"\>
    \<saml2:Issuer\>${escapeXml(issuer)}\</saml2:Issuer\>
    \<saml2:Subject\>
        \<saml2:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified"\>${escapeXml(subject)}\</saml2:NameID\>
        \<saml2:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer"\>
            \<saml2:SubjectConfirmationData NotOnOrAfter="${notOnOrAfterValue}" Recipient="${escapeXml(tokenServiceUrl)}"/\>
        \</saml2:SubjectConfirmation\>
    \</saml2:Subject\>
    \<saml2:Conditions NotBefore="${notBeforeValue}" NotOnOrAfter="${notOnOrAfterValue}"\>
        \<saml2:AudienceRestriction\>
            \<saml2:Audience\>${escapeXml(tokenServiceUrl)}\</saml2:Audience\>
        \</saml2:AudienceRestriction\>
    \</saml2:Conditions\>
    \<saml2:AuthnStatement AuthnInstant="${issueInstant}"\>
        \<saml2:AuthnContext\>
            \<saml2:AuthnContextClassRef\>urn:oasis:names:tc:SAML:2.0:ac:classes:PreviousSession\</saml2:AuthnContextClassRef\>
        \</saml2:AuthnContext\>
    \</saml2:AuthnStatement\>
    \<saml2:AttributeStatement\>
        \<saml2:Attribute Name="userId"\>
            \<saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string" xmlns:xs="http://www.w3.org/2001/XMLSchema"\>${escapeXml(subject)}\</saml2:AttributeValue\>
        \</saml2:Attribute\>
        \<saml2:Attribute Name="email"\>
            \<saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string" xmlns:xs="http://www.w3.org/2001/XMLSchema"\>f8-technical-user@example.invalid\</saml2:AttributeValue\>
        \</saml2:Attribute\>
        \<saml2:Attribute Name="groups"\>
            \<saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string" xmlns:xs="http://www.w3.org/2001/XMLSchema"\>F8\_PURCHASING\_BUYERS\</saml2:AttributeValue\>
        \</saml2:Attribute\>
    \</saml2:AttributeStatement\>
\</saml2:Assertion\>"""
    message.setProperty("samlAssertionId", assertionId)
    message.setProperty("samlIssuer", issuer)
    message.setProperty("samlSubject", subject)
    message.setProperty("samlAudience", tokenServiceUrl)
    message.setProperty("samlRecipient", tokenServiceUrl)
    message.setProperty("samlIssueInstant", issueInstant)
    message.setProperty("samlNotBefore", notBeforeValue)
    message.setProperty("samlNotOnOrAfter", notOnOrAfterValue)
    message.setProperty("samlKeyPairAlias", "f8\_saml\_bearer\_signing")
    message.setProperty("tokenServiceUrl", tokenServiceUrl)
    message.setProperty("propagationMode", "TECHNICAL\_USER\_SAML\_BEARER")
    message.setProperty("assertionStatus", "BUILT\_NOT\_SIGNED")
    message.setBody(assertion)
    message.setHeader("Content-Type", "application/xml")
    return message
}
def String escapeXml(String value) {
    if (value == null) {
        return ""
    }
    return value
        .replace("&", "&amp;")
        .replace("\<", "&lt;")
        .replace("\>", "&gt;")
        .replace("\\"", "&quot;")
        .replace("'", "&apos;")
}

#### 7.4 XML Digital Signer

<table>
<tr>
<th>  
Parâmetro
</th>
<th>  
Valor
</th>
</tr>
<tr>
<td>  
Private Key Alias
</td>
<td>  
f8\_saml\_bearer\_signing
</td>
</tr>
<tr>
<td>  
Signature Algorithm
</td>
<td>  
SHA512/RSA
</td>
</tr>
<tr>
<td>  
Digest Algorithm
</td>
<td>  
SHA256
</td>
</tr>
<tr>
<td>  
Signature Type
</td>
<td>  
Enveloped XML Signature
</td>
</tr>
<tr>
<td>  
Parent Node Name
</td>
<td>  
Assertion
</td>
</tr>
<tr>
<td>  
Parent Node Namespace
</td>
<td>  
urn:oasis:names:tc:SAML:2.0:assertion
</td>
</tr>
<tr>
<td>  
Canonicalization
</td>
<td>  
Exclusive XML Canonicalization 1.0
</td>
</tr>
<tr>
<td>  
X.509 Certificate Chain
</td>
<td>  
Enabled
</td>
</tr>
<tr>
<td>  
XML Declaration
</td>
<td>  
Excluded
</td>
</tr>
<tr>
<td>  
DOCTYPE
</td>
<td>  
Disallowed
</td>
</tr>
</table>


#### 7.5 Groovy PrepareOAuthSAMLTokenRequest.groovy
import com.sap.gateway.ip.core.customdev.util.Message
import java.nio.charset.StandardCharsets
import java.util.Base64
def Message processData(Message message) {
    String signedAssertion = message.getBody(String)
    if (!signedAssertion \|\| signedAssertion.trim().isEmpty()) {
        throw new IllegalArgumentException("The signed SAML assertion cannot be empty.")
    }
    if (!signedAssertion.contains("\<ds:Signature")) {
        throw new SecurityException("The SAML assertion does not contain an XML digital signature.")
    }
    String encodedAssertion = Base64
        .getUrlEncoder()
        .withoutPadding()
        .encodeToString(signedAssertion.getBytes(StandardCharsets.UTF\_8))
    String grantType = "urn:ietf:params:oauth:grant-type:saml2-bearer"
    String formBody = "grant\_type=${urlEncode(grantType)}&assertion=${urlEncode(encodedAssertion)}"
    message.setProperty("signedSamlAssertion", signedAssertion)
    message.setProperty("encodedAssertionLength", encodedAssertion.length().toString())
    message.setProperty("oauthGrantType", grantType)
    message.setProperty("assertionStatus", "SIGNED\_AND\_ENCODED")
    message.setProperty("tokenRequestPrepared", "true")
    message.setBody(formBody)
    message.setHeader("CamelHttpMethod", "POST")
    message.setHeader("Content-Type", "application/x-www-form-urlencoded")
    message.setHeader("Accept", "application/json")
    return message
}
def String urlEncode(String value) {
    return java.net.URLEncoder
        .encode(value, StandardCharsets.UTF\_8.name())
        .replace("+", "%20")
}

### 8. F8B.2 — Token Introspection

#### 8.1 Objetivo
  
Confirmar, por meio do Authorization Server, que o token emitido está ativo, é do tipo Bearer, pertence ao cliente esperado, não expirou e representa o subject técnico da assertion.

#### 8.2 Configuração do WSO2
\[\[resource.access\_control\]\]
context="(.\*)/oauth2/introspect(.\*)"
http\_method="all"
secure=true
allowed\_auth\_handlers="BasicClientAuthentication"

#### 8.3 Groovy ValidateOAuthTokenResponse.groovy
import com.sap.gateway.ip.core.customdev.util.Message
import groovy.json.JsonSlurper
import java.security.MessageDigest
import java.time.Instant
def Message processData(Message message) {
    Reader responseReader = message.getBody(Reader)
    String responseBody = responseReader != null ? responseReader.text : ""
    Integer responseCode = resolveHttpResponseCode(message)
    if (!responseBody \|\| responseBody.trim().isEmpty()) {
        responseBody = "{}"
    }
    def responsePayload
    try {
        responsePayload = new JsonSlurper().parseText(responseBody)
    } catch (Exception ignored) {
        responsePayload = \[
            error: "non\_json\_response",
            error\_description: responseBody.take(500)
        \]
    }
    if (responseCode \>= 400) {
        String oauthError = responsePayload.error?.toString()?.trim() ?: "oauth\_token\_exchange\_failed"
        String oauthErrorDescription = responsePayload.error\_description?.toString()?.trim()
            ?: "The WSO2 token endpoint rejected the SAML bearer assertion."
        message.setProperty("tokenExchangeStatus", "SAML\_BEARER\_TOKEN\_REJECTED")
        message.setProperty("responseCode", responseCode.toString())
        message.setProperty("responseStatusCode", "F8B-SAML-400")
        message.setProperty("responseMessage", "The WSO2 token endpoint rejected the SAML bearer token exchange.")
        message.setProperty("oauthError", oauthError)
        message.setProperty("oauthErrorDescription", oauthErrorDescription)
        message.setProperty("tokenType", "NOT\_ISSUED")
        message.setProperty("expiresIn", "NOT\_PROVIDED")
        message.setProperty("scope", "NOT\_PROVIDED")
        message.setProperty("internalAccessToken", "NOT\_AVAILABLE")
        message.setProperty("maskedAccessToken", "NOT\_ISSUED")
        message.setProperty("accessTokenSha256", "NOT\_CALCULATED")
        message.setProperty("tokenIssuedAt", "NOT\_ISSUED")
        message.setProperty("technicalPrincipal", "f8.technical.purchasing.user")
        message.setProperty("technicalPrincipalGroup", "F8\_PURCHASING\_BUYERS")
        message.setProperty("tokenIntrospectionStatus", "NOT\_EXECUTED")
        message.setHeader("CamelHttpResponseCode", 400)
        message.setHeader("Content-Type", "application/json")
        return message
    }
    String accessToken = responsePayload.access\_token?.toString()?.trim()
    String tokenType = responsePayload.token\_type?.toString()?.trim()
    String expiresIn = responsePayload.expires\_in?.toString()?.trim()
    String scope = responsePayload.scope?.toString()?.trim()
    if (!accessToken) {
        throw new SecurityException("The WSO2 response does not contain an access token.")
    }
    if (!tokenType?.equalsIgnoreCase("Bearer")) {
        throw new SecurityException("The WSO2 response does not contain a Bearer token.")
    }
    message.setProperty("tokenExchangeStatus", "SAML\_BEARER\_TOKEN\_ISSUED")
    message.setProperty("responseCode", "200")
    message.setProperty("responseStatusCode", "F8B-SAML-200")
    message.setProperty("responseMessage", "The technical user SAML bearer assertion was exchanged for an OAuth access token.")
    message.setProperty("oauthError", "NONE")
    message.setProperty("oauthErrorDescription", "NONE")
    message.setProperty("tokenType", tokenType)
    message.setProperty("expiresIn", expiresIn ?: "NOT\_PROVIDED")
    message.setProperty("scope", scope ?: "NOT\_PROVIDED")
    message.setProperty("internalAccessToken", accessToken)
    message.setProperty("maskedAccessToken", maskToken(accessToken))
    message.setProperty("accessTokenSha256", calculateSha256(accessToken))
    message.setProperty("tokenIssuedAt", Instant.now().toString())
    message.setProperty("technicalPrincipal", "f8.technical.purchasing.user")
    message.setProperty("technicalPrincipalGroup", "F8\_PURCHASING\_BUYERS")
    message.setProperty("tokenIntrospectionStatus", "PENDING")
    message.setHeader("CamelHttpResponseCode", 200)
    message.setHeader("Content-Type", "application/json")
    return message
}
def Integer resolveHttpResponseCode(Message message) {
    Map\<String, Object\> headers = message.getHeaders()
    Object responseCode = headers.get("CamelHttpResponseCode")
    if (responseCode == null) {
        return 200
    }
    try {
        return Integer.parseInt(responseCode.toString())
    } catch (Exception ignored) {
        return 200
    }
}
def String maskToken(String token) {
    if (token.length() \<= 12) {
        return "\*\*\*\*\*\*\*\*"
    }
    return token.substring(0, 8) + "-\*\*\*\*-\*\*\*\*-" + token.substring(token.length() - 4)
}
def String calculateSha256(String content) {
    MessageDigest digest = MessageDigest.getInstance("SHA-256")
    byte\[\] hash = digest.digest(content.getBytes("UTF-8"))
    return hash.collect { byteValue -\> String.format("%02x", byteValue & 0xff) }.join()
}

#### 8.4 Groovy PrepareTokenIntrospectionRequest.groovy
import com.sap.gateway.ip.core.customdev.util.Message
import java.net.URLEncoder
import java.nio.charset.StandardCharsets
def Message processData(Message message) {
    String accessToken = message.getProperty("internalAccessToken")?.toString()?.trim()
    if (!accessToken \|\| accessToken == "NOT\_AVAILABLE") {
        throw new SecurityException("The internal access token is not available for introspection.")
    }
    String formBody = "token=" + URLEncoder.encode(accessToken, StandardCharsets.UTF\_8.name())
    message.setBody(formBody)
    message.setHeader("CamelHttpMethod", "POST")
    message.setHeader("Content-Type", "application/x-www-form-urlencoded")
    message.setHeader("Accept", "application/json")
    message.setProperty("tokenIntrospectionStatus", "REQUEST\_PREPARED")
    message.setProperty("introspectionRequestPrepared", "true")
    return message
}

#### 8.5 Groovy ValidateTokenIntrospectionResponse.groovy
import com.sap.gateway.ip.core.customdev.util.Message
import groovy.json.JsonSlurper
import java.time.Instant
def Message processData(Message message) {
    Reader responseReader = message.getBody(Reader)
    String responseBody = responseReader != null ? responseReader.text : null
    if (!responseBody \|\| responseBody.trim().isEmpty()) {
        clearSensitiveToken(message)
        throw new SecurityException("The WSO2 token introspection response cannot be empty.")
    }
    def introspectionResponse
    try {
        introspectionResponse = new JsonSlurper().parseText(responseBody)
    } catch (Exception exception) {
        clearSensitiveToken(message)
        throw new IllegalArgumentException("The WSO2 token introspection response is not valid JSON.")
    }
    boolean tokenActive = introspectionResponse.active == true \|\|
        introspectionResponse.active?.toString()?.equalsIgnoreCase("true")
    String tokenType = introspectionResponse.token\_type?.toString()?.trim() ?: "NOT\_PROVIDED"
    String authenticationContext = introspectionResponse.auth?.toString()?.trim() ?: "NOT\_PROVIDED"
    String clientId = introspectionResponse.client\_id?.toString()?.trim() ?: "NOT\_PROVIDED"
    String audience = introspectionResponse.aud?.toString()?.trim() ?: "NOT\_PROVIDED"
    String subject = resolveSubject(introspectionResponse)
    String issuedAt = introspectionResponse.iat?.toString()?.trim() ?: "NOT\_PROVIDED"
    String notBefore = introspectionResponse.nbf?.toString()?.trim() ?: "NOT\_PROVIDED"
    String expiresAt = introspectionResponse.exp?.toString()?.trim() ?: "NOT\_PROVIDED"
    if (!tokenActive) {
        clearSensitiveToken(message)
        throw new SecurityException("The WSO2 introspection endpoint reported an inactive token.")
    }
    if (!tokenType.equalsIgnoreCase("Bearer")) {
        clearSensitiveToken(message)
        throw new SecurityException("The introspected token is not a Bearer token.")
    }
    if (expiresAt != "NOT\_PROVIDED" && !isExpirationInFuture(expiresAt)) {
        clearSensitiveToken(message)
        throw new SecurityException("The introspected token has already expired.")
    }
    message.setProperty("tokenExchangeStatus", "SAML\_BEARER\_TOKEN\_ISSUED\_AND\_INTROSPECTED")
    message.setProperty("responseCode", "200")
    message.setProperty("responseStatusCode", "F8B-INTROSPECTION-200")
    message.setProperty("responseMessage", "The SAML bearer access token was issued and validated through OAuth token introspection.")
    message.setProperty("tokenIntrospectionStatus", "VALIDATED")
    message.setProperty("tokenActive", "true")
    message.setProperty("introspectionTokenType", tokenType)
    message.setProperty("tokenAuthenticationContext", authenticationContext)
    message.setProperty("introspectionClientId", clientId)
    message.setProperty("introspectionAudience", audience)
    message.setProperty("introspectionSubject", subject)
    message.setProperty("introspectionIssuedAt", issuedAt)
    message.setProperty("introspectionNotBefore", notBefore)
    message.setProperty("introspectionExpiresAt", expiresAt)
    message.setProperty("tokenIntrospectedAt", Instant.now().toString())
    message.setProperty("accessTokenExposed", "false")
    clearSensitiveToken(message)
    message.setHeader("CamelHttpResponseCode", 200)
    message.setHeader("Content-Type", "application/json")
    return message
}
def String resolveSubject(def introspectionResponse) {
    List\<String\> candidates = \[
        introspectionResponse.sub?.toString()?.trim(),
        introspectionResponse.username?.toString()?.trim(),
        introspectionResponse.user\_name?.toString()?.trim()
    \]
    String subject = candidates.find { candidate -\> candidate != null && !candidate.isEmpty() }
    return subject ?: "NOT\_PROVIDED"
}
def boolean isExpirationInFuture(String expirationValue) {
    try {
        long expirationEpoch = Long.parseLong(expirationValue)
        return expirationEpoch \> Instant.now().getEpochSecond()
    } catch (Exception ignored) {
        return true
    }
}
def void clearSensitiveToken(Message message) {
    message.setProperty("internalAccessToken", "REMOVED\_AFTER\_INTROSPECTION")
}

### 9. F8B.3 — Protected Resource Authorization

#### 9.1 Política funcional

<table>
<tr>
<th>  
Operação
</th>
<th>  
Grupo exigido
</th>
<th>  
Buyers
</th>
<th>  
Managers
</th>
</tr>
<tr>
<td>  
READ
</td>
<td>  
F8\_PURCHASING\_BUYERS
</td>
<td>  
Autorizado
</td>
<td>  
Autorizado
</td>
</tr>
<tr>
<td>  
APPROVE
</td>
<td>  
F8\_PURCHASING\_MANAGERS
</td>
<td>  
Negado
</td>
<td>  
Autorizado
</td>
</tr>
<tr>
<td>  
Outro valor
</td>
<td>  
Não aplicável
</td>
<td>  
Não avaliado
</td>
<td>  
Não avaliado
</td>
</tr>
</table>

  
O technical user do F8B pertence somente a F8\_PURCHASING\_BUYERS. Por isso, READ retorna 200, enquanto APPROVE retorna 403.

#### 9.2 Runtime Configuration
X-F8-Operation  
O header declara a operação solicitada, mas não determina a identidade nem concede permissão.

#### 9.3 Groovy PrepareAuthorizationContext.groovy
import com.sap.gateway.ip.core.customdev.util.Message
import java.time.Instant
def Message processData(Message message) {
    String requestedOperation = message.getHeader("X-F8-Operation", String)?.trim()?.toUpperCase()
    String tokenActive = message.getProperty("tokenActive")?.toString()?.trim()
    String tokenIntrospectionStatus = message.getProperty("tokenIntrospectionStatus")?.toString()?.trim()
    String introspectionSubject = message.getProperty("introspectionSubject")?.toString()?.trim()
    String expectedPrincipal = message.getProperty("technicalPrincipal")?.toString()?.trim()
    String availableGroupsValue = message.getProperty("technicalPrincipalGroup")?.toString()?.trim()
    if (!requestedOperation) {
        requestedOperation = "NOT\_PROVIDED"
    }
    if (!tokenActive?.equalsIgnoreCase("true")) {
        throw new SecurityException("Authorization cannot continue because the introspected token is not active.")
    }
    if (!tokenIntrospectionStatus?.equalsIgnoreCase("VALIDATED")) {
        throw new SecurityException("Authorization cannot continue because token introspection was not validated.")
    }
    if (!introspectionSubject \|\| introspectionSubject == "NOT\_PROVIDED") {
        throw new SecurityException("Authorization cannot continue because the introspected subject is not available.")
    }
    if (!expectedPrincipal \|\| !introspectionSubject.equalsIgnoreCase(expectedPrincipal)) {
        throw new SecurityException("The introspected subject does not match the expected technical principal.")
    }
    List\<String\> availableGroups = parseGroups(availableGroupsValue)
    String requiredGroup
    String authorizationDecision
    String authorizationReason
    String operationDescription
    switch (requestedOperation) {
        case "READ":
            requiredGroup = "F8\_PURCHASING\_BUYERS"
            operationDescription = "Read SAP MM purchase requisitions"
            if (availableGroups.contains("F8\_PURCHASING\_BUYERS") \|\|
                availableGroups.contains("F8\_PURCHASING\_MANAGERS")) {
                authorizationDecision = "AUTHORIZED"
                authorizationReason = "The introspected principal belongs to a group authorized to read SAP MM purchase requisitions."
            } else {
                authorizationDecision = "DENIED"
                authorizationReason = "The introspected principal does not belong to a group authorized to read SAP MM purchase requisitions."
            }
            break
        case "APPROVE":
            requiredGroup = "F8\_PURCHASING\_MANAGERS"
            operationDescription = "Approve SAP MM purchase requisitions"
            if (availableGroups.contains("F8\_PURCHASING\_MANAGERS")) {
                authorizationDecision = "AUTHORIZED"
                authorizationReason = "The introspected principal belongs to the purchasing managers group."
            } else {
                authorizationDecision = "DENIED"
                authorizationReason = "The introspected principal belongs to the buyers group but not to the purchasing managers group."
            }
            break
        default:
            requiredGroup = "NOT\_APPLICABLE"
            operationDescription = "Unsupported protected resource operation"
            authorizationDecision = "UNSUPPORTED"
            authorizationReason = "The requested operation is not supported. Allowed operations are READ and APPROVE."
    }
    message.setProperty("requestedOperation", requestedOperation)
    message.setProperty("operationDescription", operationDescription)
    message.setProperty("protectedResource", "SAP\_MM\_PURCHASE\_REQUISITIONS")
    message.setProperty("requiredGroup", requiredGroup)
    message.setProperty("availableGroups", availableGroups.isEmpty() ? "NONE" : availableGroups.join(","))
    message.setProperty("matchedGroup", resolveMatchedGroup(authorizationDecision, requiredGroup, availableGroups))
    message.setProperty("authorizationDecision", authorizationDecision)
    message.setProperty("authorizationReason", authorizationReason)
    message.setProperty("authorizationEvaluatedAt", Instant.now().toString())
    message.setProperty("identitySourceForAuthorization", "WSO2\_TOKEN\_INTROSPECTION\_SUBJECT")
    message.setProperty("authorizationPolicy", "F8B\_PURCHASE\_REQUISITION\_GROUP\_POLICY")
    return message
}
def List\<String\> parseGroups(String groupsValue) {
    if (!groupsValue \|\| groupsValue == "NOT\_PROVIDED") {
        return \[\]
    }
    return groupsValue
        .split("\[,;\|\]")
        .collect { group -\> group.trim().toUpperCase() }
        .findAll { group -\> !group.isEmpty() }
}
def String resolveMatchedGroup(
    String authorizationDecision,
    String requiredGroup,
    List\<String\> availableGroups
) {
    if (authorizationDecision != "AUTHORIZED") {
        return "NONE"
    }
    if (availableGroups.contains(requiredGroup)) {
        return requiredGroup
    }
    if (requiredGroup == "F8\_PURCHASING\_BUYERS" &&
        availableGroups.contains("F8\_PURCHASING\_MANAGERS")) {
        return "F8\_PURCHASING\_MANAGERS"
    }
    return "NONE"
}
#### 9.4 Router

<table>
<tr>
<th>  
Rota
</th>
<th>  
Condição
</th>
</tr>
<tr>
<td>  
Authorization\_Granted
</td>
<td>  
${property.authorizationDecision} = 'AUTHORIZED'
</td>
</tr>
<tr>
<td>  
Authorization\_Denied
</td>
<td>  
${property.authorizationDecision} = 'DENIED'
</td>
</tr>
<tr>
<td>  
Unsupported\_Operation
</td>
<td>  
Default Route
</td>
</tr>
</table>


#### 9.5 Códigos HTTP

<table>
<tr>
<th>  
Situação
</th>
<th>  
Código
</th>
</tr>
<tr>
<td>  
Operação autorizada
</td>
<td>  
200
</td>
</tr>
<tr>
<td>  
Principal autenticado sem permissão
</td>
<td>  
403
</td>
</tr>
<tr>
<td>  
Operação não suportada
</td>
<td>  
400
</td>
</tr>
</table>

### 10. F8C — End-User Principal Propagation (Investigação Arquitetural)

Esta seção documenta a investigação do F8C. O objetivo era evoluir do principal técnico do F8B para um usuário humano real e propagar essa identidade até o Cloud Integration. O login interativo foi validado de ponta a ponta, mas a propagação end-user até o runtime gerenciado do CPI encontrou um boundary arquitetural no ambiente trial. Por decisão de escopo, o F8C é registrado aqui como troubleshooting técnico, sem geração de evidências, pois a pasta de evidências correspondente foi reservada para o próximo documento.

#### 10.1 Objetivo e diferença em relação ao F8B

<table>
<tr>
<th>  
Dimensão
</th>
<th>  
F8B (concluído)
</th>
<th>  
F8C (investigação)
</th>
</tr>
<tr>
<td>  
Identidade propagada
</td>
<td>  
Usuário técnico (f8.technical.purchasing.user)
</td>
<td>  
Usuário humano real autenticado no IAS
</td>
</tr>
<tr>
<td>  
Mecanismo
</td>
<td>  
OAuth 2.0 SAML Bearer (RFC 7522)
</td>
<td>  
Login interativo OIDC via Application Router e XSUAA
</td>
</tr>
<tr>
<td>  
Navegador
</td>
<td>  
Não exigido
</td>
<td>  
Exigido (fluxo interativo)
</td>
</tr>
<tr>
<td>  
Ponto crítico
</td>
<td>  
Trust do certificado no WSO2
</td>
<td>  
Trust entre XSUAA da aplicação e runtime gerenciado do CPI
</td>
</tr>
<tr>
<td>  
Resultado
</td>
<td>  
Cadeia completa validada
</td>
<td>  
Login validado; propagação até o CPI não concluída
</td>
</tr>
</table>

#### 10.2 Motivação para SAP Cloud Identity Services (IAS)
  
Durante a preparação, foi observado que o Default identity provider do SAP BTP Cockpit não envia e-mails de convite para usuários criados manualmente, enquanto o SAP Cloud Identity Services (IAS) gerencia adequadamente o ciclo de ativação e envia as mensagens necessárias. O fluxo Esqueceu a senha do IAS Admin Console também funcionou para ativação administrativa de contas. Por esse motivo, o IAS foi adotado como Identity Provider do cenário, em vez do Default IdP simplificado, aproximando o laboratório de um desenho corporativo realista de identidade.

#### 10.3 Grupos e identidades de negócio

<table>
<tr>
<th>  
Grupo IAS
</th>
<th>  
Perfil de negócio
</th>
<th>  
Uso pretendido
</th>
</tr>
<tr>
<td>  
F8\_Purchasing\_Buyers
</td>
<td>  
Comprador
</td>
<td>  
Leitura de requisições de compra SAP MM
</td>
</tr>
<tr>
<td>  
F8\_Purchasing\_Managers
</td>
<td>  
Gestor de compras
</td>
<td>  
Aprovação de requisições de compra SAP MM
</td>
</tr>
</table>

  
Foram utilizados usuários reais para testar login de Buyer e de Manager. Os e-mails e identificadores reais desses usuários não são reproduzidos neste documento público. Em material de portfólio, identidades humanas reais devem ser mascaradas ou substituídas por identidades fictícias.

#### 10.4 Componentes provisionados no Cloud Foundry

<table>
<tr>
<th>  
Componente
</th>
<th>  
Papel na arquitetura
</th>
</tr>
<tr>
<td>  
f8c-approuter
</td>
<td>  
Application Router que inicia o login OIDC e mantém a sessão do usuário
</td>
</tr>
<tr>
<td>  
f8c-xsuaa
</td>
<td>  
Service instance XSUAA responsável pela autenticação OAuth/OIDC da aplicação
</td>
</tr>
<tr>
<td>  
f8c-destination-service
</td>
<td>  
Service instance de Destination usada pelo Approuter para resolver o backend
</td>
</tr>
<tr>
<td>  
f8c-cpi-destination
</td>
<td>  
Destination lógica que aponta para o endpoint do Cloud Integration
</td>
</tr>
<tr>
<td>  
Route pública
</td>
<td>  
f8c-approuter.cfapps.\<região\>.hana.ondemand.com
</td>
</tr>
</table>

  
O runtime observado foi Node 22.x com Application Router 22.x sobre stack cflinuxfs4. O buildpack instalou o Node conforme o engine ^22 declarado no package.json.

#### 10.5 Login interativo validado
  
O fluxo interativo foi validado com sucesso. Ao acessar a Route pública, o Application Router redirecionou o navegador para o XSUAA e, na sequência, para o IAS. Após a autenticação do usuário humano real, o callback /login/callback retornou 302 e uma sessão válida foi criada no Application Router. Esse resultado comprova que a camada de autenticação humana (Browser → Approuter → XSUAA → IAS) estava operacional.

#### 10.6 Tentativas de propagação até o Cloud Integration

##### 10.6.1 Destination hardcoded persistente
  
**Sintoma:** o comando de inspeção de ambiente da aplicação exibia uma variável User-Provided destinations persistente, contendo uma destination f8c-cpi-destination com URL placeholder e forwardAuthToken=true.  
**Causa:** a variável de ambiente User-Provided sobreviveu às alterações do manifest e continuou sendo aplicada.  
**Correção:** remover explicitamente a variável de ambiente destinations e refazer o restage da aplicação. Aprendizado: variáveis User-Provided permanecem até remoção explícita e têm precedência sobre o manifest.

##### 10.6.2 Aplicação em crash após remover a variável
  
**Sintoma:** após a remoção da variável, a aplicação passou a falhar na inicialização com Route references unknown destination "f8c-cpi-destination" e Using empty destinations to run.  
**Causa:** a rota declarada no xs-app.json dependia do Destination Service, mas a service instance f8c-destination-service ainda não estava corretamente disponível e vinculada naquele momento.  
**Correção:** criar a service instance de Destination, vinculá-la à aplicação e reiniciar. A aplicação voltou ao estado running.

##### 10.6.3 Binding removido pelo manifest
  
**Sintoma:** um novo deploy exibiu um diff removendo f8c-destination-service da lista de services.  
**Causa:** o manifest havia sido reescrito contendo apenas f8c-xsuaa.  
**Correção:** manter no manifest ambas as service instances, f8c-xsuaa e f8c-destination-service, e confirmar os dois bindings ativos na aplicação.

##### 10.6.4 Basic Authentication na Destination
  
**Contexto:** o HTTPS Sender do CPI não oferecia a opção None; oferecia mecanismos como User Role e Client Certificate. A alternativa User Role exigia autenticação válida para o CPI.  
**Sintoma inicial:** o primeiro teste direto retornou 401, porque os placeholders foram colados literalmente nos campos de usuário e senha.  
**Segundo teste:** com os valores corretos e o caractere especial devidamente escapado no shell, a resposta passou a ser 500, e não mais 401.  
**Interpretação:** o 500 comprova que a Basic Authentication foi aceita pelo CPI e que o request alcançou o iFlow. O erro 500 era esperado, pois o Groovy exigia um token de usuário adicional ausente na chamada direta. Aprendizado importante: o resultado 500 diferencia falha de autenticação (401) de falha funcional interna após autenticação bem-sucedida.

##### 10.6.5 Middleware customizado para duplicar o JWT do usuário
  
**Estratégia:** a Destination usaria Basic Authentication para o CPI, enquanto o JWT do usuário seria duplicado em um header dedicado por um middleware customizado registrado no Application Router.  
**Observação:** o debug comprovou a execução do middleware, mas os logs indicaram que o header de autorização não estava presente naquele estágio do pipeline.  
**Interpretação:** o navegador mantém a sessão por cookie, e o header Authorization Bearer é construído internamente pelo Application Router em um estágio posterior do pipeline. Portanto, o hook registrado não enxerga esse header no ponto em que é executado. A estratégia foi descartada para o objetivo atual.

##### 10.6.6 Encaminhamento do token do usuário com NoAuthentication
  
**Estratégia:** utilizar o encaminhamento automático do token do usuário na Destination configurada com NoAuthentication.  
**Sintoma:** o Cloud Integration rejeitou o JWT emitido pelo XSUAA da aplicação, retornando 401.  
**Interpretação:** o runtime gerenciado do Cloud Integration não aceita, como credencial válida para o endpoint, o token emitido pelo XSUAA controlado pela aplicação.

##### 10.6.7 Destination com OAuth2JWTBearer
  
**Estratégia:** reconfigurar a Destination para OAuth2JWTBearer, informando client ID e client secret do service key do CPI, a token service URL do tenant XSUAA e o tipo de URL Dedicated.  
**Erro inicial:** ao manter o encaminhamento de token junto com OAuth2JWTBearer, o Application Router retornou um erro explícito indicando que o parâmetro de encaminhamento de token não pode ser usado em destinations com tipo de autenticação diferente de NoAuthentication. O parâmetro foi então removido.  
**Sintoma seguinte:** após a remoção, o navegador passou a receber 403, e o Monitor do Cloud Integration não registrou novas mensagens, indicando falha antes da execução do iFlow.  
**Interpretação:** a ausência de trust e de cross-consumption adequados entre o XSUAA controlado pela aplicação e o serviço gerenciado que expõe o endpoint do Cloud Integration impede a aceitação da identidade nesse desenho.

#### 10.7 Troubleshooting consolidado do F8C

<table>
<tr>
<th>  
Sintoma
</th>
<th>  
Causa
</th>
<th>  
Correção ou conclusão
</th>
</tr>
<tr>
<td>  
Destination antiga reaparecendo
</td>
<td>  
Variável User-Provided destinations persistente
</td>
<td>  
Remover a variável explicitamente e refazer o restage
</td>
</tr>
<tr>
<td>  
Route references unknown destination
</td>
<td>  
Destination Service ausente ou não vinculado
</td>
<td>  
Criar e vincular a service instance de Destination
</td>
</tr>
<tr>
<td>  
Binding removido no deploy
</td>
<td>  
Manifest reescrito somente com o XSUAA
</td>
<td>  
Manter XSUAA e Destination Service no manifest
</td>
</tr>
<tr>
<td>  
401 no teste direto ao CPI
</td>
<td>  
Placeholders colados literalmente nas credenciais
</td>
<td>  
Usar os valores reais e escapar caracteres especiais no shell
</td>
</tr>
<tr>
<td>  
500 no teste direto ao CPI
</td>
<td>  
Basic Auth aceita, mas token de usuário ausente no Groovy
</td>
<td>  
Comprova que o request alcançou o iFlow após autenticação
</td>
</tr>
<tr>
<td>  
Header de autorização ausente no middleware
</td>
<td>  
Authorization Bearer construído em estágio posterior do pipeline
</td>
<td>  
Estratégia de duplicação de JWT descartada
</td>
</tr>
<tr>
<td>  
401 com token encaminhado
</td>
<td>  
CPI não aceita o JWT do XSUAA da aplicação
</td>
<td>  
Necessário trust dedicado ao runtime gerenciado
</td>
</tr>
<tr>
<td>  
403 com OAuth2JWTBearer
</td>
<td>  
Falta de trust e cross-consumption entre XSUAA e serviço gerenciado
</td>
<td>  
Boundary arquitetural do ambiente trial
</td>
</tr>
</table>

#### 10.8 Boundary arquitetural encontrado
  
As tentativas convergiram para um mesmo ponto: a identidade humana é autenticada com sucesso pela camada Application Router, XSUAA e IAS, mas o token resultante é emitido para o XSUAA controlado pela aplicação. Para que o runtime gerenciado do Cloud Integration aceite essa identidade, é necessário um relacionamento de confiança explícito e dedicado entre esse XSUAA e o serviço gerenciado que expõe o endpoint do CPI. No ambiente trial utilizado, esse relacionamento de trust e cross-consumption não estava disponível de forma direta, o que impediu concluir a propagação end-user até o iFlow.  
Este é um boundary observado no ambiente trial deste laboratório. Não deve ser generalizado como impossibilidade universal em qualquer ambiente produtivo. Desenhos corporativos com SAP BTP e SAP Cloud Integration podem estabelecer esse trust por meios oficiais e governados, o que exigiria configuração e documentação específicas fora do escopo desta investigação.

#### 10.9 Estado final do F8C

<table>
<tr>
<th>  
Item
</th>
<th>  
Estado
</th>
</tr>
<tr>
<td>  
Login humano real
</td>
<td>  
Validado
</td>
</tr>
<tr>
<td>  
Application Router
</td>
<td>  
Running
</td>
</tr>
<tr>
<td>  
XSUAA para login
</td>
<td>  
Funcional
</td>
</tr>
<tr>
<td>  
Destination Service
</td>
<td>  
Vinculado e consultável
</td>
</tr>
<tr>
<td>  
Basic Authentication ao CPI
</td>
<td>  
Validada isoladamente
</td>
</tr>
<tr>
<td>  
Propagação end-user até o CPI
</td>
<td>  
Não concluída (boundary arquitetural)
</td>
</tr>
<tr>
<td>  
Evidências do F8C
</td>
<td>  
Não geradas (pasta reservada ao próximo documento)
</td>
</tr>
</table>

#### 10.10 Limitação consciente do F8C
  
O F8C é registrado como investigação técnica honesta. A camada de autenticação humana foi comprovadamente estabelecida, enquanto a propagação da identidade até o runtime gerenciado do Cloud Integration esbarrou na ausência de um trust dedicado no ambiente trial. Nenhum sucesso de ponta a ponta é declarado para o F8C. Os próximos cenários do Bloco F podem retomar essa fronteira com um desenho de trust apropriado ou com uma aplicação de backend própria em SAP BTP que aceite diretamente o token do usuário.

### 11. Evidências
  
As evidências abaixo referem-se exclusivamente ao F8A e ao F8B e foram validadas contra os nomes técnicos definidos para a pasta evidences/lab28. O F8C não possui evidências neste documento, pois se trata de investigação arquitetural e a pasta de evidências correspondente foi reservada para o próximo documento.

#### F8A

##### Evidência 01 — iFlow de captura do contexto
  
[F8A iFlow](../evidences/lab28/01-cpi-f8a-authenticated-principal-capture-iflow.png)  
O iFlow F8A está implantado e iniciado, contendo a captura segura do contexto e a construção da resposta de diagnóstico.

##### Evidência 02 — Allowed Headers
  
[F8A Runtime Configuration](../evidences/lab28/02-cpi-f8a-principal-capture-runtime-configuration.png)  
A Runtime Configuration permite os headers de identidade somente para detectar tentativas controladas de spoofing.

##### Evidência 03 — Baseline técnico
  
[F8A Technical Client Baseline](../evidences/lab28/03-postman-f8a-technical-client-baseline.png)  
A chamada Client Credentials retorna TECHNICAL\_CLIENT, sem principal humano e sem propagação ponta a ponta.

##### Evidência 04 — Spoofing rejeitado
  
[F8A Spoofed Principal Rejected](../evidences/lab28/04-postman-f8a-spoofed-principal-header-rejected.png)  
Os headers declarados pelo caller são detectados, mas o valor permanece não aceito como identidade confiável.

#### Preparação WSO2 e OAuth

##### Evidência 05 — Console WSO2
  
[WSO2 Console](../evidences/lab28/05-wso2-f8-identity-server-console-overview.png)  
O WSO2 Identity Server está operacional e disponível para administrar aplicações, identidades, grupos e connections.

##### Evidência 06 — Grupos de compras
  
[WSO2 Purchasing Groups](../evidences/lab28/06-wso2-f8-purchasing-user-groups-created.png)  
Foram criados grupos separados para compradores e gestores.

##### Evidência 07 — Usuários de compras
  
[WSO2 Purchasing Users](../evidences/lab28/07-wso2-f8-purchasing-users-created.png)  
Foram criadas identidades fictícias para buyer e manager. O principal técnico foi criado posteriormente e associado ao grupo Buyers, processo descrito neste documento sem captura adicional.

##### Evidência 08 — Aplicação OAuth
  
[WSO2 OAuth Application](../evidences/lab28/08-wso2-f8-oauth-application-created.png)  
A aplicação confidencial OAuth/OIDC foi criada com Client ID e Client Secret protegido.

##### Evidência 09 — Grant SAML2 habilitado
  
[WSO2 SAML2 Grant](../evidences/lab28/09-wso2-f8-saml2-bearer-grant-enabled.png)  
Os grants Client Credential e SAML2 estão habilitados, enquanto grants legados permanecem desabilitados.

##### Evidência 10 — Token local por Client Credentials
  
[WSO2 Local Token](../evidences/lab28/10-postman-f8-wso2-client-credentials-token-issued.png)  
O token endpoint local emitiu Bearer token, validando a infraestrutura OAuth antes do SAML Bearer.

##### Evidência 11 — Túnel HTTPS
  
[ngrok Tunnel](../evidences/lab28/11-ngrok-f8-wso2-public-https-tunnel-active.png)  
O túnel HTTPS temporário encaminha o domínio público ao WSO2 local na porta 9443.

##### Evidência 12 — Token endpoint público
  
[WSO2 Public Token Endpoint](../evidences/lab28/12-postman-f8-wso2-public-token-endpoint-validated.png)  
A URL pública do token endpoint foi validada com Client Credentials antes de ser utilizada pelo SAP Integration Suite.

##### Evidência 13 — Key Pair
  
[CPI SAML Signing Key Pair](../evidences/lab28/13-cpi-f8-saml-bearer-signing-key-pair-created.png)  
O Key Pair RSA de 3072 bits foi criado com CN correspondente ao principal técnico. A private key permanece no tenant.

##### Evidência 14 — Issuer, Alias e trust
  
[WSO2 Issuer Alias Validation](../evidences/lab28/14-postman-f8-wso2-saml-issuer-alias-configuration-validated.png)  
A Management API confirmou Issuer, token endpoint Alias, Connection habilitada e certificado confiável configurado.

#### F8B.1 — Emissão do token

##### Evidência 15 — Token SAML Bearer emitido
  
[SAML Bearer Token Issued](../evidences/lab28/15-postman-f8b-technical-user-saml-bearer-token-issued.png)  
A assertion assinada foi aceita pelo WSO2 e trocada por Bearer token. O token foi mascarado antes da resposta.

##### Evidência 16 — Processamento do token exchange
  
[SAML Bearer Message Processing](../evidences/lab28/16-cpi-f8b-saml-bearer-token-exchange-message-processing.png)  
O Monitor comprova a execução completa da geração, assinatura, token request, validação e resposta.

#### F8B.2 — Introspecção

##### Evidência 17 — iFlow com introspecção
  
[Token Introspection iFlow](../evidences/lab28/17-cpi-f8b-saml-bearer-token-introspection-iflow.png)  
O iFlow contém uma segunda chamada outbound dedicada à introspecção do access token.

##### Evidência 18 — Token ativo e subject técnico
  
[Token Introspection Validated](../evidences/lab28/18-postman-f8b-saml-bearer-token-introspection-validated.png)  
A introspecção confirmou token ativo, tipo Bearer e subject f8.technical.purchasing.user, sem expor o token integral.

##### Evidência 19 — Processamento da introspecção
  
[Token Introspection Processing](../evidences/lab28/19-cpi-f8b-token-introspection-message-processing.png)  
O Monitor comprova os dois ciclos outbound: emissão e introspecção.

#### F8B.3 — Autorização

##### Evidência 20 — iFlow com Router de autorização
  
[Protected Resource Authorization iFlow](../evidences/lab28/20-cpi-f8b-protected-resource-authorization-iflow.png)  
O iFlow implantado contém política de autorização com caminhos concedido, negado e operação não suportada.

##### Evidência 21 — READ autorizado
  
[Purchase Requisition Read Authorized](../evidences/lab28/21-postman-f8b-purchase-requisition-read-authorized.png)  
O principal técnico do grupo Buyers recebeu 200 OK e acessou o recurso protegido de requisições de compra.

##### Evidência 22 — Caminho autorizado
  
[Authorized Resource Processing](../evidences/lab28/22-cpi-f8b-authorized-resource-message-processing.png)  
O Router selecionou Authorization\_Granted e finalizou em End\_Authorized.

##### Evidência 23 — APPROVE negado
  
[Purchase Requisition Approval Denied](../evidences/lab28/23-postman-f8b-purchase-requisition-approval-denied.png)  
O token estava ativo, mas o principal Buyers não possuía o grupo Managers. A resposta correta foi 403 Forbidden.

##### Evidência 24 — Caminho negado
  
[Authorization Denied Processing](../evidences/lab28/24-cpi-f8b-authorization-denied-message-processing.png)  
O Router selecionou Authorization\_Denied, sem retornar dados do recurso protegido.

##### Evidência 25 — Operação não suportada
  
[Unsupported Operation Rejected](../evidences/lab28/25-postman-f8b-unsupported-operation-rejected.png)  
A operação DELETE retornou 400 Bad Request e informou as operações permitidas.

##### Evidência 26 — Caminho default
  
[Unsupported Operation Processing](../evidences/lab28/26-cpi-f8b-unsupported-operation-message-processing.png)  
A rota default tratou a operação não suportada e finalizou em End\_Unsupported.

### 12. Troubleshooting

#### 12.1 SapAuthenticatedUserName ausente
  
**Sintoma:** SecurityException no F8A.  
**Causa:** o fluxo Client Credentials autenticou uma aplicação técnica, mas não disponibilizou principal humano.  
**Correção:** classificar o contexto como AUTHENTICATED\_CLIENT\_WITHOUT\_USER\_PRINCIPAL, sem confiar em headers enviados pelo caller.

#### 12.2 Headers de spoofing não chegam ao Groovy
  
**Causa:** headers customizados não estavam permitidos na Runtime Configuration.  
**Correção:** adicionar X-Authenticated-User\|X-Principal\|X-User em Allowed Headers.

#### 12.3 502 Bad Gateway no ngrok
  
**Causa:** o túnel estava online, mas o WSO2 não estava ouvindo em localhost:9443.  
**Correção:** iniciar o WSO2, validar TcpTestSucceeded : True e manter as janelas do WSO2 e ngrok abertas.

#### 12.4 Identity provider is null
  
**Causa:** o certificado estava no truststore, mas o Issuer da assertion não estava associado a uma Connection lógica no WSO2.  
**Correção:** criar a Connection SAML e associar o certificado ao Issuer.

#### 12.5 Token Endpoint alias has not been configured
  
**Causa:** a Connection foi localizada, mas o Alias do token endpoint estava vazio.  
**Correção:** atualizar homeRealmIdentifier, alias e descrição pela Identity Provider Management API.

#### 12.6 Properties aparecem como \`$
  
**Causa:** Message Body configurado como Constant.  
**Correção:** alterar o tipo para Expression.

#### 12.7 Diferença entre 401, 403 e 400

<table>
<tr>
<th>  
Código
</th>
<th>  
Significado no cenário
</th>
</tr>
<tr>
<td>  
401
</td>
<td>  
Credencial ausente ou inválida
</td>
</tr>
<tr>
<td>  
403
</td>
<td>  
Principal autenticado, mas sem permissão
</td>
</tr>
<tr>
<td>  
400
</td>
<td>  
Operação funcional não suportada
</td>
</tr>
</table>

  
No F8C, o 401 e o 403 tiveram significados adicionais específicos de propagação: 401 indicou que o CPI não aceitou o JWT do XSUAA da aplicação, e 403 indicou ausência de trust e cross-consumption adequados para o runtime gerenciado.

### 13. Boas práticas SAP e de mercado
- Manter private keys exclusivamente em Keystore gerenciado.
- Exportar apenas certificados públicos para estabelecer trust.
- Usar assertions com validade curta e tolerância mínima de clock skew.
- Validar Issuer, Audience, Recipient, assinatura, subject e validade.
- Não usar headers customizados como identidade confiável.
- Não retornar access tokens integrais em respostas, logs ou evidências.
- Separar autenticação, introspecção e autorização funcional.
- Usar 403 Forbidden para principal autenticado sem permissão.
- Aplicar princípio do menor privilégio a usuários, grupos, scopes e aplicações administrativas.
- Rotacionar chaves e certificados com sobreposição controlada.
- Proteger endpoints administrativos e de introspecção.
- Remover túneis temporários após os testes.
- Evitar contas super administrator em integrações produtivas.
- Monitorar falhas de assinatura, issuer, audience, replay e autorização.
- Para propagação de identidade humana, estabelecer trust dedicado entre o Authorization Server da aplicação e o serviço gerenciado consumido, em vez de encaminhar tokens sem relacionamento de confiança.
- Preferir SAP Cloud Identity Services (IAS) como Identity Provider corporativo, aproveitando o ciclo de ativação e a gestão de identidade governada.
- Remover variáveis de ambiente User-Provided obsoletas antes de reconfigurar destinations no Application Router.

### 14. Recomendações para produção

<table>
<tr>
<th>  
Área
</th>
<th>  
Recomendação
</th>
</tr>
<tr>
<td>  
Authorization Server
</td>
<td>  
Implementar alta disponibilidade, backup e observabilidade
</td>
</tr>
<tr>
<td>  
Endpoint público
</td>
<td>  
Usar DNS corporativo, WAF/API Gateway e certificado válido
</td>
</tr>
<tr>
<td>  
Truststore
</td>
<td>  
Controlar senha, backup, rotação e auditoria
</td>
</tr>
<tr>
<td>  
Key Pair
</td>
<td>  
Definir expiração, rotação e procedimento de revogação
</td>
</tr>
<tr>
<td>  
Assertion
</td>
<td>  
Usar janela curta e proteção contra replay
</td>
</tr>
<tr>
<td>  
Token
</td>
<td>  
Definir TTL compatível com risco e sensibilidade do recurso
</td>
</tr>
<tr>
<td>  
Introspection
</td>
<td>  
Proteger com client dedicado e escopo mínimo
</td>
</tr>
<tr>
<td>  
Authorization
</td>
<td>  
Preferir grupos, roles e scopes provenientes do Authorization Server
</td>
</tr>
<tr>
<td>  
Logs
</td>
<td>  
Não registrar assertions completas, tokens, secrets ou private keys
</td>
</tr>
<tr>
<td>  
Resiliência
</td>
<td>  
Definir timeouts, circuit breaker e tratamento de indisponibilidade
</td>
</tr>
<tr>
<td>  
Auditoria
</td>
<td>  
Correlacionar principal, client, token hash, operação e resultado
</td>
</tr>
<tr>
<td>  
Propagação de identidade
</td>
<td>  
Estabelecer trust dedicado entre XSUAA da aplicação e runtime gerenciado, ou backend próprio que aceite o token do usuário
</td>
</tr>
</table>


#### 14.1 Limitação consciente do laboratório
  
A associação funcional ao grupo F8\_PURCHASING\_BUYERS foi mantida em property controlada pelo iFlow para exercitar autorização técnica. Em produção, grupos, roles ou scopes devem ser obtidos de claims confiáveis do token ou de uma fonte de autorização governada, e não definidos estaticamente no fluxo.

#### 14.2 Limitação do Security Material nativo
  
O artifact nativo OAuth2 SAML Bearer Assertion do SAP Integration Suite oferece perfis específicos para SuccessFactors, SAP BTP Neo e SAP BTP Cloud Foundry. Como o WSO2 é um Authorization Server externo genérico, o laboratório implementou o perfil RFC 7522 de forma vendor-neutral, mantendo a private key protegida pelo XML Digital Signer.

#### 14.3 Limitação de propagação end-user no ambiente trial
  
No F8C, a propagação da identidade humana até o runtime gerenciado do Cloud Integration não foi concluída por ausência de trust e cross-consumption dedicados no ambiente trial. Em produção, esse relacionamento pode ser estabelecido por meios oficiais e governados do SAP BTP, o que exigiria configuração específica de confiança entre o XSUAA da aplicação e o serviço gerenciado consumido.

### 15. Referências oficiais
- [SAP Help — Deploying an OAuth2 SAML Bearer Assertion](https://help.sap.com/docs/cloud-integration/sap-cloud-integration/deploying-oauth2-saml-bearer-assertion)
- [SAP Help — OAuth 2.0 SAML Bearer Assertion Authentication](https://help.sap.com/docs/CX_CNS_ESM/f1437ed0c63b464983503b1a1dc6af8a/444f4b2632d0489b9145f8af4a66b8d7.html)
- [RFC 7522 — SAML 2.0 Profile for OAuth 2.0](https://www.rfc-editor.org/info/rfc7522)
- [RFC 7662 — OAuth 2.0 Token Introspection](https://www.rfc-editor.org/info/rfc7662)
- [WSO2 — Token Introspection](https://is.docs.wso2.com/en/6.1.0/references/concepts/authorization/introspection/)
- [SAP Developer Tutorial — OAuth 2.0 SAML Bearer Assertion Flow](https://developers.sap.com/tutorials/abap-environment-business-partner-oauthsamlbearer/)
- [SAP Help — SAP Cloud Identity Services](https://help.sap.com/docs/identity-authentication)
- [SAP Help — Application Router](https://help.sap.com/docs/btp/sap-business-technology-platform/application-router)
- [SAP Help — Authorization and Trust Management (XSUAA)](https://help.sap.com/docs/btp/sap-business-technology-platform/authorization-and-trust-management-in-cloud-foundry-environment)
- [SAP Help — Destination Service](https://help.sap.com/docs/connectivity/sap-btp-connectivity-cf/consuming-destination-service)

### 16. Resultado final

<table>
<tr>
<th>  
Controle
</th>
<th>  
Resultado
</th>
</tr>
<tr>
<td>  
Contexto técnico inbound identificado
</td>
<td>  
Validado
</td>
</tr>
<tr>
<td>  
Tentativa de header spoofing rejeitada
</td>
<td>  
Validado
</td>
</tr>
<tr>
<td>  
Assertion SAML 2.0 construída
</td>
<td>  
Validado
</td>
</tr>
<tr>
<td>  
Assertion assinada com private key protegida
</td>
<td>  
Validado
</td>
</tr>
<tr>
<td>  
Trust lógico e criptográfico no WSO2
</td>
<td>  
Validado
</td>
</tr>
<tr>
<td>  
OAuth SAML Bearer token exchange
</td>
<td>  
Validado
</td>
</tr>
<tr>
<td>  
Token introspection
</td>
<td>  
Validado
</td>
</tr>
<tr>
<td>  
Subject técnico reconhecido
</td>
<td>  
Validado
</td>
</tr>
<tr>
<td>  
READ para Buyers
</td>
<td>  
Autorizado
</td>
</tr>
<tr>
<td>  
APPROVE para Buyers
</td>
<td>  
Negado com 403
</td>
</tr>
<tr>
<td>  
Operação DELETE
</td>
<td>  
Rejeitada com 400
</td>
</tr>
<tr>
<td>  
Access token integral exposto
</td>
<td>  
Não
</td>
</tr>
<tr>
<td>  
F8C — Login humano real via IAS
</td>
<td>  
Validado
</td>
</tr>
<tr>
<td>  
F8C — Basic Authentication ao CPI
</td>
<td>  
Validada isoladamente
</td>
</tr>
<tr>
<td>  
F8C — Propagação end-user até o CPI
</td>
<td>  
Não concluída (boundary arquitetural)
</td>
</tr>
<tr>
<td>  
F8C — Evidências
</td>
<td>  
Não geradas por decisão de escopo
</td>
</tr>
</table>

  
O F8A e o F8B demonstraram uma cadeia completa de autenticação, confiança federada, emissão de token, introspecção e autorização funcional. O cenário também comprovou que token válido não significa autorização irrestrita e que a identidade confiável deve ser derivada de mecanismos criptográficos e do Authorization Server, nunca de headers arbitrários enviados pelo consumidor. O F8C avançou sobre a propagação de identidade humana real: o login interativo via SAP Cloud Identity Services, Application Router e XSUAA foi validado, mas a propagação end-user até o runtime gerenciado do Cloud Integration esbarrou na ausência de um trust dedicado no ambiente trial, ficando registrada como boundary arquitetural honesto e ponto de partida para os próximos cenários do Bloco F.  
[← Bloco anterior: F7 — PGP Message-Level Security](./29-f7-pgp-message-level-security.md)  [Próximo cenário: F8D — SAML Web SSO Federation](./32-f8-saml-web-sso-federation.md)
