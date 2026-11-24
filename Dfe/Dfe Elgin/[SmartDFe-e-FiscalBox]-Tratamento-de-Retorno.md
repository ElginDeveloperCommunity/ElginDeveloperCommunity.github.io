A **SmartDFe API Nativa** recebe a requisição em formato **XML** e devolve o processamento fiscal em **JSON**.

Este tratamento é válido para:
- **NFC-e (modelo 65)**
- **NF-e (modelo 55)**

Operações contempladas:
- Emitir, Cancelar, Inutilizar, Consultar na SEFAZ, CC-e, Cancelamento por Substituição.

> **Observação:** O tratamento dos retornos baseia-se fortemente nas tags oficiais da SEFAZ, devolvidas no JSON de resposta.

---

## 1. Regra geral de tratamento dos retornos

Os retornos fiscais da api nativa xml possuem os dados entregues na raiz do objeto de resposta. 

| Campo | Descrição |
| :--- | :--- |
| `cStat` | Código oficial da SEFAZ ou do processamento local. Deve ser utilizado para definir o fluxo da aplicação. |
| `xMotivo` | Mensagem descritiva do retorno SEFAZ. Deve ser utilizada para exibição ao usuário e registro de logs. |
| `data` | Contém o conteúdo do XML autorizado ou informações do erro codificados em base64. |

> **❗ IMPORTANTE:**
> O código HTTP (200 OK) indica apenas que a API conseguiu processar a requisição e se comunicou com sucesso. A autorização ou rejeição fiscal deve sempre ser validada através do campo **`cStat`**.

**Exemplo de Retorno (Emissão):**
```json
{
  "cStat": "100",
  "xMotivo": "Autorizado o uso da NF-e",
  "data": "<XML autorizado ou erro em base64>",
  "chNFe": "35260729720473000145650270000013241180759860",
  "nNF": "1324",
  "nProto": "13526000008999199"
}
```

---

## 2. Principais códigos fiscais (cStat)

### 2.1 Autorização
| `cStat` | Descrição | Tratamento |
| :--- | :--- | :--- |
| **100** | Autorizado o uso da NF-e | Documento autorizado com sucesso. |
| **150** | Autorizado o uso da NF-e fora do prazo | Exclusivo para NF-e modelo 55. Documento autorizado pela SEFAZ. |

### 2.2 Contingência
| `cStat` | Descrição | Tratamento |
| :--- | :--- | :--- |
| **-100** | Documento emitido em contingência | Aguardar processamento posterior (Webhooks ou Consulta) e atualização do status. |

### 2.3 Eventos fiscais (Cancelamento, CC-e)
Para eventos, o `cStat` geral indica o recebimento do lote ou evento registrado:
| `cStat` | Descrição | Tratamento |
| :--- | :--- | :--- |
| **135** | Evento registrado e vinculado à NF-e | Evento processado com sucesso. (Cancelamento, Carta de Correção, etc.) |
| **136** | Evento registrado, mas não vinculado | Evento recebido pela SEFAZ, mas não associado à nota (verificar). |
| **155** | Cancelamento homologado fora de prazo | Sucesso, mas realizado fora do prazo permitido pela UF. |

### 2.3 Demais códigos
Qualquer código diferente de autorizações (100, 150) ou sucessos de evento (135, 155, 102) deve ser tratado como:
- Rejeição
- Erro de processamento
- Falha na SEFAZ

> **💡 DICA:**
> Sempre utilize `cStat` para a decisão do fluxo e `xMotivo` para identificar e exibir o motivo ao usuário.

---

## 3. Emissão de NFC-e / NF-e
**Endpoint:** `POST` `{{base}}/api/v1/nfce/xml/{{$guid}}`

No retorno de emissão, as propriedades estão no primeiro nível do JSON:

```json
{
  "Agent version": "01.03.38",
  "Error": "100",
  "cStat": "100",
  "xMotivo": "Autorizado o uso da NF-e",
  "chNFe": "35260729720473000145650270000013241180759860",
  "nNF": "1324",
  "nProto": "13526000008999199",
  "data": "<XML autorizado em base64>",
  "danfe_base64": "<DANFE em base64>"
}
```

### Tratamento do Retorno
O integrador deve validar o `cStat` (Ex: `"100"`). A aplicação deve capturar e salvar:
- **`chNFe`:** Chave de acesso do documento fiscal autorizado.
- **`nNF`:** Número da nota.
- **`nProto`:** Protocolo de autorização.
- **`data`:** Opcionalmente, pode decodificar e salvar o XML em base64.
- **`danfe_base64`:** Opcionalmente, imprimir a partir do PDF em base64.

---

## 4. Cancelamento
**Endpoint:** `POST` `{{base}}/api/v1/nfce/{{Chave NFCE}}/cancel`

Para cancelamento, o retorno da API embute os dados da resposta do evento (`infEvento`) em formato de **string JSON serializada**. Você precisará fazer o *parse* (deserialização) de `infEvento` para acessar o `cStat` da resposta.

```json
{
    "Agent version": "01.03.38",
    "chNFe": "35260729720473000145650270000013251451581997",
    "infEvento": "{\"cOrgao\":\"35\",\"cStat\":\"135\",\"chNFe\":\"35260729720473000145650270000013251451581997\",\"nProt\":\"13526000009004569\",\"xEvento\":\"Cancelamento registrado\",\"xMotivo\":\"Evento registrado e vinculado a NF-e\"}",
    "data": "<XML do evento de cancelamento em base64>",
    "ok": true
}
```

### Tratamento do Retorno
1. Validar a propriedade **`ok: true`**.
2. Deserializar o conteúdo de **`infEvento`** (string para objeto).
3. Dentro de `infEvento`, avaliar **`cStat`** e **`xMotivo`**. Se `cStat` for **135** ou **155**, o evento foi cancelado com sucesso.
4. O campo **`data`** contém o arquivo XML do evento autorizado codificado em base64.

---

## 5. Inutilização e Outros Eventos
O padrão de eventos, de forma geral, segue as validações clássicas de cStat retornados da SEFAZ, onde o código principal indicará o sucesso da operação, baseando-se sempre no manual de orientação ao contribuinte da SEFAZ.

---

## 6. Outros pontos relevantes

### 6.1. Validação do Código HTTP (Status Code)
- `200 OK`: A requisição foi processada com sucesso pela API. Em seguida, valide as regras de negócio pelo `cStat` (ou pelo `cStat` interno de `infEvento`).
- Erros de HTTP 4xx e 5xx indicam falha na formatação da requisição (JSON/XML inválido, erro interno, etc.).