# Homologação – Elgin FiscalBox

A linha completa de documentos fiscais da Elgin oferece praticidade e segurança para que a Software House possa focar no principal objetivo do seu software.

Disponibilizamos diferentes opções para emissão de documentos fiscais, além de recursos para gestão e monitoramento das operações realizadas nos ambientes de seus clientes.

<img width="886" height="489" alt="image" src="https://github.com/user-attachments/assets/42c36d43-de53-4b51-9742-8f9264999e9f" />


---

# Plataforma Completa de Emissão Fiscal

## FiscalBox (Hardware emissor)

Responsável pela emissão e processamento dos documentos fiscais.

### Recursos

- Controle de numeração fiscal
- Gestão automática de contingência
- Assinatura digital dos XMLs
- Cálculo total de impostos
- Geração automática de QR Code
- Custódia dos documentos em nuvem
- Integração simplificada para aplicações que já geram XML

---

## Portal Veraciti (Gestão e Inteligência Fiscal)

Plataforma centralizada para gestão, monitoramento e análise das operações fiscais.

### Gestão e Monitoramento

- Dashboard executivo com consolidação de receitas, impostos, produtos e indicadores
- Monitoramento de rejeições, contingências e certificados digitais
- Gestão centralizada de dispositivos emissores
- Execução de comandos remotos
- Gestão de usuários e permissões
- Download de documentos fiscais individuais ou em lote
- Custódia centralizada dos documentos
- Envio programado de documentos por e-mail
- Acompanhamento em tempo real das operações fiscais

---

## Benefícios da Solução Completa

Ao utilizar o FiscalBox em conjunto com o Portal Veraciti, sua empresa conta com uma plataforma fiscal completa que une:

- Emissão fiscal robusta e automatizada
- Monitoramento centralizado
- Governança e compliance fiscal
- Armazenamento seguro dos documentos
- Gestão remota dos dispositivos
- Inteligência operacional através de dashboards e indicadores

---

## Como iniciar a homologação? Simples, siga os passos abaixo.

## Guia de Integração — Elgin FiscalBox

### 1. Contato com a Equipe Elgin

Entre em contato com nosso time de especialistas pelo canal oficial de WhatsApp:

📱 **+55 11 99851-9559** — selecione a opção 2

Um especialista irá criar seu cadastro no portal Veraciti e gerar seu **Client ID**.

---

### 2. Instalação do Ambiente de Homologação

Não é necessário aguardar o envio do hardware para iniciar o processo de homologação.

A integração pode ser iniciada imediatamente por meio da instalação do SmartDFe, utilizando as requisições direcionadas para o ambiente local conforme abaixo:

Base URL: http://localhost:3001

Dessa forma, é possível realizar todos os testes e validações da integração antecipadamente, garantindo maior agilidade no processo de homologação.

Com o Client ID em mãos, faça o download do instalador:

🔗 [Baixar Instalador](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/tree/master/SmartDFe%20e%20FiscaBox/Instaladores)

Em seguida, siga o tutorial oficial de instalação e ativação em ambiente de homologação:

🔗 [Guia de Instalação do Elgin FiscalBox](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe%5D-Guia-de-Instala%C3%A7%C3%A3o-do-Elgin-SmartDFe)

> **OBS:** Caso já tenha recebido o FiscalBox físico, siga as instruções abaixo:
>
> 1. Ligue o FiscalBox com o cabo USB ao computador ou fonte de 5V.
> 2. Insira o cabo de rede na porta WAN.
> 3. Acesse o Portal Veraciti, vá em **Dispositivo** e clique em **Autorizar**, seguindo o mesmo passo a passo do vídeo acima.

---

### 3. Escolhendo o Tipo de Integração

Com o FiscalBox instalado e pronto para uso, escolha o tipo de integração mais adequado ao seu software. Documentações específicas estão disponíveis para cada modalidade.

O Elgin FiscalBox oferece diferentes modalidades de integração para se adaptar à arquitetura do seu software:

#### Biblioteca — DLL

Integração exclusiva para **Windows**. Oferece suporte completo para emissão, geração de XML, geração de DANFE e impressão.

🔗 [Documentação — Componente DLL](https://elgindevelopercommunity.github.io/group__g11.html)

Para utilizar esta modalidade, baixe a DLL correspondente:

🔗 [Download das Bibliotecas](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/tree/master/SmartDFe/Bibliotecas)

#### API REST (API Direta)

Utilizada para realizar a emissão e acessar os endpoints completos da SEFAZ. Retorna o XML assinado ou o resultado da requisição.

🔗 [Documentação — API Direta (FiscalBox)](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Documenta%C3%A7%C3%A3o-para-integra%C3%A7%C3%A3o)

> Para este tipo de integração, o ERP/PDV deve montar o XML. No link a seguir você encontrará alguns modelos de XML de exemplo:
>
> 🔗 [XML Exemplos](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/tree/master/XML%20Exemplos)

#### Software Exemplo de Integração

Exemplos de integração em diversas linguagens de programação estão disponíveis no repositório oficial:

🔗 [Exemplos de Integração](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/tree/master/SmartDFe%20e%20FiscaBox/Exemplos)

> Caso necessite de ajuda na escolha do modelo, nossa equipe de especialistas está disponível para orientá-lo na definição do tipo de integração e da arquitetura mais adequados às suas necessidades técnicas.

---

### 4. Integrando seu Software

Após escolher o tipo de integração e baixar todo o material de apoio, insira todas as funções presentes na documentação. Para melhor apoiá-lo, utilize como base o roteiro de integração abaixo.

#### Roteiro de Testes e Integração

Durante o processo de homologação, utilize o roteiro de testes para acompanhar a evolução da integração. Ao final, envie o roteiro preenchido ao time Elgin conforme as instruções disponíveis no link abaixo:

🔗 [Roteiro_Homologacao_SmartDFe e FiscalBox.xlsx](https://github.com/user-attachments/files/27258765/Roteiro_Homologacao_SmartDFe.e.FiscalBox.xlsx)


Durante o processo de integração, é imprescindível realizar o tratamento adequado dos retornos da API. Recomenda-se utilizar os campos cStat, xMotivo e data como referência para identificar autorizações, rejeições e demais informações presentes na resposta.

- [Campos JSON disponíveis no retorno do emissor](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe-e-FiscalBox%5D-Retorno-das-requisi%C3%A7%C3%B5es-pelo-SmartDFe-e-FiscalBox)
- [Tabela de retornos Sefaz](https://github.com/ElginDeveloperCommunity/SmartDFeElgin/wiki/%5BSmartDFe%5D-Tabela-de-Retornos)

---

## Suporte

Em caso de dúvidas durante o processo de integração ou homologação, entre em contato com a equipe de especialistas Elgin:

📱 **+55 11 99851-9559** — opção 2
