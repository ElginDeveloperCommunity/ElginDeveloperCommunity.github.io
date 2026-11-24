# Documentação do SmartDFe API Nativa (xml)

Antes de iniciar a integração, deve ser instalado o software no seu computador de desenvolvimento seguindo o procedimento do link: [Guia de Instalação do Elgin SmartDFe](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe%5D-Guia-de-Instala%C3%A7%C3%A3o-do-Elgin-SmartDFe).

Após a instalação do Elgin SmartDFe, a API será automaticamente ativada na porta 8781. Esta API não requer autenticação por token.

Esta documentação apresenta a coleção completa de endpoints da API do SmartDFe Nativa. Através dela, é possível realizar a emissão e o gerenciamento de documentos fiscais eletrônicos utilizando exclusivamente o formato XML.

# Visão Geral

API responsável pela comunicação com o **SmartDFe** e o **FiscalBox**, permitindo a emissão e o gerenciamento de documentos fiscais.

## URL Base

| Ambiente | URL Base |
|----------|----------|
| **Software (SmartDFe)** | `http://localhost:8781` |
| **Hardware (FiscalBox)** | `http://[IP_FISCALBOX]:8781` |

**Exemplo:**

```text
http://192.168.0.19:8781 - > Substitua `IP_FISCALBOX` pelo endereço IP configurado do FiscalBox na sua rede local.

```

**Formato de Envio (Payload)**

Esta API aceita somente XML para realizar as emissões. Pode ser enviado de duas formas:
- Como arquivo XML (`multipart/form-data`);
- Diretamente no corpo (`body`) da requisição em texto puro (`application/xml`).

> [!IMPORTANT]
> **Reenvio de Notas e Unicidade (UUID)**
> Ao emitir uma nota, o UUID enviado na URL deve ser **único por requisição**. Em caso de rejeição da nota, se desejar reenviá-la, é **obrigatório alterar o UUID**. Se você enviar uma requisição com um UUID já utilizado anteriormente, a API não processará o novo envio e retornará exatamente a mesma resposta original da primeira tentativa.

Contrato: {{base}}/openapi.json | Docs: {{base}}/docs

## Guia de Integração Rápida

O que facilita a integração nesta API é que, se o seu software já gera o arquivo XML da nota, você só precisa enviá-lo.

**1. Primeiro Passo:** Baixe os exemplos de XML no repositório do GitHub para comparar e entender exatamente a estrutura esperada pela API.

**2. Fluxo Principal:** Siga diretamente para a emissão de notas enviando seu XML para o endpoint de emissão (`/api/v1/nfce/xml/`).

**3. Funcionalidades Opcionais:** Endpoints referentes a **SMTP** e **Webhook** são complementares e opcionais.

---

## Categoria: 1. Emissão, Consulta, etc


### 🟢 [POST] Emitir Nota

**Método:** `POST` | **Endpoint:** `{{base}}/api/v1/nfce/xml/{{$guid}}`

Emite uma Nota Fiscal de Consumidor Eletrônica NFC-e ou NFe a partir de um XML da nota, utilizando um UUID gerado dinamicamente como identificador único da requisição.

Documentos aceitos na rota:
- NFe
- NFCe

#### Requisitos de Envio

Para realizar a emissão através deste endpoint, é obrigatório enviar os dois parâmetros a seguir:

| Parâmetro | Local | Obrigatório | Descrição |
| :--- | :--- | :---: | :--- |
| **UUID** | Path (URL) | **Sim** | Identificador único (GUID/UUID) gerado dinamicamente, passado diretamente na URL da requisição. Garante a unicidade da operação e evita duplicidade. |
| **XML** | Body | **Sim** | O conteúdo ou arquivo do XML da nota fiscal a ser emitida. |

> [!WARNING]
> **Reenvio e Unicidade do UUID**
> Em caso de rejeição, para reenviar a mesma nota é **obrigatório alterar o UUID** na URL, pois ele deve ser único por requisição. Caso o mesmo UUID seja enviado novamente, a API retornará a resposta original gerada na primeira requisição, não reprocessando a nota.

> ⚠️ **Atenção — Status HTTP x Código SEFAZ**
>
> **Status HTTP = 200** indica apenas que a comunicação entre o software e a API foi realizada com sucesso. Para determinar se a operação foi efetivamente processada, deve-se analisar o valor do campo **`cStat`**, que contém o resultado da operação e deve ser utilizado para o tratamento de erros e validações de negócio.
> 
> 💡 **Dica de Integração:** Consulte o nosso [Guia de Tratamento de Retorno](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Tratamento-de-Retorno) para entender em detalhes como lidar com os códigos.

### Body

Esta requisição aceita duas formas de envio do XML. Habilite apenas uma por vez:

#### Opção 1 — Upload de arquivo

| Campo | Tipo | Descrição |
| --- | --- | --- |
| Arquivo xml | file | Arquivo `.xml` da NFC-e / NF-e enviado diretamente do sistema de arquivos |

#### Opção 2 — XML como texto (raw)

| Campo | Tipo | Descrição |
| --- | --- | --- |
| Texto puro xml | text ou xml | Conteúdo XML da NFC-e / NF-e enviado diretamente como texto |

**Exemplo de estrutura esperada (Opção 2):**

```xml
<NFe xmlns="http://www.portalfiscal.inf.br/nfe">
  <infNFe Id="..." versao="4.00">
    ...
  </infNFe>
</NFe>
```

### O que deve ser enviado no XML?

Você deve enviar apenas a **estrutura base** da nota (grupos `<ide>`, `<emit>`, `<det>`, `<transp>`, `<pag>`, etc.).

> 💡 **A API cuida do trabalho pesado!**
> Você **não precisa** enviar:
> - **Cálculos de Totais** (grupo `<total>`)
> - **Assinatura Digital** (grupo `<Signature>`)
> - **QR Code**
> - **Numeração da Nota (`nNF`)**: Você pode enviar a tag `<nNF>` vazia ou com qualquer número, pois a API controla a numeração sequencial e irá sobrescrever esse valor. **Importante:** Você deve capturar o número gerado (campo `nNF`) no JSON de resposta da API para salvar no seu banco de dados!
> 
> A API faz os cálculos finais, gera a numeração, o QR Code e assina o XML automaticamente antes de autorizar.

**Exemplo prático de envio (Resumido):**

```xml
<?xml version="1.0" ?>
<NFe xmlns="http://www.portalfiscal.inf.br/nfe">
  <infNFe Id="NFe35260829720473000145654070000000181254843935" versao="4.00">
    <ide> ... </ide>
    <emit> ... </emit>
    <det nItem="1">
      <prod> ... </prod>
      <imposto> ... </imposto>
    </det>
    <transp> ... </transp>
    <pag> ... </pag>
    <!-- Não precisa incluir a tag <total> ou a <Signature> -->
  </infNFe>
</NFe>
```

### Exemplo cURL

**Opção 1 (Upload de arquivo):**
```bash
curl --location 'http://127.0.0.1:8781/api/v1/nfce/xml/seu-uuid-aqui' \
--form 'Arquivo xml=@"/caminho/para/seu/arquivo.xml"'
```

**Opção 2 (XML como texto - raw):**
```bash
curl --location 'http://127.0.0.1:8781/api/v1/nfce/xml/seu-uuid-aqui' \
--header 'Content-Type: application/xml' \
--data '<NFe xmlns="http://www.portalfiscal.inf.br/nfe">
  <infNFe Id="..." versao="4.00">
    ...
  </infNFe>
</NFe>'
```

### Respostas Esperadas

Quando a nota é processada, a resposta conterá o status da comunicação.

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

---
### 🟢 [POST] CancelarNota

**Método:** `POST` | **Endpoint:** `{{base}}/api/v1/nfce/{{chave}}/cancel`

Cancela uma NFC-e / NF-e previamente emitida, registrando o evento de cancelamento junto à SEFAZ.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body

```json
{
  "justificativa": "Cancelamento para testes",
  "protocolo": "{{protocolo}}"
}
```

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `justificativa` | string | Motivo do cancelamento. Deve ter entre 15 e 255 caracteres. |
| `protocolo` | string | Número do protocolo de autorização da NFC-e. |

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/nfce/SUA_CHAVE_NFCE_AQUI/cancel' \
--header 'Content-Type: application/json' \
--data '{
  "justificativa": "Cancelamento para testes",
  "protocolo": "13526000009004569"
}'
```

### Resposta de Sucesso

```json
{
    "Agent version": "01.03.38",
    "chNFe": "35260729720473000145650270000013251451581997",
    "infEvento": "{\"cOrgao\":\"35\",\"cStat\":\"135\",\"chNFe\":\"35260729720473000145650270000013251451581997\",\"dhRegEvento\":\"2026-07-31T13:31:30-03:00\",\"nProt\":\"13526000009004569\",\"nSeqEvento\":\"1\",\"tpAmb\":\"2\",\"tpEvento\":\"110111\",\"verAplic\":\"SP_EVENTOS_PL_100\",\"xEvento\":\"Cancelamento registrado\",\"xMotivo\":\"Evento registrado e vinculado a NF-e\"}",
    "ok": true
}
```

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `Agent version` | string | Versão do agente SmartDFe utilizado no processamento. |
| `chNFe` | string | Chave de acesso da NFC-e cancelada (44 dígitos). |
| `infEvento` | string | Informações do evento de cancelamento retornadas pela SEFAZ. |
| `ok` | boolean | Indica se a operação foi concluída com sucesso. |

---
### 🔵 [GET] ConsultaNota

**Método:** `GET` | **Endpoint:** `{{base}}/api/v1/nfce/{{chave}}`

Consulta os dados de uma **NFC-e (Nota Fiscal de Consumidor Eletrônica)** a partir de sua chave de acesso. A chave de acesso é um código numérico de 44 dígitos que identifica unicamente cada documento fiscal eletrônico.

### Respostas

| Código | Descrição |
| --- | --- |
| cStat | Deve considerar o campo cStat que é o retorno oficial Sefaz. |

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/nfce/SUA_CHAVE_NFCE_AQUI'
```

---
### 🟢 [POST] CancelamentoPorSubstituicao

**Método:** `POST` | **Endpoint:** `{{base}}/api/v1/nfce/{{chave}}/cancellationBySubstitution`

Cancela uma NFC-e já autorizada por meio de substituição, emitindo uma nova nota fiscal para corrigir a original. Este endpoint deve ser utilizado quando a nota a ser cancelada já foi enviada ao consumidor e o prazo regular de cancelamento simples foi ultrapassado.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |
| `Accept` | `application/json` |

### Exemplo de Body

```json
{
  "chCFeRef": "Chave da chCFeRef",
  "tpAutor": 3,
  "xJust": "emitida com erro 'Justificativa'"
}
```

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `chCFeRef` | string | Chave de acesso do CF-e de referência (nota substituída). |
| `tpAutor` | integer | Tipo do autor do cancelamento. `3` = Contribuinte. |
| `xJust` | string | Justificativa do cancelamento por substituição (mínimo 15 caracteres). |

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/nfce/SUA_CHAVE_NFCE_AQUI/cancellationBySubstitution' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Accept-Encoding: application/gzip' \
--data '{
  "chCFeRef": "Chave da chCFeRef",
  "tpAutor": 3,
  "xJust": "emitida com erro Justificativa"
}'
```

### Respostas Esperadas

| cStat | Descrição | Tratamento sugerido |
| --- | --- | --- |
| **135** | Evento registrado e vinculado à NF-e | **Sucesso.** Cancelamento por substituição homologado. |
| **136** | Evento registrado, mas não vinculado à NF-e | O evento foi recebido, porém não foi associado à NFC-e. Verificar o motivo retornado. |
| **155** | Cancelamento homologado fora de prazo | **Sucesso**, porém realizado fora do prazo permitido pela UF. |

---
### 🟢 [POST] CartaDeSubstituição

**Método:** `POST` | **Endpoint:** `{{base}}/api/v1/nfe/{{chave}}/electronicCorrectionLetter`

Emite uma **Carta de Correção Eletrônica (CC-e)** para uma NF-e ou NFC-e previamente autorizada. A CC-e permite a correção de determinados campos do documento fiscal após a sua autorização, sem a necessidade de cancelamento e reemissão do documento.

> **Atenção:** Nem todos os campos podem ser corrigidos via CC-e. Campos como valores de impostos, dados do destinatário e quantidades de itens geralmente não são elegíveis para correção. Consulte as regras da SEFAZ para a lista completa de campos corrigíveis.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |
| `Accept` | `application/json` |

### Exemplo de Body

```json
{
  "xCorrecao": "ncm invalido"
}
```

| Campo | Tipo | Obrigatório | Descrição |
|-------|------|-------------|----------|
| `xCorrecao` | string | Sim | Texto da correção descrevendo o que está sendo corrigido no documento fiscal. Mínimo de 15 caracteres conforme as regras da SEFAZ. |

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/nfe/SUA_CHAVE_NFCE_AQUI/electronicCorrectionLetter' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Accept-Encoding: application/gzip' \
--data '{
  "xCorrecao": "ncm invalido"
}'
```

---
### 🟢 [POST] InutilizarNumeracao

**Método:** `POST` | **Endpoint:** `{{base}}/api/v1/nfce/discard`

Inutiliza uma faixa de números de NFC-e que nunca serão utilizados, registrando a lacuna na SEFAZ para manter a sequência de numeração consistente.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |
| `Accept` | `application/json` |

### Exemplo de Body

```json
{
  "ano": "26",
  "cnpj": "29720473000145",
  "justificativa": "inutilização para testes",
  "numeroInicial": 28,
  "numeroFinal": 28,
  "serie": 506,
  "tpAmb": 2,
  "uf": "35",
  "mod": "55"
}
```

| Campo | Tipo | Descrição |
| --- | --- | --- |
| `ano` | string | Ano com dois dígitos da sequência de numeração a ser inutilizada (ex.: `"26"` para 2026). |
| `cnpj` | string | CNPJ da empresa emitente (somente números, sem formatação). |
| `justificativa` | string | Motivo/justificativa para a inutilização dos números (mínimo de 15 caracteres). |
| `numeroInicial` | integer | Primeiro número do documento na faixa a ser inutilizada. |
| `numeroFinal` | integer | Último número do documento na faixa a ser inutilizada. |
| `serie` | integer | Número de série do documento. |
| `tpAmb` | integer | Tipo de ambiente: `1` = Produção, `2` = Homologação (testes). |
| `uf` | string | Código IBGE do estado (UF) onde os documentos foram emitidos (ex.: `"35"` para São Paulo). |
| `mod` | string | Modelo do documento fiscal (ex.: `"55"` para NF-e, `"65"` para NFC-e). |

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/nfce/discard' \
--header 'Content-Type: application/json' \
--header 'Accept: application/json' \
--header 'Accept-Encoding: application/gzip' \
--data '{
  "ano": "26",
  "cnpj": "29720473000145",
  "justificativa": "inutilizacao para testes",
  "numeroInicial": 28,
  "numeroFinal": 28,
  "serie": 506,
  "tpAmb": 2,
  "uf": "35",
  "mod": "55"
}'
```

> **Observações:**
> - Esta operação é **irreversível**. Uma vez que os números são inutilizados na SEFAZ, eles não podem ser reutilizados.
> - Utilize `tpAmb: 2` para testes no ambiente de homologação antes de usar em produção (`tpAmb: 1`).
> - O campo `justificativa` deve conter no mínimo 15 caracteres para ser aceito pela SEFAZ.

---
### 🔵 [GET] ConsultarProcessamentoContigencia

**Método:** `GET` | **Endpoint:** `{{base}}/api/v1/nfce/processamentocontigencia`

Consulta o status do processamento das NFC-e emitidas em **modo de contingência** (off-line). Utilize este endpoint para verificar se as notas fiscais emitidas durante uma indisponibilidade da SEFAZ foram transmitidas e autorizadas com sucesso após o retorno da conectividade.  
**Ou desconsidere caso queira utilizar o Webhook para essa função.**

### Headers

| Header | Valor |
| --- | --- |
| `Accept` | `*/*` |

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/nfce/processamentocontigencia' \
--header 'Accept: */*' \
--header 'Connection: keep-alive' \
--header 'Accept-Encoding: application/gzip, deflate, br'
```

---
### 🔵 [GET] ConsultaStatusUUID

**Método:** `GET` | **Endpoint:** `{{base}}/api/v1/nfce/authorization/status/{{uuid}}`

Consulta o status da emissão de uma NFC-e pelo seu UUID. O sistema armazena as últimas 100 emissões. Retorna os dados completos da transação pelo UUID (emissão, rejeição, contingência, etc.).

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/nfce/authorization/status/SEU_UUID_AQUI'
```

---
## Categoria: 2. Webhook (OPCIONAL)

O SmartDFe dispara notificações para a URL configurada nos eventos de emissão.

### 🟢 [POST] Configurar webhook SmartDFe (OPCIONAL)

**Método:** `POST` | **Endpoint:** `{{base}}/api/v1/webhook/config`

Configura o webhook para receber notificações de eventos fiscais.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body

```json
{
  "url": "http://127.0.0.1:5050/webhook",
  "ativo": true,
  "retryMax": 3,
  "timeoutMs": 5000
}
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/webhook/config' \
--header 'Content-Type: application/json' \
--data '{
  "url": "http://127.0.0.1:5050/webhook",
  "ativo": true,
  "retryMax": 3,
  "timeoutMs": 5000
}'
```

---
### 🔵 [GET] Consultar configuração Webhook (OPCIONAL)

**Método:** `GET` | **Endpoint:** `{{base}}/api/v1/webhook/config`

Retorna a configuração atual do webhook (URL, status, retry, timeout).

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/webhook/config'
```

---
## Categoria: 3. Servidor SMTP (OPCIONAL)

Configuração e consulta do servidor SMTP para envio de e-mails com os documentos fiscais emitidos.

### 🟢 [POST] Configurar email

**Método:** `POST` | **Endpoint:** `{{base}}/api/v1/email/config`

Configura o servidor SMTP para envio de e-mails com os documentos fiscais emitidos.

### Headers

| Header | Valor |
| --- | --- |
| `Content-Type` | `application/json` |

### Exemplo de Body

```json
{
  "smtp_host": "smtp.gmail.com",
  "smtp_porta": 465,
  "smtp_usuario": "seuemail@gmail.com",
  "smtp_senha": "sua_app_password",
  "smtp_tls": false,
  "remetente_email": "seuemail@gmail.com",
  "remetente_nome": "Empresa Fiscal",
  "assunto": "Sua NFC-e nº {nNF}",
  "ativo": true
}
```

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/email/config' \
--header 'Content-Type: application/json' \
--data '{
  "smtp_host": "smtp.gmail.com",
  "smtp_porta": 465,
  "smtp_usuario": "seuemail@gmail.com",
  "smtp_senha": "sua_app_password",
  "smtp_tls": false,
  "remetente_email": "seuemail@gmail.com",
  "remetente_nome": "Empresa Fiscal",
  "assunto": "Sua NFC-e nº {nNF}",
  "ativo": true
}'
```

---
### 🔵 [GET] Consultar configuração de email

**Método:** `GET` | **Endpoint:** `{{base}}/api/v1/email/config`

Retorna os dados da configuração SMTP cadastrada (host, porta, usuário, remetente, status).

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/email/config'
```

---
## Categoria: 4. DANFE

### 🟢 [POST] Pré-visualização da DANFE modelo 55 (NF-e)

**Método:** `POST` | **Endpoint:** `{{base}}/api/v1/nfe/danfe_preview`

Gera uma prévia da DANFE de uma NF-e antes de sua emissão definitiva, permitindo validar as informações do documento e evitar cancelamentos decorrentes de erros.
📌 Disponível exclusivamente para o modelo 55 (NF-e).

### Body

| Campo | Tipo | Descrição |
| --- | --- | --- |
| Arquivo xml | file | Arquivo `.xml` da NF-e enviado diretamente do sistema de arquivos |

### Exemplo cURL

```bash
curl --location 'http://127.0.0.1:8781/api/v1/nfe/danfe_preview' \
--form 'Arquivo xml=@"/caminho/para/seu/arquivo.xml"'
```

---

## Tratamento de Retorno
Para entender em detalhes como lidar com os códigos de aprovação, rejeição ou contingência (`cStat`), consulte o nosso guia completo de tratamento de retorno:
🔗 [Guia de Tratamento de Retorno](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Tratamento-de-Retorno)

---

## Finalização e Homologação
Após concluir a integração de todas as etapas críticas e realizar os testes no ambiente de homologação, é necessário preencher o formulário de homologação.
Para enviar suas evidências e validar sua integração junto à Elgin, acesse o link abaixo:
🔗 [Acessar Formulário de Homologação](https://forms.cloud.microsoft/r/zAVEtYCceC)

---

## Suporte
Em caso de dúvidas durante o processo de integração ou homologação, entre em contato com a equipe de especialistas Elgin:
📱 +55 11 99851-9559 — opção 2