# SmartDFe API Nativa — Documentação para Integração (via XML)

API responsável pela comunicação com o software **SmartDFe** e o dispositivo **FiscalBox** para emissão e gerenciamento de documentos fiscais eletrônicos, com envio do XML diretamente na requisição.

Ao instalar o **SmartDFe** ou o **FiscalBox**, uma API REST local estará disponível na porta **3001** (padrão). Basta integrar sua aplicação aos endpoints apresentados nesta documentação para utilizar os recursos de emissão fiscal disponibilizados pela solução.

---

## Visão Geral

### URL Base

| Ambiente | URL |
|----------|-----|
| Local (SmartDFe) | `http://localhost:3001` |
| FiscalBox (rede local) | `http://[IP_FISCALBOX]:3001` |
| Exemplo FiscalBox | `http://192.168.0.19:3001` |

### Formas de Envio do XML

A API suporta duas formas de envio do XML nas requisições de emissão:

| Forma | Content-Type | Descrição |
|-------|-------------|-----------|
| Upload de arquivo | `multipart/form-data` | Envia o arquivo `.xml` salvo em disco |
| XML como texto | `application/xml` ou `text/plain` | Envia o conteúdo XML diretamente no corpo da requisição |

> **Atenção:** Habilite apenas **uma** das formas por vez na requisição.

---

## Retornos e Tratamento de Erros

As respostas dos endpoints retornam um JSON com o campo **`cStat`**, que é o código de status oficial da SEFAZ.

| Campo | Descrição |
|-------|-----------|
| `cStat` | Código de status oficial SEFAZ. **É este campo que deve ser usado para validações de negócio.** |
| `xMotivo` | Descrição textual do resultado (motivo de sucesso ou rejeição). |
| `data` | Conteúdo da resposta (ex.: XML autorizado em Base64). |

> ⚠️ **Importante:** O **Status HTTP = 200** indica apenas que a comunicação entre o software e a API foi realizada com sucesso. Para determinar se a operação foi efetivamente processada, analise sempre o campo **`cStat`**.

### Referências úteis

- [Campos JSON disponíveis no retorno do emissor](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Campos-Json-Retorno-Emissor)
- [Tabela de retornos SEFAZ](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe%5D-Tabela-de-Retornos)
- [Retorno Webhook Fiscal](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Retorno-Webhook-Fiscal)

---

## Variáveis de rota

| Variável | Descrição |
|----------|-----------|
| `{{URL Base}}` | URL base do SmartDFe ou FiscalBox. |
| `{{UUID}}` | UUID da requisição de emissão. |
| `{{Chave NFCE}}` | Chave de acesso da NFC-e (44 dígitos). |
| `{{Protocolo NFCE}}` | Protocolo de autorização da NFC-e. |
| `{{UUID NFCE}}` | UUID da NFC-e utilizado para consulta de status. |
| `{{nNF}}` | Número da nota fiscal. |

### Exemplo

http
GET {{URL Base}}/v1/nfce/{{UUID NFCE}}

---

## Grupos de Endpoints

- [1. Emissão, Consulta, etc.](#1-emissão-consulta-etc)
- [2. Webhook](#2-webhook)
- [3. Servidor SMTP](#3-servidor-smtp)
- [4. DANFE](#4-danfe)

---

## 1. Emissão, Consulta, etc.

Endpoints de emissão, consulta, cancelamentos, inutilização, entre outros.

---

### 1.1 EmitirNotaUUID

Emite uma **NFC-e (modelo 65)** ou **NF-e (modelo 55)** a partir de um XML, utilizando um UUID gerado dinamicamente como identificador único da requisição.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `POST` |
| **URL** | `{{URL Base}}/api/v1/nfce/xml/{{$guid}}` |
| **Autenticação** | Nenhuma |

#### Documentos aceitos

- NF-e (modelo 55)
- NFC-e (modelo 65)

#### Body (form-data)

Habilite apenas **uma** das opções abaixo:

**Opção 1 — Upload de arquivo:**

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `Arquivo xml` | `file` | Arquivo `.xml` da NFC-e / NF-e enviado diretamente do sistema de arquivos |

**Opção 2 — XML como texto:**

| Campo | Tipo | Descrição |
|-------|------|-----------|
| XML como texto | `application/xml` ou `text/plain` | Envia o conteúdo XML diretamente no corpo da requisição |

**Estrutura esperada do XML:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<NFe xmlns="http://www.portalfiscal.inf.br/nfe">
  <infNFe Id="NFe35260729720473000145650010000011381427313773" versao="4.00">
    ...
  </infNFe>
</NFe>
```

#### Resposta de Sucesso (cStat 100)

```json
{
  "Agent version": "01.03.38",
  "Error": "100",
  "IpPDV": "::1",
  "Tempo comando(ms)": "433",
  "Tempo médio SEFAZ(ms)": "354",
  "cNF": "18075986",
  "cStat": "100",
  "xMotivo": "Autorizado o uso da NF-e",
  "chNFe": "35260729720473000145650270000013241180759860",
  "command": "EmitirNota",
  "dhRecibo": "31/07/2026 11:12:58",
  "modeloDFe": "65",
  "nNF": "1324",
  "nProto": "13526000008999199",
  "pendingContingencyInvoices": "0",
  "serie": "27",
  "totalNF": "15.11",
  "tpAmb": "2",
  "tpEmis": "1",
  "uuid": "49783142-0c86-45e4-9c6e-8fc154e543d1",
  "versao_base": "2026.07-multi-r2",
  "data": "<XML autorizado em base64>",
  "danfe_base64": "<DANFE em base64>",
  "digVal": "bmVZIrIwg9z2DAo9IOcJRxjcXMg=",
  "SignatureValue": "<assinatura digital>",
  "certificateExpirationDate": "2027-01-15 12:39:25",
  "certificareExpiryCountdown": "168",
  "licenseExpiryCountdown": "Perpetual"
}
```

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `cStat` | string | Código de status SEFAZ. `100` = Autorizado |
| `xMotivo` | string | Descrição do resultado |
| `chNFe` | string | Chave de acesso (44 dígitos) |
| `nProto` | string | Protocolo de autorização |
| `uuid` | string | UUID da requisição |
| `data` | string | XML autorizado em Base64 |
| `danfe_base64` | string | DANFE em Base64 |
| `tpAmb` | string | Ambiente: `1` = Produção, `2` = Homologação |

Usar link na cessão **Referências úteis** no inicio da documentação para ter acesso a documentação de retornos.

#### Pós-resposta:

Cada requisição de emissão deve utilizar um UUID único. Esse identificador é utilizado pela API para identificar a operação, evitando o processamento duplicado da mesma solicitação (idempotência).

A API é responsável por todo o processo fiscal de geração do documento, incluindo:

Controle da sequência da numeração da NFC-e;
Geração da chave de acesso;
Assinatura digital do XML;
Geração do QR Code;
Retorno dos identificadores do documento autorizado.

Por esse motivo, recomenda-se capturar e armazenar as informações retornadas na resposta da API, pois elas serão utilizadas em operações posteriores, como impressão, consulta, cancelamento, entre outras opções.
---

### 1.2 CancelarNota

Cancela uma **NFC-e / NF-e** previamente autorizada, registrando o evento de cancelamento junto à SEFAZ.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `POST` |
| **URL** | `{{URL Base}}/api/v1/nfce/{{Chave NFCE}}/cancel` |
| **Autenticação** | Nenhuma |

#### Variáveis de Rota

| Variável | Descrição |
|----------|-----------|
| `{{Chave NFCE}}` | Chave de acesso da NFC-e a ser cancelada (44 dígitos) |

#### Body (JSON)

```json
{
  "justificativa": "Cancelamento para testes",
  "protocolo": "{{Protocolo NFCE}}"
}
```

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `justificativa` | string | Motivo do cancelamento. Deve ter entre **15 e 255** caracteres. |
| `protocolo` | string | Número do protocolo de autorização da NFC-e. |

#### Resposta de Sucesso

```json
{
  "Agent version": "01.03.38",
  "chNFe": "35260729720473000145650270000013251451581997",
  "infEvento": "{\"cOrgao\":\"35\",\"cStat\":\"135\",\"chNFe\":\"...\",\"dhRegEvento\":\"2026-07-31T13:31:30-03:00\",\"nProt\":\"13526000009004569\",\"nSeqEvento\":\"1\",\"tpAmb\":\"2\",\"tpEvento\":\"110111\",\"verAplic\":\"SP_EVENTOS_PL_100\",\"xEvento\":\"Cancelamento registrado\",\"xMotivo\":\"Evento registrado e vinculado a NF-e\"}",
  "ok": true
}
```

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `chNFe` | string | Chave de acesso da NFC-e cancelada |
| `infEvento.cStat` | string | `135` = Evento registrado e vinculado à NF-e |
| `infEvento.nProt` | string | Protocolo do evento de cancelamento |
| `infEvento.tpEvento` | string | `110111` = Cancelamento |
| `ok` | boolean | `true` se operação concluída com sucesso |

---

### 1.3 ConsultaNota

Consulta os dados de uma **NFC-e / NF-e** a partir de sua chave de acesso.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `GET` |
| **URL** | `{{URL Base}}/api/v1/nfce/{{Chave NFCE}}` |
| **Autenticação** | Nenhuma |

#### Variáveis de Rota

| Variável | Descrição |
|----------|-----------|
| `{{Chave NFCE}}` | Chave de acesso da NFC-e (44 dígitos) |

> A chave de acesso é um código numérico de **44 dígitos** que identifica unicamente cada documento fiscal eletrônico.

#### Resposta

Analise o campo `cStat` para determinar o resultado — este é o retorno oficial SEFAZ.

---

### 1.4 CancelamentoPorSubstituicao

Cancela uma **NFC-e** já autorizada por meio de substituição, emitindo uma nova nota fiscal para corrigir a original. Utilize quando a nota a ser cancelada já foi enviada ao consumidor e o prazo regular de cancelamento simples foi ultrapassado.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `POST` |
| **URL** | `{{URL Base}}/api/v1/nfce/{{Chave NFCE}}/cancellationBySubstitution` |
| **Autenticação** | Nenhuma |

#### Headers

| Header | Valor |
|--------|-------|
| `Content-Type` | `application/json` |
| `Accept` | `application/json` |
| `Accept-Encoding` | `application/gzip` |
| `User-Agent` | `Go-http-client/1.1` |

#### Body (JSON)

```json
{
  "chCFeRef": "Chave da chCFeRef",
  "tpAutor": 3,
  "xJust": "emitida com erro 'Justificativa'"
}
```

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `chCFeRef` | string | Chave de acesso do CF-e de referência (nota substituída). |
| `tpAutor` | integer | Tipo do autor do cancelamento. `3` = Contribuinte. |
| `xJust` | string | Justificativa do cancelamento (mínimo **15 caracteres**). |

#### Códigos de Resposta (cStat)

| cStat | Descrição | Tratamento sugerido |
|-------|-----------|---------------------|
| `128` | Lote de Evento Processado | Verifique o cStat do evento individual. |
| `135` | Evento registrado e vinculado à NF-e | ✅ **Sucesso.** Cancelamento por substituição homologado. |
| `136` | Evento registrado, mas não vinculado à NF-e | Verificar o motivo retornado. |
| `155` | Cancelamento homologado fora de prazo | ✅ **Sucesso**, porém realizado fora do prazo permitido pela UF. |

---

### 1.5 CartaDeSubstituição (CC-e)

Emite uma **Carta de Correção Eletrônica (CC-e)** para uma NF-e previamente autorizada, permitindo a correção de determinados campos sem cancelamento.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `POST` |
| **URL** | `{{URL Base}}/api/v1/nfe/{{Chave NFCE}}/electronicCorrectionLetter` |
| **Autenticação** | Nenhuma |

> ⚠️ **Atenção:** Nem todos os campos podem ser corrigidos via CC-e. Campos como valores de impostos, dados do destinatário e quantidades de itens geralmente **não são elegíveis** para correção. Consulte as regras da SEFAZ.

#### Headers

| Header | Valor |
|--------|-------|
| `Content-Type` | `application/json` |
| `Accept` | `application/json` |
| `Accept-Encoding` | `application/gzip` |
| `User-Agent` | `Go-http-client/1.1` |

#### Body (JSON)

```json
{
  "xCorrecao": "ncm invalido"
}
```

| Campo | Tipo | Obrigatório | Descrição |
|-------|------|-------------|-----------|
| `xCorrecao` | string | ✅ Sim | Texto da correção. Mínimo de **15 caracteres** conforme as regras da SEFAZ. |

#### Resposta

Retorna um JSON com o resultado do envio da CC-e à SEFAZ, incluindo o protocolo de autorização e os detalhes do evento em caso de sucesso. Analise o campo `cStat`.

---

### 1.6 InutilizarNumeracao

Inutiliza uma faixa de números de NFC-e que nunca serão utilizados, registrando a lacuna na SEFAZ para manter a sequência de numeração consistente.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `POST` |
| **URL** | `{{URL Base}}/api/v1/nfce/discard` |
| **Autenticação** | Nenhuma |

#### Headers

| Header | Valor |
|--------|-------|
| `Content-Type` | `application/json` |
| `Accept` | `application/json` |
| `Accept-Encoding` | `application/gzip` |
| `User-Agent` | `Go-http-client/1.1` |

#### Body (JSON)

```json
{
  "ano": "26",
  "cnpj": "29720473000145",
  "justificativa": "inutilização para testes",
  "numeroInicial": 28,
  "numeroFinal": 28,
  "serie": 506,
  "tpAmb": 2,
  "uf": "35"
}
```

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `ano` | string | Ano com dois dígitos (ex.: `"26"` para 2026). |
| `cnpj` | string | CNPJ da empresa emitente (somente números, sem formatação). |
| `justificativa` | string | Motivo da inutilização. Mínimo de **15 caracteres**. |
| `numeroInicial` | integer | Primeiro número da faixa a ser inutilizada. |
| `numeroFinal` | integer | Último número da faixa. Use o mesmo valor de `numeroInicial` para inutilizar um único número. |
| `serie` | integer | Número de série do documento. |
| `tpAmb` | integer | Tipo de ambiente: `1` = Produção, `2` = Homologação. |
| `uf` | string | Código IBGE do estado (ex.: `"35"` para São Paulo). |

> ⚠️ **Atenção:** Esta operação é **irreversível**. Uma vez que os números são inutilizados na SEFAZ, eles não podem ser reutilizados.

---

### 1.7 ConsultarProcessamentoContigencia

Consulta o status do processamento das NFC-e emitidas em **modo de contingência** (off-line). Utilize para verificar se as notas emitidas durante uma indisponibilidade da SEFAZ foram transmitidas e autorizadas com sucesso.

> 💡 Desconsidere este endpoint se preferir utilizar o **Webhook** para esta função.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `GET` |
| **URL** | `{{URL Base}}/api/v1/nfce/processamentocontigencia` |
| **Autenticação** | Nenhuma |
| **Body** | Nenhum |

#### Headers

| Header | Valor |
|--------|-------|
| `User-Agent` | `PostmanRuntime/7.44.1` |
| `Accept` | `*/*` |
| `Connection` | `keep-alive` |
| `Accept-Encoding` | `application/gzip, deflate, br` |

#### Resposta

Retorna os documentos processados em contingência. Analise os campos `cStat` e `xMotivo` para determinar se o processamento foi concluído.

> ⚠️ **Atenção:** Este endpoint deve ser consultado após o restabelecimento da conexão com a SEFAZ. Notas não autorizadas podem exigir reenvio ou cancelamento.

---

### 1.8 ConsultaStatusUUID

Consulta o status da emissão de uma **NFC-e** pelo seu UUID. Armazena as **100 últimas emissões**.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `GET` |
| **URL** | `{{URL Base}}/api/v1/nfce/authorization/status/{{UUID NFCE}}` |
| **Autenticação** | Nenhuma |

#### Variáveis de Rota

| Variável | Descrição |
|----------|-----------|
| `{{UUID NFCE}}` | UUID retornado pela API após a emissão |

#### Headers

| Header | Valor |
|--------|-------|
| `User-Agent` | `PostmanRuntime/7.44.1` |
| `Accept` | `*/*` |
| `Connection` | `keep-alive` |
| `Accept-Encoding` | `application/gzip, deflate, br` |

#### Resposta

Retorna os dados completos da transação pelo UUID (emissão, rejeição, contingência, etc.).

---

## 2. Webhook

Endpoints de configuração e consulta do webhook do SmartDFe.

O SmartDFe dispara notificações para a URL configurada nos seguintes eventos:

| Evento | Descrição |
|--------|-----------|
| Nota autorizada | Documento fiscal autorizado pela SEFAZ |
| Nota rejeitada | Documento fiscal rejeitado pela SEFAZ |
| Nota cancelada | Documento fiscal cancelado |
| Contingência ativada | Ativado via portal |
| Contingência desativada | Desativado via portal |
| Contingência sincronizada | Ciclos de contingência e envio de notas pendentes |

---

### 2.1 Configurar Webhook SmartDFe

Configura o webhook para receber notificações de eventos fiscais.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `POST` |
| **URL** | `{{URL Base}}/api/v1/webhook/config` |

#### Body (JSON)

```json
{
  "url": "http://127.0.0.1:5050/webhook",
  "ativo": true,
  "retryMax": 3,
  "timeoutMs": 5000
}
```

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `url` | string | URL do endpoint que receberá as notificações do webhook. |
| `ativo` | boolean | `true` para ativar, `false` para desativar o webhook. |
| `retryMax` | integer | Número máximo de tentativas de reenvio em caso de falha. |
| `timeoutMs` | integer | Timeout em milissegundos para a chamada ao endpoint do webhook. |

---

### 2.2 Consultar Configuração Webhook

Retorna a configuração atual do webhook.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `GET` |
| **URL** | `{{URL Base}}/api/v1/webhook/config` |

#### Resposta

Retorna os dados da configuração atual do webhook (URL, status, retry, timeout).

---

## 3. Servidor SMTP

Endpoints de configuração e consulta do servidor SMTP para envio de e-mails com os documentos fiscais emitidos.

---

### 3.1 Configurar Email

Configura o servidor SMTP para envio de e-mails com os documentos fiscais emitidos.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `POST` |
| **URL** | `{{URL Base}}/api/v1/email/config` |

#### Body (JSON)

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

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `smtp_host` | string | Endereço do servidor SMTP (ex.: `smtp.gmail.com`). |
| `smtp_porta` | integer | Porta configurada no servidor SMTP (ex.: `465` para SSL, `587` para TLS). |
| `smtp_usuario` | string | Usuário de autenticação no servidor SMTP (geralmente o e-mail). |
| `smtp_senha` | string | Senha de autenticação. Para Gmail, utilize uma **App Password**. |
| `smtp_tls` | boolean | `true` para usar conexão TLS, `false` para SSL. |
| `remetente_email` | string | E-mail do remetente exibido nas mensagens. |
| `remetente_nome` | string | Nome do remetente exibido nas mensagens. |
| `assunto` | string | Assunto do e-mail. Suporta variável `{nNF}` para o número da nota. |
| `ativo` | boolean | `true` para habilitar o envio de e-mails, `false` para desabilitar. |

---

### 3.2 Consultar Configuração de Email

Retorna a configuração atual do servidor SMTP.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `GET` |
| **URL** | `{{URL Base}}/api/v1/email/config` |

#### Resposta

Retorna os dados da configuração SMTP cadastrada (host, porta, usuário, remetente, status).

---

## 4. DANFE

**Pré-visualização da DANFE** — Gera uma prévia da DANFE antes da emissão da NF-e, permitindo validar as informações do documento e evitar cancelamentos decorrentes de erros.

> 📌 **Disponível exclusivamente para o modelo 55 (NF-e).**

---

### 4.1 Pré-visualização da DANFE modelo 55 (NF-e)

Gera uma prévia da DANFE de uma NF-e antes de sua emissão definitiva.

| Propriedade | Valor |
|-------------|-------|
| **Método** | `POST` |
| **URL** | `{{URL Base}}/api/v1/nfe/danfe_preview` |

#### Body (form-data)

| Campo | Tipo | Descrição |
|-------|------|-----------|
| `Arquivo xml` | `file` | Arquivo `.xml` da NF-e (modelo 55) para pré-visualização. |


---

## Sumário de Endpoints

| # | Nome | Método | URL |
|---|------|--------|-----|
| 1.1 | EmitirNotaUUID | `POST` | `/api/v1/nfce/xml/{{UUID}}` |
| 1.2 | CancelarNota | `POST` | `/api/v1/nfce/{{Chave NFCE}}/cancel` |
| 1.3 | ConsultaNota | `GET` | `/api/v1/nfce/{{Chave NFCE}}` |
| 1.4 | CancelamentoPorSubstituicao | `POST` | `/api/v1/nfce/{{Chave NFCE}}/cancellationBySubstitution` |
| 1.5 | CartaDeSubstituição (CC-e) | `POST` | `/api/v1/nfe/{{Chave NFCE}}/electronicCorrectionLetter` |
| 1.6 | InutilizarNumeracao | `POST` | `/api/v1/nfce/discard` |
| 1.7 | ConsultarProcessamentoContigencia | `GET` | `/api/v1/nfce/processamentocontigencia` |
| 1.8 | ConsultaStatusUUID | `GET` | `/api/v1/nfce/authorization/status/{{UUID NFCE}}` |
| 2.1 | Configurar Webhook | `POST` | `/api/v1/webhook/config` |
| 2.2 | Consultar Configuração Webhook | `GET` | `/api/v1/webhook/config` |
| 3.1 | Configurar Email (SMTP) | `POST` | `/api/v1/email/config` |
| 3.2 | Consultar Configuração de Email | `GET` | `/api/v1/email/config` |
| 4.1 | Pré-visualização DANFE (NF-e modelo 55) | `POST` | `/api/v1/nfe/danfe_preview` |

---
