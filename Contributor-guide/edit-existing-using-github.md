---
title: Editar um artigo existente do Windows Server usando o GitHub e o Visual Studio Code
description: Como editar um artigo existente relacionado ao Windows Server, usando o GitHub e o Visual Studio Code, como um funcionário da Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d681d2fc2b69898e841932a95738b89515ffb51a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70865081"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>Editar um artigo existente do Windows Server usando o GitHub e o Visual Studio Code

Há dois locais separados onde mantemos o conteúdo técnico do Windows Server. Um dos locais é público (windowsserverdocs) enquanto o outro é privado (windowsserverdocs-PR). Quem você está determina em qual local você contribui:

- **Sou um funcionário da Microsoft.** Como funcionário da Microsoft, você tem opções, com base no que está tentando fazer:

    - **Crie um artigo novo.** Para criar um novo artigo, você deve criar e configurar sua conta e ferramentas do GitHub, bifurcar e clonar o repositório windowsserverdocs-PR, configurar sua ramificação remota, criar o artigo e, finalmente, criar uma nova solicitação de pull para aprovação e publicação. Para essas instruções, você pode seguir as instruções no artigo [criar novos artigos do Windows Server usando o GitHub e Visual Studio Code](create-new-using-github.md) .

    - **Faça grandes alterações em um artigo existente.** Para fazer alterações substanciais em um artigo existente, você pode seguir as instruções neste artigo.

    - **Faça alterações secundárias em um artigo existente.** Para fazer alterações secundárias em um artigo existente, você pode seguir as instruções contidas no artigo [atualizar artigos existentes do Windows Server usando um navegador da Web e o GitHub](github-browser-updates.md) .

- **Não sou um funcionário da Microsoft.** Como funcionário que não seja da Microsoft, você deve contribuir para o local público. Para obter informações sobre como fazer isso, consulte o artigo [contribuindo para a documentação técnica do Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) .

## <a name="switch-your-repo-and-create-a-new-branch"></a>Alternar seu repositório e criar um novo Branch

Siga estas etapas para editar um artigo existente.

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>Criar uma nova ramificação e localizar o arquivo que você deseja atualizar

Antes de começar a trabalhar com seu conteúdo, você deve primeiro alterar para o repositório windowsserverdocs-PR e, em seguida, localizar o artigo que deseja atualizar.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Para criar uma nova ramificação no git bash

- Abra o Git bash e digite os comandos (um de cada vez):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >É altamente recomendável nomear seu Branch como algo óbvio e exclusivo para que você possa encontrá-lo novamente mais tarde.

    Após a conclusão dos comandos, você estará em seu novo Branch e estará pronto para editar o arquivo.

#### <a name="to-locate-your-article-and-make-your-edits"></a>Para localizar seu artigo e fazer suas edições

1. Abra Visual Studio Code e vá para **arquivo**, selecione **abrir pasta**e, em seguida, vá para o local do GitHub da pasta que contém o artigo que você deseja editar.

2. No painel do **Explorer** , selecione o arquivo.

3. Atualize as informações na página e, em seguida, selecione **arquivo** > **salvar**.

### <a name="preview-your-text"></a>Visualizar seu texto

Depois de atualizar o texto, você deve Visualizar as alterações para verificar se elas aparecem corretamente.

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

Depois de concluir as atualizações, você deverá obter aprovação do seu gravador (Aguarde algum tempo para isso) para publicação.

#### <a name="to-submit-your-pull-request"></a>Para enviar sua solicitação de pull

1. Vá para https://github.com/MicrosoftDocs/windowsserverdocs-pr e selecione a guia **solicitações de pull** .

2. Na área **revisores** do painel direito, selecione o ícone de engrenagem e, em seguida, insira o alias _windowsservercontent_ para revisão.

    Um membro do alias _windowsservercontent_ revisará suas alterações ou adicionará comentários sobre as coisas que devem ser alteradas antes que a mesclagem possa ocorrer.

3. Digite **#sign** nos comentários para que os revisores saibam que você está entregando para revisão e publicação. O comentário de **#sign** :

    - Atualiza o rótulo de sua solicitação de pull de não **mesclar** para **pronto para mesclar**.

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

- [Aprenda a ramificação do git] (https://learngitbranching.js.org/ (Ótimo para aprendizes visuais!))

### <a name="markdown"></a>Markdown

- [Nossas diretrizes de redução interna](https://review.docs.microsoft.com/help/contribute/markdown-reference?branch=master)

- [External, tutorial do GitHub](https://www.markdowntutorial.com/)