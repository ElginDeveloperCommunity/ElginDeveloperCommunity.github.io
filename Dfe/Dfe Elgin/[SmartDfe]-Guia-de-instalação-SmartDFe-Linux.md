> Documento de instrução para instalação do SmartDFe no Linux
> Aplica-se a: Linux **x86_64**, com **Systemd** e **GLIBC 2.17 ou superior**

## Visão Geral

A implantação do SmartDFe no Linux é dividida em três etapas:

1. **Verificação dos pré-requisitos** do equipamento.
2. **Instalação** do pacote no PDV via terminal.
3. **Autorização e configuração** do dispositivo no portal **Veraciti**.

Ao final, o dispositivo estará autorizado e configurado, e o SmartDFe ficará pronto para emissão de documentos fiscais.

---

## 1. Pré-requisitos

Antes de iniciar a instalação, verifique se o equipamento atende aos requisitos mínimos de compatibilidade.

Execute os comandos abaixo no terminal Linux:

| Verificação | Comando | Requisito |
|---|---|---|
| Arquitetura | `uname -m` | `x86_64` |
| Gerenciador de serviços | `systemctl --version` | Systemd instalado |
| Versão da GLIBC | `ldd --version` | 2.17 ou superior |

> **Importante:** caso algum requisito não seja atendido, interrompa a instalação e verifique a compatibilidade do sistema operacional.

---

## 2. Instalação no PDV

Após confirmar os pré-requisitos, localize o pacote de instalação e execute os comandos abaixo **na sequência apresentada**, respeitando os diretórios indicados.

### Passo 1 — Extrair o pacote

```bash
tar -xzf smartdfe-box-linux-x64-20261112-2031.tar.gz
```

### Passo 2 — Acessar o diretório de instalação

```bash
cd smartdfe-linux-x64-20261112-2031
```

### Passo 3 — Executar o instalador

```bash
sudo ./instalar.sh --client-id=SEU_CLIENT_ID
```

O `Client ID` é fornecido pela **Elgin**. Substitua `SEU_CLIENT_ID` pelo identificador informado pela equipe Elgin para o cliente.

Após executar o instalador, aguarde a conclusão do processo.

### Serviços criados pela instalação

O instalador registra três serviços no **Systemd**:

| Serviço | Função |
|---|---|
| `smartnfce-emissor` | Emissor de NFC-e. |
| `smartnfce-contingencia` | Emissor em contingência. |
| `smartdfehub` | Comunicação do SmartDFe Hub. |

Para verificar se os serviços estão ativos, execute:

```bash
systemctl status smartnfce-emissor smartnfce-contingencia smartdfehub
```

> ✅ **Instalação no PDV concluída.** Prossiga para a etapa de autorização e configuração via portal **Veraciti**.

---

## 3. Configuração no Portal Veraciti

### 3.1 Autorização do dispositivo

1. Acesse o portal **Veraciti** em [https://console.veraciti.com.br/](https://console.veraciti.com.br/) — ou pela URL configurada para a sua empresa, caso utilize o modelo **white label**.
2. Navegue até a aba **Dispositivos**.
3. Clique em **Autorizar** no dispositivo desejado.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/40b0ad1f-4a8c-4cad-b681-affc73758814" />

4. Selecione se é uma **empresa existente** ou uma **nova empresa**. Em seguida:
   - Faça o upload do **certificado digital**.
   - Preencha a **senha** do certificado.
   - Clique em **Próximo**.
   - Nesta etapa, se for uma nova empresa, também será solicitada a **Inscrição Estadual** e o **Regime Tributário**.

<img width="600" alt="Upload do certificado digital e dados da empresa" src="https://github.com/user-attachments/assets/b8f1a21d-286e-49f7-a344-7942bbac32f6" />

5. Informe o número de série que será utilizado para emissão:
   - Para manter uma série já em uso, insira o **último número emitido** de NF-e ou NFC-e.
   - Para iniciar uma nova série, deixe o campo **em branco**.
   - Clique em **Próximo**.

<img width="600" alt="Informação do número de série para emissão" src="https://github.com/user-attachments/assets/50da8b81-9345-430b-9658-e2153b407f10" />

6. Escolha o **ambiente de emissão**:
   - **Produção** — para uso em ambiente do cliente (emissão real).
   - **Homologação** — para testes e desenvolvimento.

<img width="600" alt="Seleção do ambiente de emissão" src="https://github.com/user-attachments/assets/4239ffd7-d022-4fb0-8e47-648d0dfdaa44" />

7. Clique em **Ativar e Vincular** para concluir. O comando de configuração será enviado ao dispositivo.

---

### 3.2 Acompanhamento do status do dispositivo

Para verificar o resultado da configuração:

1. Clique no dispositivo configurado.
2. Role até o final da página, na seção **Histórico de Comandos**.
3. Verifique o comando enviado e seu respectivo status:

| Status | Descrição |
|---|---|
| **Pendente** | Aguardando o emissor receber o comando. |
| **Confirmado** | Emissor recepcionou o comando com sucesso. |
| **Executando** | Comando em processo de execução. |
| **Sucesso** | Comando concluído com êxito. |

Quando o status estiver como **Sucesso**, o software estará pronto para uso.

<img width="600" alt="Histórico de comandos e status do dispositivo" src="https://github.com/user-attachments/assets/f31af131-deb6-4a8e-8b58-444df5c5ea5a" />

---

## 4. Conclusão

Pronto. O SmartDFe está instalado e configurado, e você já pode realizar a emissão de documentos fiscais.

---

## 5. Suporte

Em caso de dúvidas durante o processo de instalação, entre em contato com a equipe de especialistas Elgin.

Em caso de erros durante a instalação, encaminhe as mensagens apresentadas no terminal para a equipe de suporte SmartDFe.