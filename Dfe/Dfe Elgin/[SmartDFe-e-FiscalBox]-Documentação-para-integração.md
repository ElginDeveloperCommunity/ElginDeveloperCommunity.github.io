# API SmartDFe / FiscalBox — Documentação de Integração

---

## Visão Geral

API responsável pela comunicação com o software **SmartDFe** e o dispositivo **FiscalBox** para emissão e gerenciamento de documentos fiscais.

| Versão               | Base URL                     |
| -------------------- | ---------------------------- |
| Software (SmartDFe)  | `http://localhost:3001`      |
| Hardware (FiscalBox) | `http://[IP_FISCALBOX]:3001` |

> **Exemplo:** `http://192.168.0.19:3001` — utilize o IP configurado na sua rede.

---

## Retornos e tratamentos de erros

As respostas dos endpoints retornam um JSON com o campo `cStat`, que é o código de status oficial da Sefaz. Recomenda-se utilizar os campos `xMotivo` e `data` para entender rejeições e o conteúdo da resposta.

Referências úteis:

- [Campos JSON disponíveis no retorno do emissor](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Campos-Json-Retorno-Emissor)
- [Tabela de retornos Sefaz](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe%5D-Tabela-de-Retornos)
- [Retorno Webhook Fiscal](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Retorno-Webhook-Fiscal)

---

## Variáveis de rota

| Variável          | Descrição                                          |
| ----------------- | -------------------------------------------------- |
| `{{URL Base}}`| URL base do SmartDFe ou FiscalBox                  |
| `{{UUID}}`        | UUID da requisição de emissão                      |
| `{{Chave NFCE}}`  | Chave de acesso de 44 dígitos da NFC-e             |
| `{{Protocolo NFCE}}` | Protocolo de autorização da NFC-e              |
| `{{UUID NFCE}}`   | UUID da nota para consulta de status               |
| `{{nNF}}`         | Número da nota fiscal                              |


---

## Endpoints — Emissão, Consulta, etc.

---

### 1. EmitirNota (por UUID)

Envia o XML de uma NFC-e ou NF-e para emissão, identificada por um UUID.

- **Método:** `POST`
- **URL:** `{{URL Base}}/api/v1/nfce/xml/{{UUID}}`

**Headers:**

| Header               | Valor                                        |
| -------------------- | -------------------------------------------- |
| `Content-Type`       | `multipart/form-data` (application/octet-stream para o arquivo) |
| `Accept`             | `application/xml`                           |
| `Content-Disposition`| `form-data; name="xml"; filename="document.xml"` |

**Body (form-data/raw):**

| Campo  | Tipo             | Obrigatório | Descrição                                                                      |
| ------ | ---------------- | ----------- | ------------------------------------------------------------------------------ |
| `xml`  | Arquivo ou Texto | Sim         | Pode ser enviado como um arquivo `.xml` ou como o conteúdo XML em texto puro.  |
| `uuid` | String           | Sim         | Identificador único da requisição.                                             |

**Exemplo 1 – Envio de arquivo:**
- Chave: `xml`
- Tipo: `File`
- Valor: `nota.xml`

**Exemplo 2 – Envio de XML em texto:**
- Chave: `xml`
- Tipo: `Text`
- Valor:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<NFe>
    ...
</NFe>
```

**Resposta:** Retorna o resultado do processamento contendo o código de status (`cStat`), mensagem de retorno e demais informações da NFC-e/NF-e emitida ou rejeitada.

---

### 2. ConsultarNota

Consulta o status de uma NFC-e pela chave de acesso.

- **Método:** `GET`
- **URL:** `{{URL Base}}/api/v1/nfce/{{Chave NFCE}}`

**Resposta:** Retorna o `cStat` com o resultado e os dados pertinentes ao documento fiscal.

---

### 3. CancelarNota

Cancela uma NFC-e autorizada.

- **Método:** `POST`
- **URL:** `{{URL Base}}/api/v1/nfce/{{Chave NFCE}}/cancel`

**Body (JSON):**

```json
{
  "justificativa": "Cancelamento para testes",
  "protocolo": "{{Protocolo NFCE}}"
}
```

| Campo          | Tipo   | Descrição                                              |
| -------------- | ------ | ------------------------------------------------------ |
| `justificativa`| String | Justificativa do cancelamento (mínimo 15 caracteres)   |
| `protocolo`    | String | Número do protocolo de autorização da NFC-e            |

**Resposta:** Retorna o `cStat` com o resultado e os dados pertinentes ao cancelamento.

---

### 4. CancelamentoPorSubstituição

Cancela uma NFC-e substituindo por outra nota.

- **Método:** `POST`
- **URL:** `{{URL Base}}/api/v1/nfce/{{Chave NFCE}}/cancellationBySubstitution`

**Headers:**

| Header         | Valor              |
| -------------- | ------------------ |
| `Content-Type` | `application/json` |
| `Accept`       | `application/json` |

**Body (JSON):**

```json
{
  "chCFeRef": "Chave da chCFeRef",
  "tpAutor": 3,
  "xJust": "emitida com erro 'Justificativa'"
}
```

| Campo      | Tipo    | Descrição                                                    |
| ---------- | ------- | ------------------------------------------------------------ |
| `chCFeRef` | String  | Chave de acesso da nota substituta                           |
| `tpAutor`  | Integer | Tipo do autor da substituição                                |
| `xJust`    | String  | Justificativa da substituição (mínimo 15 caracteres)         |

**Resposta:** Retorna o `cStat` com o resultado e os dados pertinentes ao documento fiscal.

---

### 5. CartaDeSubstituição (CC-e)

Emite uma Carta de Correção Eletrônica (CC-e).

- **Método:** `POST`
- **URL:** `{{URL Base}}/api/v1/nfe/{{Chave NFCE}}/electronicCorrectionLetter`

**Headers:**

| Header         | Valor              |
| -------------- | ------------------ |
| `Content-Type` | `application/json` |
| `Accept`       | `application/json` |

**Body (JSON):**

```json
{
  "xCorrecao": "ncm invalido"
}
```

| Campo       | Tipo   | Descrição                                                        |
| ----------- | ------ | ---------------------------------------------------------------- |
| `xCorrecao` | String | Descrição da correção a ser aplicada (mínimo 15 caracteres)      |

**Resposta:** Retorna o `cStat` com o resultado e os dados pertinentes ao documento fiscal.

---

### 6. InutilizarNumeração

Solicita a inutilização de numeração de notas não utilizadas.

- **Método:** `POST`
- **URL:** `{{URL Base}}/api/v1/nfce/discard`

**Headers:**

| Header         | Valor              |
| -------------- | ------------------ |
| `Content-Type` | `application/json` |
| `Accept`       | `application/json` |

**Body (JSON):**

```json
{
  "ano": "26",
  "cnpj": "29720473000145",
  "justificativa": "inutilização para testes",
  "numeroFinal": 848,
  "numeroInicial": 848,
  "serie": 27,
  "tpAmb": 2,
  "uf": "35"
}
```

| Campo          | Tipo    | Descrição                                                          |
| -------------- | ------- | ------------------------------------------------------------------ |
| `ano`          | String  | Ano de referência (2 dígitos, ex: `"26"`)                          |
| `cnpj`         | String  | CNPJ do emitente (sem formatação)                                  |
| `justificativa`| String  | Justificativa da inutilização (mínimo 15 caracteres)               |
| `numeroFinal`  | Integer | Número final da faixa a inutilizar                                 |
| `numeroInicial`| Integer | Número inicial da faixa a inutilizar                               |
| `serie`        | Integer | Série da NFC-e                                                     |
| `tpAmb`        | Integer | Tipo de ambiente: `1` = Produção, `2` = Homologação                |
| `uf`           | String  | Código IBGE da UF                                                  |

**Resposta:** Retorna o `cStat` com o resultado e os dados pertinentes à numeração inutilizada.

---

### 7. ConsultarProcessamentoContingência

Consulta notas emitidas em contingência e retorna o resultado do processamento.

> **Atenção:** A execução deste endpoint apaga o histórico de contingência.

- **Método:** `GET`
- **URL:** `{{URL Base}}/api/v1/nfce/processamentocontigencia`

**Resposta:** Retorna os documentos processados em contingência pelo SmartDFe ou FiscalBox.

---

### 8. ConsultarStatusUUID

Consulta o status da emissão de uma NFC-e pelo seu UUID. Armazena as 100 últimas emissões.

- **Método:** `GET`
- **URL:** `{{URL Base}}/api/v1/nfce/authorization/status/{{UUID NFCE}}`

**Parâmetros de rota:**

| Parâmetro   | Descrição                                    |
| ----------- | -------------------------------------------- |
| `UUID NFCE` | UUID enviado no momento da emissão da nota   |

**Resposta:** Retorna os dados completos da transação pelo UUID (emissão, rejeição, contingência, etc.).

---

## Endpoints — Webhook

---

### 9. Configurar Webhook

Configura o webhook para receber notificações do SmartDFe sobre eventos fiscais.

- **Método:** `POST`
- **URL:** `{{URL Base}}/api/v1/webhook/config`

**Body (JSON):**

```json
{
  "url": "http://127.0.0.1:5050/webhook",
  "ativo": true,
  "retryMax": 3,
  "timeoutMs": 5000
}
```

| Campo       | Tipo    | Descrição                                                              |
| ----------- | ------- | ---------------------------------------------------------------------- |
| `url`       | String  | URL que receberá as notificações do webhook                            |
| `ativo`     | Boolean | Define se o webhook está habilitado (`true`) ou desabilitado (`false`) |
| `retryMax`  | Integer | Número máximo de tentativas de reenvio em caso de falha                |
| `timeoutMs` | Integer | Tempo máximo de espera por resposta em milissegundos                   |

---

### 10. Consultar Configuração Webhook

Retorna a configuração atual do webhook.

- **Método:** `GET`
- **URL:** `{{URL Base}}/api/v1/webhook/config`

**Resposta:** Retorna os dados da configuração atual do webhook (URL, status, retry, timeout).

---

## Endpoints — Servidor SMTP

---

### 11. Configurar Email (SMTP)

Configura o servidor SMTP para envio de e-mails com os documentos fiscais emitidos.

- **Método:** `POST`
- **URL:** `{{URL Base}}/api/v1/email/config`

**Body (JSON):**

```json
{
  "smtp_host":       "smtp.gmail.com",
  "smtp_porta":      465,
  "smtp_usuario":    "seuemail@gmail.com",
  "smtp_senha":      "sua_app_password",
  "smtp_tls":        false,
  "remetente_email": "seuemail@gmail.com",
  "remetente_nome":  "Empresa Fiscal",
  "assunto":         "Sua NFC-e nº {nNF}",
  "ativo":           true
}
```

| Campo             | Tipo    | Descrição                                                           |
| ----------------- | ------- | ------------------------------------------------------------------- |
| `smtp_host`       | String  | Endereço do servidor SMTP (ex: `smtp.gmail.com`)                    |
| `smtp_porta`      | Integer | Porta do servidor SMTP (ex: `465` para SSL, `587` para TLS)         |
| `smtp_usuario`    | String  | Usuário de autenticação no servidor SMTP                            |
| `smtp_senha`      | String  | Senha ou App Password do usuário SMTP                               |
| `smtp_tls`        | Boolean | Habilita conexão TLS (`true`) ou SSL (`false`)                      |
| `remetente_email` | String  | E-mail exibido como remetente nas mensagens enviadas                |
| `remetente_nome`  | String  | Nome exibido como remetente nas mensagens enviadas                  |
| `assunto`         | String  | Assunto do e-mail — suporta variável `{nNF}` para número da nota    |
| `ativo`           | Boolean | Define se o envio de e-mail está habilitado                         |

---

### 12. Consultar Configuração de Email

Retorna a configuração atual do servidor SMTP.

- **Método:** `GET`
- **URL:** `{{URL Base}}/api/v1/email/config`

**Resposta:** Retorna os dados da configuração SMTP cadastrada (host, porta, usuário, remetente, status).

---

## Collection para download

[API SmartDFe.postman_collection.json](https://github.com/user-attachments/files/29252283/API.SmartDFe.postman_collection.json)
