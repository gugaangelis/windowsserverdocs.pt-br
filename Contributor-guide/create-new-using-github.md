---
title: Criar novos artigos do Windows Server usando o GitHub e Visual Studio Code
description: Como criar novos artigos relacionados ao Windows Server, usando o GitHub e Visual Studio Code, como um funcionário da Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d75bf135266a4783ab2617977b344782cea679ef
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624567"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>Criar novos artigos do Windows Server usando o GitHub e Visual Studio Code

Há dois locais separados, onde podemos manter conteúdo técnico do Windows Server. Um dos locais é público (windowsserverdocs) enquanto a outra é privada (pr windowsserverdocs). Quem é você determina qual local contribuem para:

- **Eu sou um funcionário da Microsoft.** Como funcionário da Microsoft, você tem opções, com base no qual você está tentando fazer:

    - **Crie um artigo inteiramente novo.** Para criar um novo artigo, você deve criar e configurar sua conta do GitHub e ferramentas, bifurcar e clonar o repositório windowsserverdocs-pr, configurar seu branch remoto, criar o artigo e, finalmente, crie uma nova solicitação de pull para aprovação e publicação. Para obter essas instruções, continue lendo este artigo.

    - **Fazer grandes alterações em um artigo existente.** Para fazer alterações significativas em um artigo existente, você pode seguir as instruções de [editar um artigo existente do Windows Server usando o GitHub e Visual Studio Code](edit-existing-using-github.md) artigo.

    - **Fazer pequenas alterações em um artigo existente.** Para fazer pequenas alterações em um artigo existente, você pode seguir as instruções de [atualizam os artigos existentes do Windows Server usando um navegador da web e o GitHub](github-browser-updates.md) artigo.

- **Eu não sou um funcionário da Microsoft.** Como um funcionário não são da Microsoft, você deve contribuir para o local público. Para obter informações sobre como fazer isso, consulte a [contribuindo para a documentação técnica do Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) artigo.

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar a trabalhar no repositório, você deve criar e configurar sua conta do GitHub, configurar a verificação de dois fatores e instalar e configurar todas as ferramentas necessárias. Se você tiver feito isso, você pode pular para a [bifurcar a seção de repositório](#fork-the-repository) deste artigo.

1. [Criar um perfil e conta do GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [Vincular sua conta de sua conta da Microsoft e as organizações da Microsoft e MicrosoftDocs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [Ativar a verificação de dois fatores em](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [Autorizar o sistema de compilação para acessar sua conta do GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [Instalar o Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [Instale o GitHub e suas ferramentas](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [Instalar o pacote de criação do Docs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>Configurar sua própria versão do repositório

Depois que você criou e configurar sua conta do GitHub e ferramentas, você pode criar uma versão pessoal do repositório. Isso é onde você criará suas ramificações e fazer todas as suas alterações.

### <a name="fork-the-repository"></a>Divida o repositório

Você precisa de uma cópia local dos arquivos de origem, portanto, você pode criar solicitações de pull da bifurcação para o repositório de produção.

#### <a name="to-fork-the-repository"></a>Para criar fork do repositório

1. Entre sua conta do GitHub e vá para https://github.com/microsoftdocs/windowsserverdocs-pr.

2. Selecione **bifurcação**.

    ![Botão de bifurcação realçado na página](media/create-new-using-github/fork-button.png)

3. Selecione sua conta do GitHub como o local de bifurcação.

    ![Botão de bifurcação realçado na página](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>Clone o repositório

Você precisa clonar uma cópia local do repositório logon em seu dispositivo local do get de repositório.

#### <a name="to-clone-the-repository"></a>Para clonar o repositório

1. Vá para https://github.com/settings/developerse, em seguida, selecione **tokens de acesso pessoal** no painel esquerdo.

2. Selecione **gerar novo token**, dê um nome exclusivo e significativo ao seu token, selecione todos os escopos disponíveis e, em seguida, selecione **gerar token**.

3. Copie o token e colocá-lo em um lugar seguro. Você precisará dela para o restante do processo e depois que você sair da página, você não poderá voltar a ela.

4. Abra um comando do Git Bash e altere os diretórios para onde você deseja armazenar seu repositório. Recomendamos o uso, `C:\users\<your_name>\GitHub`.

5. Digite os comandos a seguir usando as informações específicas, um de cada vez, clonar seu repositório e configurar seus branches remotos:

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. Execute este comando para verificar se que as referências remotas está configurada corretamente:

    `git remote -v`

7. Você deverá ver algo parecido com esta saída:

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    Se sua saída remota não ter esta aparência, você pode tentar novamente pela primeira execução `git remote remove upstream`.

## <a name="create-a-branch-and-a-new-article"></a>Criar uma ramificação e um novo artigo

Siga estas etapas para criar um artigo.

### <a name="create-a-new-branch-and-a-new-file"></a>Criar uma nova ramificação e um novo arquivo

Antes de você pode começar a trabalhar em seu conteúdo, você deve criar um novo branch no repositório local.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Para criar um novo branch no Git Bash

- Abra o Git Bash e digite os comandos (um por vez):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >É altamente recomendável nomear sua ramificação algo óbvio e exclusivo para que possa localizá-lo novamente mais tarde.

    Depois de concluir os comandos, você usará em seu novo branch e pronto para criar o novo arquivo. Você só precisará alterar para o repositório de pr windowsserverdocs uma vez por instância de seu Git Bash. Se você fechar o Git Bash, você precisará alterar diretórios novamente depois de abri-lo.

#### <a name="to-create-a-new-file-in-your-branch"></a>Para criar um novo arquivo na sua ramificação

1. Abra o Windows Explorer, vá para seu diretório do GitHub e criar um novo arquivo de texto na estrutura de pastas. O nome do arquivo deve ser todas as letras minúsculas e são separados por hifens. Por exemplo, _what-for-windows-server.md_.

     Você também deve alterar a extensão de arquivo de. txt para. md. Na mensagem de erro que aparece, selecione **Sim** para salvar o arquivo com a nova extensão de arquivo.

2. Abra o Visual Studio Code e vá para **arquivo**, selecione **Abrir pasta**e, em seguida, vá para o local do GitHub do arquivo criado na etapa 1.

3. Dos **Explorer** painel, selecione o novo arquivo.

4. Adicionar texto à página e, em seguida, selecione **arquivo** > **salvar**.

### <a name="preview-your-text"></a>Visualizar seu texto

Depois de adicionar o texto para o novo arquivo, você deve visualizar as alterações para garantir que eles sejam exibidos corretamente.

#### <a name="to-preview-your-text"></a>Para visualizar seu texto

1. No Visual Studio Code, selecione qualquer uma da **visualização** botões no canto superior direito.

    ![ícone do botão de visualização](media/create-new-using-github/preview-button-full-page.png): Alterna para uma visualização de página inteira do seu conteúdo.

    ![ícone do botão de visualização](media/create-new-using-github/preview-button-side-by-side.png): Abre a página de visualização ao lado de sua página de trabalho, lado a lado.

2. Verifique se que o artigo aborda como você espera que ela para procurar.

    Depois que você tiver certeza se parece certo, você pode confirmar suas alterações e criar uma solicitação de pull para publicação.

### <a name="commit-your-changes"></a>Confirme suas alterações

Depois de verificar se que o texto está correto, você pode confirmar suas alterações para a versão local do seu repositório.

#### <a name="to-commit-your-changes"></a>Para confirmar as alterações

- Abra o Git Bash e digite os comandos (um por vez, removendo as marcas opcionais):

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    O comando de status do git opcional mostra quais arquivos foram modificados como parte dessa confirmação. O opcional git pull pulls de mestres de upstream para baixo as alterações de conteúdo mais recente do MicrosoftDocs branch mestre, sincronizando o conteúdo local com o conteúdo mestre primário. Isso ajuda a mostrar possíveis conflitos de mesclagem antecipadamente para que possa corrigi-los antes de chegar ao estágio de PR.

### <a name="submit-a-pull-request-for-review-and-publication"></a>Enviar uma solicitação de pull para análise e publicação

Depois de concluir seu artigo, você deve obter aprovação do seu gravador (permitir algum tempo para isso) para publicação.

#### <a name="to-submit-your-pull-request"></a>Para enviar sua solicitação de pull

1. Vá para https://github.com/MicrosoftDocs/windowsserverdocs-pr e selecione o **solicitações Pull** guia.

2. No **revisores** área do painel direito, selecione o ícone de engrenagem e, em seguida, insira o _windowsservercontent_ alias para revisão.

    Um membro do _windowsservercontent_ alias irá examinar suas alterações ou adicionar comentários sobre as coisas que devem ser alterados antes de mesclar pode acontecer.

3. Tipo de **#sign-off** em comentários para que os revisores saibam estiver entregando para revisão e publicação. O **#sign-off** comentário:

    - Atualiza o rótulo para a sua solicitação de pull de **fazer não mesclar** à **pronto para mesclagem**.

    - Permite que o alias e gravadores de saber o que você está pronto para ter seu conteúdo revisado.

    - Permite que os administradores Saiba que, após a aprovação, seu conteúdo está pronto ir ao vivo.

    >[!Important]
    >Depois de adicionar o comentário #sign-off, um membro da equipe windowsservercontent Revise o texto e enviá-los para dominar, portanto, ele sairá com o próximo agendado publicar vivos (10h30 e dias da semana de 3:30 pm).
    >
    >Se você não adicionar #sign-off como um comentário de final na sua PR, seu conteúdo permanecerão na fila sem que está sendo enviada por push ao mestre e, finalmente, útil.

## <a name="related-information"></a>Informações relacionadas

Para obter mais informações sobre o GitHub e a linguagem markdown, consulte:

### <a name="git-concepts"></a>Conceitos do Git

- [Introdução do manual Git guias do GitHub](https://guides.github.com/introduction/git-handbook/)

- [Projetos de bifurcação de guias do GitHub](https://guides.github.com/activities/forking/)

- [Guias do GitHub-Compreendendo o fluxo do GitHub](https://guides.github.com/introduction/flow/)

- [Aprenda a ramificação de Git](https://learngitbranching.js.org/ (excelente para aprendizes visual!))

### <a name="markdown"></a>Markdown

- [Nossas diretrizes de markdown interno](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [Tutorial do GitHub externo,](https://www.markdowntutorial.com/)
