# <a name="contributing-to-windows-server-technical-documentation"></a>Contribuindo para a documentação técnica do Windows Server

Obrigado por seu interesse na documentação técnica do Windows Server! Agradecemos seus comentários, edições e adições para nossos documentos. Há dois locais separados, onde podemos manter conteúdo técnico do Windows Server. Um dos locais é público (windowsserverdocs) enquanto a outra é privada (pr windowsserverdocs). Quem é você determina qual local contribuem para:

- **Eu não sou um funcionário da Microsoft.** Como um funcionário não são da Microsoft, você deve contribuir para o local público. Para obter informações sobre como fazer isso, continue lendo este artigo.

- **Eu sou um funcionário da Microsoft.** Como funcionário da Microsoft, você tem opções, com base no qual você está tentando fazer:

    - **Crie um artigo inteiramente novo.** Para criar um novo artigo, você deve criar e configurar sua conta do GitHub e ferramentas, bifurcar e clonar o repositório windowsserverdocs-pr, configurar seu branch remoto, criar o artigo e, finalmente, crie uma nova solicitação de pull para aprovação e publicação. Para essas instruções, consulte a [criar novos artigos do Windows Server usando o GitHub e Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/create-new-using-github.md) artigo.

    - **Fazer grandes alterações em um artigo existente.** Para fazer alterações significativas em um artigo existente, você pode seguir as instruções de [editar um artigo existente do Windows Server usando o GitHub e Visual Studio Code](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/edit-existing-using-github.md) artigo.

    - **Fazer pequenas alterações em um artigo existente.** Para fazer pequenas alterações em um artigo existente, você pode seguir as instruções de [atualizam os artigos existentes do Windows Server usando um navegador da web e o GitHub](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/Contributor-guide/github-browser-updates.md) artigo.

## <a name="sign-a-cla"></a>Assine um CLA

Todos os colaboradores que estão ***não*** um funcionário da Microsoft deve [assinar um contrato de licenciamento de contribuição (CLA) do Microsoft](https://cla.microsoft.com/) antes de editar qualquer repositórios da Microsoft. Se você já tiver editado em repositórios da Microsoft no passado, Parabéns!
Você já concluiu essa etapa.

## <a name="editing-topics"></a>Tópicos de edição

Tentamos tornar a edição de um arquivo existente e público mais simples possível.

### <a name="to-edit-a-topic"></a>Para editar um tópico

1. Vá para a página sobre https://docs.microsoft.com/windows-server que você deseja atualizar e, em seguida, selecione **editar**.

    ![Web do GitHub, mostrando o link de edição](media/contribute-link.png)

2. Entrar para (ou se inscrever para) uma conta do GitHub.

    Você deve ter uma conta do GitHub para chegar à página que permite que você edite um tópico.

3. Selecione o **Lápis** ícone (na caixa vermelha) para editar o conteúdo.

    ![Web do GitHub, mostrando o ícone de lápis na caixa vermelha](media/pencil-icon.png)

4. Usando a linguagem Markdown, faça as alterações para o tópico. Para obter informações sobre como editar o conteúdo usando o Markdown, consulte:

    - **Se você estiver vinculado à organização da Microsoft no GitHub:** [Guia do Colaborador do Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/Contributor-guide)

    - **Se você estiver fora da Microsoft:** [Dominando o Markdown](https://guides.github.com/features/mastering-markdown/)

5. Faça a alteração sugerida e, em seguida, selecione **visualizar alterações** para verificar se elas parecerem corretas.

    ![Web do GitHub, mostrando a guia Visualizar alterações](media/preview-changes.png)

6. Quando você terminar o tópico de edição, role até a parte inferior da página, digite um nome descritivo para sua bifurcação e, em seguida, clique em **propor alteração de arquivo** para criar a bifurcação em sua conta pessoal do GitHub.

    ![Web do GitHub, mostrando o botão de alteração de arquivo propor](media/propose-file-change.png)

    O **comparando alterações** tela é exibida visualizar quais são as alterações entre a bifurcação e o conteúdo original.

7. Sobre o **comparando alterações** tela, você verá se houver problemas com o arquivo que você está fazendo check-in.

    Se não houver nenhum problema, você verá a mensagem **capaz de mesclar**.

    ![Web do GitHub, mostrar o comparando altera a tela](media/compare-changes.png)

8. Selecione **criar solicitação de pull**.

    Solicitações de pull permitem que você informar aos outros sobre as alterações que você já enviou por push para uma ramificação em um repositório no GitHub. Depois que uma solicitação de pull for aberta, você pode discutir e revisar as alterações potenciais com colaboradores e adicionar acompanhamento confirmações antes que as alterações sejam mescladas no branch de base. Para obter mais informações, consulte [sobre solicitações de pull](https://help.github.com/articles/about-pull-requests)

9. Insira um título e uma descrição para fornecer o aprovador do contexto apropriado sobre as novidades na solicitação.

10. Role até a parte inferior da página, certificando-se de que somente os arquivos alterados são nesta solicitação pull. Caso contrário, você poderia substituir as alterações de outras pessoas.

11. Selecione **criar solicitação de pull** novamente para realmente enviar a solicitação de pull.

    A solicitação de pull é enviada para o gravador do tópico e suas edições são revisadas. Se sua solicitação for aceita, as atualizações são publicadas.

## <a name="resources"></a>Recursos

- Você pode usar seu editor de texto favorito para editar Markdown. É recomendável [Visual Studio Code](https://code.visualstudio.com/), um editor leve de software livre gratuita da Microsoft.