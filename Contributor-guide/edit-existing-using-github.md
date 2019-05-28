---
title: Editar um artigo existente do Windows Server usando o GitHub e Visual Studio Code
description: Como editar um artigos relacionados ao Windows Server existente, usando o GitHub e Visual Studio Code, como um funcionário da Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/06/2019
ms.openlocfilehash: d2d95cc28089ceb74bf9690f6bd78611e7d9a27a
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624579"
---
# <a name="edit-an-existing-windows-server-article-using-github-and-visual-studio-code"></a>Editar um artigo existente do Windows Server usando o GitHub e Visual Studio Code

Há dois locais separados, onde podemos manter conteúdo técnico do Windows Server. Um dos locais é público (windowsserverdocs) enquanto a outra é privada (pr windowsserverdocs). Quem é você determina qual local contribuem para:

- **Eu sou um funcionário da Microsoft.** Como funcionário da Microsoft, você tem opções, com base no qual você está tentando fazer:

    - **Crie um artigo inteiramente novo.** Para criar um novo artigo, você deve criar e configurar sua conta do GitHub e ferramentas, bifurcar e clonar o repositório windowsserverdocs-pr, configurar seu branch remoto, criar o artigo e, finalmente, crie uma nova solicitação de pull para aprovação e publicação. Para essas instruções, você pode seguir as instruções de [criar novos artigos do Windows Server usando o GitHub e Visual Studio Code](create-new-using-github.md) artigo.

    - **Fazer grandes alterações em um artigo existente.** Para fazer alterações significativas em um artigo existente, você pode seguir as instruções neste artigo.

    - **Fazer pequenas alterações em um artigo existente.** Para fazer pequenas alterações em um artigo existente, você pode seguir as instruções de [atualizam os artigos existentes do Windows Server usando um navegador da web e o GitHub](github-browser-updates.md) artigo.

- **Eu não sou um funcionário da Microsoft.** Como um funcionário não são da Microsoft, você deve contribuir para o local público. Para obter informações sobre como fazer isso, consulte a [contribuindo para a documentação técnica do Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) artigo.

## <a name="switch-your-repo-and-create-a-new-branch"></a>Alternar seu repositório e criar um novo branch

Siga estas etapas para editar um artigo existente.

### <a name="create-a-new-branch-and-locate-the-file-you-want-to-update"></a>Criar um novo branch e localize o arquivo que você deseja atualizar

Antes que você pode começar a trabalhar em seu conteúdo, você deve primeiro alterar para o repositório windowsserverdocs-pr e, em seguida, localize o artigo que você deseja atualizar.

#### <a name="to-create-a-new-branch-in-git-bash"></a>Para criar um novo branch no Git Bash

- Abra o Git Bash e digite os comandos (um por vez):

    ```markdown
    cd windowsserverdocs-pr

    git checkout –B <name_of_your_new_branch>

    git push origin <name_of_your_new_branch>
    ```

    >[!Note]
    >É altamente recomendável nomear sua ramificação algo óbvio e exclusivo para que possa localizá-lo novamente mais tarde.

    Depois de concluir os comandos, você usará em seu novo branch e está pronto para editar seu arquivo.

#### <a name="to-locate-your-article-and-make-your-edits"></a>Para localizar seu artigo e faça as edições

1. Abra o Visual Studio Code e vá para **arquivo**, selecione **Abrir pasta**e, em seguida, vá para o local do GitHub da pasta que contém o artigo que você deseja editar.

2. Dos **Explorer** painel, selecione o arquivo.

3. Atualizar as informações na página e, em seguida, selecione **arquivo** > **salvar**.

### <a name="preview-your-text"></a>Visualizar seu texto

Depois de atualizar o texto, você deve visualizar as alterações para garantir que eles sejam exibidos corretamente.

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

Depois de concluir as atualizações, você deve obter aprovação do seu gravador (permitir algum tempo para isso) para publicação.

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