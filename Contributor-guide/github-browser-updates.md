---
title: Editar artigos existentes do Windows Server usando um navegador da web e o GitHub
description: Como fazer edições rápidas a documentação do Windows Server existente usando um navegador da web e o GitHub, como funcionário da Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 05/02/2019
ms.openlocfilehash: d4574de7774a43092815a3d154020559c50fdcf9
ms.sourcegitcommit: 29ad32b9dea298a7fe81dcc33d2a42d383018e82
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/15/2019
ms.locfileid: "65624593"
---
# <a name="update-existing-windows-server-articles-using-a-web-browser-and-github"></a>Atualizar artigos existentes do Windows Server usando um navegador da web e o GitHub

Há dois locais separados, onde podemos manter conteúdo técnico do Windows Server. Um dos locais é público (windowsserverdocs) enquanto a outra é privada (pr windowsserverdocs). Quem é você determina qual local contribuem para:

- **Eu não sou um funcionário da Microsoft.** Como um funcionário não são da Microsoft, você deve contribuir para o local público. Para obter informações sobre como fazer isso, consulte a [Contributing.md](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) arquivo.

- **Eu sou um funcionário da Microsoft.** Como funcionário da Microsoft, você tem opções, com base no qual você está tentando fazer:

    - **Crie um artigo inteiramente novo.** Para criar um novo artigo, você deve criar e configurar sua conta do GitHub e ferramentas, bifurcar e clonar o repositório windowsserverdocs-pr, configurar seu branch remoto, criar o artigo e, finalmente, crie uma nova solicitação de pull para aprovação e publicação. Para essas instruções, consulte a [criar novos artigos do Windows Server usando o GitHub e Visual Studio Code](create-new-using-github.md) artigo.

    - **Fazer grandes alterações em um artigo existente.** Para fazer alterações significativas em um artigo existente, você pode seguir as instruções de [editar um artigo existente do Windows Server usando o GitHub e Visual Studio Code](edit-existing-using-github.md) artigo.

    - **Fazer pequenas alterações em um artigo existente.** Para fazer pequenas alterações em um artigo existente, você pode seguir as instruções neste artigo.

    > [!IMPORTANT]
    > Todos os repositórios que publicam no docs.microsoft.com adotaram o [Microsoft código de conduta de software livre](https://opensource.microsoft.com/codeofconduct/) ou o [código de conduta de .NET Foundation](https://dotnetfoundation.org/code-of-conduct). Para obter mais informações, consulte o [código de conduta perguntas frequentes sobre](https://opensource.microsoft.com/codeofconduct/faq/). Ou entre em contato com [ opencode@microsoft.com ](mailto:opencode@microsoft.com), ou [ conduct@dotnetfoundation.org ](mailto:conduct@dotnetfoundation.org) com perguntas ou comentários.
    >
    > Correções secundárias ou esclarecimentos para documentação e exemplos de código em repositórios públicos são cobertos pelo [termos de uso de docs.microsoft.com](https://docs.microsoft.com/legal/termsofuse). As alterações novas ou significativas geram um comentário na solicitação de pull, solicitando que você envie um contrato de licença de contribuição (CLA) online, se você não for um funcionário da Microsoft. Você precisa para concluir o formulário online antes, podemos examinar ou aceitar sua solicitação de pull.

## <a name="quick-edits-to-existing-articles-using-github-and-a-web-browser"></a>Edições rápidas para os artigos existentes usando o GitHub e um navegador da web

As edições rápidas simplificam o processo de relatar e corrigir pequenos erros e omissões em documentos. Apesar de todos os esforços, pequena gramática e erros de ortografia _fazer_ passarão em nossos documentos publicados.

1. Acesse https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/WindowsServerDocs.

2. Navegue até o artigo que você deseja editar e, em seguida, selecione a **editar esse arquivo** botão.

   ![Editar este botão de arquivo](media/github-browser-updates/edit-this-file.png)

3. Editar o tópico, em seguida, role para baixo até a parte inferior, descrever brevemente as alterações e, em seguida, selecione **confirmar alterações**.

    ![Confirmar as alterações com informações de título](media/github-browser-updates/commit-changes.png)

## <a name="submit-the-pull-request"></a>Enviar a solicitação de pull

Depois de criar sua solicitação de pull, você deve enviá-lo para aprovação e publicação.

### <a name="to-submit-your-pull-request"></a>Para enviar sua solicitação de pull

1. Sobre o **abrir uma solicitação de pull** página, atualize sua mensagem de confirmação para torná-lo mais apropriada para uma PR. Por exemplo:  Corrigi o erro de digitação no primeiro parágrafo.

2. Certifique-se de que somente as confirmações e os arquivos que você espera ser incluídos estão incluídos. Verifique também que a PR vai para o branch correto no repositório upstream, mestre (geralmente) ou um branch de lançamento (ocasionalmente).

3. No **revisores** área do painel direito, selecione o ícone de engrenagem e, em seguida, insira _windowsservercontent_. Um membro do alias será no ponto para rever as alterações e o seu pull de mesclagem da solicitação ou adicione comentários sobre as coisas para alterar antes da mesclagem.

4. Selecione **criar solicitação de pull**. A nova solicitação de pull é vinculada ao seu branch de trabalho na sua bifurcação. Até que a PR será mesclada, todas as novas confirmações que enviar por push para o mesmo branch de trabalho na sua bifurcação são incluídas automaticamente na PR. Um novo rótulo é adicionado à sua solicitação de pull que diz **fazer não mesclar**. Isso simplesmente significa que seu conteúdo ainda está em andamento e não deve ser revisado ou enviados por push para o site ao vivo.

5. Quando você estiver pronto para alguém no alias para examinar seu conteúdo, você deve adicionar o texto **#sign-off** aos comentários. Este comentário:

    - Atualiza o rótulo para a sua solicitação de pull de **fazer não mesclar** à **pronto para mesclagem**.

    - Permite que o alias e gravadores de saber o que você está pronto para ter seu conteúdo revisado.

    - Permite que os administradores Saiba que, após a aprovação, seu conteúdo está pronto ir ao vivo.