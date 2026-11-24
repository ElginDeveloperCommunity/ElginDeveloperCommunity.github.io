
**Caminho:** `Empresas` (`/companies`) > `[empresa]` > `Mais ações` > `Excluir`

Procedimento **exclusivo** para retirar um CNPJ da base do parceiro e **encerrar a licença**, para que o CNPJ **deixe de ser cobrado**. Use **somente** quando a empresa saiu definitivamente da base: CNPJ baixado, encerramento de atividade, distrato ou migração.

## Regras do procedimento

| # | Regra |
| :-- | :--- |
| **1** | **Só exclua** CNPJ que saiu definitivamente da base do parceiro. Exclusão não é teste, não é ajuste e não é pausa. |
| **2** | **O backup dos XMLs é obrigatório e vem antes da exclusão.** Sem backup validado, não exclua. |
| **3** | A exclusão **encerra a licença** e a cobrança **dali em diante**. Faturas já emitidas seguem a regra contratual. |
| **4** | **Uma empresa por vez:** faça o backup, confira, exclua e valide. Depois passe para o próximo CNPJ. |
| **5** | Quem executa é o **usuário primário (ADM)** com permissão de **exclusão** em **Empresas**. |
| **6** | A exclusão é **definitiva**. Recadastrar o mesmo CNPJ cria um cadastro novo, **com nova licença e nova cobrança**, e **sem** o histórico anterior. |

> [!WARNING]
> **A exclusão corta o acesso a todos os XMLs do CNPJ** (emitidos, recebidos, eventos, MDF-e/CT-e). A Elgin mantém a guarda pelo **prazo exigido pela legislação**, mas a recuperação é só por **solicitação formal**, sem prazo imediato. **Baixe e valide os XMLs antes de excluir** — depois, o portal não os exibe mais.

---

## 1. Backup em lote (obrigatório antes de excluir)

1. Acesse **Contabilidade > Downloads Avulsos** (`/accounting/requests`).
2. Informe o **período completo**: da **primeira nota** da empresa até **hoje**.
3. Selecione **somente a empresa que será excluída**.
4. Processe o pacote e acompanhe até o status **concluído**.
5. Baixe o arquivo e confira os XMLs de cada nota e seus eventos.
6. Repita o processo para **MDF-e / CT-e**, se houver.
7. Salve em local seguro, **fora do portal** (servidor, nuvem corporativa ou mídia de backup).

> [!TIP]
> Pacotes muito grandes podem demorar a processar. Se necessário, divida o backup em **faixas de período** (por ano ou por semestre) e baixe cada faixa separadamente — juntas, elas devem cobrir o período integral.

**Confira antes de seguir** — os 4 itens precisam estar marcados:

- [ ] O download cobre da **primeira nota** até hoje, no ambiente de **produção**.
- [ ] Inclui **notas recebidas** (emitidas por fornecedores contra o CNPJ) e **eventos** (cancelamentos, inutilizações, manifestações).
- [ ] Inclui **MDF-e / CT-e**, se a empresa operar transporte.
- [ ] Backup salvo em **local seguro**, fora do portal.

> [!IMPORTANT]
> O XML é o documento fiscal válido. A Elgin guarda os documentos pelo **prazo exigido pela legislação**. Em caso de **exclusão da empresa**, a orientação é que você guarde a **sua própria cópia** dos XMLs pelo prazo legal: o acesso pelo portal é encerrado, e a sua cópia garante a consulta imediata sempre que precisar.

---

## 2. Checklist antes de excluir

- [ ] Empresa **confirmada fora da base** (motivo registrado).
- [ ] Backup **completo e validado** (seção 1).

> [!CAUTION]
> Se qualquer item acima estiver desmarcado, **pare**. Não é possível desfazer a exclusão.

---

## 3. Excluir

1. **Empresas** (`/companies`) > localize e abra a empresa (nome, ícone de olho ou **Ver detalhes** no mapa).
2. Confira o **CNPJ** em **Informações Gerais** — precisa ser exatamente o CNPJ do backup.
3. **Mais ações** > **Excluir** > confirme.
4. Volte à lista e verifique que o CNPJ **não aparece mais**.

> [!NOTE]
> A opção **Excluir** depende do perfil: na prática, apenas **usuário primário (ADM)** com permissão de exclusão em **Empresas**.


<img width="800" alt="image" src="https://github.com/user-attachments/assets/add532fe-941f-458f-b926-35ea04ebdc16" />


---

## 4. Depois da exclusão

- **Licença/cobrança:** encerrada — o CNPJ deixa de ser cobrado. Confirme com o comercial/financeiro. **Faturas já emitidas** seguem a regra contratual.
- **Comprovação:** arquive o **backup** e o **comprovante de conclusão do download**. É essa documentação que sustenta a solicitação de encerramento da cobrança.
- **Documentos:** sem acesso pelo portal; recuperação só por solicitação formal à Elgin.
- **Dispositivos:** são **excluídos junto com a empresa** — não é necessário revincular nem desabilitar antes.
- **Grupos:** são **excluídos junto com a empresa** — não é necessário revincular nem desabilitar antes.
- **Recadastro:** cadastro novo, com **nova licença e nova cobrança**; o histórico anterior não volta ao portal.

---

## 5. Perguntas frequentes

**A exclusão interrompe a cobrança da licença?**
Sim — encerra a licença do CNPJ e a cobrança dali em diante. Se a empresa apenas parar de usar e continuar cadastrada, a licença continua ativa e cobrada. Faturas já emitidas seguem a regra contratual.

**Quando devo excluir?**
Quando a empresa **não faz mais parte da base do parceiro**: CNPJ baixado, encerramento de atividade, distrato ou migração.

**A Elgin guarda as notas depois da exclusão?**
Sim, pelo **prazo exigido pela legislação**. A recuperação é por **solicitação formal**, com CNPJ, período e motivo, e não é imediata — por isso a orientação é guardar a **sua própria cópia** dos XMLs antes de excluir.

**Posso recadastrar com o mesmo CNPJ?**
Sim, como cadastro novo (sem histórico), com nova licença e nova cobrança.

**Preciso tratar os dispositivos antes?**
Não. Eles **são excluídos junto com a empresa**.

**Que perfil pode excluir?**
Somente **usuário primário (ADM)** com permissão de **exclusão** em **Empresas**.

---

