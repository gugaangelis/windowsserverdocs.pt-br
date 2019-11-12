---
title: Introdução ao cliente da Área de Trabalho do Windows
description: Informações básicas sobre o cliente da Área de Trabalho do Windows.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 10/31/2019
ms.localizationpriority: medium
ms.openlocfilehash: aff7e6e1f37cad66530679ade024c089a4ba034e
ms.sourcegitcommit: 1da993bbb7d578a542e224dde07f93adfcd2f489
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/04/2019
ms.locfileid: "73567171"
---
# <a name="get-started-with-the-windows-desktop-client"></a>Introdução ao cliente da Área de Trabalho do Windows

>Aplica-se a: Windows 10 e Windows 7

É possível usar o cliente da Área de Trabalho Remota para que a Área de Trabalho do Windows acesse áreas de trabalho e aplicativos do Windows usando outro dispositivo Windows.

> [!NOTE]
> - Esta documentação não é para o cliente da Conexão de Área de Trabalho Remota (MSTSC) fornecido com o Windows. É para o novo cliente da Área de Trabalho Remota (MSRDC).
> - No momento, esse cliente só dá suporte ao acesso a aplicativos e áreas de trabalho remotos da [Área de Trabalho Virtual do Windows](https://aka.ms/wvd).
> - Curioso sobre as novas versões para o cliente da Área de Trabalho do Windows? Confira as [Novidades do cliente de Área de Trabalho do Windows](windowsdesktop-whatsnew.md)

## <a name="install-the-client"></a>Instalar o cliente

Escolha o cliente que corresponda à sua versão do Windows:

- [Windows 64 bits](https://go.microsoft.com/fwlink/?linkid=2068602)
- [Windows 32 bits – Versão Prévia](https://go.microsoft.com/fwlink/?linkid=2098960)
- [Windows ARM64 – Versão Prévia](https://go.microsoft.com/fwlink/?linkid=2098961)

É possível instalar o cliente do usuário atual, que não requer direitos de administrador, ou seu administrador pode instalar e configurar o cliente para que todos os usuários no dispositivo possam acessá-lo.

Depois de instalar o cliente, será possível iniciá-lo por meio do menu Iniciar pesquisando por **Área de Trabalho Remota**.

## <a name="update-the-client"></a>Atualizar o cliente

Você será notificado sempre que uma nova versão do cliente estiver disponível, desde que seu administrador não tenha desabilitado as notificações. A notificação será exibida na Central de Conexões ou na Central de Ações do Windows. Para atualizar o cliente, basta selecionar a notificação.

Também é possível pesquisar manualmente novas atualizações para o cliente:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) na barra de comandos na parte superior do cliente.
2. Selecione **Sobre** no menu suspenso.
3. Toque em **Verificar se há atualizações**.
4. Se houver uma atualização disponível, toque em **Instalar atualização** para atualizar o cliente.

## <a name="feeds"></a>Feeds

Obtenha a lista de recursos gerenciados que você pode acessar, como aplicativos e áreas de trabalho, assinando o feed que seu administrador forneceu. Quando você assina, os recursos são disponibilizados no PC local. No momento, o cliente da Área de Trabalho do Windows dá suporte a recursos publicados na Área de Trabalho Virtual do Windows.

### <a name="subscribe-to-a-feed"></a>Assinar um feed

1. Na página principal do cliente, também conhecida como Central de Conexão, toque em **Assinar**.
2. Entre com sua conta quando solicitado.
3. Os recursos serão exibidos na Central de Conexão agrupados por Workspace.

É possível iniciar recursos com um dos seguintes métodos:

- Acesse a Central de Conexão e clique duas vezes em um recurso para iniciá-lo.
- Também é possível acessar o menu Iniciar e procurar uma pasta com o nome do Workspace ou inserir o nome do recurso na barra de pesquisa.

### <a name="workspace-details"></a>Detalhes do Workspace

Após assinar, será possível exibir outras informações sobre um Workspace no painel Detalhes:

- O nome do Workspace
- A URL e o nome de usuário usados para assinar
- O número de aplicativos e áreas de trabalho
- A data/hora da última atualização
- O status da última atualização

Acessar o painel Detalhes:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) ao lado do Workspace.
2. Selecione **Detalhes** no menu suspenso.
3. O painel Detalhes é exibido no lado direito do cliente.

Depois de assinar, o Workspace será atualizado automaticamente de forma regular. Os recursos podem ser adicionados, alterados ou removidos com base nas alterações realizadas pelo seu administrador.

Também é possível procurar manualmente atualizações para os recursos, quando necessário, selecionando **Atualizar agora** no painel Detalhes.

### <a name="unsubscribe-from-a-feed"></a>Cancelar a assinatura de um feed

Esta seção ensinará como cancelar a assinatura de um feed. É possível cancelar a assinatura para assinar novamente com uma conta diferente ou remover seus recursos do sistema.

1. Na Central de Conexão, toque no menu de estouro ( **...** ) ao lado do Workspace.
2. Selecione **Cancelar assinatura** no menu suspenso.
3. Examine a caixa de diálogo e selecione **Continuar**.

## <a name="managed-desktops"></a>Áreas de trabalho gerenciadas

Os workspaces podem conter vários recursos gerenciados, incluindo áreas de trabalho. Ao acessar uma área de trabalho gerenciada, você tem acesso a todos os aplicativos instalados por seu administrador.

### <a name="desktop-settings"></a>Configurações da área de trabalho

É possível definir algumas das configurações para recursos da área de trabalho a fim de garantir que a experiência atenda às suas necessidades. Para acessar a lista de configurações disponíveis:

1. Na Central de Conexões, clique com o botão direito do mouse em um recurso da área de trabalho.
2. Selecione **Configurações** no menu suspenso.
3. O painel Configurações será exibido no lado direito do cliente que exibe o nome da área de trabalho.

O cliente usará as configurações definidas pelo administrador, a menos que você desative a opção **Usar configurações padrão**. Isso permite que você configure as seguintes opções:

- **Usar todos os monitores** alterna a sessão da área de trabalho entre o uso de todos os monitores locais disponíveis e apenas um monitor.
- **Iniciar em tela inteira** determina se a sessão será iniciada no modo de tela inteira ou de janela. Essa configuração é habilitada automaticamente ao usar todos os monitores.
- **Atualizar a resolução ao redimensionar** altera o comportamento quando você redimensiona a sessão no modo de janela. Se habilitada, a resolução da área de trabalho remota será atualizada para corresponder ao tamanho da janela local. Se desabilitada, a sessão reterá a resolução especificada em **Resolução** por toda a duração. Essa configuração é habilitada automaticamente ao usar todos os monitores.
- A **Resolução** permite que você especifique a resolução da área de trabalho remota. A sessão reterá essa resolução por toda a duração. Essa configuração será desabilitada automaticamente caso a resolução seja definida como atualizar ao redimensionar.
- **Alterar o tamanho do texto e dos aplicativos** especifica o tamanho do conteúdo da sessão. Essa configuração se aplica somente ao se conectar ao Windows 8.1 e posterior ou ao Windows Server 2012 R2 e posterior. Essa configuração será desabilitada automaticamente caso a resolução seja definida como atualizar ao redimensionar.
- **Ajustar a sessão à janela** determina como a sessão é exibida quando a resolução da área de trabalho remota difere do tamanho da janela local. Quando habilitado, o conteúdo da sessão será redimensionado para se ajustar dentro da janela enquanto preserva a taxa de proporção da sessão. Quando desabilitadas, as barras de rolagem ou as áreas pretas serão mostradas quando a resolução e o tamanho da janela não corresponderem.

## <a name="provide-feedback"></a>Fornecer comentários

Tem uma sugestão de recursos ou deseja relatar um problema? Conte-nos usando o [Hub de Comentários](feedback-hub://?tabid=2&contextid=883). Você também pode acessar o Hub de Feedback por meio do cliente:

1. Na Central de Conexão, toque no menu de estouro ( **...** ) na barra de comandos na parte superior do cliente.
2. Selecione **Comentários** no menu suspenso para abrir o Hub de Comentários.
