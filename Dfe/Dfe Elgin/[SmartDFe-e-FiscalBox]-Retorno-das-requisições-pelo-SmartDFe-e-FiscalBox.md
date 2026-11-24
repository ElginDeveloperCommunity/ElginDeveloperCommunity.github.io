Para realizar o tratamento correto dos retornos do emissor fiscal, o software parceiro deve utilizar como referência principal os campos **cStat** e **xMotivo**, presentes no JSON de resposta retornado pelo SmartDFe/FiscalBox.

### Campos principais

#### cStat

Campo oficial utilizado pela SEFAZ para indicar o resultado da solicitação fiscal.  
Este campo informa se a NF-e/NFC-e foi:

- Autorizada
- Rejeitada
- Denegada
- Processada com alertas

**Exemplo:**
```json
"cStat": "100"
```

#### xMotivo

Campo oficial utilizado pela SEFAZ para descrever o motivo do retorno da requisição.  
Pode indicar:

- Aprovação do documento
- Motivo de rejeição
- Erros fiscais
- Mensagens de processamento

**Exemplo:**
```json
"xMotivo": "Autorizado o uso da NF-e"
```

---

Ao enviar um XML de NF-e (modelo 55) ou NFC-e (modelo 65) utilizando o SmartDFe, o sistema retornará um conjunto de informações contendo dados de processamento interno, validações da SEFAZ e informações do documento fiscal.

O campo **cStat** reflete o resultado da requisição e deve ser usado como parâmetro de retorno no tratamento de erro.

---

## Campos retornados e suas especificações

| Campo | Descrição | Especificação |
|-------|-----------|---------------|
| Agent version | Versão do agente emissor SmartDFe ou FiscalBox utilizada na operação. Exemplo: `"Agent version": "01.02.27"` | Retorno para todos os tipos de integração |
| body | Todos os campos descritos abaixo são retornados dentro do objeto principal body, que concentra as informações operacionais e fiscais da requisição processada. | Retorno para integração via dll |
| Error | Código de status retornado pelo emissor ou pela SEFAZ indicando sucesso ou erro no processamento. | Retorno para todos os tipos de integração |
| IpPDV | Endereço IP do ponto de venda (PDV) que originou a requisição. Exemplo: `"IpPDV": "::ffff:192.168.15.9"` | Retorno para todos os tipos de integração |
| SignatureValue | Assinatura digital da NF-e/NFC-e gerada a partir do certificado digital, garantindo autenticidade e integridade do documento. | Retorno para todos os tipos de integração |
| Tempo comando (ms) | Tempo total, em milissegundos, que o sistema interno levou para executar o comando solicitado. | Retorno para todos os tipos de integração |
| Tempo médio SEFAZ (ms) | Tempo médio de resposta da SEFAZ durante o envio ou consulta do documento fiscal. | Retorno para todos os tipos de integração |
| tpAmb | Tipo de ambiente: 1 = Produção, 2 = Homologação. | Retorno para todos os tipos de integração |
| cStat | Código de status retornado pela SEFAZ ou pelo emissor. | Retorno para todos os tipos de integração |
| cNF | Código numérico que compõe a Chave de Acesso da NF-e/NFC-e. Exemplo: `"cNF": "18445253"` | Retorno para todos os tipos de integração |
| xMotivo | Descrição do motivo do retorno da SEFAZ ou do emissor. Exemplo: `"xMotivo": "Autorizado o uso da NF-e"` | Retorno para todos os tipos de integração |
| certificareExpiryCountdown | Quantidade de dias restantes para a expiração do certificado digital. | Retorno para todos os tipos de integração |
| certificateExpirationDate | Data de expiração do certificado digital utilizado na assinatura. | Retorno para todos os tipos de integração |
| chNFe | Chave de acesso da NF-e/NFC-e (44 dígitos), identificador único do documento fiscal. | Retorno para todos os tipos de integração |
| command | Comando executado no processo (ex.: emitir, consultar, cancelar). | Retorno para todos os tipos de integração |
| data | Dados adicionais enviados ou retornados durante o processamento da operação. | Retorno para todos os tipos de integração |
| dhRecibo | Data e hora do recibo retornado pela SEFAZ quando o documento é autorizado. | Retorno para todos os tipos de integração |
| digVal | Hash do XML assinado (Digital Value), utilizado pela SEFAZ para validação da integridade. | Retorno para todos os tipos de integração |
| licenseExpiryCountdown | Quantidade de dias restantes para a expiração da licença do software emissor. | Retorno para todos os tipos de integração |
| modeloDFe | Modelo do documento fiscal: 65 = NFC-e, 55 = NF-e. | Retorno para todos os tipos de integração |
| nNF | Número da nota fiscal. | Retorno para todos os tipos de integração |
| nProto | Número do protocolo de autorização retornado pela SEFAZ. | Retorno para todos os tipos de integração |
| danfe_base64| Conteúdo do DANFE em formato PDF codificado em Base64, quando configurado para gerar. | Retorno para integração API direta e dll |
| pdfBase64 | Conteúdo do DANFE em formato PDF codificado em Base64, quando gerado no processo. | Retorno para integração E1 APINOTAS |
| pdfStatus | Status da geração do PDF (ex.: Success, Error, Pending). | Retorno para integração E1 APINOTAS |
| pendingContingencyInvoices | Quantidade de notas emitidas em contingência que ainda aguardam envio à SEFAZ. | Retorno para todos os tipos de integração |
| serie | Série da nota fiscal. | Retorno para todos os tipos de integração |
| totalNF | Valor total da nota fiscal. | Retorno para todos os tipos de integração |
| tpEmis | Tipo de emissão: 1 = Normal, 9 = Contingência Offline, entre outros. | Retorno para todos os tipos de integração |
| uuid | Identificador único interno da operação no sistema emissor. | Retorno para todos os tipos de integração |

---

## Campos de eventos e retorno SEFAZ

| Campo | Descrição | Tipo de Integração |
|-------|-----------|-------------------|
| cOrgao | Código do órgão autorizador da SEFAZ responsável pelo processamento. | Retorno SEFAZ |
| dhRegEvento | Data e hora do registro do evento de cancelamento. | Retorno para todos os tipos de integração. Funções que geram eventos. |
| nProt | Número do protocolo do evento de cancelamento retornado pela SEFAZ. | Retorno para todos os tipos de integração. Funções que geram eventos. |
| nSeqEvento | Número sequencial do evento registrado. | Retorno para todos os tipos de integração. Funções que geram eventos. |
| tpEvento | Código do tipo de evento fiscal (110111 = Cancelamento). | Retorno para todos os tipos de integração. Funções que geram eventos. |
| verAplic | Versão da aplicação SEFAZ responsável pelo processamento. | Retorno para todos os tipos de integração. Funções que geram eventos. |
| xEvento | Nome do evento registrado (ex.: Cancelamento registrado). | Retorno para todos os tipos de integração. Funções que geram eventos. |
| status | Código de status HTTP retornado pela API indicando o resultado da requisição. | Retorno para integração E1 APINOTAS |
| message | Mensagem descritiva do processamento da operação (ex.: Nota emitida com sucesso). | Retorno para integração E1 APINOTAS |
| pdf_response | Objeto que concentra as informações relacionadas à geração do DANFE em PDF. | Retorno para integração E1 APINOTAS |
| base64 | Conteúdo do DANFE em formato PDF codificado em Base64, quando disponível. | Retorno para integração E1 APINOTAS |
| status (pdf_response) | Status da geração do PDF (ex.: gerado_com_sucesso, erro, pendente). | Retorno para integração E1 APINOTAS |

---

Essas informações permitem ao integrador acompanhar o status da nota, validar autorizações, capturar o XML autorizado para impressão, monitorar tempos de resposta e tratar cenários de erro ou contingência.
