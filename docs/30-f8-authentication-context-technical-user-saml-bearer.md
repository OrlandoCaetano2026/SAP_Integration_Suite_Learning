# F8A–F8B — Authentication Context and Technical User OAuth 2.0 SAML Bearer

> **Bloco F — Segurança | Documento 30**  
> **Escopo:** F8A Authentication Context Capture e F8B Technical User SAML Bearer, incluindo token exchange, introspecção e autorização de recurso protegido.

[← Bloco anterior: F7 — PGP Message-Level Security](./29-f7-pgp-message-level-security.md) | [Próximo cenário: F8C–F8E — End-User Principal Propagation and Authorization](./31-f8-end-user-principal-propagation-authorization.md)

---

## 1. Visão executiva

Este laboratório implementa uma cadeia completa de segurança e autorização baseada em identidade técnica entre o SAP Integration Suite e o WSO2 Identity Server. O cenário parte de uma constatação importante: uma chamada inbound autenticada por OAuth Client Credentials comprova a identidade da aplicação cliente, mas não disponibiliza automaticamente um usuário humano confiável ao Integration Flow.

O laboratório foi dividido em duas partes complementares:

- **F8A — Authentication Context Capture:** classifica o contexto inbound, comprova a ausência de um principal humano no fluxo Client Credentials e rejeita tentativas de identity spoofing por headers HTTP.
- **F8B — Technical User OAuth 2.0 SAML Bearer:** cria uma assertion SAML 2.0 de curta duração, assina o XML com uma private key protegida no Keystore do SAP Integration Suite, troca a assertion por um access token OAuth no WSO2, introspecta o token e aplica autorização funcional a um recurso protegido de SAP MM.

O resultado final comprova quatro controles distintos:

1. **Autenticação do caller:** o endpoint inbound foi acessado por uma aplicação autorizada.
2. **Confiança federada:** o WSO2 confiou no certificado público associado à chave privada do SAP Integration Suite.
3. **Token exchange:** a assertion SAML assinada foi aceita como authorization grant conforme o perfil OAuth 2.0 SAML Bearer.
4. **Autorização funcional:** o principal técnico foi autorizado a consultar requisições de compra, impedido de aprová-las e bloqueado ao solicitar uma operação não suportada.

---

## 2. Perfil técnico do cenário

| Item | Valor |
|---|---|
| Integration Flow F8A | `F8A_MM_Authenticated_Principal_Capture` |
| Integration Flow F8B | `F8B_MM_SAML_Bearer_Technical_User_Token` |
| Endpoint F8A | `/f8a/mm/authenticated-principal` |
| Endpoint F8B | `/f8b/mm/saml-bearer/technical-user/token` |
| Authorization Server | WSO2 Identity Server 7.0.0 |
| Java Runtime | Eclipse Temurin JDK 17 |
| OAuth Grant | `urn:ietf:params:oauth:grant-type:saml2-bearer` |
| Introspection | OAuth 2.0 Token Introspection |
| Technical Principal | `f8.technical.purchasing.user` |
| Buyers Group | `F8_PURCHASING_BUYERS` |
| Managers Group | `F8_PURCHASING_MANAGERS` |
| Key Pair Alias | `f8_saml_bearer_signing` |
| Key Type | RSA |
| Key Size | 3072 bits |
| Certificate Signature Algorithm | `SHA512withRSA` |
| XML Signature Algorithm | `SHA512/RSA` |
| XML Digest Algorithm | `SHA256` |
| Token Endpoint | `https://<ngrok-domain>/oauth2/token` |
| Introspection Endpoint | `https://<ngrok-domain>/oauth2/introspect` |
| Protected Resource | `SAP_MM_PURCHASE_REQUISITIONS` |
| Supported Operations | `READ`, `APPROVE` |

> O domínio ngrok utilizado no laboratório é temporário. Em produção, deve ser substituído por endpoint estável, certificado público confiável, controle de rede e disponibilidade compatível com o SLA da integração.

---

## 3. Arquitetura

### 3.1 F8A — Contexto inbound

```text
Postman
→ HTTPS Sender protegido por ESBMessaging.send
→ captura do contexto de autenticação
→ classificação do principal
→ detecção de headers declarados pelo caller
→ resposta de diagnóstico
```

### 3.2 F8B — Technical User SAML Bearer

```text
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
```

### 3.3 Cadeia de confiança

```text
SAP Integration Suite Keystore
→ private key f8_saml_bearer_signing
→ assina a assertion

WSO2 Identity Server
→ certificado público correspondente
→ valida a assinatura
→ associa Issuer e token endpoint Alias
→ emite e introspecta o access token
```

---

## 4. Fundamentos de segurança

### 4.1 Client Credentials não representa automaticamente um usuário humano

O grant Client Credentials autentica a aplicação cliente. No F8A, o runtime confirmou o acesso ao endpoint, mas não disponibilizou `SapAuthenticatedUserName` nem outro principal humano confiável. O contexto foi classificado como `TECHNICAL_CLIENT`.

### 4.2 Header declarado pelo caller não é identidade autenticada

Headers como `X-Authenticated-User`, `X-Principal` e `X-User` são dados controlados pelo consumidor. O laboratório permitiu esses headers apenas para detectar a tentativa, mas nunca os utilizou como fonte confiável de identidade.

### 4.3 SAML Bearer não é SAML Web SSO

| OAuth 2.0 SAML Bearer | SAML Web SSO |
|---|---|
| Troca uma assertion por access token | Cria uma sessão de login federado |
| Não exige navegador | Normalmente envolve navegador |
| Usa token endpoint | Usa SSO URL e ACS URL |
| Adequado a chamadas backend | Adequado a autenticação interativa |

### 4.4 Autenticação e autorização são controles diferentes

| Controle | Pergunta respondida |
|---|---|
| Autenticação | Quem ou qual aplicação apresentou a credencial? |
| Introspecção | O token está ativo e a qual contexto pertence? |
| Autorização | O principal pode executar a operação solicitada? |

Um token ativo não concede automaticamente todas as operações. O `403 Forbidden` do teste de aprovação comprova que o principal estava autenticado, mas não possuía o grupo funcional exigido.

---

## 5. F8A — Authentication Context Capture

### 5.1 Estrutura do iFlow

```text
HTTPS Sender
→ Capture_Authenticated_Principal
→ Build_Principal_Diagnostic_Response
→ End
```

### 5.2 HTTPS Sender

| Parâmetro | Valor |
|---|---|
| Address | `/f8a/mm/authenticated-principal` |
| Authorization | `User Role` |
| User Role | `ESBMessaging.send` |
| CSRF Protected | Disabled |

### 5.3 Runtime Configuration

```text
X-Authenticated-User|X-Principal|X-User
```

Esses headers foram liberados somente para o teste controlado de identity spoofing.

### 5.4 Groovy `CaptureAuthenticatedPrincipal.groovy`

```groovy
import com.sap.gateway.ip.core.customdev.util.Message
import java.security.MessageDigest
import java.time.Instant

def Message processData(Message message) {
    Map<String, Object> headers = message.getHeaders()
    String sapAuthenticatedUserName = readHeader(headers, "SapAuthenticatedUserName")
    String camelAuthenticatedUser = readHeader(headers, "CamelAuthenticatedUser")
    String servletRemoteUser = readServletRemoteUser(headers)
    String claimedPrincipal = getFirstAvailableHeader(
        headers,
        ["X-Authenticated-User", "X-Principal", "X-User"]
    )
    String authenticatedPrincipal = firstNonEmptyValue(
        [sapAuthenticatedUserName, camelAuthenticatedUser, servletRemoteUser]
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
        principalCaptureStatus = "AUTHENTICATED_PRINCIPAL_AVAILABLE"
    } else {
        authenticatedPrincipal = "NOT_EXPOSED_BY_RUNTIME"
        identitySource = "SERVICE_INSTANCE_AUTHENTICATION_CONTEXT"
        principalType = "TECHNICAL_CLIENT"
        principalCaptureStatus = "AUTHENTICATED_CLIENT_WITHOUT_USER_PRINCIPAL"
    }
    boolean claimedPrincipalProvided = claimedPrincipal != null && !claimedPrincipal.trim().isEmpty()
    String principalSha256 = authenticatedPrincipal == "NOT_EXPOSED_BY_RUNTIME"
        ? "NOT_CALCULATED"
        : calculateSha256(authenticatedPrincipal.toLowerCase())
    List<String> diagnosticHeaderNames = getDiagnosticHeaderNames(headers)
    message.setProperty("authenticatedPrincipal", authenticatedPrincipal)
    message.setProperty("principalType", principalType)
    message.setProperty("principalSha256", principalSha256)
    message.setProperty("identitySource", identitySource)
    message.setProperty("principalCaptureStatus", principalCaptureStatus)
    message.setProperty("spoofingAttemptDetected", claimedPrincipalProvided.toString())
    message.setProperty("claimedPrincipal", claimedPrincipalProvided ? claimedPrincipal : "NOT_PROVIDED")
    message.setProperty("claimedPrincipalAccepted", "false")
    message.setProperty(
        "diagnosticHeaderNames",
        diagnosticHeaderNames.isEmpty() ? "NONE" : diagnosticHeaderNames.join(", ")
    )
    message.setProperty("propagationStage", "F8A_PRINCIPAL_CAPTURE_BASELINE")
    message.setProperty("propagationMode", "NOT_DETERMINED")
    message.setProperty("endToEndPropagation", "NOT_EXECUTED")
    message.setProperty("capturedAt", Instant.now().toString())
    message.setHeader("Content-Type", "application/json")
    return message
}

def String readHeader(Map<String, Object> headers, String headerName) {
    Object headerValue = headers.get(headerName)
    if (headerValue == null) {
        return null
    }
    String normalizedValue = headerValue.toString().trim()
    return normalizedValue.isEmpty() ? null : normalizedValue
}

def String readServletRemoteUser(Map<String, Object> headers) {
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

def String getFirstAvailableHeader(Map<String, Object> headers, List<String> headerNames) {
    for (String headerName : headerNames) {
        String headerValue = readHeader(headers, headerName)
        if (headerValue) {
            return headerValue
        }
    }
    return null
}

def String firstNonEmptyValue(List<String> values) {
    return values.find { value -> value != null && !value.trim().isEmpty() }
}

def String determineIdentitySource(
    String sapAuthenticatedUserName,
    String camelAuthenticatedUser,
    String servletRemoteUser
) {
    if (sapAuthenticatedUserName) {
        return "SAP_AUTHENTICATED_USER_NAME"
    }
    if (camelAuthenticatedUser) {
        return "CAMEL_AUTHENTICATED_USER"
    }
    if (servletRemoteUser) {
        return "HTTPS_SERVLET_REMOTE_USER"
    }
    return "UNKNOWN"
}

def String classifyPrincipal(String authenticatedPrincipal) {
    String normalizedPrincipal = authenticatedPrincipal.toLowerCase()
    List<String> technicalPatterns = [
        "client", "service", "technical", "oauth", "runtime", "integration", "sb-"
    ]
    boolean technicalPatternDetected = technicalPatterns.any { pattern ->
        normalizedPrincipal.contains(pattern)
    }
    if (technicalPatternDetected) {
        return "TECHNICAL_CLIENT"
    }
    if (normalizedPrincipal.contains("@") && normalizedPrincipal.contains(".")) {
        return "HUMAN_USER_CANDIDATE"
    }
    return "AUTHENTICATED_PRINCIPAL"
}

def List<String> getDiagnosticHeaderNames(Map<String, Object> headers) {
    List<String> allowedHeaderNames = [
        "SapAuthenticatedUserName",
        "CamelAuthenticatedUser",
        "CamelHttpMethod",
        "CamelHttpPath",
        "CamelServletContextPath",
        "CamelHttpServletRequest"
    ]
    return allowedHeaderNames.findAll { headerName -> headers.containsKey(headerName) }
}

def String calculateSha256(String content) {
    MessageDigest digest = MessageDigest.getInstance("SHA-256")
    byte[] hash = digest.digest(content.getBytes("UTF-8"))
    return hash.collect { byteValue -> String.format("%02x", byteValue & 0xff) }.join()
}
```

### 5.5 Testes do F8A

| Teste | Entrada | Resultado |
|---|---|---|
| Baseline | Client Credentials, sem headers de identidade | `TECHNICAL_CLIENT`, sem principal humano |
| Spoofing | `X-Authenticated-User` e `X-Principal` | Detectado, mas `claimedPrincipalAccepted=false` |

---

## 6. Preparação do WSO2 Identity Server

### 6.1 Identidades e grupos

| Identidade | Buyers | Managers |
|---|:---:|:---:|
| `buyer.user` | Sim | Não |
| `purchasing.manager` | Sim | Sim |
| `f8.technical.purchasing.user` | Sim | Não |

### 6.2 Aplicação OAuth

| Parâmetro | Configuração |
|---|---|
| Application | `F8 SAP Integration Suite SAML Bearer Client` |
| Type | Standard-Based Application |
| Protocol | OAuth 2.0/OpenID Connect |
| Client Type | Confidential |
| Grants | Client Credential, SAML2 |
| Password Grant | Disabled |
| Implicit Grant | Disabled |

### 6.3 Endpoint público temporário

```text
https://<ngrok-domain>
```

```text
Token endpoint: https://<ngrok-domain>/oauth2/token
Introspection endpoint: https://<ngrok-domain>/oauth2/introspect
```

### 6.4 Key Pair de assinatura

| Parâmetro | Valor |
|---|---|
| Alias | `f8_saml_bearer_signing` |
| Common Name | `f8.technical.purchasing.user` |
| Key Type | RSA |
| Key Size | 3072 bits |
| Signature Algorithm | `SHA512withRSA` |
| Private Key | Mantida exclusivamente no Keystore do SAP Integration Suite |
| Public Certificate | Importado no truststore do WSO2 |

### 6.5 Trust lógico do Issuer

| Campo | Valor |
|---|---|
| Connection | `F8 SAP Integration Suite SAML Assertion Issuer` |
| Issuer | `F8 SAP Integration Suite SAML Assertion Issuer` |
| Alias | `https://<ngrok-domain>/oauth2/token` |
| Trusted Certificate | Certificado público do alias `f8_saml_bearer_signing` |

O certificado público estabelece confiança criptográfica. O Issuer e o Alias estabelecem a associação lógica usada pelo processador SAML Bearer do WSO2.

---

## 7. F8B.1 — SAML Bearer Token Exchange

### 7.1 Estrutura inicial

```text
HTTPS Sender
→ Build_SAML_Bearer_Assertion
→ Sign_SAML_Bearer_Assertion
→ Prepare_OAuth_Token_Request
→ Request Reply Token
→ WSO2 Token Endpoint
→ Validate_OAuth_Token_Response
```

### 7.2 Assertion SAML

| Elemento | Valor |
|---|---|
| Version | `2.0` |
| Issuer | `F8 SAP Integration Suite SAML Assertion Issuer` |
| Subject/NameID | `f8.technical.purchasing.user` |
| Confirmation Method | `urn:oasis:names:tc:SAML:2.0:cm:bearer` |
| Audience | Token endpoint público |
| Recipient | Token endpoint público |
| NotBefore | Horário atual menos 30 segundos |
| NotOnOrAfter | Horário atual mais 5 minutos |
| Group Attribute | `F8_PURCHASING_BUYERS` |

### 7.3 Groovy `BuildSAMLBearerAssertion.groovy`

```groovy
import com.sap.gateway.ip.core.customdev.util.Message
import java.time.Instant
import java.time.temporal.ChronoUnit
import java.util.UUID

def Message processData(Message message) {
    Instant now = Instant.now()
    Instant notBefore = now.minus(30, ChronoUnit.SECONDS)
    Instant notOnOrAfter = now.plus(5, ChronoUnit.MINUTES)
    String assertionId = "_${UUID.randomUUID().toString()}"
    String issuer = "F8 SAP Integration Suite SAML Assertion Issuer"
    String subject = "f8.technical.purchasing.user"
    String tokenServiceUrl = "https://<ngrok-domain>/oauth2/token"
    String issueInstant = now.toString()
    String notBeforeValue = notBefore.toString()
    String notOnOrAfterValue = notOnOrAfter.toString()
    String assertion = """<saml2:Assertion xmlns:saml2="urn:oasis:names:tc:SAML:2.0:assertion" ID="${assertionId}" IssueInstant="${issueInstant}" Version="2.0">
    <saml2:Issuer>${escapeXml(issuer)}</saml2:Issuer>
    <saml2:Subject>
        <saml2:NameID Format="urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified">${escapeXml(subject)}</saml2:NameID>
        <saml2:SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
            <saml2:SubjectConfirmationData NotOnOrAfter="${notOnOrAfterValue}" Recipient="${escapeXml(tokenServiceUrl)}"/>
        </saml2:SubjectConfirmation>
    </saml2:Subject>
    <saml2:Conditions NotBefore="${notBeforeValue}" NotOnOrAfter="${notOnOrAfterValue}">
        <saml2:AudienceRestriction>
            <saml2:Audience>${escapeXml(tokenServiceUrl)}</saml2:Audience>
        </saml2:AudienceRestriction>
    </saml2:Conditions>
    <saml2:AuthnStatement AuthnInstant="${issueInstant}">
        <saml2:AuthnContext>
            <saml2:AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PreviousSession</saml2:AuthnContextClassRef>
        </saml2:AuthnContext>
    </saml2:AuthnStatement>
    <saml2:AttributeStatement>
        <saml2:Attribute Name="userId">
            <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string" xmlns:xs="http://www.w3.org/2001/XMLSchema">${escapeXml(subject)}</saml2:AttributeValue>
        </saml2:Attribute>
        <saml2:Attribute Name="email">
            <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string" xmlns:xs="http://www.w3.org/2001/XMLSchema">f8-technical-user@example.invalid</saml2:AttributeValue>
        </saml2:Attribute>
        <saml2:Attribute Name="groups">
            <saml2:AttributeValue xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="xs:string" xmlns:xs="http://www.w3.org/2001/XMLSchema">F8_PURCHASING_BUYERS</saml2:AttributeValue>
        </saml2:Attribute>
    </saml2:AttributeStatement>
</saml2:Assertion>"""
    message.setProperty("samlAssertionId", assertionId)
    message.setProperty("samlIssuer", issuer)
    message.setProperty("samlSubject", subject)
    message.setProperty("samlAudience", tokenServiceUrl)
    message.setProperty("samlRecipient", tokenServiceUrl)
    message.setProperty("samlIssueInstant", issueInstant)
    message.setProperty("samlNotBefore", notBeforeValue)
    message.setProperty("samlNotOnOrAfter", notOnOrAfterValue)
    message.setProperty("samlKeyPairAlias", "f8_saml_bearer_signing")
    message.setProperty("tokenServiceUrl", tokenServiceUrl)
    message.setProperty("propagationMode", "TECHNICAL_USER_SAML_BEARER")
    message.setProperty("assertionStatus", "BUILT_NOT_SIGNED")
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
        .replace("<", "&lt;")
        .replace(">", "&gt;")
        .replace("\"", "&quot;")
        .replace("'", "&apos;")
}
```

### 7.4 XML Digital Signer

| Parâmetro | Valor |
|---|---|
| Private Key Alias | `f8_saml_bearer_signing` |
| Signature Algorithm | `SHA512/RSA` |
| Digest Algorithm | `SHA256` |
| Signature Type | Enveloped XML Signature |
| Parent Node Name | `Assertion` |
| Parent Node Namespace | `urn:oasis:names:tc:SAML:2.0:assertion` |
| Canonicalization | Exclusive XML Canonicalization 1.0 |
| X.509 Certificate Chain | Enabled |
| XML Declaration | Excluded |
| DOCTYPE | Disallowed |

### 7.5 Groovy `PrepareOAuthSAMLTokenRequest.groovy`

```groovy
import com.sap.gateway.ip.core.customdev.util.Message
import java.nio.charset.StandardCharsets
import java.util.Base64

def Message processData(Message message) {
    String signedAssertion = message.getBody(String)
    if (!signedAssertion || signedAssertion.trim().isEmpty()) {
        throw new IllegalArgumentException("The signed SAML assertion cannot be empty.")
    }
    if (!signedAssertion.contains("<ds:Signature")) {
        throw new SecurityException("The SAML assertion does not contain an XML digital signature.")
    }
    String encodedAssertion = Base64
        .getUrlEncoder()
        .withoutPadding()
        .encodeToString(signedAssertion.getBytes(StandardCharsets.UTF_8))
    String grantType = "urn:ietf:params:oauth:grant-type:saml2-bearer"
    String formBody = "grant_type=${urlEncode(grantType)}&assertion=${urlEncode(encodedAssertion)}"
    message.setProperty("signedSamlAssertion", signedAssertion)
    message.setProperty("encodedAssertionLength", encodedAssertion.length().toString())
    message.setProperty("oauthGrantType", grantType)
    message.setProperty("assertionStatus", "SIGNED_AND_ENCODED")
    message.setProperty("tokenRequestPrepared", "true")
    message.setBody(formBody)
    message.setHeader("CamelHttpMethod", "POST")
    message.setHeader("Content-Type", "application/x-www-form-urlencoded")
    message.setHeader("Accept", "application/json")
    return message
}

def String urlEncode(String value) {
    return java.net.URLEncoder
        .encode(value, StandardCharsets.UTF_8.name())
        .replace("+", "%20")
}
```

---

## 8. F8B.2 — Token Introspection

### 8.1 Objetivo

Confirmar, por meio do Authorization Server, que o token emitido está ativo, é do tipo Bearer, pertence ao cliente esperado, não expirou e representa o subject técnico da assertion.

### 8.2 Configuração do WSO2

```toml
[[resource.access_control]]
context="(.*)/oauth2/introspect(.*)"
http_method="all"
secure=true
allowed_auth_handlers="BasicClientAuthentication"
```

### 8.3 Groovy `ValidateOAuthTokenResponse.groovy`

```groovy
import com.sap.gateway.ip.core.customdev.util.Message
import groovy.json.JsonSlurper
import java.security.MessageDigest
import java.time.Instant

def Message processData(Message message) {
    Reader responseReader = message.getBody(Reader)
    String responseBody = responseReader != null ? responseReader.text : ""
    Integer responseCode = resolveHttpResponseCode(message)
    if (!responseBody || responseBody.trim().isEmpty()) {
        responseBody = "{}"
    }
    def responsePayload
    try {
        responsePayload = new JsonSlurper().parseText(responseBody)
    } catch (Exception ignored) {
        responsePayload = [
            error: "non_json_response",
            error_description: responseBody.take(500)
        ]
    }
    if (responseCode >= 400) {
        String oauthError = responsePayload.error?.toString()?.trim() ?: "oauth_token_exchange_failed"
        String oauthErrorDescription = responsePayload.error_description?.toString()?.trim()
            ?: "The WSO2 token endpoint rejected the SAML bearer assertion."
        message.setProperty("tokenExchangeStatus", "SAML_BEARER_TOKEN_REJECTED")
        message.setProperty("responseCode", responseCode.toString())
        message.setProperty("responseStatusCode", "F8B-SAML-400")
        message.setProperty("responseMessage", "The WSO2 token endpoint rejected the SAML bearer token exchange.")
        message.setProperty("oauthError", oauthError)
        message.setProperty("oauthErrorDescription", oauthErrorDescription)
        message.setProperty("tokenType", "NOT_ISSUED")
        message.setProperty("expiresIn", "NOT_PROVIDED")
        message.setProperty("scope", "NOT_PROVIDED")
        message.setProperty("internalAccessToken", "NOT_AVAILABLE")
        message.setProperty("maskedAccessToken", "NOT_ISSUED")
        message.setProperty("accessTokenSha256", "NOT_CALCULATED")
        message.setProperty("tokenIssuedAt", "NOT_ISSUED")
        message.setProperty("technicalPrincipal", "f8.technical.purchasing.user")
        message.setProperty("technicalPrincipalGroup", "F8_PURCHASING_BUYERS")
        message.setProperty("tokenIntrospectionStatus", "NOT_EXECUTED")
        message.setHeader("CamelHttpResponseCode", 400)
        message.setHeader("Content-Type", "application/json")
        return message
    }
    String accessToken = responsePayload.access_token?.toString()?.trim()
    String tokenType = responsePayload.token_type?.toString()?.trim()
    String expiresIn = responsePayload.expires_in?.toString()?.trim()
    String scope = responsePayload.scope?.toString()?.trim()
    if (!accessToken) {
        throw new SecurityException("The WSO2 response does not contain an access token.")
    }
    if (!tokenType?.equalsIgnoreCase("Bearer")) {
        throw new SecurityException("The WSO2 response does not contain a Bearer token.")
    }
    message.setProperty("tokenExchangeStatus", "SAML_BEARER_TOKEN_ISSUED")
    message.setProperty("responseCode", "200")
    message.setProperty("responseStatusCode", "F8B-SAML-200")
    message.setProperty("responseMessage", "The technical user SAML bearer assertion was exchanged for an OAuth access token.")
    message.setProperty("oauthError", "NONE")
    message.setProperty("oauthErrorDescription", "NONE")
    message.setProperty("tokenType", tokenType)
    message.setProperty("expiresIn", expiresIn ?: "NOT_PROVIDED")
    message.setProperty("scope", scope ?: "NOT_PROVIDED")
    message.setProperty("internalAccessToken", accessToken)
    message.setProperty("maskedAccessToken", maskToken(accessToken))
    message.setProperty("accessTokenSha256", calculateSha256(accessToken))
    message.setProperty("tokenIssuedAt", Instant.now().toString())
    message.setProperty("technicalPrincipal", "f8.technical.purchasing.user")
    message.setProperty("technicalPrincipalGroup", "F8_PURCHASING_BUYERS")
    message.setProperty("tokenIntrospectionStatus", "PENDING")
    message.setHeader("CamelHttpResponseCode", 200)
    message.setHeader("Content-Type", "application/json")
    return message
}

def Integer resolveHttpResponseCode(Message message) {
    Map<String, Object> headers = message.getHeaders()
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
    if (token.length() <= 12) {
        return "********"
    }
    return token.substring(0, 8) + "-****-****-" + token.substring(token.length() - 4)
}

def String calculateSha256(String content) {
    MessageDigest digest = MessageDigest.getInstance("SHA-256")
    byte[] hash = digest.digest(content.getBytes("UTF-8"))
    return hash.collect { byteValue -> String.format("%02x", byteValue & 0xff) }.join()
}
```

### 8.4 Groovy `PrepareTokenIntrospectionRequest.groovy`

```groovy
import com.sap.gateway.ip.core.customdev.util.Message
import java.net.URLEncoder
import java.nio.charset.StandardCharsets

def Message processData(Message message) {
    String accessToken = message.getProperty("internalAccessToken")?.toString()?.trim()
    if (!accessToken || accessToken == "NOT_AVAILABLE") {
        throw new SecurityException("The internal access token is not available for introspection.")
    }
    String formBody = "token=" + URLEncoder.encode(accessToken, StandardCharsets.UTF_8.name())
    message.setBody(formBody)
    message.setHeader("CamelHttpMethod", "POST")
    message.setHeader("Content-Type", "application/x-www-form-urlencoded")
    message.setHeader("Accept", "application/json")
    message.setProperty("tokenIntrospectionStatus", "REQUEST_PREPARED")
    message.setProperty("introspectionRequestPrepared", "true")
    return message
}
```

### 8.5 Groovy `ValidateTokenIntrospectionResponse.groovy`

```groovy
import com.sap.gateway.ip.core.customdev.util.Message
import groovy.json.JsonSlurper
import java.time.Instant

def Message processData(Message message) {
    Reader responseReader = message.getBody(Reader)
    String responseBody = responseReader != null ? responseReader.text : null
    if (!responseBody || responseBody.trim().isEmpty()) {
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
    boolean tokenActive = introspectionResponse.active == true ||
        introspectionResponse.active?.toString()?.equalsIgnoreCase("true")
    String tokenType = introspectionResponse.token_type?.toString()?.trim() ?: "NOT_PROVIDED"
    String authenticationContext = introspectionResponse.auth?.toString()?.trim() ?: "NOT_PROVIDED"
    String clientId = introspectionResponse.client_id?.toString()?.trim() ?: "NOT_PROVIDED"
    String audience = introspectionResponse.aud?.toString()?.trim() ?: "NOT_PROVIDED"
    String subject = resolveSubject(introspectionResponse)
    String issuedAt = introspectionResponse.iat?.toString()?.trim() ?: "NOT_PROVIDED"
    String notBefore = introspectionResponse.nbf?.toString()?.trim() ?: "NOT_PROVIDED"
    String expiresAt = introspectionResponse.exp?.toString()?.trim() ?: "NOT_PROVIDED"
    if (!tokenActive) {
        clearSensitiveToken(message)
        throw new SecurityException("The WSO2 introspection endpoint reported an inactive token.")
    }
    if (!tokenType.equalsIgnoreCase("Bearer")) {
        clearSensitiveToken(message)
        throw new SecurityException("The introspected token is not a Bearer token.")
    }
    if (expiresAt != "NOT_PROVIDED" && !isExpirationInFuture(expiresAt)) {
        clearSensitiveToken(message)
        throw new SecurityException("The introspected token has already expired.")
    }
    message.setProperty("tokenExchangeStatus", "SAML_BEARER_TOKEN_ISSUED_AND_INTROSPECTED")
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
    List<String> candidates = [
        introspectionResponse.sub?.toString()?.trim(),
        introspectionResponse.username?.toString()?.trim(),
        introspectionResponse.user_name?.toString()?.trim()
    ]
    String subject = candidates.find { candidate -> candidate != null && !candidate.isEmpty() }
    return subject ?: "NOT_PROVIDED"
}

def boolean isExpirationInFuture(String expirationValue) {
    try {
        long expirationEpoch = Long.parseLong(expirationValue)
        return expirationEpoch > Instant.now().getEpochSecond()
    } catch (Exception ignored) {
        return true
    }
}

def void clearSensitiveToken(Message message) {
    message.setProperty("internalAccessToken", "REMOVED_AFTER_INTROSPECTION")
}
```

---

## 9. F8B.3 — Protected Resource Authorization

### 9.1 Política funcional

| Operação | Grupo exigido | Buyers | Managers |
|---|---|:---:|:---:|
| `READ` | `F8_PURCHASING_BUYERS` | Autorizado | Autorizado |
| `APPROVE` | `F8_PURCHASING_MANAGERS` | Negado | Autorizado |
| Outro valor | Não aplicável | Não avaliado | Não avaliado |

O technical user do F8B pertence somente a `F8_PURCHASING_BUYERS`. Por isso, `READ` retorna 200, enquanto `APPROVE` retorna 403.

### 9.2 Runtime Configuration

```text
X-F8-Operation
```

O header declara a operação solicitada, mas não determina a identidade nem concede permissão.

### 9.3 Groovy `PrepareAuthorizationContext.groovy`

```groovy
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
        requestedOperation = "NOT_PROVIDED"
    }
    if (!tokenActive?.equalsIgnoreCase("true")) {
        throw new SecurityException("Authorization cannot continue because the introspected token is not active.")
    }
    if (!tokenIntrospectionStatus?.equalsIgnoreCase("VALIDATED")) {
        throw new SecurityException("Authorization cannot continue because token introspection was not validated.")
    }
    if (!introspectionSubject || introspectionSubject == "NOT_PROVIDED") {
        throw new SecurityException("Authorization cannot continue because the introspected subject is not available.")
    }
    if (!expectedPrincipal || !introspectionSubject.equalsIgnoreCase(expectedPrincipal)) {
        throw new SecurityException("The introspected subject does not match the expected technical principal.")
    }
    List<String> availableGroups = parseGroups(availableGroupsValue)
    String requiredGroup
    String authorizationDecision
    String authorizationReason
    String operationDescription
    switch (requestedOperation) {
        case "READ":
            requiredGroup = "F8_PURCHASING_BUYERS"
            operationDescription = "Read SAP MM purchase requisitions"
            if (availableGroups.contains("F8_PURCHASING_BUYERS") ||
                availableGroups.contains("F8_PURCHASING_MANAGERS")) {
                authorizationDecision = "AUTHORIZED"
                authorizationReason = "The introspected principal belongs to a group authorized to read SAP MM purchase requisitions."
            } else {
                authorizationDecision = "DENIED"
                authorizationReason = "The introspected principal does not belong to a group authorized to read SAP MM purchase requisitions."
            }
            break
        case "APPROVE":
            requiredGroup = "F8_PURCHASING_MANAGERS"
            operationDescription = "Approve SAP MM purchase requisitions"
            if (availableGroups.contains("F8_PURCHASING_MANAGERS")) {
                authorizationDecision = "AUTHORIZED"
                authorizationReason = "The introspected principal belongs to the purchasing managers group."
            } else {
                authorizationDecision = "DENIED"
                authorizationReason = "The introspected principal belongs to the buyers group but not to the purchasing managers group."
            }
            break
        default:
            requiredGroup = "NOT_APPLICABLE"
            operationDescription = "Unsupported protected resource operation"
            authorizationDecision = "UNSUPPORTED"
            authorizationReason = "The requested operation is not supported. Allowed operations are READ and APPROVE."
    }
    message.setProperty("requestedOperation", requestedOperation)
    message.setProperty("operationDescription", operationDescription)
    message.setProperty("protectedResource", "SAP_MM_PURCHASE_REQUISITIONS")
    message.setProperty("requiredGroup", requiredGroup)
    message.setProperty("availableGroups", availableGroups.isEmpty() ? "NONE" : availableGroups.join(","))
    message.setProperty("matchedGroup", resolveMatchedGroup(authorizationDecision, requiredGroup, availableGroups))
    message.setProperty("authorizationDecision", authorizationDecision)
    message.setProperty("authorizationReason", authorizationReason)
    message.setProperty("authorizationEvaluatedAt", Instant.now().toString())
    message.setProperty("identitySourceForAuthorization", "WSO2_TOKEN_INTROSPECTION_SUBJECT")
    message.setProperty("authorizationPolicy", "F8B_PURCHASE_REQUISITION_GROUP_POLICY")
    return message
}

def List<String> parseGroups(String groupsValue) {
    if (!groupsValue || groupsValue == "NOT_PROVIDED") {
        return []
    }
    return groupsValue
        .split("[,;|]")
        .collect { group -> group.trim().toUpperCase() }
        .findAll { group -> !group.isEmpty() }
}

def String resolveMatchedGroup(
    String authorizationDecision,
    String requiredGroup,
    List<String> availableGroups
) {
    if (authorizationDecision != "AUTHORIZED") {
        return "NONE"
    }
    if (availableGroups.contains(requiredGroup)) {
        return requiredGroup
    }
    if (requiredGroup == "F8_PURCHASING_BUYERS" &&
        availableGroups.contains("F8_PURCHASING_MANAGERS")) {
        return "F8_PURCHASING_MANAGERS"
    }
    return "NONE"
}
```

### 9.4 Router

| Rota | Condição |
|---|---|
| `Authorization_Granted` | `${property.authorizationDecision} = 'AUTHORIZED'` |
| `Authorization_Denied` | `${property.authorizationDecision} = 'DENIED'` |
| `Unsupported_Operation` | Default Route |

### 9.5 Códigos HTTP

| Situação | Código |
|---|---:|
| Operação autorizada | 200 |
| Principal autenticado sem permissão | 403 |
| Operação não suportada | 400 |

---

## 10. Evidências

> Todas as referências abaixo foram validadas contra os nomes técnicos definidos para a pasta `evidences/lab30`.

### F8A

#### Evidência 01 — iFlow de captura do contexto

![F8A iFlow](../evidences/lab30/01-cpi-f8a-authenticated-principal-capture-iflow.png)

O iFlow F8A está implantado e iniciado, contendo a captura segura do contexto e a construção da resposta de diagnóstico.

#### Evidência 02 — Allowed Headers

![F8A Runtime Configuration](../evidences/lab30/02-cpi-f8a-principal-capture-runtime-configuration.png)

A Runtime Configuration permite os headers de identidade somente para detectar tentativas controladas de spoofing.

#### Evidência 03 — Baseline técnico

![F8A Technical Client Baseline](../evidences/lab30/03-postman-f8a-technical-client-baseline.png)

A chamada Client Credentials retorna `TECHNICAL_CLIENT`, sem principal humano e sem propagação ponta a ponta.

#### Evidência 04 — Spoofing rejeitado

![F8A Spoofed Principal Rejected](../evidences/lab30/04-postman-f8a-spoofed-principal-header-rejected.png)

Os headers declarados pelo caller são detectados, mas o valor permanece não aceito como identidade confiável.

### Preparação WSO2 e OAuth

#### Evidência 05 — Console WSO2

![WSO2 Console](../evidences/lab30/05-wso2-f8-identity-server-console-overview.png)

O WSO2 Identity Server está operacional e disponível para administrar aplicações, identidades, grupos e connections.

#### Evidência 06 — Grupos de compras

![WSO2 Purchasing Groups](../evidences/lab30/06-wso2-f8-purchasing-user-groups-created.png)

Foram criados grupos separados para compradores e gestores.

#### Evidência 07 — Usuários de compras

![WSO2 Purchasing Users](../evidences/lab30/07-wso2-f8-purchasing-users-created.png)

Foram criadas identidades fictícias para buyer e manager. O principal técnico foi criado posteriormente e associado ao grupo Buyers, processo descrito neste documento sem captura adicional.

#### Evidência 08 — Aplicação OAuth

![WSO2 OAuth Application](../evidences/lab30/08-wso2-f8-oauth-application-created.png)

A aplicação confidencial OAuth/OIDC foi criada com Client ID e Client Secret protegido.

#### Evidência 09 — Grant SAML2 habilitado

![WSO2 SAML2 Grant](../evidences/lab30/09-wso2-f8-saml2-bearer-grant-enabled.png)

Os grants Client Credential e SAML2 estão habilitados, enquanto grants legados permanecem desabilitados.

#### Evidência 10 — Token local por Client Credentials

![WSO2 Local Token](../evidences/lab30/10-postman-f8-wso2-client-credentials-token-issued.png)

O token endpoint local emitiu Bearer token, validando a infraestrutura OAuth antes do SAML Bearer.

#### Evidência 11 — Túnel HTTPS

![ngrok Tunnel](../evidences/lab30/11-ngrok-f8-wso2-public-https-tunnel-active.png)

O túnel HTTPS temporário encaminha o domínio público ao WSO2 local na porta 9443.

#### Evidência 12 — Token endpoint público

![WSO2 Public Token Endpoint](../evidences/lab30/12-postman-f8-wso2-public-token-endpoint-validated.png)

A URL pública do token endpoint foi validada com Client Credentials antes de ser utilizada pelo SAP Integration Suite.

#### Evidência 13 — Key Pair

![CPI SAML Signing Key Pair](../evidences/lab30/13-cpi-f8-saml-bearer-signing-key-pair-created.png)

O Key Pair RSA de 3072 bits foi criado com CN correspondente ao principal técnico. A private key permanece no tenant.

#### Evidência 14 — Issuer, Alias e trust

![WSO2 Issuer Alias Validation](../evidences/lab30/14-postman-f8-wso2-saml-issuer-alias-configuration-validated.png)

A Management API confirmou Issuer, token endpoint Alias, Connection habilitada e certificado confiável configurado.

### F8B.1 — Emissão do token

#### Evidência 15 — Token SAML Bearer emitido

![SAML Bearer Token Issued](../evidences/lab30/15-postman-f8b-technical-user-saml-bearer-token-issued.png)

A assertion assinada foi aceita pelo WSO2 e trocada por Bearer token. O token foi mascarado antes da resposta.

#### Evidência 16 — Processamento do token exchange

![SAML Bearer Message Processing](../evidences/lab30/16-cpi-f8b-saml-bearer-token-exchange-message-processing.png)

O Monitor comprova a execução completa da geração, assinatura, token request, validação e resposta.

### F8B.2 — Introspecção

#### Evidência 17 — iFlow com introspecção

![Token Introspection iFlow](../evidences/lab30/17-cpi-f8b-saml-bearer-token-introspection-iflow.png)

O iFlow contém uma segunda chamada outbound dedicada à introspecção do access token.

#### Evidência 18 — Token ativo e subject técnico

![Token Introspection Validated](../evidences/lab30/18-postman-f8b-saml-bearer-token-introspection-validated.png)

A introspecção confirmou token ativo, tipo Bearer e subject `f8.technical.purchasing.user`, sem expor o token integral.

#### Evidência 19 — Processamento da introspecção

![Token Introspection Processing](../evidences/lab30/19-cpi-f8b-token-introspection-message-processing.png)

O Monitor comprova os dois ciclos outbound: emissão e introspecção.

### F8B.3 — Autorização

#### Evidência 20 — iFlow com Router de autorização

![Protected Resource Authorization iFlow](../evidences/lab30/20-cpi-f8b-protected-resource-authorization-iflow.png)

O iFlow implantado contém política de autorização com caminhos concedido, negado e operação não suportada.

#### Evidência 21 — READ autorizado

![Purchase Requisition Read Authorized](../evidences/lab30/21-postman-f8b-purchase-requisition-read-authorized.png)

O principal técnico do grupo Buyers recebeu `200 OK` e acessou o recurso protegido de requisições de compra.

#### Evidência 22 — Caminho autorizado

![Authorized Resource Processing](../evidences/lab30/22-cpi-f8b-authorized-resource-message-processing.png)

O Router selecionou `Authorization_Granted` e finalizou em `End_Authorized`.

#### Evidência 23 — APPROVE negado

![Purchase Requisition Approval Denied](../evidences/lab30/23-postman-f8b-purchase-requisition-approval-denied.png)

O token estava ativo, mas o principal Buyers não possuía o grupo Managers. A resposta correta foi `403 Forbidden`.

#### Evidência 24 — Caminho negado

![Authorization Denied Processing](../evidences/lab30/24-cpi-f8b-authorization-denied-message-processing.png)

O Router selecionou `Authorization_Denied`, sem retornar dados do recurso protegido.

#### Evidência 25 — Operação não suportada

![Unsupported Operation Rejected](../evidences/lab30/25-postman-f8b-unsupported-operation-rejected.png)

A operação `DELETE` retornou `400 Bad Request` e informou as operações permitidas.

#### Evidência 26 — Caminho default

![Unsupported Operation Processing](../evidences/lab30/26-cpi-f8b-unsupported-operation-message-processing.png)

A rota default tratou a operação não suportada e finalizou em `End_Unsupported`.

---

## 11. Troubleshooting

### 11.1 `SapAuthenticatedUserName` ausente

**Sintoma:** SecurityException no F8A.

**Causa:** o fluxo Client Credentials autenticou uma aplicação técnica, mas não disponibilizou principal humano.

**Correção:** classificar o contexto como `AUTHENTICATED_CLIENT_WITHOUT_USER_PRINCIPAL`, sem confiar em headers enviados pelo caller.

### 11.2 Headers de spoofing não chegam ao Groovy

**Causa:** headers customizados não estavam permitidos na Runtime Configuration.

**Correção:** adicionar `X-Authenticated-User|X-Principal|X-User` em Allowed Headers.

### 11.3 `502 Bad Gateway` no ngrok

**Causa:** o túnel estava online, mas o WSO2 não estava ouvindo em `localhost:9443`.

**Correção:** iniciar o WSO2, validar `TcpTestSucceeded : True` e manter as janelas do WSO2 e ngrok abertas.

### 11.4 `Identity provider is null`

**Causa:** o certificado estava no truststore, mas o Issuer da assertion não estava associado a uma Connection lógica no WSO2.

**Correção:** criar a Connection SAML e associar o certificado ao Issuer.

### 11.5 `Token Endpoint alias has not been configured`

**Causa:** a Connection foi localizada, mas o Alias do token endpoint estava vazio.

**Correção:** atualizar `homeRealmIdentifier`, `alias` e descrição pela Identity Provider Management API.

### 11.6 Properties aparecem como `${property...}`

**Causa:** Message Body configurado como Constant.

**Correção:** alterar o tipo para Expression.

### 11.7 Diferença entre 401, 403 e 400

| Código | Significado no cenário |
|---|---|
| 401 | Credencial ausente ou inválida |
| 403 | Principal autenticado, mas sem permissão |
| 400 | Operação funcional não suportada |

---

## 12. Boas práticas SAP e de mercado

1. Manter private keys exclusivamente em Keystore gerenciado.
2. Exportar apenas certificados públicos para estabelecer trust.
3. Usar assertions com validade curta e tolerância mínima de clock skew.
4. Validar Issuer, Audience, Recipient, assinatura, subject e validade.
5. Não usar headers customizados como identidade confiável.
6. Não retornar access tokens integrais em respostas, logs ou evidências.
7. Separar autenticação, introspecção e autorização funcional.
8. Usar `403 Forbidden` para principal autenticado sem permissão.
9. Aplicar princípio do menor privilégio a usuários, grupos, scopes e aplicações administrativas.
10. Rotacionar chaves e certificados com sobreposição controlada.
11. Proteger endpoints administrativos e de introspecção.
12. Remover túneis temporários após os testes.
13. Evitar contas super administrator em integrações produtivas.
14. Monitorar falhas de assinatura, issuer, audience, replay e autorização.

---

## 13. Recomendações para produção

| Área | Recomendação |
|---|---|
| Authorization Server | Implementar alta disponibilidade, backup e observabilidade |
| Endpoint público | Usar DNS corporativo, WAF/API Gateway e certificado válido |
| Truststore | Controlar senha, backup, rotação e auditoria |
| Key Pair | Definir expiração, rotação e procedimento de revogação |
| Assertion | Usar janela curta e proteção contra replay |
| Token | Definir TTL compatível com risco e sensibilidade do recurso |
| Introspection | Proteger com client dedicado e escopo mínimo |
| Authorization | Preferir grupos, roles e scopes provenientes do Authorization Server |
| Logs | Não registrar assertions completas, tokens, secrets ou private keys |
| Resiliência | Definir timeouts, circuit breaker e tratamento de indisponibilidade |
| Auditoria | Correlacionar principal, client, token hash, operação e resultado |

### 13.1 Limitação consciente do laboratório

A associação funcional ao grupo `F8_PURCHASING_BUYERS` foi mantida em property controlada pelo iFlow para exercitar autorização técnica. Em produção, grupos, roles ou scopes devem ser obtidos de claims confiáveis do token ou de uma fonte de autorização governada, e não definidos estaticamente no fluxo.

### 13.2 Limitação do Security Material nativo

O artifact nativo `OAuth2 SAML Bearer Assertion` do SAP Integration Suite oferece perfis específicos para SuccessFactors, SAP BTP Neo e SAP BTP Cloud Foundry. Como o WSO2 é um Authorization Server externo genérico, o laboratório implementou o perfil RFC 7522 de forma vendor-neutral, mantendo a private key protegida pelo XML Digital Signer.

---

## 14. Referências oficiais

- [SAP Help — Deploying an OAuth2 SAML Bearer Assertion](https://help.sap.com/docs/cloud-integration/sap-cloud-integration/deploying-oauth2-saml-bearer-assertion)
- [SAP Help — OAuth 2.0 SAML Bearer Assertion Authentication](https://help.sap.com/docs/CX_CNS_ESM/f1437ed0c63b464983503b1a1dc6af8a/444f4b2632d0489b9145f8af4a66b8d7.html)
- [RFC 7522 — SAML 2.0 Profile for OAuth 2.0](https://www.rfc-editor.org/info/rfc7522)
- [RFC 7662 — OAuth 2.0 Token Introspection](https://www.rfc-editor.org/info/rfc7662)
- [WSO2 — Token Introspection](https://is.docs.wso2.com/en/6.1.0/references/concepts/authorization/introspection/)
- [SAP Developer Tutorial — OAuth 2.0 SAML Bearer Assertion Flow](https://developers.sap.com/tutorials/abap-environment-business-partner-oauthsamlbearer/)

---

## 15. Resultado final

| Controle | Resultado |
|---|---|
| Contexto técnico inbound identificado | Validado |
| Tentativa de header spoofing rejeitada | Validado |
| Assertion SAML 2.0 construída | Validado |
| Assertion assinada com private key protegida | Validado |
| Trust lógico e criptográfico no WSO2 | Validado |
| OAuth SAML Bearer token exchange | Validado |
| Token introspection | Validado |
| Subject técnico reconhecido | Validado |
| READ para Buyers | Autorizado |
| APPROVE para Buyers | Negado com 403 |
| Operação DELETE | Rejeitada com 400 |
| Access token integral exposto | Não |

O F8A e o F8B demonstraram uma cadeia completa de autenticação, confiança federada, emissão de token, introspecção e autorização funcional. O cenário também comprovou que token válido não significa autorização irrestrita e que a identidade confiável deve ser derivada de mecanismos criptográficos e do Authorization Server, nunca de headers arbitrários enviados pelo consumidor.

---

[← Bloco anterior: F7 — PGP Message-Level Security](./29-f7-pgp-message-level-security.md) | [Próximo cenário: F8C–F8E — End-User Principal Propagation and Authorization](./31-f8-end-user-principal-propagation-authorization.md)
