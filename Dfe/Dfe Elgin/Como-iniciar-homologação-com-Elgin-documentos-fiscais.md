## Homologação – Elgin Documentos Fiscais

**SmartDFe** – Software Emissor para Windows | **FiscalBox** – Hardware Emissor | **Veraciti Emissor** – Emissor Cloud

A linha completa de documentos fiscais da Elgin oferece praticidade e segurança para que a Software House possa focar no principal objetivo do seu software.

Disponibilizamos diferentes opções para emissão de documentos fiscais, além de recursos para gestão e monitoramento das operações realizadas nos ambientes de seus clientes:

- **SmartDFe – Software Emissor** para Windows.
- **FiscalBox – Hardware Emissor.**
- **Veraciti Emissor – Emissor Cloud.**

![Elgin SmartDFe Dashboard](https://github.com/user-attachments/assets/d7414932-c5ae-468d-b206-b60234ed5a34)

---

## 📌 Índice
- [Plataforma Completa de Emissão Fiscal](#plataforma-completa-de-emissão-fiscal)
  - [SmartDFe — Software Emissor](#smartdfe--software-emissor)
  - [FiscalBox — Hardware Emissor](#fiscalbox--hardware-emissor)
  - [Veraciti Emissor — Emissor Cloud](#veraciti-emissor--emissor-cloud)
  - [Portal Veraciti (Gestão e Inteligência Fiscal)](#portal-veraciti-gestão-e-inteligência-fiscal)
- [Benefícios da Solução Completa](#benefícios-da-solução-completa)
- [Guia de Integração — Elgin Documentos Fiscais](#guia-de-integração--elgin-documentos-fiscais)
  - [1. Contato com a Equipe Elgin](#1-contato-com-a-equipe-elgin)
  - [2. Escolha da Arquitetura](#2-escolha-da-arquitetura)
  - [3. Instalação do Ambiente de Homologação](#3-instalação-do-ambiente-de-homologação)
  - [4. Escolhendo o Tipo de Integração](#4-escolhendo-o-tipo-de-integração)
  - [5. Integrando seu Software](#5-integrando-seu-software)
- [Suporte](#suporte)

---

## Plataforma Completa de Emissão Fiscal

### Recursos de Emissão — SmartDFe, FiscalBox e Veraciti Emissor

Os três emissores compartilham o mesmo motor fiscal da Elgin:

- ✅ Controle de numeração fiscal
- ✅ Assinatura digital dos XMLs
- ✅ Cálculo total de impostos
- ✅ Geração automática de QR Code
- ✅ Auxílio na geração do XML
- ✅ Integração simplificada para aplicações que já geram XML
- ✅ Geração do DANFE / documentos auxiliares
- ✅ Suporte à impressão em impressoras térmicas (Elgin e outros fabricantes compatíveis)
- ✅ Custódia dos documentos em nuvem + Portal Veraciti

### SmartDFe — Software Emissor
Software emissor para Windows e Android (via AAR), instalado no PDV ou servidor local. Ideal para quem quer aproveitar o hardware existente.
**Diferencial:** gestão automática de contingência offline.

### FiscalBox — Hardware Emissor
Hardware dedicado com o mesmo motor do SmartDFe, com operação local e sincronização com a nuvem para custódia e gestão. Ideal para multi-PDVs, operação sem depender de PC/servidor e redução de suporte local.
**Diferencial:** appliance plug-and-play, alta disponibilidade offline, gestão remota centralizada.

### Veraciti Emissor — Emissor Cloud
Emissor 100% online via API REST, sem necessidade de instalação local. Ideal para ERP SaaS, varejo com internet estável e operação multi-loja centralizada.
**Diferencial:** zero infraestrutura local, escala elástica.

#### Tabela comparativa

| Recurso | SmartDFe | FiscalBox | Veraciti Cloud |
|---|---|---|---|
| Controle de numeração, assinatura, impostos, QR Code, DANFE, impressão | ✅ | ✅ | ✅ |
| Custódia em nuvem + Portal Veraciti | ✅ | ✅ | ✅ |
| Contingência automática offline (sem internet) | ✅ | ✅ | — *exige internet, só cobre SEFAZ fora do ar* |
| Instalação local | Windows / Android | Não — hardware dedicado | Não — só API Cloud |
| Integração | DLL / API Nativa XML / API Hub JSON | DLL / API Nativa XML / API Hub JSON | API Cloud REST |

### Portal Veraciti (Gestão e Inteligência Fiscal)
Plataforma centralizada para gestão, monitoramento e análise das operações fiscais.

**Gestão e Monitoramento:**
- 📊 Dashboard executivo com consolidação de receitas, impostos, produtos e indicadores
- 🛡️ Monitoramento de rejeições, contingências e certificados digitais
- 🖥️ Gestão centralizada de dispositivos emissores
- ⚙️ Execução de comandos remotos
- 👥 Gestão de usuários e permissões
- 📥 Download de documentos fiscais individuais ou em lote
- ☁️ Custódia centralizada dos documentos
- 📧 Envio programado de documentos por e-mail
- ⏱️ Acompanhamento em tempo real das operações fiscais

---

## Benefícios da Solução Completa

Ao utilizar o **SmartDFe**, **FiscalBox** e **Veraciti Emissor** em conjunto com o **Portal Veraciti**, sua empresa conta com uma plataforma fiscal completa que une:
- Emissão fiscal robusta e automatizada
- Monitoramento centralizado
- Governança e compliance fiscal
- Armazenamento seguro dos documentos
- Gestão remota dos dispositivos
- Inteligência operacional através de dashboards e indicadores

---

## Guia de Integração — Elgin Documentos Fiscais

Como iniciar a homologação? Simples, siga os passos abaixo.

### 1. Contato com a Equipe Elgin
Entre em contato com nosso time de especialistas pelo canal oficial de WhatsApp:
- 📱 **+55 11 99851-9559** — selecione a **opção 2**

Um especialista irá criar seu cadastro no portal Veraciti (**obrigatório**) e gerar seu **Client ID** (utilizado apenas na arquitetura Híbrida).

### 2. Escolha da Arquitetura
Antes de instalar o ambiente, você deve definir qual tipo de arquitetura deseja utilizar no seu projeto. A escolha da arquitetura define quais produtos você poderá utilizar:

#### ☁️ 100% Cloud
**Produto utilizável:** **Veraciti Emissor – Emissor Cloud**
Emissão 100% online, não sendo possível emitir sem internet.
- **Contingência:** Suporte para contingência apenas quando a SEFAZ está fora do ar, mas **exige internet** para realizar o acesso à API.

#### 🔄 Híbrida (Local + Cloud)
**Produtos utilizáveis:** **SmartDFe – Software Emissor (Windows / Android)** e **FiscalBox – Hardware Emissor**
Emissor local com custódia e gestão Cloud em tempo real.
- **Alta Disponibilidade:** Não para de emitir sem internet e não para de emitir se a SEFAZ estiver fora do ar.
- **Sincronização:** Todas as notas são sincronizadas com a nuvem e armazenadas em tempo real. Caso fique sem internet, as notas emitidas offline serão sincronizadas assim que a conexão retornar.

> 📌 **Resumo:** Escolheu **100% Cloud** → usa o **Veraciti Emissor**. Escolheu **Híbrida** → usa o **SmartDFe** ou o **FiscalBox**. Independente da arquitetura, a custódia e gestão são feitas pelo **Portal Veraciti**.

> ❓ **Dúvida sobre o funcionamento dos emissores e qual arquitetura escolher?**
> Agende uma reunião inicial de homologação, nossos especialistas irão te atender e auxiliá-los a escolher o modelo ideal para o seu software.
> 🔗 [Agendar Reunião Inicial de Homologação](https://bookings.cloud.microsoft/book/Homologaes@elgin.com.br/?ismsaljsauthenabled)

### 3. Instalação do Ambiente de Homologação

O processo de configuração do ambiente varia conforme a arquitetura escolhida no passo anterior:

#### Para Arquitetura Híbrida (Local + Cloud)
Com o **Client ID** em mãos, faça o download do instalador:

🔗 [Baixar Instalador](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/tree/master/SmartDFe%20e%20FiscaBox/Instaladores)

Em seguida, siga o tutorial oficial de instalação e ativação em ambiente de homologação:

🔗 [Guia de Instalação do Elgin SmartDFe](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe%5D-Guia-de-Instala%C3%A7%C3%A3o-do-Elgin-SmartDFe)

#### Para Arquitetura 100% Cloud
Acesse o Portal Veraciti e siga os passos abaixo para gerar sua credencial:
1. Clique no menu **Credencial** e selecione **Nova Credencial**.
2. Escolha o **modelo de API**.
3. Defina se a **numeração** será gerenciada pelo seu sistema ou pelo Veraciti.
4. Selecione a **empresa** e o **ambiente de homologação**.
5. Escolha os **documentos desejados** para aquela credencial e clique em **Salvar**.

Pronto! Uma credencial será gerada e você estará apto a iniciar a integração.

### 4. Escolhendo o Tipo de Integração
A plataforma Elgin Documentos Fiscais oferece diferentes modalidades de integração para se adaptar à arquitetura do seu software:
- Biblioteca — DLL
- API REST
- Outros modelos de integração

> 💡 **Nota:** Nossa equipe de especialistas está disponível para orientar você na escolha da abordagem mais adequada às suas necessidades técnicas. Além disso, o SmartDFe oferece suporte completo para geração e impressão de PDF dos documentos fiscais.

Após a instalação, escolha o tipo de integração mais adequado ao seu software. Documentações específicas estão disponíveis para cada modalidade:

#### 🔄 Integrações para Arquitetura Híbrida
> 💡 **Nota:** Essas integrações são exclusivas para a **Arquitetura Híbrida** e podem ser utilizadas no SmartDFe Windows, SmartDFe Android e FiscalBox.

##### 📌 Biblioteca — DLL
Oferece suporte completo para emissão, geração de XML, geração de DANFE e impressão.

🔗 [Documentação — Componente DLL](https://elgindevelopercommunity.github.io/group__g11.html)  

🔗 [Download das Bibliotecas](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/tree/master/SmartDFe/Bibliotecas)

##### 📌 API Nativa (XML)
Utilizada para realizar a emissão e acessar os endpoints completos da SEFAZ utilizando arquivos XML. Retorna o XML assinado ou o resultado da requisição.

🔗 [Documentação — API Nativa (XML)](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-API-Nativa-(xml))

##### 📌 API Hub (JSON)
Oferece suporte completo para emissão, geração de DANFE e impressão via spooler do Windows utilizando comunicação baseada em JSON.

🔗 [Documentação — API Hub (JSON)](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-API-Hub-(json))

#### ☁️ Integração para Arquitetura 100% Cloud
> 💡 **Nota:** Essa integração é destinada exclusivamente à **Arquitetura 100% Cloud**.

##### 📌 API Cloud (REST)
A comunicação é realizada via API de forma totalmente online.

🔗 [Documentação — API Veraciti](https://docs.veraciti.com.br/)

#### 📌 Software Exemplo de Integração
Exemplos de integração em diversas linguagens de programação estão disponíveis no repositório oficial:

🔗 [Exemplos de Integração](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/tree/master/SmartDFe%20e%20FiscaBox/Exemplos)

### 5. Integrando seu Software
Após escolher o tipo de integração e baixar todo o material de apoio, insira todas as funções presentes na documentação. Para melhor apoiá-lo, utilize como base o roteiro de integração abaixo.

#### Roteiro de Testes e Integração
Durante o processo de homologação, utilize o roteiro de testes para acompanhar a evolução da integração. Ao final, envie o roteiro preenchido ao time Elgin conforme as instruções disponíveis no link abaixo:

🔗 [Roteiro de Homologação (SmartDFe e FiscalBox).xlsx](https://github.com/user-attachments/files/32749099/Roteiro_Homologacao_SmartDFe.e.FiscalBox_v4_.xlsx)

Cada forma de integração escolhida terá referenciado em sua documentação o guia de tratamento de retorno.

- 🔗 [Tabela de retornos Sefaz](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe%5D-Tabela-de-Retornos) — tabela para consultar o cStat da SEFAZ e seus significados.

---

## Suporte

Em caso de dúvidas durante o processo de integração ou homologação, entre em contato com a equipe de especialistas Elgin:
📱 **+55 11 99851-9559** — selecione a **opção 2**
