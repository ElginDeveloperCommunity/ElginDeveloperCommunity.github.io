# Como iniciar com Elgin Veraciti: Emissores Terceiros


<img height="250" alt="WhatsApp Image 2026-04-28 at 14 43 57 (1)" src="https://github.com/user-attachments/assets/b58e831d-60f1-4111-8252-4856518318d7" />

---

A linha completa de documentos fiscais da Elgin oferece praticidade e segurança para que a Software House possa focar no principal objetivo do seu software.

Para softwares que já possuem emissor fiscal próprio (emissores terceiros), disponibilizamos o **SmartDispatcher**: um software para Windows que monitora o diretório onde seu sistema armazena os XMLs, realizando a custódia dos documentos fiscais de forma automática no Portal Veraciti — **sem necessidade de integração por código**.

---

## 📌 Índice
- [Plataforma Completa — Emissores Terceiros](#plataforma-completa--emissores-terceiros)
  - [SmartDispatcher (Software Captor)](#smartdispatcher-software-captor)
  - [Portal Veraciti (Gestão e Inteligência Fiscal)](#portal-veraciti-gestão-e-inteligência-fiscal)
- [Benefícios da Solução](#benefícios-da-solução)
- [Guia de Integração — SmartDispatcher](#guia-de-integração--smartdispatcher)
  - [1. Contato com a Equipe Elgin](#1-contato-com-a-equipe-elgin)
  - [2. Instalação do Componente](#2-instalação-do-componente)
  - [3. Autorização na Plataforma Veraciti](#3-autorização-na-plataforma-veraciti)
- [Funcionalidades](#funcionalidades)
- [Suporte](#suporte)

---

## Plataforma Completa — Emissores Terceiros

### SmartDispatcher (Software Captor)
Responsável por capturar os XMLs de documentos fiscais emitidos por sistemas de terceiros. É um serviço para Windows que monitora um ou mais diretórios configurados, capturando automaticamente os XMLs dos documentos fiscais e seus respectivos eventos.

**Recursos:**
- ✅ Compatível exclusivamente com sistema operacional Windows
- ✅ Pode ser instalado no computador (PDV) ou em servidor local dedicado
- ✅ Captura documentos fiscais **NF-e** e **NFC-e**
- ✅ Captura NF-e / NFC-e autorizadas, emitidas em contingência e sem protocolo (com consulta automática no Veraciti)*
- ✅ Captura eventos de cancelamento e inutilização
- ✅ Armazena os documentos em nuvem por até **11 anos**
- ✅ Instalação autônoma, sem necessidade de integração por código
- ✅ Monitora a pasta selecionada e todas as suas subpastas

> \* Os documentos que não possuírem protocolo de autorização serão consultados no Veraciti, que realizará a obtenção e a inserção do protocolo de autorização, desde que o certificado digital da empresa tenha sido previamente carregado (upload) no portal.

> ⚠️ **Recomendação:** a pasta monitorada deve conter apenas documentos fiscais já emitidos, ou seja, que já tenham sido transmitidos à SEFAZ para autorização. Todos os XMLs devem permanecer no mesmo diretório monitorado, incluindo documentos autorizados e seus respectivos eventos.

### Portal Veraciti (Gestão e Inteligência Fiscal)
Plataforma centralizada para gestão, monitoramento e análise das operações fiscais.

**Gestão e Monitoramento:**
- 📊 Dashboard executivo com consolidação de receitas, impostos, produtos e indicadores
- 🛡️ Monitoramento de rejeições e certificados digitais
- 🖥️ Gestão centralizada de dispositivos (SmartDispatcher)
- 👥 Gestão de usuários e permissões
- 📥 Download de documentos fiscais individuais ou em lote
- ☁️ Custódia centralizada dos documentos por até 11 anos
- 📧 Envio programado de documentos por e-mail
- ⏱️ Acompanhamento em tempo real das operações fiscais

---

## Benefícios da Solução

Ao utilizar o **SmartDispatcher** em conjunto com o **Portal Veraciti**, sua empresa conta com uma plataforma fiscal completa que une:
- Custódia automática sem alterar seu emissor atual
- Monitoramento centralizado
- Governança e compliance fiscal
- Armazenamento seguro dos documentos por até 11 anos
- Gestão remota dos dispositivos
- Inteligência operacional através de dashboards e indicadores

---

## Guia de Integração — SmartDispatcher

Como iniciar a homologação? Simples, siga os passos abaixo.

### 1. Contato com a Equipe Elgin
Entre em contato com nosso time de especialistas pelo canal oficial de WhatsApp:
- 📱 **+55 11 99851-9559** — selecione a **opção 2**

Um especialista irá criar seu cadastro no portal Veraciti (**obrigatório**) e gerar:
- **Client ID** — código da sua empresa gerado pela plataforma


Tenha em mãos para a instalação:

- Client ID fornecido pela Elgin
- CNPJ do cliente
- Caminho da pasta onde os XMLs são armazenados

### 2. Instalação do Componente
Com o **Client ID** em mãos, faça o download do instalador:
🔗 [Baixar Instalador](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/tree/master/PortalVeraciti)

Em seguida, siga o tutorial oficial de instalação e ativação:
🔗 [Guia de Instalação do Componente SmartDispatcher](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BElgin-Veraciti%5D--Guia-de-Instala%C3%A7%C3%A3o-do-Componente-SmartDispatcher)

**Resumo do procedimento:**
1. Clique com o botão direito sobre o instalador e selecione **Executar como administrador**.
2. Aceite os termos de uso e clique em **Avançar**.
3. Na tela **Componentes**, selecione **Dispatcher (HUB)** e clique em **Avançar**.
4. Informe o **Client ID** e o **CNPJ do Cliente** e clique em **Avançar**.
5. Informe o **Diretório Monitorado** (pasta onde o sistema emissor grava os XMLs) e clique em **Avançar**.
   > O SmartDispatcher monitora a pasta selecionada e todas as suas subpastas. Caso o sistema organize os XMLs em várias pastas, selecione apenas a pasta principal que contém todas elas.
6. Clique em **Instalar** e, ao término, clique em **Concluir**.

### 3. Autorização na Plataforma Veraciti
1. Acesse a plataforma **Veraciti**.
2. Abra a aba **Dispositivos**.
3. Localize o dispositivo recém-instalado.
4. Clique em **Autorizar**.
5. Selecione a opção **Apenas autorizar**.

✅ Após a autorização, o SmartDispatcher iniciará automaticamente o monitoramento da pasta configurada e enviará os XMLs para a plataforma Veraciti, onde ficarão armazenados para consulta.

---

## Funcionalidades

O SmartDispatcher realiza automaticamente:
- Captura dos XMLs de documentos fiscais (NF-e e NFC-e)
- Atualização de XMLs de eventos e cancelamentos
- Monitoramento do certificado digital
- Suporte para gestão do certificado digital em arquitetura local
- Custódia dos documentos na nuvem

**Documentos capturados:**
- NF-e autorizadas
- NFC-e autorizadas
- NFC-e e NF-e sem autorização (caso não obtenha protocolo por erro de resposta da SEFAZ)
- Documentos emitidos em contingência
- Eventos de cancelamento
- Eventos de inutilização

---

## Suporte

Em caso de dúvidas durante o processo de instalação ou homologação, entre em contato com a equipe de especialistas Elgin:
📱 **+55 11 99851-9559** — selecione a **opção 2**
