# Webhook Fiscal — SmartDFe e FiscalBox

## Visão Geral

O Webhook fiscal é configurado durante a **instalação e ativação** do **SmartDFe** ou **FiscalBox**. Uma vez ativo, ele notifica o PDV em tempo real sobre os eventos relacionados à emissão de documentos fiscais, eliminando a necessidade de consultas periódicas ao motor fiscal.

Cada notificação é enviada via requisição HTTP POST para a URL configurada, sempre com o mesmo envelope JSON, variando apenas o campo `evento` e o conteúdo de `dados`.

> **Uso recomendado:** utilize o **retorno da API** como fluxo principal de tratamento no PDV. Utilize o **webhook** para tratar contingência, inutilização e demais atualizações assíncronas.
>
> **Exemplo:** ao emitir uma nota em contingência, o PDV recebe `cStat -100` no retorno da API. Posteriormente, via webhook, recebe `nota_autorizada` ou `nota_rejeitada` quando o SmartDFe sincronizar a nota. Capture e grave o número da nota (`nNF`) e a chave (`chNFe`) e atualize o registro no PDV quando o webhook chegar.

---

## Envelope JSON

Todos os eventos seguem a mesma estrutura de envelope:

```json
{
  "agente":    "<identificador do motor fiscal>",
  "evento":    "<nome do evento>",
  "timestamp": "<data e hora ISO 8601>",
  "versao":    "1.0",
  "dados":     { }
}
```

| Campo       | Tipo            | Descrição                                              |
|-------------|-----------------|--------------------------------------------------------|
| `agente`    | string          | Identificador do motor. Ex.: `SmartNFCe`               |
| `evento`    | string          | Nome do evento. Ver tabela abaixo                      |
| `timestamp` | string ISO 8601 | Momento da emissão do webhook. Ex.: `2026-09-11T14:26:15` |
| `versao`    | string          | Versão do contrato. Atual: `1.0`                       |
| `dados`     | object          | Conteúdo variável por evento                           |

O conteúdo do objeto `dados` varia conforme o evento, conforme detalhado nas seções abaixo.

---

## Eventos Disparados

| Evento                      | Disparado quando                                                          |
|-----------------------------|---------------------------------------------------------------------------|
| `nota_autorizada`           | Retorno `cStat` 100 ou 150 – autorização (online ou pós-contingência)     |
| `nota_emitida_contingencia` | Retorno `cStat` -100 – nota gravada em contingência offline               |
| `nota_rejeitada`            | Retorno `cStat` diferente de 100, 150, -100 e 102 – rejeição SEFAZ            |
| `prevalidacao`              | Validação local antes da SEFAZ – avisa inconsistências (ex.: NCM inexistente) |
| `inutilizacao_realizada`    | Inutilização de número homologada (`cStat` 102)                 |
| `nota_cancelada`            | Cancelamento simples ou por substituição processado com sucesso           |
| `contingencia_ativada`      | Motor entra em modo contingência (automático ou manual)                   |
| `contingencia_desativada`   | Motor sai do modo contingência (automático ou manual)                     |
| `contingencia_sincronizada` | Ciclo de reenvio de contingência concluído                                |

---

## Payloads por Evento

### `nota_autorizada`

Emitido quando a SEFAZ retorna `cStat 100` (autorização normal ou pós-contingência) ou `cStat 150` (autorizado fora de prazo, para NF-e).

**Exemplo 1 – autorização online (`nNF 58`, sem `origem`):**

```json
{
  "agente": "SmartNFCe",
  "dados": {
    "CNPJ": "29604796000173",
    "cStat": 100,
    "chNFe": "13260929604796000173650270000000581580830432",
    "dhRecbto": "18/09/2026 09:46:34",
    "modelo": "65",
    "nNF": 58,
    "nProt": "113260013549184",
    "uuid": "394c76dd-f573-4c61-8e41-72bb9b900573"
  },
  "evento": "nota_autorizada",
  "timestamp": "2026-09-18T10:46:34",
  "versao": "1.0"
}
```

**Exemplo 2 – autorização pós-contingência (`nNF 60`, com `origem: contingencia`):**

```json
{
  "agente": "SmartNFCe",
  "dados": {
    "CNPJ": "29604796000173",
    "cStat": 100,
    "chNFe": "13260929604796000173650270000000609532099891",
    "dhRecbto": "18/09/2026 09:51:22",
    "modelo": "NFC-e",
    "nNF": 60,
    "nProt": "113260013549482",
    "origem": "contingencia",
    "uuid": "41d1b5a8-04a3-48d8-a124-ab92f70e3406"
  },
  "evento": "nota_autorizada",
  "timestamp": "2026-09-18T10:51:22",
  "versao": "1.0"
}
```

| Campo      | Descrição                                                                         |
|------------|-----------------------------------------------------------------------------------|
| `CNPJ`     | CNPJ do emitente                                                                  |
| `cStat`    | Código de status da SEFAZ (100 ou 150)                                        |
| `chNFe`    | Chave de acesso da NF-e / NFC-e (44 dígitos)                                      |
| `dhRecbto` | Data e hora de recebimento pela SEFAZ – formato `DD/MM/AAAA HH:mm:ss`            |
| `modelo`   | Modelo do documento: `NFC-e` / `NF-e` ou `65` / `55`                 |
| `nNF`      | Número da nota fiscal                                                             |
| `nProt`    | Número do protocolo de autorização                                                |
| `uuid`     | Identificador único local da nota – usar para vincular com `nota_emitida_contingencia` |
| `origem`   | `contingencia` quando veio de sincronização offline. Ausente = autorização online |

> **Fluxo contingência:** a mesma nota primeiro gera `nota_emitida_contingencia` (`cStat -100`) e, após sincronizar, gera `nota_autorizada` com `origem: contingencia`. Use `chNFe` + `uuid` para vincular os dois eventos. Ex.: `nNF 60` (`chNFe ...609532099891`, `uuid 41d1b5a8...`) – note que `modelo` veio como `"65"` na contingência e `"NFC-e"` na autorização.

---

### `nota_emitida_contingencia`

Emitido quando a nota é gravada em contingência offline (`cStat -100`), antes da transmissão à SEFAZ.

```json
{
  "agente": "SmartNFCe",
  "dados": {
    "CNPJ": "29604796000173",
    "cStat": -100,
    "chNFe": "13260929604796000173650270000000609532099891",
    "modelo": "65",
    "nNF": 60,
    "uuid": "41d1b5a8-04a3-48d8-a124-ab92f70e3406"
  },
  "evento": "nota_emitida_contingencia",
  "timestamp": "2026-09-18T10:51:01",
  "versao": "1.0"
}
```

| Campo    | Descrição                              |
|----------|----------------------------------------|
| `CNPJ`   | CNPJ do emitente                       |
| `cStat`  | `-100` – pendente de transmissão       |
| `chNFe`  | Chave de acesso gerada offline         |
| `modelo` | `NFC-e` / `65`         |
| `nNF`    | Número da nota                         |
| `uuid`   | Identificador único local da nota      |

> **Atenção:** uma nota com `cStat -100` ainda **não foi autorizada** pela SEFAZ. Libere a venda ao cliente, mas aguarde `nota_autorizada` (com `origem: contingencia`) ou `nota_rejeitada` após a sincronização.

> **Atenção ao campo `modelo`:** pode vir como `"65"` ou `"NFC-e"` (ex.: ambos os exemplos de 18/09/2026 usam `"65"`). O PDV deve normalizar (`65` == `NFC-e`).

---

### `nota_rejeitada`

Emitido quando a SEFAZ retorna um código de status diferente de 100, 150, -100 ou 102.

```json
{
  "agente": "SmartNFCe",
  "dados": {
    "cStat": 778,
    "nNF": 219,
    "origem": "contingencia",
    "xMotivo": "Rejeição: Informado NCM inexistente [nItem: 1]"
  },
  "evento": "nota_rejeitada",
  "timestamp": "2026-09-11T14:26:00",
  "versao": "1.0"
}
```

| Campo     | Descrição                                                                        |
|-----------|----------------------------------------------------------------------------------|
| `cStat`   | Código de rejeição SEFAZ. Ex.: `778` (NCM inexistente)                           |
| `nNF`     | Número da nota rejeitada                                                         |
| `origem`  | `contingencia` se a rejeição ocorreu na sincronização; ausente se online |
| `xMotivo` | Descrição textual do motivo. Inclui `[nItem: X]` quando o erro é por item        |

**Ação recomendada no PDV:** exibir `xMotivo` ao operador, permitir correção e reemissão com novo `nNF` ou mesma numeração conforme regra fiscal.

---

### `prevalidacao`

Emitido pela validação local do motor **antes** do envio à SEFAZ, quando algum campo não passa nas regras fiscais (ex.: NCM inexistente). Não é rejeição SEFAZ – é um aviso prévio para corrigir antes de transmitir.

```json
{
  "agente": "SmartNFCe",
  "dados": {
    "CNPJ": "29604796000173",
    "chNFe": "13260929604796000173650270000000619744158805",
    "nNF": "61",
    "serie": "27",
    "versao_base": "2026.07-multi-r2",
    "violacoes": [
      {
        "cStat": 778,
        "detalhe": "item 1: NCM=00063021 nao consta na tabela oficial",
        "id": "NCM-INEXISTENTE",
        "severidade": "avisar",
        "xMotivo": "Rejeicao: NCM informado nao consta na tabela"
      }
    ]
  },
  "evento": "prevalidacao",
  "timestamp": "2026-09-18T10:56:11",
  "versao": "1.0"
}
```

| Campo          | Descrição                                                              |
|----------------|------------------------------------------------------------------------|
| `CNPJ`         | CNPJ do emitente                                                       |
| `chNFe`        | Chave da nota com pendência                                            |
| `nNF`          | Número da nota – **veio como string** (`"61"`). Aceitar string ou número |
| `serie`        | Série da nota (string, ex.: `"27"`)                                    |
| `versao_base`  | Versão da base de regras usada na validação                            |
| `violacoes`    | Lista de inconsistências encontradas (array)                           |
| `violacoes[].cStat` | Código SEFAZ correspondente que a SEFAZ retornaria. Ex.: `778`    |
| `violacoes[].detalhe` | Detalhe por item. Ex.: `item 1: NCM=...`                       |
| `violacoes[].id` | Código interno da regra. Ex.: `NCM-INEXISTENTE`                   |
| `violacoes[].severidade` | `avisar` = alerta, não bloqueou o envio neste caso          |
| `violacoes[].xMotivo` | Descrição do motivo                                            |

**Ação recomendada no PDV:** exibir `violacoes[].detalhe` / `xMotivo` ao operador, corrigir o cadastro (ex.: NCM do item 1) e reemitir. Se ignorado, a SEFAZ tende a retornar `nota_rejeitada` com o mesmo `cStat` (ex.: `778`).

---

### `inutilizacao_realizada`

Emitido quando uma inutilização de numeração é homologada pela SEFAZ (`cStat 102`).

```json
{
  "agente": "SmartNFCe",
  "dados": {
    "cStat": 102,
    "modelo": "65",
    "nNF": 219,
    "serie": "28",
    "xMotivo": "Inutilização de número homologado"
  },
  "evento": "inutilizacao_realizada",
  "timestamp": "2026-09-11T14:26:08",
  "versao": "1.0"
}
```

| Campo     | Descrição                                          |
|-----------|----------------------------------------------------|
| `cStat`   | `102` – inutilização homologada                    |
| `modelo`  | Modelo inutilizado. Pode vir como `65` ou `NFC-e`  |
| `nNF`     | Número inutilizado                                 |
| `serie`   | Série da nota (string, ex.: `"28"`)                |
| `xMotivo` | Retorno SEFAZ                                      |

> **Uso típico:** pular numeração com falha (ex.: `nNF 219` rejeitado com erro cadastral e não reutilizável). Após `inutilizacao_realizada`, o número não pode mais ser usado para emissão.

---

### `nota_cancelada`

Emitido após um cancelamento simples ou por substituição ser processado com sucesso.

```json
{
  "agente": "SmartNFCe",
  "dados": {
    "CNPJ": "29604796000173",
    "chNFe": "13260929604796000173650270000000571492261641"
  },
  "evento": "nota_cancelada",
  "timestamp": "2026-09-18T10:53:35",
  "versao": "1.0"
}
```

| Campo   | Descrição                |
|---------|--------------------------|
| `CNPJ`  | CNPJ do emitente         |
| `chNFe` | Chave da nota cancelada  |

> Se precisar do `nNF` ou motivo do cancelamento, vincule pelo `chNFe` com o evento `nota_autorizada` anterior.

---

### `contingencia_ativada`

Emitido quando o motor entra em modo contingência, seja por falha de comunicação (automático) ou por ação do operador via dashboard (manual).

```json
{
  "agente": "SmartNFCe",
  "evento": "contingencia_ativada",
  "timestamp": "2026-06-23T17:14:21",
  "versao": "1.0",
  "dados": {
    "motivo": "Dashboard local"
  }
}
```

| Campo    | Descrição                                                          |
|----------|--------------------------------------------------------------------|
| `motivo` | Origem da ativação. Ex.: `Dashboard local`, `Falha de comunicação` |

**Ação PDV:** sinalizar operador (banner "MODO CONTINGÊNCIA"), continuar vendendo – cada venda gerará `nota_emitida_contingencia`.

---

### `contingencia_desativada`

Emitido quando o motor retorna ao modo online, encerrando o período de contingência.

```json
{
  "agente": "SmartNFCe",
  "evento": "contingencia_desativada",
  "timestamp": "2026-06-23T17:14:21",
  "versao": "1.0",
  "dados": {
    "motivo": "Dashboard local"
  }
}
```

| Campo    | Descrição                                                              |
|----------|------------------------------------------------------------------------|
| `motivo` | Origem da desativação. Ex.: `Dashboard local`, `Conexão restabelecida` |

---

### `contingencia_sincronizada`

Emitido ao final de cada ciclo de reenvio das notas emitidas em contingência, informando quantas foram sincronizadas e quantas ainda aguardam transmissão.

```json
{
  "agente": "SmartNFCe",
  "evento": "contingencia_sincronizada",
  "timestamp": "2026-06-23T17:18:31",
  "versao": "1.0",
  "dados": {
    "sincronizadas": 0,
    "ainda_pendentes": 1
  }
}
```

| Campo             | Descrição                                                |
|-------------------|----------------------------------------------------------|
| `sincronizadas`   | Quantidade de notas transmitidas com sucesso neste ciclo |
| `ainda_pendentes` | Quantidade de notas que ainda aguardam transmissão       |

**Exemplo de sequência completa:**
1. `contingencia_ativada` → 2. `nota_emitida_contingencia` (nNF 220) → 3. `contingencia_desativada` → 4. `nota_autorizada` (nNF 220, `origem: contingencia`) → 5. `contingencia_sincronizada` (`sincronizadas: 1, ainda_pendentes: 0`).

> Se alguma nota falhar na sincronização, virá `nota_rejeitada` com `origem: contingencia` em vez de `nota_autorizada`.

---

## Guia rápido de implementação no PDV

1. **Use o retorno da API como fluxo principal.** Use o webhook como complemento para atualização assíncrona (contingência, inutilização e demais eventos).
2. **Roteie por `evento` + `cStat`:**
   - `100` ou `150` → autorizada (finalizar, imprimir DANFE, guardar `nProt` + `chNFe`). `150` = autorizado fora de prazo (NF-e)
   - `-100` → contingência pendente (liberar cliente, marcar como "a sincronizar")
   - `102` + `inutilizacao_realizada` → baixar número da sequência
   - demais `cStat` + `nota_rejeitada` → exibir `xMotivo`, bloquear finalização
2. **Trate `origem` como opcional:** `if dados.get("origem") == "contingencia"` → atualizar registro criado offline em vez de criar novo.
3. **Capture `nNF` + `chNFe`:** ao receber `-100` (via API ou webhook), grave os dois campos. Quando chegar `nota_autorizada` / `nota_rejeitada` com `origem: contingencia`, localize pelo `chNFe` e atualize o status.
4. **Trate `modelo` com alias:** `65` == `NFC-e`, `55` == `NF-e`. Normalize com `.upper().replace("-", "")`.
5. **Logue tudo:** grave `timestamp`, `evento`, `nNF`, `chNFe`, `cStat` para diagnóstico.

---

## Changelog v1 → v2 (11/09/2026)

- Adicionado `origem: contingencia` em `nota_autorizada` e `nota_rejeitada`.
- `modelo` passa a aceitar `NFC-e` / `NF-e` por extenso (antes só `65` / `55`).
- Evento `inutilizacao_realizada` (`cStat 102` com `serie` + `xMotivo`).
- Exemplos atualizados com payloads reais de 11/09/2026.
- Adicionadas seções de contrato e implementação.
