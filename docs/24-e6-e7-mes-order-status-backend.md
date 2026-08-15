## 🏭 E6+E7 — MES Order Status: Backend HTTPS com Data Store

**Bloco:** E — API Management  
**Cenário:** Backend dedicado para E6 (JSON to XML) + E7 (Assign Message)  
**Status:** ✅ Concluído e testado de ponta a ponta  
**Data de execução:** 15/08/2026  
**iFlow:** `E6_E7_MES_OrderStatus_ProcessDirect`  
**Data Store:** `MES_OrderStatus_Store`

### 📌 Contexto de Negócio

Este laboratório implementa o backend síncrono que sustentará os cenários **E6 (JSON to XML)** e **E7 (Assign Message)** no SAP API Management.

O processo simula a comunicação entre o sistema **MES** e o **SAP ERP** para registrar e consultar o status de integração de uma ordem. O conteúdo é persistido no Data Store interno do SAP Cloud Integration, utilizando `idRastreamento` como chave técnica para acompanhar o ciclo da mensagem.

O backend disponibiliza uma operação interna de escrita, responsável por criar ou atualizar o status da ordem, e uma operação de consulta que será posteriormente exposta pelo API Management ao consumidor legado. A transformação JSON para XML, a inclusão de headers e o controle condicional do campo técnico `filaDestino` pertencem à etapa seguinte do cenário.

> **Escopo deste documento:** implementação e validação do backend no SAP Cloud Integration. As policies JSON to XML, Assign Message, OAuth scopes e Developer Apps serão documentadas separadamente, com uma nova pasta de evidências iniciando em `01`.

### 🎯 Objetivos

- Criar um endpoint HTTPS único para escrita e consulta de status.
- Persistir mensagens no Data Store `MES_OrderStatus_Store`.
- Permitir sobrescrita controlada pelo mesmo `idRastreamento`.
- Consultar registros existentes e enriquecer a resposta com `consultadoEm`.
- Retornar respostas funcionais para registro inexistente e operação inválida.
- Preparar um backend JSON reutilizável pelo API Management.
- Preservar `filaDestino` no backend para posterior tratamento condicional pela policy E7.

### 🏷️ Nomenclatura e Recursos Técnicos

### 3.1 Nome funcional

`E6+E7 - MES Order Status API`

### 3.2 Identificador técnico do iFlow

`E6_E7_MES_OrderStatus_ProcessDirect`

O caractere `+` não é aceito no identificador técnico do Integration Flow. Por isso, o artefato utiliza underscores. O sufixo `ProcessDirect` foi preservado no artefato já criado, embora a implementação final utilize um único HTTPS Sender com roteamento interno por método e caminho HTTP.

### 3.3 Recursos utilizados

- **HTTPS Sender:** `/e6e7/mes-order-status/*`
- **Data Store:** `MES_OrderStatus_Store`
- **Visibilidade:** `Integration Flow`
- **Entry ID:** `${property.idRastreamento}`
- **Autorização:** role `ESBMessaging.send`

### 🔄 Fluxo de Negócio

O sistema MES envia informações de acompanhamento de uma ordem para a camada de integração. Cada registro recebe um `idRastreamento`, usado como chave técnica durante todo o ciclo.

O backend possibilita dois usos internos:

1. **WRITE:** grava ou atualiza o status da ordem.
2. **GET:** recupera o status pelo `idRastreamento`.

Em uma etapa posterior, somente a consulta GET será exposta pelo API Management ao parceiro legado. O endpoint WRITE permanecerá interno.

### 🏗️ Arquitetura da Solução

```text
Postman ou sistema interno
        |
        | HTTPS
        v
E6_E7_MES_OrderStatus_ProcessDirect
        |
        v
Prepare_Request
        |
        v
Route_Operation
   |          |                 |
   |          |                 |
 WRITE       GET        INVALID_OPERATION
   |          |                 |
   v          v                 v
Groovy      Groovy        Resposta MES-400
   |          |
   v          v
Data Store  Data Store
 Write        Get
   |          |
   v          v
MES_OrderStatus_Store
```

### 📡 Contratos HTTP

### 6.1 WRITE

```http
POST /http/e6e7/mes-order-status/write
Content-Type: application/json
Accept: application/json
```

Payload:

```json
{
  "idRastreamento": "TRK-58291",
  "numeroPedido": "4500001234",
  "origem": "MES",
  "destino": "ERP",
  "statusIntegracao": "AGUARDANDO_ENTREGA_ERP",
  "recebidoEm": "2026-08-15T17:20:00-03:00",
  "tentativasReprocessamento": 0,
  "filaDestino": "producao_pedidos"
}
```

Resposta de sucesso:

```http
HTTP/1.1 201 Created
Content-Type: application/json
```

```json
{
  "status": "CREATED",
  "codigo": "MES-201",
  "mensagem": "Status do pedido gravado com sucesso",
  "idRastreamento": "TRK-58291"
}
```

### 6.2 GET existente

```http
GET /http/e6e7/mes-order-status/status/TRK-58291
Accept: application/json
```

Resposta:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "idRastreamento": "TRK-58291",
  "numeroPedido": "4500001234",
  "origem": "MES",
  "destino": "ERP",
  "statusIntegracao": "REPROCESSAMENTO_AGENDADO",
  "recebidoEm": "2026-08-15T17:20:00-03:00",
  "tentativasReprocessamento": 1,
  "filaDestino": "producao_pedidos",
  "consultadoEm": "2026-08-15T21:57:26.597352389Z"
}
```

### 6.3 GET inexistente

```http
GET /http/e6e7/mes-order-status/status/TRK-99999
```

Resposta:

```http
HTTP/1.1 404 Not Found
Content-Type: application/json
```

```json
{
  "status": "NOT_FOUND",
  "codigo": "MES-404",
  "mensagem": "Id de rastreamento nao encontrado",
  "idRastreamento": "TRK-99999"
}
```

### 6.4 Operação inválida

```http
PUT /http/e6e7/mes-order-status/invalid
```

Resposta:

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json
```

```json
{
  "status": "BAD_REQUEST",
  "codigo": "MES-400",
  "mensagem": "Operacao invalida. Utilize POST /write ou GET /status/{idRastreamento}"
}
```

### 🛠️ Construção do iFlow

### 7.1 HTTPS Sender

Configuração:

```text
Address: /e6e7/mes-order-status/*
Authorization: User Role
User Role: ESBMessaging.send
CSRF Protected: desmarcado
```

O wildcard permite concentrar as operações WRITE e GET no mesmo endpoint técnico.

### 7.2 Prepare_Request

O Content Modifier cria as propriedades:

```text
requestMethod = ${header.CamelHttpMethod}
requestPath   = ${header.CamelHttpPath}
```

As propriedades são úteis para rastreabilidade. O Router foi configurado diretamente com os headers do adapter, evitando diferenças de interpretação de path no runtime.

### 7.3 Route_Operation

Condição WRITE:

```text
${header.CamelHttpMethod} = 'POST' and ${header.CamelHttpPath} contains 'write'
```

Condição GET:

```text
${header.CamelHttpMethod} = 'GET' and ${header.CamelHttpPath} contains 'status'
```

A terceira saída é a Default Route `INVALID_OPERATION`.

### 💻 Groovy Scripts

### 8.1 Validate_And_Prepare_Status

Responsabilidades:

- validar JSON de entrada;
- confirmar que o payload é um objeto;
- validar campos obrigatórios;
- aceitar corretamente `tentativasReprocessamento = 0`;
- normalizar o identificador para maiúsculas;
- converter tentativas para inteiro;
- rejeitar valores negativos;
- criar a property `idRastreamento`;
- preservar JSON válido para persistência.

```groovy
import com.sap.gateway.ip.core.customdev.util.Message
import groovy.json.JsonSlurper
import groovy.json.JsonOutput

def Message processData(Message message) {

    def reader = message.getBody(java.io.Reader)

    if (reader == null) {
        throw new IllegalArgumentException(
            "O payload JSON de status do pedido nao foi informado."
        )
    }

    def json

    try {
        json = new JsonSlurper().parse(reader)
    } catch (Exception exception) {
        throw new IllegalArgumentException(
            "O payload informado nao possui um JSON valido."
        )
    }

    if (!(json instanceof Map)) {
        throw new IllegalArgumentException(
            "O payload deve ser um objeto JSON."
        )
    }

    def mandatoryFields = [
        "idRastreamento",
        "numeroPedido",
        "origem",
        "destino",
        "statusIntegracao",
        "recebidoEm",
        "tentativasReprocessamento",
        "filaDestino"
    ]

    def missingFields = mandatoryFields.findAll { field ->
        !json.containsKey(field) ||
        json[field] == null ||
        json[field].toString().trim().isEmpty()
    }

    if (!missingFields.isEmpty()) {
        throw new IllegalArgumentException(
            "Campos obrigatorios ausentes ou vazios: " +
            missingFields.join(", ")
        )
    }

    def trackingId = json.idRastreamento
        .toString()
        .trim()
        .toUpperCase()

    if (!(trackingId ==~ /^TRK-[A-Z0-9]+(?:-[A-Z0-9]+)*$/)) {
        throw new IllegalArgumentException(
            "Formato invalido para idRastreamento. " +
            "Utilize TRK- seguido do identificador."
        )
    }

    def retryCount

    try {
        retryCount = Integer.parseInt(
            json.tentativasReprocessamento.toString()
        )
    } catch (Exception ignored) {
        throw new IllegalArgumentException(
            "tentativasReprocessamento deve ser um numero inteiro."
        )
    }

    if (retryCount < 0) {
        throw new IllegalArgumentException(
            "tentativasReprocessamento nao pode ser negativo."
        )
    }

    json.idRastreamento = trackingId
    json.tentativasReprocessamento = retryCount

    message.setProperty("idRastreamento", trackingId)
    message.setHeader("Content-Type", "application/json")

    message.setBody(
        JsonOutput.prettyPrint(
            JsonOutput.toJson(json)
        )
    )

    return message
}
```

### 8.2 Extract_Tracking_Id

A primeira versão dependia da existência literal de `/status/` em `CamelHttpPath`. No runtime, o HTTPS Adapter apresentou o caminho relativo de forma diferente, causando erro técnico.

A versão final considera `CamelHttpPath` e `CamelHttpUri`, localizando diretamente o padrão `TRK-...`.

```groovy
import com.sap.gateway.ip.core.customdev.util.Message
import java.net.URLDecoder
import java.nio.charset.StandardCharsets
import java.util.regex.Pattern

def Message processData(Message message) {

    def camelHttpPath = message.getHeader(
        "CamelHttpPath",
        String
    )

    def camelHttpUri = message.getHeader(
        "CamelHttpUri",
        String
    )

    def requestLocation = [
        camelHttpPath,
        camelHttpUri
    ].findAll { value ->
        value != null && !value.trim().isEmpty()
    }.join(" ")

    if (requestLocation.trim().isEmpty()) {
        throw new IllegalArgumentException(
            "O caminho ou URI HTTP nao foi disponibilizado pelo adapter."
        )
    }

    def decodedLocation

    try {
        decodedLocation = URLDecoder.decode(
            requestLocation,
            StandardCharsets.UTF_8.name()
        )
    } catch (Exception exception) {
        throw new IllegalArgumentException(
            "Nao foi possivel interpretar o caminho da requisicao."
        )
    }

    def matcher = Pattern
        .compile(/TRK-[A-Za-z0-9]+(?:-[A-Za-z0-9]+)*/)
        .matcher(decodedLocation)

    if (!matcher.find()) {
        throw new IllegalArgumentException(
            "Nao foi encontrado um idRastreamento valido na requisicao. " +
            "Formato esperado: TRK-xxxxx."
        )
    }

    def trackingId = matcher
        .group()
        .trim()
        .toUpperCase()

    message.setProperty(
        "idRastreamento",
        trackingId
    )

    message.setHeader(
        "X-Tracking-ID",
        trackingId
    )

    return message
}
```

### 8.3 Enrich_Order_Status_Response

Responsabilidades:

- validar o conteúdo recuperado;
- confirmar JSON em formato de objeto;
- validar a presença de `idRastreamento`;
- adicionar `consultadoEm` em UTC;
- manter `filaDestino` no backend;
- retornar `application/json`.

```groovy
import com.sap.gateway.ip.core.customdev.util.Message
import groovy.json.JsonSlurper
import groovy.json.JsonOutput
import java.time.Instant

def Message processData(Message message) {

    def reader = message.getBody(java.io.Reader)

    if (reader == null) {
        throw new IllegalStateException(
            "O Data Store Get nao retornou payload para enriquecimento."
        )
    }

    def json

    try {
        json = new JsonSlurper().parse(reader)
    } catch (Exception exception) {
        throw new IllegalStateException(
            "O conteudo recuperado do Data Store nao possui JSON valido."
        )
    }

    if (!(json instanceof Map)) {
        throw new IllegalStateException(
            "O conteudo recuperado do Data Store deve ser um objeto JSON."
        )
    }

    if (
        !json.containsKey("idRastreamento") ||
        json.idRastreamento == null ||
        json.idRastreamento.toString().trim().isEmpty()
    ) {
        throw new IllegalStateException(
            "O registro recuperado nao possui idRastreamento."
        )
    }

    json.consultadoEm = Instant.now().toString()

    message.setBody(
        JsonOutput.prettyPrint(
            JsonOutput.toJson(json)
        )
    )

    message.setHeader(
        "Content-Type",
        "application/json"
    )

    message.setHeader(
        "X-Tracking-ID",
        json.idRastreamento.toString()
    )

    return message
}
```

### 🗄️ Persistência no Data Store

### 9.1 Write_Order_Status

```text
Data Store Name: MES_OrderStatus_Store
Visibility: Integration Flow
Entry ID: ${property.idRastreamento}
Encrypt Stored Message: marcado
Overwrite Existing Message: marcado
Expiration Period: 30 dias
```

A opção de sobrescrita permite atualizar o ciclo da mesma ordem sem criar uma entrada duplicada.

### 9.2 MES_OrderStatus_Get

```text
Data Store Name: MES_OrderStatus_Store
Visibility: Integration Flow
Entry ID: ${property.idRastreamento}
Delete On Completion: desmarcado
Throw Exception on Missing Entry: desmarcado
```

A opção `Throw Exception on Missing Entry` permanece desmarcada para permitir que a ausência do registro seja transformada em resposta funcional `404`, em vez de erro técnico `500`.

### 🔀 Roteamento FOUND e NOT_FOUND

Condição FOUND:

```text
${header.SAP_DataStoreEntryFound} = 'true'
```

A rota NOT_FOUND é a Default Route.

### FOUND

```text
Data Store Get
→ Enrich_Order_Status_Response
→ Get_Success_Response
→ HTTP 200
```

### NOT_FOUND

```text
Data Store Get
→ Not_Found_Response
→ HTTP 404
```

### 🧪 Testes Executados

### 11.1 WRITE inicial

- ID: `TRK-58291`
- Status: `AGUARDANDO_ENTREGA_ERP`
- Tentativas: `0`
- Resultado: `201 Created`
- Data Store: uma entrada criada

### 11.2 WRITE com sobrescrita

- Mesmo ID: `TRK-58291`
- Novo status: `REPROCESSAMENTO_AGENDADO`
- Tentativas: `1`
- Resultado: `201 Created`
- Data Store: permaneceu com uma única entrada

### 11.3 GET existente

- ID: `TRK-58291`
- Resultado: `200 OK`
- Valores sobrescritos recuperados
- Campo `consultadoEm` adicionado dinamicamente
- Campo `filaDestino` preservado

### 11.4 GET inexistente

- ID: `TRK-99999`
- Resultado: `404 Not Found`
- Resposta funcional `MES-404`

### 11.5 Operação inválida

- Método: `PUT`
- Recurso: `/invalid`
- Resultado: `400 Bad Request`
- Resposta funcional `MES-400`

### 📸 Evidências

> Todas as evidências deste documento pertencem à pasta específica deste laboratório e iniciam em `01`. A numeração não deve ser continuada em documentos ou laboratórios posteriores.

Pasta:

```text
evidences/lab22/
```

1. [Visão geral do iFlow sem erros](../evidences/lab22/01-iflow-e6-e7-overview-no-errors.png)
2. [Deployment do iFlow iniciado](../evidences/lab22/02-iflow-deployment-started.png)
3. [WRITE inicial com HTTP 201](../evidences/lab22/03-postman-write-new-201.png)
4. [Entrada criada no Data Store](../evidences/lab22/04-datastore-entry-created.png)
5. [Trace do caminho WRITE](../evidences/lab22/05-trace-write-flow-path.png)
6. [Payload antes do Data Store Write](../evidences/lab22/06-message-content-before-datastore-write.png)
7. [Resposta interna do WRITE](../evidences/lab22/07-message-content-write-response.png)
8. [WRITE com sobrescrita e HTTP 201](../evidences/lab22/08-postman-write-overwrite-201.png)
9. [Data Store após sobrescrita](../evidences/lab22/09-datastore-entry-after-overwrite.png)
10. [Payload atualizado antes da sobrescrita](../evidences/lab22/10-message-content-overwrite-before-write.png)
11. [GET existente com HTTP 200](../evidences/lab22/11-postman-get-existing-200.png)
12. [Trace do caminho GET FOUND](../evidences/lab22/12-trace-get-found-path.png)
13. [Payload GET enriquecido](../evidences/lab22/13-message-content-get-enriched.png)
14. [GET inexistente com HTTP 404](../evidences/lab22/14-postman-get-not-found-404.png)
15. [Trace do caminho GET NOT_FOUND](../evidences/lab22/15-trace-get-not-found-path.png)
16. [Resposta interna NOT_FOUND](../evidences/lab22/16-message-content-not-found-response.png)
17. [Operação inválida com HTTP 400](../evidences/lab22/17-postman-invalid-operation-400.png)
18. [Trace da operação inválida](../evidences/lab22/18-trace-invalid-operation-path.png)
19. [Payload interno da operação inválida](../evidences/lab22/19-message-content-invalid-operation.png)

### 🔍 Troubleshooting e Lições Aprendidas

### 13.1 Router direcionando POST para INVALID_OPERATION

**Sintoma:** resposta `MES-400` mesmo com método POST.

**Causa:** o Router dependia de properties cujo conteúdo não refletia corretamente os headers do runtime.

**Correção:** uso direto de `CamelHttpMethod` e `CamelHttpPath` nas condições.

### 13.2 GET direcionando para INVALID_OPERATION

**Sintoma:** resposta `MES-400` na consulta.

**Causa:** URL do Postman sem o segmento `/status/`.

**Correção:**

```text
/http/e6e7/mes-order-status/status/TRK-58291
```

### 13.3 Erro 500 no Extract_Tracking_Id

**Sintoma:** `IllegalArgumentException` solicitando o formato `/status/{idRastreamento}`.

**Causa:** script dependente da ocorrência literal `/status/` em `CamelHttpPath`.

**Correção:** leitura combinada de `CamelHttpPath` e `CamelHttpUri`, seguida de busca pelo padrão `TRK-...`.

### 13.4 Validação incorreta do valor zero

**Risco:** `tentativasReprocessamento = 0` ser interpretado como ausência do campo.

**Correção:** validação baseada em `containsKey`, `null` e conteúdo vazio, sem avaliação booleana direta do valor.

### 🧠 Decisões Técnicas

### 14.1 Data Store interno

O cenário usa o Data Store interno do SAP Cloud Integration. Não há banco Neon ou persistência externa.

### 14.2 Visibilidade Integration Flow

A visibilidade foi mantida como `Integration Flow` porque WRITE e GET estão no mesmo iFlow. Caso outro iFlow precise atualizar diretamente o mesmo Data Store no futuro, a visibilidade deverá ser reavaliada.

### 14.3 Campo filaDestino

`filaDestino` é preservado no backend porque representa informação técnica útil ao time de Operações/Integração. Na etapa de API Management:

- consumidor legado sem scope interno: não receberá `filaDestino`;
- consumidor interno: receberá o conteúdo completo.

### 14.4 Endpoint WRITE interno

O endpoint WRITE não será exposto ao parceiro legado. O API Proxy será direcionado ao contrato de consulta GET.

### ✅ Conclusão

O backend foi validado com sucesso nos cinco comportamentos definidos:

- criação;
- sobrescrita;
- leitura existente;
- leitura inexistente;
- operação inválida.

O resultado fornece uma base consistente para a etapa seguinte, que adicionará governança e transformação no API Management:

- OAuth e scopes;
- Developer Apps separados;
- Assign Message;
- correlação de chamadas;
- ocultação condicional de `filaDestino`;
- conversão JSON para XML.

**Recursos praticados:** HTTPS Sender · Router por método e caminho · Groovy Script · Data Store Write · Data Store Get · Sobrescrita controlada · Tratamento funcional de HTTP 200, 201, 400 e 404 · Trace e Message Content

**Cenário anterior:** [E4+E5 — Traffic Management com Quota e Spike Arrest](./23-e4-e5-quota-spike-arrest.md)  
**Próximo cenário:** [E6+E7 — API Management com JSON to XML e Assign Message](./25-e6-e7-api-management-xml-assign-message.md)

### 🛠️ Ferramentas utilizadas

- **SAP Integration Suite — Cloud Integration**
- **SAP Data Store** para persistência interna
- **Groovy** para validação, extração de identificador e enriquecimento
- **Postman** para testes funcionais dos endpoints
- **Visual Studio Code** para documentação e versionamento
- **Git e GitHub** para controle de versão

### 🧭 Navegação

- [📚 Índice da documentação](./README.md)
- [⬅️ Bloco anterior: E4+E5 — Traffic Management com Quota e Spike Arrest](./23-e4-e5-quota-spike-arrest.md)
- [➡️ Próximo cenário: E6+E7 — API Management com JSON to XML e Assign Message](./25-e6-e7-api-management-xml-assign-message.md)

### 👤 Autor / 📬 Contato

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Orlando%20Caetano-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/orlando-caetano/)
[![GitHub](https://img.shields.io/badge/GitHub-OrlandoCaetano2026-181717?logo=github&logoColor=white)](https://github.com/OrlandoCaetano2026)

**Orlando Caetano**  
Especialista SAP • Integração • Inteligência Artificial  
Consultor SAP MM com know-how em PP, QM e WM

![SAP MM](https://img.shields.io/badge/SAP-MM-0FAAFF?logo=sap&logoColor=white) ![SAP PP](https://img.shields.io/badge/SAP-PP-0FAAFF?logo=sap&logoColor=white) ![SAP QM](https://img.shields.io/badge/SAP-QM-0FAAFF?logo=sap&logoColor=white) ![SAP WM](https://img.shields.io/badge/SAP-WM-0FAAFF?logo=sap&logoColor=white)

> 📌 Projeto de estudo e portfólio. Os cenários SAP MM, PP e QM são simulações educativas para prática de integração.
