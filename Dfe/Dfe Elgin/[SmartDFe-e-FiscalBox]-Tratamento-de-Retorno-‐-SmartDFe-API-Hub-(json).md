# Tratamento de Retorno - SmartDFe API Hub (json)

A **SmartDFe API Hub** utiliza um padrão único de retorno em formato **JSON** para as operações fiscais.

Este tratamento é válido para:
- **NFC-e (modelo 65)**
- **NF-e (modelo 55)**

Operações contempladas:
- Emitir, Consultar na SEFAZ, Download XML, DANFE, Cancelar, Inutilizar e Eventos fiscais.

> **Observação:** O tratamento dos retornos é o mesmo para NFC-e e NF-e.

---

## 1. Regra geral de tratamento dos retornos

Todos os retornos fiscais da SmartDFe possuem uma estrutura baseada nos seguintes campos:

| Campo | Descrição |
| :--- | :--- |
| `sefaz.codigo` | Código retornado pela SEFAZ ou pelo processamento fiscal. Deve ser utilizado para definir o fluxo da aplicação. |
| `sefaz.mensagem` | Mensagem descritiva do retorno. Deve ser utilizada para exibição ao usuário e registro de logs. |
| `status` | Representa a situação atual do documento fiscal. |

> **❗ IMPORTANTE:**
> O código HTTP da resposta indica apenas se a API SmartDFe conseguiu processar a requisição. A autorização ou rejeição fiscal deve sempre ser validada através do campo **`sefaz.codigo`**.

**Exemplo de Retorno:**
```json
{
    "sefaz": {
        "codigo": "100",
        "mensagem": "Autorizado o uso da NF-e"
    },
    "status": "autorizado"
}
```
**Interpretação:**
- **HTTP 200** → Requisição processada pela API.
- **sefaz.codigo = 100** → Documento autorizado pela SEFAZ.
- **status = autorizado** → Situação atual do documento.

---

## 2. Principais códigos fiscais

### 2.1 Autorização
| `sefaz.codigo` | Descrição | Tratamento |
| :--- | :--- | :--- |
| **100** | Autorizado o uso da NF-e | Documento autorizado com sucesso. |
| **150** | Autorizado o uso da NF-e fora do prazo | Exclusivo para NF-e modelo 55. Documento autorizado pela SEFAZ. |

### 2.2 Contingência
| `sefaz.codigo` | Descrição | Tratamento |
| :--- | :--- | :--- |
| **-100** | Documento emitido em contingência | Aguardar processamento posterior e atualização do status. |

> **⚠️ ATENÇÃO:**
> Quando o retorno possuir `sefaz.codigo = -100`, a autorização definitiva ocorrerá posteriormente. Recomenda-se utilizar o Webhook SmartDFe para receber automaticamente a autorização e o novo protocolo da SEFAZ.

Consulte a documentação de [Webhooks](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Retorno-Webhook-Fiscal) para maiores detalhes.

### 2.3 Eventos fiscais
| `sefaz.codigo` | Descrição | Tratamento |
| :--- | :--- | :--- |
| **135** | Evento registrado e vinculado à NF-e | Evento processado com sucesso. (Aplicável para Cancelamento, Carta de Correção e Outros eventos) |

### 2.4 Inutilização
| `sefaz.codigo` | Descrição | Tratamento |
| :--- | :--- | :--- |
| **102** | Inutilização de número homologada | Faixa de numeração inutilizada com sucesso. |

### 2.5 Demais códigos
Qualquer código diferente dos apresentados deve ser tratado como:
- Rejeição
- Erro de processamento
- Operação não concluída

> **💡 DICA:**
> Sempre utilize `sefaz.codigo` para a decisão do fluxo e `sefaz.mensagem` para identificar e exibir o motivo.

---

## 3. Emissão de NFC-e / NF-e
**Endpoint:** `POST` `{{base}}/nfce` (ou `/nfe`)

```json
{
    "certificado": { "dias_para_vencer": 162, "validade": "2027-01-15 12:39:25" },
    "chave": "35260829720473000145650270000013821564064564",
    "contingencia": { "pendentes": 0 },
    "id": "NFE-1786018477",
    "licenca": { "validade": "Perpetual" },
    "links": {
        "pdf": "/api/v1/nfce/35260829720473000145650270000013821564064564/pdf",
        "xml": "/api/v1/nfce/35260829720473000145650270000013821564064564/xml"
    },
    "numero": "1382",
    "protocolo": "13526000009091525",
    "ref": "NFE-1786018477",
    "sefaz": {
        "codigo": "100",
        "mensagem": "Autorizado o uso da NF-e"
    },
    "status": "autorizado",
    "tempo": { "Tempo comando(ms)": "740", "Tempo médio SEFAZ(ms)": "328" },
    "tipo": "nfce"
}
```

### Tratamento do Retorno
O integrador deve sempre validar o `sefaz.codigo`:

| Código | Resultado |
| :--- | :--- |
| **100** | Documento autorizado |
| **150** | NF-e autorizada fora do prazo |
| **-100** | Emitido em contingência |
| **Outros** | Rejeição ou erro |

Além disso, a aplicação deve capturar e salvar os seguintes dados de retorno:
- **`chave`:** Identificação do documento fiscal
- **`numero`:** Número da nota (gerado pelo SmartDFe)
- **`protocolo`:** Protocolo de autorização da SEFAZ
- **`sefaz.mensagem`:** Mensagem detalhada do retorno

> **💡 DICA:**
> **Captura de Chave e Número:** Ao emitir um documento, o SmartDFe gera a chave e realiza o controle numérico automaticamente. Portanto, é essencial que a aplicação sempre capture os campos `chave` e `numero` presentes na resposta, além do `protocolo` de autorização.

> **⚠️ AVISO:**
> **Autorização em Contingência (Webhook):** Quando o documento for emitido em contingência (`-100`), a autorização definitiva ocorrerá posteriormente. Você deve capturar essa autorização e o novo protocolo assim que ocorrerem. Recomenda-se utilizar o **Webhook** para capturar essas informações de forma automática. Consulte a documentação de Webhook para maiores detalhes.

---

## 4. Cancelamento
**Endpoint:** `POST` `{{base}}/nfce/{{chave}}/cancelamento` (ou `DELETE` `{{base}}/nfce/{{chave}}`)

```json
{
    "chave": "35260829720473000145650270000013831412547292",
    "protocolo": "13526000009091584",
    "sefaz": { "codigo": "135", "mensagem": "Evento registrado e vinculado a NF-e" },
    "status": "cancelado",
    "tipo": "nfce"
}
```

### Tratamento do Retorno
Validar `sefaz.codigo` e utilizar `status` para identificar a situação atual (Ex: `"status": "cancelado"`).

| Código | Resultado |
| :--- | :--- |
| **135** | Evento registrado e vinculado à NF-e |
| **Outros** | Rejeição ou erro |

---

## 5. Inutilização
**Endpoint:** `POST` `{{base}}/nfce/inutilizacao`

```json
{
    "faixa": { "final": 16, "inicial": 16, "serie": 78 },
    "protocolo": "",
    "sefaz": { "codigo": "102", "mensagem": "Inutilização de número homologado" },
    "status": "inutilizado",
    "tipo": "nfce"
}
```

### Tratamento do Retorno
Validar `sefaz.codigo` e utilizar `status` para identificar a situação (Ex: `"status": "inutilizado"`).

| Código | Resultado |
| :--- | :--- |
| **102** | Inutilização homologada |
| **Outros** | Rejeição ou erro |

---

## 6. Consulta na SEFAZ
**Endpoint:** `GET` `{{base}}/nfce/{{chave}}/situacao`

A consulta retorna a situação atual do documento fiscal.

### Tratamento do Retorno
Validar `sefaz.codigo` e utilizar o campo `situacao` para o estado atual (Ex: `"situacao": "cancelado"`).

| Código | Resultado |
| :--- | :--- |
| **100** | Documento autorizado |
| **101** | Cancelamento homologado |
| **Outros** | Outras situações |

---

## 7. Download de arquivos
- **XML Autorizado:** `GET` `{{base}}/nfce/{{chave}}/xml` - Retorna o XML autorizado pela SEFAZ.
- **DANFE PDF:** `GET` `{{base}}/nfce/{{chave}}/pdf` - Retorna o DANFE em PDF.
- **ESC/POS:** `GET` `{{base}}/nfce/{{chave}}/escpos` - Retorna o código ESC/POS para impressão térmica.

---

## 8. Outros pontos relevantes

Ao consumir a API, considere as seguintes diretrizes:

### 8.1. Validação do Código HTTP (Status Code)

Antes de validar o conteúdo do payload (`sefaz.codigo`, `status`, etc.), verifique o **HTTP Status Code** retornado pela API.

* **`200 OK` / `201 Created` / `202 Accepted`** → A requisição foi processada com sucesso pela API. Em seguida, valide o resultado da operação por meio das regras de negócio, principalmente pelo `sefaz.codigo`.
* **`400 Bad Request`** → Erro na estrutura ou nos dados enviados na requisição, como JSON inválido, campos obrigatórios ausentes ou parâmetros incorretos.
* **`401 Unauthorized` / `403 Forbidden`** → Problemas de autenticação, autorização ou licença.
* **`422 Unprocessable Entity`** → A requisição possui uma estrutura válida, porém foi rejeitada durante a validação das regras de negócio, podendo indicar uma **rejeição da SEFAZ**.
* **`500 Internal Server Error`** → Erro inesperado ou interno na API do SmartDFe.

### 8.2. Validação do Retorno da SEFAZ

Após confirmar que o HTTP Status Code indica processamento da requisição, valide os campos de retorno da SEFAZ:

* **`sefaz.codigo = 100`** → Documento autorizado pela SEFAZ.
* **`status = autorizado`** → Documento encontra-se autorizado no SmartDFe.

> **Importante:** O HTTP `200` indica que a requisição foi processada com sucesso pela API. Ele **não significa, necessariamente, que o documento fiscal foi autorizado pela SEFAZ**. A autorização deve ser confirmada pelo retorno `sefaz.codigo = 100` e pelo `status = autorizado`.


### 8.3. Extração e Uso do Objeto `links`
O retorno da emissão já fornece o nó `links`:
```json
"links": {
    "pdf": "/api/v1/nfce/35260829720473000145650270000013821564064564/pdf",
    "xml": "/api/v1/nfce/35260829720473000145650270000013821564064564/xml"
}
```
Você pode configurar testes na collection para extrair essas URLs parciais e realizar o download automático.