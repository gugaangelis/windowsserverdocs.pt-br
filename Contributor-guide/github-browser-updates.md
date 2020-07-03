---
title: Editar artigos existentes do Windows Server usando um navegador da Web e o GitHub
description: Como fazer edições rápidas na documentação existente do Windows Server usando um navegador da Web e o GitHub, como um funcionário da Microsoft.
author: eross-msft
ms.author: lizross
ms.date: 07/02/2020
ms.openlocfilehash: 61ceb05b6cb9602faaa4d17b4dc2d978cb571ddb
ms.sourcegitcommit: d754f9e39fc28e4c8768b77bcc9d02ffe2fa6535
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85911226"
---
# <a name="update-existing-windows-server-and-azure-stack-hci-articles-using-a-web-browser-and-github"></a>Atualizar artigos do Windows Server e Azure Stack HCI existentes usando um navegador da Web e o GitHub

Há dois locais separados onde mantemos o conteúdo técnico do Windows Server. Um dos locais é público (windowsserverdocs) enquanto o outro é privado (windowsserverdocs-PR). Quem você está determina em qual local você contribui:

- **Não sou um funcionário da Microsoft.** Como funcionário que não seja da Microsoft, você deve contribuir para o local público. Para obter informações sobre como fazer isso, consulte o arquivo [Contributing.MD](https://github.com/MicrosoftDocs/windowsserverdocs/blob/master/CONTRIBUTING.md) .

- **Sou um funcionário da Microsoft.** Como funcionário da Microsoft, você tem opções, com base no que está tentando fazer:

    - **Crie um artigo novo.** Para criar um novo artigo, você deve criar e configurar sua conta e ferramentas do GitHub, bifurcar e clonar o repositório windowsserverdocs-PR, configurar sua ramificação remota, criar o artigo e, finalmente, criar uma nova solicitação de pull para aprovação e publicação. Para estas instruções, consulte o artigo [criar novos artigos do Windows Server usando o GitHub e Visual Studio Code](create-new-using-github.md) .

    - **Faça grandes alterações em um artigo existente.** Para fazer alterações substanciais em um artigo existente, você pode seguir as instruções no artigo [Editar um servidor do Windows existente usando o GitHub e Visual Studio Code](edit-existing-using-github.md) .

    - **Faça alterações secundárias em um artigo existente.** Para fazer alterações secundárias em um artigo existente, siga as instruções neste artigo.

    > [!IMPORTANT]
    > Todos os repositórios que publicam no docs.microsoft.com adotaram o [Código de Conduta de Software Livre da Microsoft](https://opensource.microsoft.com/codeofconduct/) ou o [Código de Conduta de .NET Foundation](https://dotnetfoundation.org/code-of-conduct). Para obter mais informações, consulte [Perguntas frequentes sobre código de conduta](https://opensource.microsoft.com/codeofconduct/faq/). Ou entre em contato com [opencode@microsoft.com](mailto:opencode@microsoft.com) ou [conduct@dotnetfoundation.org](mailto:conduct@dotnetfoundation.org) com perguntas ou comentários.
    >
    > Os [Termos de Uso de docs.microsoft.com](https://docs.microsoft.com/legal/termsofuse) incluem correções secundárias ou esclarecimentos sobre a documentação, além de exemplos de código nos repositórios públicos. Quando há alterações novas ou significativas, é gerado um comentário na solicitação de pull, pedindo que você envie um CLA (Contrato de Licença de Contribuição) online caso não seja um funcionário da Microsoft. Para analisarmos ou aceitarmos sua solicitação de pull, primeiro preencha o formulário online.

## <a name="quick-edits-to-existing-articles-using-github-and-a-web-browser"></a>Edições rápidas para artigos existentes usando o GitHub e um navegador da Web

Com as edições rápidas, ficou mais fácil relatar e corrigir pequenos erros e omissões nos documentos. Apesar de todos os esforços, pequenos erros gramaticais e ortográficos _aparecem_ em nossos documentos publicados.

1. Siga as instruções em [configuração da conta do GitHub](https://review.docs.microsoft.com/en-us/help/contribute/contribute-get-started-setup-github?branch=master).

1. Vá para o repositório privado do [Windows Server](https://github.com/MicrosoftDocs/windowsserverdocs-pr/tree/master/WindowsServerDocs) ou [Azure Stack HCI](https://github.com/MicrosoftDocs/azure-stack-docs-pr/tree/master/azure-stack/hci) . Os repositórios privados são monitorados com mais frequência, de modo que nosso tempo de aprovação é mais rápido, eles se beneficiam de maiores verificações de qualidade e fornecem a capacidade de exibir o conteúdo em preparo, pois ele será exibido em nosso site ativo.

2. Navegue até o artigo que você deseja editar e, em seguida, selecione o botão **Editar este arquivo** .

   ![Botão Editar este arquivo](media/github-browser-updates/edit-this-file.png)

3. Edite o tópico, em seguida, role para baixo até a parte inferior, Descreva brevemente as alterações e, em seguida, selecione **confirmar alterações**.

    ![Confirmar alterações com informações de título](media/github-browser-updates/commit-changes.png)

## <a name="submit-the-pull-request"></a>Enviar a solicitação de pull

Depois de criar sua solicitação de pull, você deve enviá-la para aprovação e publicação.

### <a name="to-submit-your-pull-request"></a>Para enviar sua solicitação de pull

1. Na página **abrir uma solicitação de pull** , atualize sua mensagem de confirmação para torná-la mais apropriada para uma pr. Por exemplo: corrigir erros de digitação no primeiro parágrafo.

2. Certifique-se de que apenas as confirmações e os arquivos esperados sejam incluídos. Verifique também se a PR vai para o Branch correto no repositório upstream, seja mestre (normalmente) ou um Branch de lançamento (ocasionalmente).

3. Na área **revisores** do painel direito, selecione o ícone de engrenagem e, em seguida, insira _windowsservercontent_. Um membro do alias estará no ponto para revisar suas alterações e mesclar sua solicitação de pull ou adicionar comentários sobre as coisas a serem alteradas antes da mesclagem.

4. Selecione **Criar solicitação de pull**. A nova PR está vinculada à sua ramificação de trabalho em sua bifurcação. Até que a PR seja mesclada, todas as novas confirmações que você enviar por push para o mesmo Branch de trabalho na bifurcação serão incluídas automaticamente na PR. Um novo rótulo é adicionado à sua solicitação de pull que diz, **não mesclar**. Isso simplesmente significa que seu conteúdo ainda está em andamento e não deve ser revisado ou enviado por push para o site ativo.

5. Quando estiver pronto para alguém do alias para revisar o conteúdo, você deverá adicionar o texto, **#sign-** lo aos comentários. Este comentário:

    - Atualiza o rótulo de sua solicitação de pull de não **mesclar** para **pronto para mesclar**.

    - Permite que o alias e os gravadores saibam que você está pronto para que seu conteúdo seja revisado.

    - Permite que os administradores saibam que, após a aprovação, seu conteúdo está pronto para uso.
