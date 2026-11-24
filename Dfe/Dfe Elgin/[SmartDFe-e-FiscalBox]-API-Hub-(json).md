# Documentação do SmartDFe API Hub (json)

Antes de iniciar a integração, deve ser instalado o software no seu computador de desenvolvimento seguindo o procedimento do link: [Guia de Instalação do Elgin SmartDFe](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe%5D-Guia-de-Instala%C3%A7%C3%A3o-do-Elgin-SmartDFe).

Após a instalação do Elgin SmartDFe, a API será automaticamente ativada na porta 8780. Esta API não requer autenticação por token.

Esta documentação apresenta a coleção completa de endpoints da API do SmartDFe Hub. Através dela, é possível realizar o gerenciamento completo de NFC-e (modelo 65) e NF-e (modelo 55), incluindo operações como: emissão, consulta, obtenção de XML e DANFE, cancelamento, inutilização e registro de eventos.

# Visão Geral

API responsável pela comunicação com o **SmartDFe** e o **FiscalBox**, permitindo a emissão e o gerenciamento de documentos fiscais.

## URL Base

| Ambiente | URL Base |
|----------|----------|
| **Software (SmartDFe)** | `http://localhost:8780` |
| **Hardware (FiscalBox)** | `http://[IP_FISCALBOX]:8780` |

**Exemplo:**

```text
http://192.168.0.19:8780 - > Substitua `IP_FISCALBOX` pelo endereço IP configurado do FiscalBox na sua rede local.

```

**Formato de Envio (Payload)**

Esta API foi projetada para receber requisições utilizando exclusivamente o formato **JSON**. 
Os dados de emissão devem ser enviados diretamente no corpo da requisição (`body`) em formato bruto (`raw`), utilizando o cabeçalho `Content-Type: application/json`.

> [!IMPORTANT]
> **Reenvio de Notas e Unicidade (UUID / `ref`)**
> Ao emitir uma nota, o campo `ref` (UUID) deve ser **único por requisição**. Em caso de rejeição da nota, se desejar reenviá-la, é **obrigatório alterar o UUID**. Se você enviar uma requisição com um UUID já utilizado anteriormente, a API não processará o novo envio e retornará exatamente a mesma resposta original da primeira tentativa.
    

Contrato: {{base}}/openapi.json | Docs: {{base}}/docs

## Guia de Integração Rápida
**1. Primeiro Passo:** A primeira ação obrigatória é **Configurar o Emitente** (`/config/emitente`). Sem isso, as emissões não funcionarão.

**2. Fluxo Principal:** A partir da configuração do emitente, você já pode seguir para a emissão de notas (NFC-e ou NF-e).

**3. Funcionalidades Opcionais:** Endpoints referentes a **SMTP**, **Webhook** e **Serviços** são complementares.

---

## Categoria: 1. Configuração Inicial (Obrigatória)


### 🟢 [POST] Configurar Emitente

**Método:** `POST` | **Endpoint:** `{{base}}/config/emitente`

Cadastra ou atualiza os dados da empresa emitente (CNPJ, Inscrição Estadual, Endereço, etc) associada ao token de acesso.

Os dados fiscais e de endereço devem ser precisos, pois serão validados pela SEFAZ.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body
```json
{
  "emitente": {
    "CEP": "03679000",
    "CNPJ": "29720473000145",
    "CRT": "1",
    "IE": "128237060114",
    "UF": "SP",
    "cMun": "3550308",
    "cPais": "1058",
    "cUF": "35",
    "fone": "",
    "nro": "871",
    "xBairro": "VILA SAO FRANCISCO (ZONA LESTE)",
    "xCpl": "",
    "xFant": "29.720.473 FERNANDA SERAFIM ABRANTES",
    "xLgr": "RUA JOAO JOSE DE QUEIROZ",
    "xMun": "SAO PAULO",
    "xNome": "29.720.473 FERNANDA SERAFIM ABRANTES",
    "xPais": "Brasil"
  },
  "status": "ok"
}
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/config/emitente' \
--header 'Content-Type: application/json' \
--data '{
  "emitente": {
    "CEP": "03679000",
    "CNPJ": "29720473000145",
    "CRT": "1",
    "IE": "128237060114",
    "UF": "SP",
    "cMun": "3550308",
    "cPais": "1058",
    "cUF": "35",
    "fone": "",
    "nro": "871",
    "xBairro": "VILA SAO FRANCISCO (ZONA LESTE)",
    "xCpl": "",
    "xFant": "29.720.473 FERNANDA SERAFIM ABRANTES",
    "xLgr": "RUA JOAO JOSE DE QUEIROZ",
    "xMun": "SAO PAULO",
    "xNome": "29.720.473 FERNANDA SERAFIM ABRANTES",
    "xPais": "Brasil"
  },
  "status": "ok"
}'
```

---
### 🔵 [GET] Consultar Configuração do Emitente

**Método:** `GET` | **Endpoint:** `{{base}}/config/emitente`

Retorna os dados cadastrais da empresa emitente atual vinculada ao token.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/config/emitente' \
```

---
## Categoria: 2. NFC-e (mod 65)


### 🟢 [POST] Emitir NFC-e

**Método:** `POST` | **Endpoint:** `{{base}}/nfce`

Emite uma **Nota Fiscal de Consumidor Eletrônica (NFC-e)** através da API SmartDFe Hub.


---

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body

O corpo da requisição deve ser enviado em JSON com os seguintes campos:

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `ref` | string | Referência única do pedido. Suporta a variável dinâmica `{{$timestamp}}` para garantir unicidade. |
| `ambiente` | string | Ambiente de emissão. Use `homologacao` para testes ou `producao` para emissão real. |
| `itens` | array | Lista de itens da nota fiscal. |
| `pagamentos` | array | Lista de formas de pagamento. |

> [!WARNING]
> **Reenvio e Unicidade do UUID (`ref`)**
> Em caso de rejeição, para reenviar a mesma nota é **obrigatório alterar o UUID/ref**, pois ele deve ser único por requisição. Caso o mesmo UUID seja enviado novamente, a API retornará a resposta original gerada na primeira requisição, não reprocessando a nota.

#### Objeto `itens[]`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `descricao` | string | Descrição do produto. |
| `quantidade` | number | Quantidade do item. |
| `valor_unitario` | number | Valor unitário do item. |
| `ncm` | string | Código NCM (Nomenclatura Comum do Mercosul) do produto. |
| `cfop` | string | Código Fiscal de Operações e Prestações (CFOP). |
| `gtin` | string | Código de barras GTIN do produto. Use `"SEM GTIN"` quando não houver. |
| `tributacao.icms.csosn` | string | Código de Situação da Operação no Simples Nacional (CSOSN). |

#### Objeto `pagamentos[]`

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `forma` | string | Forma de pagamento (ex: `dinheiro`, `cartao_credito`, `cartao_debito`, `pix`). |
| `valor` | number | Valor pago nesta forma de pagamento. |


🔗 [referencia_campos_nota_json.json](https://github.com/user-attachments/files/32749269/referencia_campos_nota_json.json)

---

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body

``` json
{
  "ref": "PEDIDO-6a1b17e9-3c44-4410-b18f-92341b9c02f7",
  "ambiente": "homologacao",
  "itens": [
    {
      "descricao": "Arroz Branco",
      "quantidade": 1.0,
      "valor_unitario": 12.0,
      "ncm": "10063021",
      "cfop": "5102",
      "gtin": "SEM GTIN",
      "tributacao": {
        "origem": 0,
        "icms": {
          "csosn": "102",
          "modalidade_base": 0
        },
        "pis": {
          "cst": "49"
        },
        "cofins": {
          "cst": "49"
        },
        "ibs_cbs": {
          "cst": "000",
          "cClassTrib": "000001",
          "pIBSUF": 0.1,
          "pIBSMun": 0.0,
          "pCBS": 0.9
        }
      }
    }
  ],
  "pagamentos": [
    {
      "forma": "credito",
      "valor": 12.0,
      "bandeira": "01",
      "cnpj_credenciadora": "45984642000100",
      "autorizacao": "123456",
      "integrado": true
    }
  ]
}

 ```

---

### Resposta de Sucesso

Quando a NFC-e é autorizada, a resposta contém o campo `status` com o valor `"autorizado"` e o campo `chave` com a chave de acesso da nota. A chave é automaticamente salva na variável de coleção `chave` pelo script de testes.

``` json
{
    "certificado": {
        "dias_para_vencer": 163,
        "validade": "2027-01-15 12:39:25"
    },
    "chave": "35260829720473000145650270000013461232717455",
    "contingencia": {
        "pendentes": 0
    },
    "id": "NFE-1785870087",
    "licenca": {
        "validade": "Perpetual"
    },
    "links": {
        "pdf": "/api/v1/nfce/35260829720473000145650270000013461232717455/pdf",
        "xml": "/api/v1/nfce/35260829720473000145650270000013461232717455/xml"
    },
    "numero": "1346",
    "protocolo": "13526000009074823",
    "ref": "NFE-1785870087",
    "sefaz": {
        "codigo": "100",
        "mensagem": "Autorizado o uso da NF-e"
    },
    "status": "autorizado",
    "tempo": {
        "Tempo comando(ms)": "467",
        "Tempo médio SEFAZ(ms)": "396"
    },
    "tipo": "nfce"
}

 ```

> ⚠️ **Atenção — HTTP Status x Código SEFAZ**
>
> O código HTTP indica o resultado do processamento da requisição pela API, enquanto o campo `sefaz.codigo` indica o resultado do processamento fiscal.
>
> * **HTTP 200** → A requisição foi processada com sucesso pela API. Isso **não significa necessariamente que o documento foi autorizado pela SEFAZ**. Para confirmar a autorização, valide o campo `sefaz.codigo`.
> * **HTTP 422** → A requisição foi recebida, porém não pôde ser processada devido a uma rejeição ou erro de regra de negócio. Nesse caso, consulte os dados retornados pela API para identificar o motivo da rejeição.
>

> 💡 **Dica de Integração:** Para entender em detalhes como lidar com os códigos de aprovação, rejeição ou contingência (`sefaz.codigo`), consulte o nosso [Guia de Tratamento de Retorno](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Tratamento-de-Retorno-%E2%80%90-SmartDFe-API-Hub-(json)).

---


### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfce' \
--header 'Content-Type: application/json' \
--data '{
  "ref": "NFE-123456789",
  "ambiente": "homologacao",
  "itens": [
    {
      "descricao": "Bolo de Limão",
      "quantidade": 2.0,
      "valor_unitario": 2.86,
      "ncm": "22030000",
      "cfop": "5102",
      "gtin": "SEM GTIN",
      "tributacao": {
        "origem": 0,
        "icms": { "csosn": "400", "modalidade_base": 0 },
        "pis": { "cst": "99", "aliquota": 1.65 },
        "cofins": { "cst": "01", "aliquota": 7.6 }
      }
    }
  ],
  "pagamentos": [
    {
      "forma": "debito",
      "valor": 5.72,
      "bandeira": "01",
      "cnpj_credenciadora": "45984642000100",
      "autorizacao": "123456",
      "integrado": true
    }
  ]
}'
```

---
### 🟢 [POST] Cancelar NFC-e
**Método:** `POST`  
**URL:** `{{base}}/nfce/{{chave}}/cancelamento`

Cancela uma **NFC-e (Nota Fiscal de Consumidor Eletrônica – mod. 65)** previamente autorizada pela SEFAZ.

#### Pré-requisitos

- A NFC-e deve estar com status **autorizado**.
    
- O cancelamento deve ser solicitado dentro do prazo legal permitido pela SEFAZ (geralmente 30 minutos após a autorização, podendo variar por UF).
    
- A variável `{{chave}}` deve conter a chave de acesso (44 dígitos) da NFC-e a ser cancelada. Ela é preenchida automaticamente pelo script de teste da requisição **Emitir NFC-e**.
    

#### URL

`POST {{base}}/nfce/{{chave}}/cancelamento`

| Variável | Descrição |
| --- | --- |
| `{{base}}` | URL base da API (ex.: `http://127.0.0.1:8780/api/v1`) |
| `{{chave}}` | Chave de acesso de 44 dígitos da NFC-e |

#### Body (JSON)

``` json
{
  "justificativa": "cancelamento de teste via colecao postman"
}

 ```

| Campo | Tipo | Obrigatório | Descrição |
| --- | --- | --- | --- |
| `justificativa` | string | Sim | Motivo do cancelamento. Mínimo de 15 caracteres conforme exigência da SEFAZ. |

#### Respostas esperadas

| Status | Descrição |
| --- | --- |
| `200 OK` | Cancelamento registrado com sucesso na SEFAZ. |
| `400 Bad Request` | Dados inválidos ou justificativa fora do padrão. |
| `422 Unprocessable Entity` | NFC-e não elegível para cancelamento (prazo expirado, status inválido, etc.). |

### Exemplo de retorno (200 OK)

``` json
{
  "chave": "35260829720473000145650270000013451235796910",
  "protocolo": "13526000009074374",
  "sefaz": {
    "codigo": "135",
    "mensagem": "Evento registrado e vinculado a NF-e"
  },
  "status": "cancelado",
  "tipo": "nfce"
}

 ```

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `chave` | string | Chave de acesso de 44 dígitos da NFC-e cancelada. |
| `protocolo` | string | Número do protocolo de registro do evento na SEFAZ. |
| `sefaz.codigo` | string | Código de retorno da SEFAZ. `135` indica evento registrado com sucesso. |
| `sefaz.mensagem` | string | Mensagem descritiva do retorno da SEFAZ. |
| `status` | string | Status final da NFC-e após o processamento. Esperado: `cancelado`. |
| `tipo` | string | Tipo do documento fiscal. Valor fixo: `nfce`. |

> **Sugestão de tratamento:** Capture os campos `sefaz.codigo`, `sefaz.mensagem` e `status` para validar se o cancelamento foi concluído com sucesso ou para identificar possíveis falhas no processamento. 
  

#### Observações

- Após o cancelamento, a NFC-e não pode ser reativada.
    
- O evento de cancelamento é transmitido à SEFAZ e o XML do evento fica disponível para consulta.
    
- Para cancelar uma **NF-e (mod. 55)**, utilize a requisição **Cancelar NF-e** (`POST {{base}}/nfe/{{chave}}/cancelamento`).

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body
```json
{ "justificativa": "cancelamento de teste via colecao postman" }
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfce/SUA_CHAVE_AQUI/cancelamento' \
--header 'Content-Type: application/json' \
--data '{
  "justificativa": "cancelamento de teste via colecao postman"
}'
```

---
### 🟢 [POST] Inutilizar Faixa de Numeração NFC-e

**Método:** `POST` | **Endpoint:** `{{base}}/nfce/inutilizacao`

Este endpoint realiza a **inutilização de uma faixa de números de série de NFC-e** junto à SEFAZ. Utilize-o quando determinados números de uma série não forem utilizados e precisarem ser inutilizados para manter a sequência fiscal correta.


#### Método e URL


POST {{base}}/nfce/inutilizacao

#### Cabeçalhos

| Chave          | Valor            |
|----------------|------------------|
| Content-Type   | application/json |

#### Corpo da Requisição (JSON)

```json
{
  "serie": 1,
  "inicial": 900,
  "final": 902,
  "justificativa": "inutilizacao de teste via colecao postman"
}
```

#### Descrição dos Campos

| Campo          | Tipo    | Descrição                                                                 |
|----------------|---------|---------------------------------------------------------------------------|
| `serie`        | inteiro | Número da série da NFC-e a ser inutilizada.                               |
| `inicial`      | inteiro | Número inicial da faixa de numeração a ser inutilizada.                   |
| `final`        | inteiro | Número final da faixa de numeração a ser inutilizada.                     |
| `justificativa`| string  | Texto com a justificativa da inutilização (mínimo de 15 caracteres).      |

#### Caso de Uso

Utilize este endpoint quando houver números de NFC-e que não foram emitidos e não serão mais utilizados — por exemplo, após falhas no sistema, cancelamentos de contingência ou reorganização de séries. A inutilização é obrigatória para evitar inconsistências na numeração fiscal perante a SEFAZ.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body
```json
{ "serie": 1, "inicial": 900, "final": 902, "justificativa": "inutilizacao de teste via colecao postman" }
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfce/inutilizacao' \
--header 'Content-Type: application/json' \
--data '{
  "serie": 1,
  "inicial": 900,
  "final": 902,
  "justificativa": "inutilizacao de teste via colecao postman"
}'
```

---

### 🔵 [GET] Consultar Situação de NFC-e na Sefaz

**Método:** `GET` | **Endpoint:** `{{base}}/nfce/{{chave}}/situacao`

Consulta a situação de uma **NFC-e (Nota Fiscal de Consumidor Eletrônica)** diretamente na Sefaz, retornando o status atual do documento fiscal na base de dados do fisco.

#### Endpoint

```
GET {{base}}/nfce/{{chave}}/situacao

 ```

#### Variáveis utilizadas

| Variável | Descrição |
| --- | --- |
| `{{base}}` | URL base da API SmartDFe Hub |
| `{{chave}}` | Chave de acesso da NFC-e (44 dígitos) |

#### Resposta de exemplo

``` json
{
    "chave": "35260829720473000145650270000013471131355156",
    "protocolo": "13526000009075715",
    "sefaz": {
        "codigo": "101",
        "mensagem": "Cancelamento de NF-e homologado"
    },
    "situacao": "cancelado",
    "tipo": "nfce"
}

 ```

#### Campos da resposta

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `chave` | string | Chave de acesso da NFC-e consultada |
| `sefaz.codigo` | string | Código de retorno da Sefaz |
| `sefaz.mensagem` | string | Mensagem descritiva retornada pela Sefaz |
| `situacao` | string | Situação atual do documento (ex: `autorizado`, `cancelado`, `desconhecido`) |
| `tipo` | string | Tipo do documento fiscal (`nfce`) |

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfce/SUA_CHAVE_AQUI/situacao' \
```

---
### 🔵 [GET] Consultar NFC-e-Ref/uuid
**Método:** `GET`  
**URL:** `{{base}}/nfce/{{chave}}`

Consulta os dados de uma **NFC-e (Nota Fiscal de Consumidor Eletrônica, modelo 65)** localmente no emissor, identificada pela sua chave de acesso ou UUID interno.

#### Endpoint

`GET {{base}}/nfce/{{chave}}`

#### Variáveis de ambiente

| Variável | Descrição |
| --- | --- |
| `{{base}}` | URL base da API (ex.: `http://127.0.0.1:8780/api/v1`) |
| `{{chave}}` | Chave de acesso (44 dígitos) ou UUID interno da NFC-e. Preenchida automaticamente pelo script de teste da requisição **Emitir NFC-e**. |

#### Resposta

Retorna os dados completos da NFC-e consultada, incluindo status de autorização, XML e informações do documento.

#### Observações

- A variável `{{chave}}` é populada automaticamente após a emissão de uma NFC-e pelo endpoint **Emitir NFC-e**, por meio do test script daquela requisição.
    
- Certifique-se de que o ambiente correto está selecionado com as variáveis `base` preenchidas antes de executar esta requisição.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfce/SUA_CHAVE_AQUI' \
```

---
### 🔵 [GET] XML autorizado

**Método:** `GET` | **Endpoint:** `{{base}}/nfce/{{chave}}/xml`

Retorna o arquivo XML da NFC-e (Nota Fiscal de Consumidor Eletrônica, modelo 65) **autorizada** pela SEFAZ, identificada pela chave de acesso informada.

Este endpoint é utilizado para obter o XML oficial da nota após a autorização fiscal, podendo ser usado para armazenamento, reenvio ao consumidor ou integração com outros sistemas.

#### Variáveis utilizadas

| Variável | Descrição |
| --- | --- |
| `{{base}}` | URL base da API SmartDFe Hub. Exemplo: `http://127.0.0.1:8780/api/v1` |
| `{{chave}}` | Chave de acesso da NFC-e (44 dígitos) ou UUID interno. Preenchida automaticamente pelo endpoint de emissão via script de teste. |

### Resposta esperada

- **200 OK** — Retorna o conteúdo XML da NFC-e autorizada.
    
- **404 Not Found** — Chave de acesso não encontrada ou nota não autorizada.
    
- **401 Unauthorized** — Token inválido ou ausente.
    

---

### Observações

- A variável `{{chave}}` é preenchida automaticamente pelo script de teste do endpoint **Emitir NFC-e**, dispensando preenchimento manual no fluxo padrão.
    
- O XML retornado é o documento fiscal oficial assinado digitalmente pela SEFAZ.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfce/SUA_CHAVE_AQUI/xml' \
```

---
### 🔵 [GET] DANFE (PDF)

**Método:** `GET` | **Endpoint:** `{{base}}/nfce/{{chave}}/pdf`

Gera e retorna o arquivo PDF do Documento Auxiliar da Nota Fiscal (DANFE) correspondente à nota informada.

É necessário que a nota esteja autorizada ou em contingência.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfce/SUA_CHAVE_AQUI/pdf' \
```

---
### 🔵 [GET] Escpos (Impressão Térmica)

**Método:** `GET` | **Endpoint:** `{{base fiscalbox}}/nfce/{{chave}}/escpos`

Retorna os comandos Esc/POS para impressão direta em impressoras térmicas não-fiscais.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfce/SUA_CHAVE_AQUI/escpos' \
```

---
## Categoria: 3. NF-e (mod 55)


### 🟢 [POST] Emitir NF-e (mod 55)

**Método:** `POST` | **Endpoint:** `{{base}}/nfe`

Emite uma **Nota Fiscal Eletrônica (NF-e)** de modelo 55. Diferente da NFC-e, a NF-e normalmente requer dados completos do destinatário e transporte.

### Observações

- Requer emitente com CRT 3 (Regime Normal) dependendo da configuração tributária, ou CRT 1 com as devidas CSOSN.
    
- A tributação deve ser detalhada por item (CST/CSOSN + alíquotas).
    
- Para emissão em produção, altere o campo `ambiente` de `homologacao` para `producao`.

> [!WARNING]
> **Reenvio e Unicidade do UUID (`ref`)**
> Em caso de rejeição, para reenviar a mesma nota é **obrigatório alterar o UUID/ref**, pois ele deve ser único por requisição. Caso o mesmo UUID seja enviado novamente, a API retornará a resposta original gerada na primeira requisição, não reprocessando a nota.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body
```json
{
  "ref": "NFE-NORMAL-{{$timestamp}}",
  "ambiente": "homologacao", "natureza_operacao": "venda",
  "finalidade": "normal",
  "presenca": "presencial",

  "destinatario": {
    "documento": "45762835880",
    "nome": "NF-E EMITIDA EM AMBIENTE DE HOMOLOGACAO - SEM VALOR FISCAL",
    "indicador_ie": "nao_contribuinte",
    "endereco": {
      "logradouro": "Rua das Flores",
      "numero": "123",
      "bairro": "Centro",
      "codigo_municipio": "3550308",
      "municipio": "São Paulo",
      "uf": "SP",
      "cep": "01001000"
    }
  },

  "itens": [
    {
      "codigo": "123",
      "descricao": "NFCe - Homologacao",
      "quantidade": 1,
      "valor_unitario": 1.00,
      "unidade": "UN",
      "gtin": "SEM GTIN",
      "ncm": "22030000",
      "cfop": "5102",
      "cest": "0302100",

      "tributacao": {
        "origem": "0",

        "icms": {
          "csosn": "102"
        },

        "pis": {
          "cst": "99",
          "aliquota": 1.65
        },

        "cofins": {
          "cst": "01",
          "aliquota": 7.60
        },

        "ibs_cbs": {
          "cst": "000",
          "cClassTrib": "000001",
          "pIBSUF": 0.10,
          "pIBSMun": 0.00,
          "pCBS": 0.90,

          "uf": {
            "pRedAliq": 0,
            "pAliqEfet": 0,
            "pDif": 0,
            "vDif": 0,
            "vDevTrib": 0
          },

          "mun": {
            "pRedAliq": 0,
            "pAliqEfet": 0,
            "pDif": 0,
            "vDif": 0,
            "vDevTrib": 0
          },

          "cbs": {
            "pRedAliq": 0,
            "pAliqEfet": 0,
            "pDif": 0,
            "vDif": 0,
            "vDevTrib": 0
          }
        }
      }
    }
  ],

  "pagamentos": [
    {
      "forma": "dinheiro",
      "valor": 100.00
    }
  ],

  "transporte": {
    "modFrete": "9"
  }
}
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfe' \
--header 'Content-Type: application/json' \
--data '{
  "ref": "NFE-NORMAL-123456789",
  "ambiente": "homologacao",
  "natureza_operacao": "venda",
  "finalidade": "normal",
  "presenca": "presencial",
  "destinatario": {
    "documento": "45762835880",
    "nome": "NF-E EMITIDA EM HOMOLOGACAO",
    "indicador_ie": "nao_contribuinte",
    "endereco": {
      "logradouro": "Rua das Flores",
      "numero": "123",
      "bairro": "Centro",
      "codigo_municipio": "3550308",
      "municipio": "São Paulo",
      "uf": "SP",
      "cep": "01001000"
    }
  },
  "itens": [
    {
      "codigo": "123",
      "descricao": "Item de Teste",
      "quantidade": 1,
      "valor_unitario": 1.00,
      "unidade": "UN",
      "gtin": "SEM GTIN",
      "ncm": "22030000",
      "cfop": "5102",
      "cest": "0302100",
      "tributacao": {
        "origem": "0",
        "icms": { "csosn": "102" },
        "pis": { "cst": "99", "aliquota": 1.65 },
        "cofins": { "cst": "01", "aliquota": 7.60 }
      }
    }
  ],
  "pagamentos": [
    {
      "forma": "dinheiro",
      "valor": 1.00
    }
  ],
  "transporte": {
    "modFrete": "9"
  }
}'
```

---
### 🟢 [POST] Cancelar NF-e

**Método:** `POST` | **Endpoint:** `{{base}}/nfe/{{chave}}/cancelamento`

Cancela uma **NF-e (Nota Fiscal Eletrônica – mod. 55)** previamente autorizada pela SEFAZ.

#### Body
Envie um JSON com a `justificativa` (mínimo de 15 caracteres) explicando o motivo do cancelamento.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body
```json
{ "justificativa": "cancelamento de teste da nf-e em homologacao via colecao" }
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfe/SUA_CHAVE_AQUI/cancelamento' \
--header 'Content-Type: application/json' \
--data '{
  "justificativa": "cancelamento de teste da nf-e em homologacao via colecao"
}'
```

---
### 🔵 [GET] Consultar Situação da NF-e na Sefaz

**Método:** `GET` | **Endpoint:** `{{base}}/nfe/{{chave}}/situacao`

Consulta o status oficial da NF-e (mod 55) na Sefaz.

Retorna se a nota está autorizada, cancelada ou rejeitada.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfe/SUA_CHAVE_AQUI/situacao' \
```

---
### 🔵 [GET] Consultar NF-e Local

**Método:** `GET` | **Endpoint:** `{{base}}/nfe/{{chave}}`

Consulta os dados completos de uma NF-e localmente no hub da API, sem bater na SEFAZ.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfe/SUA_CHAVE_AQUI' \
```

---
### 🔵 [GET] XML Autorizado

**Método:** `GET` | **Endpoint:** `{{base}}/nfe/{{chave}}/xml`

Retorna o arquivo XML oficial da nota, devidamente assinado e com o protocolo de autorização da Sefaz anexado.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfe/SUA_CHAVE_AQUI/xml' \
```

---
### 🔵 [GET] DANFE (PDF)

**Método:** `GET` | **Endpoint:** `{{base}}/nfe/{{chave}}/pdf`

Gera e retorna o arquivo PDF do Documento Auxiliar da Nota Fiscal (DANFE) correspondente à nota informada.

É necessário que a nota esteja autorizada.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfe/SUA_CHAVE_AQUI/pdf' \
```

---
### 🟢 [POST] Carta de Correção (CC-e)

**Método:** `POST` | **Endpoint:** `{{base}}/nfe/{{chave}}/eventos`

Registra um evento de Carta de Correção para retificar erros em campos específicos da NF-e que não invalidam o documento.

#### Body
O campo `evento` deve ser `"carta_correcao"` e o campo `correcao` deve conter o texto detalhando o que está sendo ajustado.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body
```json
{ "evento": "carta_correcao", "correcao": "correcao de dados nao fiscais do documento em homologacao" }
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfe/SUA_CHAVE_AQUI/eventos' \
--header 'Content-Type: application/json' \
--data '{
  "evento": "carta_correcao",
  "correcao": "correcao de dados nao fiscais do documento em homologacao"
}'
```

---
### 🟢 [POST] Inutilizar Faixa de Numeração NF-e

**Método:** `POST` | **Endpoint:** `{{base}}/nfe/inutilizacao`

Inutiliza uma faixa de numeração de NF-e que não foi utilizada e que precisa ser inutilizada junto à SEFAZ para evitar furos na sequência numérica.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body
```json
{
  "serie": 78,
  "inicial": 16,
  "final": 16,
  "justificativa": "inutilizacao de teste homologação"
}
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/nfe/inutilizacao' \
--header 'Content-Type: application/json' \
--data '{
  "serie": 78,
  "inicial": 16,
  "final": 16,
  "justificativa": "inutilizacao de teste via colecao postman"
}'
```

---
## Categoria: 4. Serviços Complementares e Integração Avançada


### 🟢 [POST] Configurar Webhook

**Método:** `POST` | **Endpoint:** `{{base}}/webhook`

Define a URL de Webhook para recebimento de notificações assíncronas do SmartDFe Hub, como retornos da Sefaz (autorização, rejeição, etc).

Você deve enviar no body (JSON) a URL que receberá os POSTs de notificação.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body
```json
{
    "Agent version": "01.03.48",
    "ativo": true,
    "command": "",
    "retryMax": 3,
    "secret": "",
    "timeoutMs": 5000,
    "url": "http://127.0.0.1:8780/api/v1/webhook/inbound/"
}
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/webhook' \
--header 'Content-Type: application/json' \
--data '{
    "Agent version": "01.03.48",
    "ativo": true,
    "command": "",
    "retryMax": 3,
    "secret": "",
    "timeoutMs": 5000,
    "url": "http://127.0.0.1:8780/api/v1/webhook/inbound/"
}'
```

---
### 🔵 [GET] Consultar Configuração de Webhook

**Método:** `GET` | **Endpoint:** `{{base}}/webhook`

Retorna a URL do Webhook atualmente configurada no sistema para o token ativo.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/webhook' \
```

---

### 🔵 [GET] Consultar Configuração SMTP

**Método:** `GET` | **Endpoint:** `{{base}}/smtp`

Retorna as configurações atuais do servidor SMTP utilizado para envio automático de e-mails de notas autorizadas.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/smtp' \
```

---
### 🟢 [POST] Configurar SMTP

**Método:** `POST` | **Endpoint:** `{{base}}/smtp`

Configura as credenciais e parâmetros do servidor SMTP (como Host, Porta, Usuário e Senha) para o envio de e-mails com XML e PDF para os clientes.

#### Body

O body em formato JSON deve conter os parâmetros de envio de email conforme o exemplo fornecido.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body
```json
{
  "smtp_host":       "smtp.gmail.com",                  // Endereço do servidor SMTP
  "smtp_porta":      465,                               // numero da porta configurada no smtp
  "smtp_usuario":    "botautomacaopedidos@gmail.com",   // Usuário no servidor SMTP
  "smtp_senha":      "****",                            // App Password
  "smtp_tls":        false,                             // Conexão TLS
  "remetente_email": "botautomacaopedidos@gmail.com",   // E-mail remetente
  "remetente_nome":  "Empresa Fiscal",                  // Nome exibido no e-mail
  "assunto":         "Sua NFC-e nº {nNF}",              // Assunto
  "ativo":           true                               //SMTP Está habilitada?
}
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/smtp' \
--header 'Content-Type: application/json' \
--data '{
  "smtp_host":       "smtp.gmail.com",
  "smtp_porta":      465,
  "smtp_usuario":    "botautomacaopedidos@gmail.com",
  "smtp_senha":      "****",
  "smtp_tls":        false,
  "remetente_email": "botautomacaopedidos@gmail.com",
  "remetente_nome":  "Empresa Fiscal",
  "assunto":         "Sua NFC-e nº {nNF}",
  "ativo":           true
}'
```

---
### 🔵 [GET] Consultar IE por CNPJ

**Método:** `GET` | **Endpoint:** `{{base}}/util/ie/{cnpj}/?uf={uf}`

Consulta a Inscrição Estadual e o status cadastral de uma empresa na Sefaz a partir do seu CNPJ.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/util/ie/29720473000145/?uf=sp' \
```

---

### 🔵 [GET] Capacidades da API

**Método:** `GET` | **Endpoint:** `{{base}}/capacidades`

Retorna os recursos e capacidades suportados pelo SmartDFe Hub, como status de serviços e módulos habilitados.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/capacidades' \
```

---
### 🔵 [GET] Health Check

**Método:** `GET` | **Endpoint:** `{{base}}/health`

Verifica a saúde e disponibilidade da API e seus serviços dependentes.

Retorna o status atual de operação do sistema.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/health'
```

---
### 🔵 [GET] Contrato OpenAPI

**Método:** `GET` | **Endpoint:** `{{base}}/openapi.json`

Retorna o contrato completo da API no formato OpenAPI (Swagger), contendo a especificação técnica de todos os endpoints disponíveis.

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8780/api/v1/openapi.json'
```

---

## Finalização e Homologação
Após concluir a integração de todas as etapas críticas e realizar os testes no ambiente de homologação, é necessário preencher o formulário de homologação.
Para enviar suas evidências e validar sua integração junto à Elgin, acesse o link abaixo:
🔗 [Acessar Formulário de Homologação](https://forms.cloud.microsoft/r/zAVEtYCceC)

---

## Suporte
Em caso de dúvidas durante o processo de integração ou homologação, entre em contato com a equipe de especialistas Elgin:
📱 +55 11 99851-9559 — opção 2