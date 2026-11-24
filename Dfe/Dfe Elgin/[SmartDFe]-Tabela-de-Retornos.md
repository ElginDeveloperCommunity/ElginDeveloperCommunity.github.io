Nessa documentação você encontrará códigos de retorno do software de emissão da Elgin e os retornos padrão da Sefaz. Use a documentação [[SmartDFe e FiscalBox] Campos Json Retorno Emissor
](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Retorno-das-requisi%C3%A7%C3%B5es-pelo-SmartDFe-e-FiscalBox) para entender quais os campos de retornos do SmartDFe e fiscalbox.

Exemplo de fluxo para tratamento de retorno:

<img width="750"  alt="Fluxo_retorno_SmartDFe" src="https://github.com/user-attachments/assets/f439b6ea-13a5-4c89-b777-2a9821c28906" />

# Tabela de Retornos campo cStat - SmartNFCe

## Retorno do Emissor

| Código | Descrição |
|--------|-----------|
| -1 | Licença expirada ou timeout. Validar no campo xMotivo |
| -2 | Certificado digital vencido |
| -3 | Erro de schema – xml contém erros |
| -4 | UUID já utilizado |
| -100 | Documento fiscal emitido em contingência |

## Retorno da SEFAZ

### Códigos de Status (100-199) - Processamento

| Código | Descrição |
|--------|-----------|
| 100 | Autorizado o uso da NF-e |
| 101 | Cancelamento de NF-e homologado |
| 102 | Inutilização de número homologado |
| 103 | Lote recebido com sucesso |
| 104 | Lote processado |
| 105 | Lote em processamento |
| 106 | Lote não localizado |
| 107 | Serviço em Operação |
| 108 | Serviço Paralisado Momentaneamente (curto prazo) |
| 109 | Serviço Paralisado sem Previsão |
| 110 | Uso Denegado |
| 111 | Consulta cadastro com uma ocorrência |
| 112 | Consulta cadastro com mais de uma ocorrência |
| 113 | SCAN será desabilitado para a UF às hh:mm |
| 114 | SCAN desabilitado pela SEFAZ Origem |
| 128 | Lote de Evento Processado |
| 135 | Evento registrado e vinculado a NF-e |
| 136 | Evento registrado, mas não vinculado a NF-e |
| 137 | Nenhum documento localizado para o Destinatário |
| 138 | Documento localizado para o Destinatário |
| 139 | Pedido de Download processado |
| 140 | Download disponibilizado |
| 150 | Autorizado o uso da NF-e |
| 151 | Cancelamento de NF-e homologado fora de prazo |
| 155 | Cancelamento de NF-e homologado fora de prazo |

### Códigos de Rejeição (200-299) - Dados Cadastrais

| Código | Descrição |
|--------|-----------|
| 201 | Rejeição: O número máximo de numeração de NF-e a inutilizar ultrapassou o limite |
| 202 | Rejeição: Falha no reconhecimento da autoria ou integridade do arquivo digital |
| 203 | Rejeição: Emissor não habilitado para emissão de NF-e |
| 204 | Duplicidade de NF-e |
| 205 | NF-e está denegada na base de dados da SEFAZ |
| 206 | Rejeição: NF-e já está inutilizada na Base de dados da SEFAZ |
| 207 | Rejeição: CNPJ do emitente inválido |
| 208 | Rejeição: CNPJ do destinatário inválido |
| 209 | Rejeição: IE do emitente inválida |
| 210 | Rejeição: IE do destinatário inválida |
| 211 | Rejeição: IE do substituto inválida |
| 212 | Rejeição: Data de emissão NF-e posterior a data de recebimento |
| 213 | Rejeição: CNPJ-Base do Emitente difere do CNPJ-Base do Certificado Digital |
| 214 | Rejeição: Tamanho da mensagem excedeu o limite estabelecido |
| 215 | Rejeição: Falha no schema XML |
| 216 | Rejeição: Chave de Acesso difere da cadastrada |
| 217 | Rejeição: NF-e não consta na base de dados da SEFAZ |
| 218 | NF-e já está cancelada na base de dados da SEFAZ |
| 219 | Rejeição: Circulação da NF-e verificada |
| 225 | Rejeição: Falha no Schema XML da NF-e |
| 226 | Rejeição: Código da UF do Emitente diverge da UF autorizadora |
| 227 | Rejeição: Erro na Chave de Acesso - Campo ID |
| 228 | Rejeição: Data de Emissão muito atrasada |
| 232 | Rejeição: IE do destinatário não informada |
| 233 | Rejeição: IE do destinatário não cadastrada |
| 234 | Rejeição: IE do destinatário não vinculada ao CNPJ |
| 245 | Rejeição: CNPJ Emitente não cadastrado |
| 246 | Rejeição: CNPJ Destinatário não cadastrado |
| 252 | Rejeição: Ambiente informado diverge do Ambiente de recebimento |

### Códigos de Rejeição (300-399) - Validações de Cadastro

| Código | Descrição |
|--------|-----------|
| 300 | Rejeição: Uso Denegado - Irregularidade fiscal do emitente |
| 301 | Rejeição: Uso Denegado - Irregularidade fiscal do destinatário |
| 302 | Uso Denegado: Irregularidade fiscal do destinatário |
| 303 | Uso Denegado: Destinatário não habilitado a operar na UF |
| 305 | Rejeição: Destinatário bloqueado na UF |
| 306 | Rejeição: IE do destinatário não está ativa na UF |
| 320 | Rejeição: Contranota de Produtor referencia somente NF de outro emitente |
| 321 | Rejeição: NF-e de devolução de mercadoria não possui documento fiscal referenciado |
| 327 | Rejeição: CFOP inválido para Nota Fiscal com finalidade de devolução de mercadoria [nItem:nnn] |
| 328 | Rejeição: CFOP de devolução de mercadoria para NF-e que não tem finalidade de devolução de mercadoria |
| 344 | Rejeição: Duplicidade de NF-e informada na Exportação Indireta |
| 345 | Rejeição: Chave de Acesso informada na Exportação Indireta não consta como NF-e referenciada |
| 346 | Rejeição: Somatório das quantidades informadas na Exportação Indireta não corresponde a quantidade total do item |
| 347 | Rejeição: Descrição do combustível diverge da descrição adotada pela ANP |
| 352 | Rejeição: Valor do ICMS Diferido no CST=51 difere do produto Valor ICMS Operação e percentual diferimento |
| 353 | Rejeição: Valor do ICMS no CST=51 não corresponde a diferença do ICMS operação e ICMS diferido |
| 354 | Rejeição: Informado grupo de devolução de tributos para NF-e que não tem finalidade de devolução de mercadoria |
| 355 | Rejeição: Informar o local de saída do País no caso da exportação |
| 356 | Rejeição: Informar o local de saída do País somente no caso da exportação |
| 357 | Rejeição: Chave de Acesso do grupo de Exportação Indireta inexistente [nRef: xxx] |
| 358 | Rejeição: Chave de Acesso do grupo de Exportação Indireta cancelada ou denegada [nRef: xxx] |
| 359 | Rejeição: NF-e de venda a Órgão Público sem informar a Nota de Empenho |
| 360 | Rejeição: NF-e com Nota de Empenho inválida para a UF |
| 361 | Rejeição: NF-e com Nota de Empenho inexistente na UF |
| 362 | Rejeição: Venda de combustível sem informação do Transportador |
| 364 | Rejeição: Total do valor da dedução do ISS difere do somatório dos itens |
| 365 | Rejeição: Total de outras retenções difere do somatório dos itens |
| 374 | Rejeição: CFOP incompatível com o grupo de tributação [nItem:nnn] |
| 388 | Rejeição: Código de Situação Tributária do IPI incompatível com o Código de Enquadramento Legal do IPI |
| 389 | Rejeição: Código Município ISSQN inexistente {nItem:nnn} |
| 390 | Rejeição: Nota Fiscal com grupo de devolução de tributos {nItem:nnn} |
| 391 | Rejeição: Não informados os dados do cartão de crédito/débito nas Formas de Pagamento |
| 392 | Rejeição: Não informados os dados da operação de pagamento por cartão de crédito/débito |
| 393 | Rejeição: NF-e com o grupo de Informações Suplementares |
| 394 | Rejeição: Nota Fiscal sem a informação do QR-Code |
| 395 | Rejeição: Endereço do site da UF da Consulta via QRCode diverge do previsto |
| 396 | Rejeição: Parâmetro do QR-Code inexistente (chAcesso) |
| 397 | Rejeição: Parâmetro do QR-Code divergente da Nota Fiscal (chAcesso) |
| 398 | Rejeição: Parâmetro nVersao do QR-Code difere do previsto |
| 399 | Rejeição: Parâmetro de Identificação do destinatário no QR-Code para Nota Fiscal sem identificação do destinatário |

### Códigos de Rejeição (400-499) - Validações Técnicas

| Código | Descrição |
|--------|-----------|
| 400 | Rejeição: Parâmetro do QR-Code não está no formato hexadecimal (dhEmi) |
| 402 | Rejeição: XML da área de dados com codificação diferente de UTF-8 |
| 403 | Rejeição: O grupo de informações da NF-e avulsa é de uso exclusivo do Fisco |
| 404 | Rejeição: Uso de prefixo de namespace não permitido |
| 405 | Rejeição: Código do país do emitente: dígito inválido |
| 406 | Rejeição: Código do país do destinatário: dígito inválido |
| 407 | Rejeição: O CPF só pode ser informado no campo emitente para a NF-e avulsa |
| 408 | Rejeição: Evento não disponível para Autor pessoa física |
| 409 | Rejeição: Campo cUF inexistente no elemento nfeCabecMsg do SOAP Header |
| 410 | Rejeição: UF informada no campo cUF não é atendida pelo Web Service |
| 411 | Rejeição: Campo versaoDados inexistente no elemento nfeCabecMsg do SOAP Header |
| 417 | Rejeição: Total do ICMS superior ao valor limite estabelecido |
| 418 | Rejeição: Total do ICMS ST superior ao valor limite estabelecido |
| 420 | Rejeição: Cancelamento para NF-e já cancelada |
| 450 | Rejeição: Modelo da NF-e diferente de 55 |
| 451 | Rejeição: Processo de emissão informado inválido |
| 452 | Rejeição: Solicitada resposta assíncrona para lote com somente uma NFC-e |
| 479 | Rejeição: Data de Emissão anterior a data de credenciamento ou anterior a Data de Abertura do estabelecimento |
| 480 | Rejeição: Código Município do Emitente diverge do cadastrado na UF |
| 481 | Rejeição: Código Regime Tributário do emitente diverge do cadastro na SEFAZ |
| 482 | Rejeição: Código do Município do Destinatário diverge do cadastrado na UF |
| 483 | Rejeição: Valor do desconto maior que valor do produto [nItem:nnn] |
| 484 | Rejeição: Chave de Acesso com tipo de emissão diferente de 4 |
| 485 | Rejeição: Duplicidade de numeração do EPEC (Modelo, CNPJ, Série e Número) |
| 486 | Rejeição: Não informado o Grupo de Autorização para UF que exige a identificação |
| 487 | Rejeição: Escritório de Contabilidade não cadastrado na SEFAZ |
| 488 | Rejeição: Vendas do Emitente incompatíveis com o Porte da Empresa |
| 489 | Rejeição: CNPJ informado inválido (DV ou zeros) |
| 490 | Rejeição: CPF informado inválido (DV ou zeros) |
| 491 | Rejeição: O tpEvento informado inválido |
| 492 | Rejeição: O verEvento informado inválido |
| 496 | Rejeição: Não informado o tipo de integração no pagamento com cartão de crédito/débito |

### Códigos de Rejeição (500-599) - Validações de Dados

| Código | Descrição |
|--------|-----------|
| 501 | Rejeição: Pedido de Cancelamento intempestivo (NF-e autorizada a mais de 7 dias) |
| 502 | Rejeição: Erro na Chave de Acesso - Campo Id não corresponde à concatenação dos campos correspondentes |
| 506 | Rejeição: Data de Saída menor que a Data de Emissão |
| 539 | Rejeição: Duplicidade de NF-e, com diferença na Chave de Acesso |
| 546 | Rejeição: NF-e de devolução com valor do ICMS superior à NF-e devolvida |
| 566 | Rejeição: NF-e de devolução com valor do FCP superior à NF-e devolvida |
| 570 | Rejeição: Tipo de Emissão 3, 6 ou 7 só é válido nas contingências SCAN/SVC |
| 587 | Rejeição: Usar somente o namespace padrão da NF-e |
| 588 | Rejeição: Não é permitida a presença de caracteres de edição no início/fim da mensagem |
| 589 | Rejeição: Número do NSU informado superior ao maior NSU da base de dados da SEFAZ |
| 590 | Rejeição: Informado CST para emissor do Simples Nacional (CRT=1) |
| 591 | Rejeição: Informado CSOSN para emissor que não é do Simples Nacional (CRT diferente de 1) |
| 592 | Rejeição: A NF-e deve ter pelo menos um item de produto sujeito ao ICMS |
| 593 | Rejeição: CNPJ-Base consultado difere do CNPJ-Base do Certificado Digital |
| 594 | Rejeição: O número de sequencia do evento informado é maior que o permitido |
| 595 | Rejeição: Obrigatória a informação da justificativa do evento |
| 596 | Rejeição: Evento apresentado fora do prazo |
| 597 | Rejeição: CFOP de Importação e não informado dados de IPI |
| 600 | Rejeição: CSOSN incompatível na operação com Não Contribuinte |

### Códigos de Rejeição (600-699) - Validações de Operações

| Código | Descrição |
|--------|-----------|
| 622 | Rejeição: Chave de acesso de NF-e inválida (Ano < 2005 ou Ano maior que Ano corrente) |
| 623 | Rejeição: Chave de acesso de NF-e inválida (Mês = 0 ou Mês > 12) |
| 624 | Rejeição: Chave de acesso de NF-e inválida (CNPJ zerado ou digito inválido) |
| 625 | Rejeição: Chave de acesso de NFe inválida (modelo diferente de 55) |
| 626 | Rejeição: Chave de acesso de NF-e inválida (número NF = 0) / CFOP de operação isenta para ZFM diferente do previsto |
| 628 | Rejeição: Total da NF superior ao valor limite estabelecido pela SEFAZ |
| 660 | Rejeição: CFOP de Combustível e não informado grupo de combustível da NF-e |
| 673 | Rejeição: CT-e informado não pode estar cancelado/denegado na base da SEFAZ |
| 690 | Rejeição: Pedido de cancelamento para NF-e com CT-e ou MDF-e |
| 693 | Rejeição: Alíquota de ICMS superior a definida para a operação interestadual |
| 694 | Rejeição: Não informado o grupo de ICMS para a UF de destino |
| 696 | Rejeição: Operação com não contribuinte deve indicar operação com consumidor final |

### Códigos de Rejeição (700-799) - Validações de Tributos

| Código | Descrição |
|--------|-----------|
| 700 | Rejeição: Total da quantidade tributada do ICMS monofásico próprio difere do somatório dos itens |
| 701 | Rejeição: Não informada Nota Fiscal referenciada |
| 797 | Rejeição: Valor total do ICMS Interestadual da UF de destino difere do somatório dos itens |
| 798 | Rejeição: Valor total do ICMS relativo Fundo de Combate à Pobreza (FCP) da UF de destino difere do somatório do valor dos itens |
| 799 | Rejeição: Valor total do ICMS Interestadual da UF de destino difere do somatório dos itens |

### Códigos de Rejeição (800-899) - Validações Diversas

| Código | Descrição |
|--------|-----------|
| 805 | Rejeição: A SEFAZ do destinatário não permite Contribuinte Isento de Inscrição Estadual |
| 806 | Rejeição: Operação com ICMS-ST sem informação do CEST |
| 807 | Rejeição: NFC-e com grupo de ICMS para a UF do destinatário |
| 817 | Rejeição: Unidade Tributável incompatível com o NCM informado na operação com Comércio Exterior [nItem:nnn] |
| 861 | Rejeição: Total do FCP difere do somatório dos itens |
| 862 | Rejeição: Total do FCP ST difere do somatório dos itens |
| 866 | Rejeição: Ausência de troco quando o valor dos pagamentos informados for maior que o total da nota |
| 869 | Rejeição: Valor do troco incorreto |
| 870 | Rejeição: Data de validade incompatível com data de fabricação {nItem:nnn} |
| 871 | Rejeição: O campo Forma de Pagamento deve ser preenchido com a opção "Sem Pagamento" |
| 872 | Rejeição: Informado Duplicata Mercantil como Forma de Pagamento |
| 873 | Rejeição: Operação com medicamentos não informado os campos de rastreabilidade {nItem:nnn} |
| 874 | Rejeição: Percentual de FCP inválido {nItem:nnn} |
| 875 | Rejeição: Percentual de FCP ST inválido {nItem:nnn} |
| 876 | Rejeição: Operação interestadual para Consumidor Final e valor do FCP informado |
| 877 | Rejeição: Nota Fiscal sem a informação da URL de consulta por chave de acesso |
| 878 | Rejeição: Endereço do site da UF da Consulta por chave de acesso diverge do previsto |
| 879 | Rejeição: Informado item "Produzido em Escala NÃO Relevante" e não informado CNPJ do Fabricante |
| 897 | Rejeição: Código numérico em formato inválido |

### Códigos de Rejeição (900-999) - Validações Específicas

| Código | Descrição |
|--------|-----------|
| 900 | Rejeição: Data de vencimento da parcela não informada ou menor que a data de emissão |
| 904 | Rejeição: Informado indevidamente campo valor de pagamento |
| 922 | Rejeição: Contranota de Produtor só pode referenciar NF-e ou NF de Produtor Modelo 4 |
| 923 | Rejeição: Referenciado documento de operação interna em operação interestadual ou com o exterior |
| 924 | Rejeição: Informado Cupom Fiscal referenciado |
| 925 | Rejeição: NF-e com identificação de estrangeiro e inscrição estadual informada para destinatário |
| 926 | Rejeição: Operação com Exterior e país de destino igual a Brasil |
| 927 | Rejeição: Número do item fora da ordem sequencial |
| 928 | Rejeição: Informado código de benefício fiscal para CST sem benefício fiscal [nItem: nnn] |
| 929 | Rejeição: Informado CST de diferimento sem as informações de diferimento [nItem: nnn] |
| 930 | Rejeição: CST com benefício fiscal e não informado o código de benefício fiscal [nItem: nnn] |
| 931 | Rejeição: CST não corresponde ao tipo de código de benefício fiscal [nItem: nnn] |
| 932 | Rejeição: Informada modalidade de determinação da BC da ST como MVA e não informado o campo pMVAST [nItem: nnn] |
| 933 | Rejeição: Informada modalidade de determinação da BC da ST diferente de MVA e informado o campo pMVAST [nItem: nnn] |
| 934 | Rejeição: Não informado valor do ICMS desonerado ou o Motivo de desoneração [nItem: nnn] |
| 935 | Rejeição: Valor total da Base de Cálculo superior ao valor limite estabelecido |
| 936 | Rejeição: Razão Social do emitente diverge do informado no cadastro da SEFAZ |
| 938 | Rejeição: Não informada vBCSTRet, pST, vICMSSubstituto e vICMSSTRet |
| 946 | Rejeição: Informado código de benefício fiscal incorreto ou inexistente na UF [nItem: nnn] |
| 958 | Rejeição: Somatório dos percentuais originários para a UF do combustível diverge de 100 |
| 960 | Rejeição: Obrigatório o preenchimento de Grupo de Tributação do ICMS monofásica sobre combustíveis |
| 963 | Rejeição: Tipo de pagamento não aceita o grupo de cartões ou boletos. [nOcor:999] |
| 965 | Rejeição: Valor do troco acima do permitido |
| 972 | Rejeição: Obrigatória as informações do responsável técnico |
| 977 | Rejeição: Identificador do CSRT revogado |
| 997 | Rejeição: Série já vinculada a outra Inscrição Estadual deste CNPJ |
| 998 | Uso Denegado: Emissão monitorada de NF-e |
| 999 | Rejeição: Erro não catalogado (informar a mensagem de erro capturado no tratamento da exceção) |

### Códigos de Rejeição (1000+) - Novas Validações

| Código | Descrição |
|--------|-----------|
| 1026 | Rejeição: Alíquota do IBS da UF deve ser igual a 0,1% para documento emitido em 2026 |
| 1037 | Rejeição: Alíquota da CBS deve ser igual a 0,9% para documento emitido em 2026 |
| 1115 | Rejeição: IBS/CBS não informado |
| 1148 | Rejeição: NF-e referenciada de pagamento antecipado informada indevidamente |
| 1149 | Rejeição: NF-e referenciada de pagamento antecipado inexistente |
| 1150 | Rejeição: NF-e referenciada de pagamento antecipado deve ser do tipo débito |

---

## Observações Importantes

1. **Códigos 100-199**: Indicam processamento bem-sucedido ou status da operação
2. **Códigos 200-999**: Indicam rejeições que impedem a autorização da NF-e
3. **Códigos com [nItem:nnn]**: Indicam o item específico da nota que apresenta problema
4. **Códigos 300-303**: Indicam denegação de uso (problemas fiscais graves)

## Fontes e Atualizações

Esta tabela é baseada nas Notas Técnicas da SEFAZ e pode sofrer alterações. Para informações sempre atualizadas, consulte:

- **Portal Nacional da NF-e**: https://www.nfe.fazenda.gov.br
- **Manual de Orientação do Contribuinte (MOC)**
- **Notas Técnicas vigentes**

**Última atualização**: Outubro 2025