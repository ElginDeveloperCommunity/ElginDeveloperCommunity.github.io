# Instalação e Configuração do Elgin SmartDFe

Este guia descreve o processo completo de instalação do **Elgin SmartDFe** no PDV (Ponto de Venda) e sua configuração no portal **Veraciti**.

---

## ⚠️ Pré-requisitos

### 1. Computador com internet (obrigatório)

É necessário um computador com acesso à internet para instalar e utilizar o **SmartDFe**.

### 2. Hardware recomendável e Sistema Operacional

**Hardware recomendável:**
- Processador: Intel Core i3 ou equivalente, 2 núcleos, 2.0 GHz ou superior
- Memória RAM: **4 GB mínimo** (8 GB recomendado)
- Armazenamento: **10 GB livres em disco**

**Sistema Operacional:**
- Recomendado: **Windows 10 ou superior (64 bits)**
- Funciona também no Windows 7 e 8/8.1, desde que atualizados e com o C++ Redistribuível compatível instalado

### 3. Dados fiscais

Antes de iniciar, certifique-se de ter em mãos os seguintes itens:

- Certificado digital do cliente (modelo A1)
- Inscrição Estadual (IE)
- Regime Tributário
- Credenciamento ativo para emissão de **NFC-e** na **SEFAZ** do seu estado

---

## 1. Instalação Local (PDV)

1. Após baixar o instalador do **SmartDFe** no repositório do [GitHub da Elgin]

(https://github.com/ElginDeveloperCommunity/SmartDFeElgin/tree/master/SmartDFe%20e%20FiscaBox/Instaladores), localize o arquivo no computador.

2. Clique com o botão direito do mouse sobre o instalador e selecione **"Executar como administrador"**.


<img width="600" alt="image" src="https://github.com/user-attachments/assets/044f4ee4-4d32-4e4a-8ebf-20fbf74507d0" /> 
<br>
<br>

3. A tela de instalação será exibida. Leia os termos de uso, aceite-os e clique em **Avançar**.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/431c78e7-4a5e-4d05-945f-d19ee3ed5569" />
<br>
<br>

4. Na tela seguinte, serão exibidas três opções de instalação. Selecione **Emissor + Contingência** e clique em **Avançar**.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/3969fdf4-1471-4504-bab6-28cee6527032" />
<br>
<br>

5. Nessa etapa:
   - Insira o **Client ID** fornecido pela Elgin para sua empresa.
   - Clique em **Avançar**. E espere a barra de progresso carregar até o final.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/e006cb93-b78b-4fd4-b035-6c99c4963a90" />
<br>
<br>

6. Confirme novamente o **Client ID** quando aparecer a tela e clique em **Vincular dispositivo**.
<img width="600" alt="image" src="https://github.com/user-attachments/assets/3d2d2451-e8a3-4476-a52a-64c5cdf0d974" />
<br>
<br>

7. Ao gerar o código, um pedido de autorização irá para o portal Veraciti. Você deve seguir o próximo passo para autorizar e configurar.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/d9e0f445-0188-4605-aab1-c629e62497d8" />
<br>
<br>


> ✅ A instalação no PDV está concluída. Prossiga para a etapa de autorização e configuração via portal **Veraciti**.

---

## 2. Configuração no Portal Veraciti

### 2.1 Autorização do Dispositivo

1. Acesse o portal **Veraciti**.

2. Navegue até a aba **Dispositivos**.

3. Clique em **Autorizar** no dispositivo desejado.

<img width="600" alt="7" src="https://github.com/user-attachments/assets/ce81498f-176b-4abc-ad7c-8f256c780175" />



4. Selecione se é uma **empresa existente** ou uma **nova empresa**. Em seguida:
   - Faça o upload do **certificado digital**.
   - Preencha a **senha** do certificado.
   - Clique em **Próximo**.
   - Nessa etapa, se for uma nova empresa, irá solicitar também a Inscrição Estadual e o Regime Tributário.


<img width="600" alt="image" src="https://github.com/user-attachments/assets/b8f1a21d-286e-49f7-a344-7942bbac32f6" />
<br>
<br>

5. Informe o número de série que será utilizado para emissão:
   - Para manter uma série já em uso, insira o **último número emitido** de NF-e ou NFC-e.
   - Para iniciar uma nova série, deixe o campo **em branco**.
   - Clique em **Próximo**.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/50da8b81-9345-430b-9658-e2153b407f10" />
<br>
<br>

6. Escolha o **ambiente de emissão**:
   - **Produção** — para uso em ambiente do cliente (emissão real).
   - **Homologação** — para testes e desenvolvimento.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/4239ffd7-d022-4fb0-8e47-648d0dfdaa44" />
<br>
<br>

7. Clique em **Ativar e Vincular** para concluir. O comando de configuração será enviado ao dispositivo.

---

### 2.2 Acompanhamento do Status do Dispositivo

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

Quando o status estiver como "Sucesso" o software estará pronto para uso.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/f31af131-deb6-4a8e-8b58-444df5c5ea5a" />
<br>
<br>

---

**PDV**: A tela do PDV receberá uma confirmação de configuração concluída. Nessa parte, você deve clicar para confirmar e concluir conforme as imagens abaixo.

<img width="600" alt="image" src="https://github.com/user-attachments/assets/3ccc7f20-05ac-40d7-87c2-2cbd42aa1ae7" />
<br>
<br>


<img width="600" alt="image" src="https://github.com/user-attachments/assets/504826df-a07a-4f6c-859b-9e22bd637665" />
<br>
<br>

Pronto, agora seu SmartDFe está instalado e configurado. Você pode realizar a emissão de documentos fiscais.


### Vídeo tutorial de instalação

https://github.com/user-attachments/assets/2468f58c-91da-45b9-8709-0d522ec3494d

---

Suporte
Em caso de dúvidas durante o processo de instalação, entre em contato com a equipe de especialistas Elgin:

📱 +55 11 99851-9559 — opção 1 
