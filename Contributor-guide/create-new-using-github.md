---
title: Criar novos artigos do Windows Server usando o GitHub e o Visual Studio Code
description: Como criar novos artigos relacionados ao Windows Server, usando o GitHub e o Visual Studio Code, como um funcionário da Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: f5e7e3d0cd17c64175fddaaac73c12daa2c2a32c
ms.sourcegitcommit: ffd9c42374c7448deb5f53f7a865cb427b5e4e9e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887954"
---
# <a name="create-new-windows-server-articles-using-github-and-visual-studio-code"></a>Criar novos artigos do Windows Server usando o GitHub e o Visual Studio Code

Há dois locais separados onde mantemos o conteúdo técnico do Windows Server. Um dos locais é público (windowsserverdocs) enquanto o outro é privado (windowsserverdocs-PR). Quem você está determina em qual local você contribui:

- **Sou um funcionário da Microsoft.** Como funcionário da Microsoft, você tem opções, com base no que está tentando fazer:

    - **Crie um artigo novo.** Para criar um novo artigo, você deve criar e configurar sua conta e ferramentas do GitHub, bifurcar e clonar o repositório windowsserverdocs-PR, configurar sua ramificação remota, criar o artigo e, finalmente, criar uma nova solicitação de pull para aprovação e publicação. Para essas instruções, continue lendo este artigo.

    - **Faça grandes alterações em um artigo existente.** Para fazer alterações substanciais em um artigo existente, você pode seguir as instruções no artigo [Editar um servidor do Windows existente usando o GitHub e Visual Studio Code](edit-existing-using-github.md) .

    - **Faça alterações secundárias em um artigo existente.** Para fazer alterações secundárias em um artigo existente, você pode seguir as instruções contidas no artigo [atualizar artigos existentes do Windows Server usando um navegador da Web e o GitHub](github-browser-updates.md) .

- **Não sou um funcionário da Microsoft.** Como funcionário que não seja da Microsoft, você deve contribuir para o local público. Para obter informações sobre como fazer isso, consulte o artigo [contribuindo para a documentação técnica do Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) .

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar a trabalhar no repositório, você deve criar e configurar sua conta do GitHub, configurar a verificação de dois fatores e instalar e configurar todas as ferramentas necessárias. Se você já fez isso, pode pular para a [seção bifurcar o repositório](#fork-the-repository) deste artigo.

1. [Criar uma conta e um perfil do GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#create-a-github-account-and-set-up-your-profile)

2. [Vincule sua conta ao seu conta Microsoft e às organizações Microsoft e MicrosoftDocs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#link-your-github-and-microsoft-accounts)

3. [Ativar a verificação de dois fatores](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#enable-two-factor-authentication-and-create-an-access-token)

4. [Autorizar o sistema de compilação a acessar sua conta do GitHub](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-github?branch=master#authorize-the-ops-build-system-to-access-your-github-account)

5. [Instalar Visual Studio Code](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-visual-studio-code)

6. [Instalar o GitHub e suas ferramentas](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-git-client-tools)

7. [Instalar o pacote de criação do docs](https://review.docs.microsoft.com/help/contribute/contribute-get-started-setup-tools?branch=master#install-the-docs-authoring-pack)

## <a name="set-up-your-own-version-of-the-repo"></a>Configurar sua própria versão do repositório

Depois de criar e configurar sua conta e ferramentas do GitHub, você pode criar uma versão pessoal do seu repositório. É aqui que você criará seus branches e fará todas as alterações.

### <a name="fork-the-repository"></a>Divida o repositório

Você precisa de uma cópia local dos arquivos de origem, para que possa criar solicitações de pull de sua bifurcação para o repositório de produção.

#### <a name="to-fork-the-repository"></a>Para bifurcar o repositório

1. Entre em sua conta do GitHub e vá para https://github.com/microsoftdocs/windowsserverdocs-pr.

2. Selecione **bifurcação**.

    ![Botão de bifurcação realçado na página](media/create-new-using-github/fork-button.png)

3. Selecione sua conta do GitHub como o local de bifurcação.

    ![Botão de bifurcação realçado na página](media/create-new-using-github/fork-location.png)

### <a name="clone-the-repository"></a>Clone o repositório

Você precisa clonar o repositório para obter uma cópia local do repositório em seu dispositivo local.

#### <a name="to-clone-the-repository"></a>Para clonar o repositório

1. Vá para https://github.com/settings/developers e selecione tokens de **acesso pessoal** no painel esquerdo.

2. Selecione **gerar novo token**, dê ao token um nome significativo e exclusivo, selecione todos os escopos disponíveis e, em seguida, selecione **gerar token**.

3. Copie o token e coloque-o em algum lugar seguro. Você precisará disso para o restante do processo e, depois de sair da página, não conseguirá voltar a ele.

4. Abra um comando do git bash e altere os diretórios para onde você deseja armazenar o repositório. É recomendável usar `C:\users\<your_name>\GitHub`o,.

5. Digite os comandos a seguir usando suas informações específicas, uma de cada vez, para clonar seu repositório e configurar suas ramificações remotas:

    ```markdown

    git clone https://<your_github_username>:<your_personal_access_token>@github.com/<your_github_username>/windowsserverdocs-pr.git

    cd windowsserverdocs-pr

    git remote add upstream https://<your_github_username>:<your_personal_access_token>@github.com/MicrosoftDocs/windowsserverdocs-pr.git

    git fetch upstream master
    ```

6. Execute este comando para verificar se o seu remoto está configurado corretamente:

    `git remote -v`

7. Você verá algo semelhante a esta saída:

    ```markdown
    $ git remote -v

    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (fetch)
    origin https://github.com/<your_github_username>/windowsserverdocs-pr.git (push)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (fetch)
    upstream https://github.com/MicrosoftDocs/windowsserverdocs-pr.git (push)
    ```

    Se a saída remota não tiver esta aparência, você poderá tentar novamente executando `git remote remove upstream`a primeira vez.

## <a name="create-a-branch-and-a-new-article"></a>Criar uma ramificação e um novo artigo

Siga estas etapas para criar um artigo.

### <a name="create-a-new-branch-and-a-new-file"></a>Criar uma nova ramificação e um novo arquivo

Antes de começar a trabalhar em seu conteúdo, você deve criar uma nova ramificação em seu repositório local.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Para criar uma nova ramificação no git bash

- Abra o Git bash e digite os comandos (um de cada vez):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >É altamente recomendável nomear seu Branch como algo óbvio e exclusivo para que você possa encontrá-lo novamente mais tarde.

    Após a conclusão dos comandos, você estará em seu novo Branch e estará pronto para criar o novo arquivo. Você só precisa alterar para o repositório windowsserverdocs-PR uma vez por instância de seu git bash. Se você fechar o Git bash, será necessário alterar os diretórios novamente depois de abri-lo.

#### <a name="to-create-a-new-file-in-your-branch"></a>Para criar um novo arquivo em sua ramificação

1. Abra o Windows Explorer, vá para o diretório do GitHub e crie um novo arquivo de texto na estrutura de pastas. O nome do arquivo deve estar em letras minúsculas e separados por hifens. Por exemplo, _What-is-Windows-Server.MD_.

     Você também deve alterar a extensão de arquivo de. txt para. MD. Na mensagem de erro exibida, selecione **Sim** para salvar o arquivo com a nova extensão de arquivo.

2. Abra Visual Studio Code e vá para **arquivo**, selecione **abrir pasta**e, em seguida, vá para o local do GitHub do arquivo criado na etapa 1.

3. No painel do **Explorer** , selecione o novo arquivo.

4. Adicione o texto à página. Se você estiver criando um novo artigo, certifique-se [de adicionar as marcas de metadados necessárias ao seu artigo relacionado ao Windows Server](metadata-requirements-for-articles.md).

5. Selecione **arquivo** > **salvar**.

### <a name="preview-your-text"></a>Visualizar seu texto

Depois de adicionar o texto ao novo arquivo, você deve Visualizar as alterações para ter certeza de que elas aparecem corretamente.

#### <a name="to-preview-your-text"></a>Para visualizar o texto

1. Em Visual Studio Code, selecione um dos botões de **Visualização** no canto superior direito.

    ![ícone do botão Visualizar](media/create-new-using-github/preview-button-full-page.png): Alterna para uma visualização de página inteira do seu conteúdo.

    ![ícone do botão Visualizar](media/create-new-using-github/preview-button-side-by-side.png): Abre a página de visualização próxima à sua página de trabalho, lado a lado.

2. Verifique se o artigo tem a aparência esperada.

    Depois de ter certeza de que está certo, você pode confirmar suas alterações e criar uma solicitação de pull para publicação.

### <a name="commit-your-changes"></a>Confirmar suas alterações

Depois de verificar se o texto está correto, você pode confirmar suas alterações na versão local do seu repositório.

#### <a name="to-commit-your-changes"></a>Para confirmar suas alterações

- Abra o Git bash e digite os comandos (um de cada vez, removendo as marcas OPCIONais):

    ```markdown
    OPTIONAL: git status

    git add .

    git commit -m “public comment about what change is”

    OPTIONAL: git pull upstream master

    git push origin <name_of_your_new_branch>

    ```

    O comando opcional de status do git mostra quais arquivos foram modificados como parte dessa confirmação. O mestre pull de upstream extra do git extrai as alterações de conteúdo mais recentes do Branch mestre MicrosoftDocs, sincronizando seu conteúdo local com o conteúdo mestre primário. Isso ajuda a mostrar todos os conflitos potenciais de mesclagem antecipadamente para que você possa corrigi-los antes de chegar ao estágio de PR.

### <a name="submit-a-pull-request-for-review-and-publication"></a>Enviar uma solicitação de pull para revisão e publicação

Depois de concluir seu artigo, você deve obter aprovação do seu gravador (Aguarde algum tempo para isso) para publicação.

#### <a name="to-submit-your-pull-request"></a>Para enviar sua solicitação de pull

1. Vá para https://github.com/MicrosoftDocs/windowsserverdocs-pr e selecione a guia **solicitações de pull** .

2. Na área revisores do painel direito, selecione o ícone de engrenagem e, em seguida, insira o alias _windowsservercontent_ para revisão.

    Um membro do alias _windowsservercontent_ revisará suas alterações ou adicionará comentários sobre as coisas que devem ser alteradas antes que a mesclagem possa ocorrer.

3. Digite **#sign** nos comentários para que os revisores saibam que você está entregando para revisão e publicação. O comentário de **#sign** :

    - Atualiza o rótulo de sua solicitação de pull de não Mesclar para **pronto para mesclar**.

    - Permite que o alias e os gravadores saibam que você está pronto para que seu conteúdo seja revisado.

    - Permite que os administradores saibam que, após a aprovação, seu conteúdo está pronto para uso.

    >[!Important]
    >Depois de adicionar o comentário de #sign, um membro da equipe do windowsservercontent examinará o texto e o enviará por push para o mestre para que ele vá para a próxima publicação agendada ao vivo (10: às 9h30 e 3: às 16h30 dias da semana).
    >
    >Se você não adicionar #sign como um comentário final à sua PR, seu conteúdo permanecerá na fila sem ser enviado por push ao mestre e, por fim, ao vivo.

## <a name="related-information"></a>Informações relacionadas

Para obter mais informações sobre o GitHub e a linguagem de redução, consulte:

### <a name="git-concepts"></a>Conceitos do git

- [Guias do GitHub-introdução ao manual do git](https://guides.github.com/introduction/git-handbook/)

- [Guias do GitHub – projetos de bifurcação](https://guides.github.com/activities/forking/)

- [Guias do GitHub-noções básicas sobre o fluxo do GitHub](https://guides.github.com/introduction/flow/)

- [Aprenda] a ramificação do git (https://learngitbranching.js.org/ (Ótimo para aprendizes visuais!))

### <a name="markdown"></a>Markdown

- [Nossas diretrizes de redução interna](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [External, tutorial do GitHub](https://www.markdowntutorial.com/)
