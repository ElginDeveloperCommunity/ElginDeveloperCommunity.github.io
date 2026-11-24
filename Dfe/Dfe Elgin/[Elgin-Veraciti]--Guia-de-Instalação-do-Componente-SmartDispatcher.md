# Procedimento de Instalação — Elgin SmartDispatcher

<img height="250" alt="WhatsApp Image 2026-04-28 at 14 43 57 (1)" src="https://github.com/user-attachments/assets/b58e831d-60f1-4111-8252-4856518318d7" />


---

Este documento descreve o **Elgin SmartDispatcher**, componente para emissores terceiros que realiza a captura e custódia de documentos fiscais no portal **Veraciti**.

---

## 1. Visão geral

O **Elgin SmartDispatcher** é um software instalado no sistema operacional Windows que monitora diretórios configurados e captura os XMLs dos documentos fiscais e seus eventos relacionados.

É um serviço para Windows que monitora um ou mais diretórios configurados, capturando automaticamente os XMLs dos documentos fiscais e seus respectivos eventos.

**Características principais:**

| Característica | Descrição |
| -------------- | --------- |
| Sistema operacional | Compatível exclusivamente com Windows |
| Local de instalação | Computador PDV do estabelecimento ou servidor local dedicado |
| Documentos | Captura documentos fiscais NF-e e NFC-e |
| Custódia | Captura e custódia dos documentos no portal Veraciti, com armazenamento em nuvem segura por até 11 anos |
| Integração | Não requer integração por código — apenas a instalação do componente |

---

## 2. Funcionalidades

O SmartDispatcher realiza automaticamente:

* Captura dos XMLs de documentos fiscais
* Atualização de XMLs de eventos e cancelamentos
* Monitoramento do certificado digital
* Suporte para gestão do certificado digital em arquitetura local
* Custódia dos documentos na nuvem

---

## 3. Documentos capturados

Todos os XMLs devem permanecer no mesmo diretório monitorado, incluindo documentos autorizados e seus respectivos eventos.

O SmartDispatcher captura:

* NF-e autorizadas
* NFC-e autorizadas
* NFC-e e NF-e sem autorização (caso não obtenha protocolo por erro de resposta da SEFAZ)
* Documentos emitidos em contingência
* Eventos de cancelamento
* Eventos de inutilização

> [!NOTE]
> Os documentos que não possuírem protocolo de autorização serão consultados no Veraciti, que realizará a obtenção e a inserção do protocolo de autorização, desde que o certificado digital da empresa tenha sido previamente carregado (upload) no portal ou que o XML com o protocolo seja inserido posteriormente na pasta monitorada.

> [!TIP]
> Recomenda-se que a pasta monitorada contenha apenas documentos fiscais já emitidos, ou seja, que já tenham sido transmitidos à SEFAZ para autorização e seus eventos associados.

---

## 4. Pré-requisitos

Antes de iniciar a instalação, tenha em mãos:

* Licença fornecida pela Elgin
* Client ID fornecido pela Elgin
* CNPJ do cliente
* Caminho da pasta onde os XMLs são armazenados

---

## 5. Passo 1: Download e instalação

1. Acesse o repositório oficial da Elgin no GitHub e faça o download do instalador do SmartDispatcher:
   * [Repositório oficial - SmartDFeElgin](https://github.com/ElginDeveloperCommunity/SmartDFeElgin)
2. Clique com o botão direito sobre o instalador e selecione **Executar como administrador**.
3. Aceite os termos de uso.
4. Clique em **Avançar**.
5. Na tela **Componentes**, selecione **Dispatcher (HUB)** e clique em **Avançar**.
6. Após concluir esta etapa, prossiga para o Passo 2.

---

## 6. Passo 2: Configuração inicial

Preencha os campos conforme a tabela abaixo:

| Campo | Descrição |
| ----- | --------- |
| `Client ID` | Código gerado pela Elgin ao cadastrar a empresa na plataforma Veraciti |
| `Licença` | Código de licença fornecido pela Elgin |
| `CNPJ do Cliente` | CNPJ da empresa que utilizará o SmartDispatcher |
| `Diretório Monitorado` | Pasta onde o sistema emissor grava os XMLs dos documentos fiscais |

**Passo a passo:**

1. Informe o `Client ID`, a `Licença` e o `CNPJ do Cliente`.
2. Clique em **Avançar**.
3. Informe o `Diretório Monitorado`.

> [!WARNING]
> O SmartDispatcher monitora a pasta selecionada e todas as suas subpastas.
> Caso o sistema emissor organize os XMLs em várias pastas, selecione apenas a pasta principal que contém todas elas.

4. Clique em **Avançar**.
5. Clique em **Instalar**.
6. Ao término da instalação, clique em **Concluir**.
7. Em seguida, prossiga para o Passo 3.

---

## Resumo rápido (checklist)

1. [ ] Separar `Client ID`, `CNPJ do Cliente` e caminho da pasta de XMLs
2. [ ] Baixar instalador no repositório oficial e executar como administrador
3. [ ] Selecionar componente `Dispatcher (HUB)`
4. [ ] Informar `Client ID` e `CNPJ do Cliente`
5. [ ] Informar `Diretório Monitorado` (pasta principal, com subpastas incluídas)
6. [ ] Concluir instalação e validar captura e custódia no portal Veraciti
