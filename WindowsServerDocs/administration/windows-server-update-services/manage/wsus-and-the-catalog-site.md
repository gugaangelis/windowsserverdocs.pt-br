---
title: WSUS e o site do catálogo
description: Tópico do Windows Server Update Service (WSUS) - como importar hotfixes para o WSUS acessando o site do catálogo do Microsoft Update
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f19a8659-5a96-4fdd-a052-29e4547fe51a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cced9a930b63baa79b4addb429c562c38d6da01
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843877"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS e o site do catálogo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O Site do catálogo é o local da Microsoft do qual você pode importar drivers de hardware e hotfixes.

## <a name="the-microsoft-update-catalog-site"></a>Site do catálogo do Microsoft Update
Para importar hotfixes para o WSUS, você deve acessar o Site do catálogo do Microsoft Update em um computador do WSUS. Qualquer computador que tenha o console administrativo do WSUS instalado, se ele é um servidor WSUS, ou não pode ser usado para importar hotfixes do Site do catálogo. Você deve fazer logon no computador como administrador para importar os hotfixes.

#### <a name="to-access-the-microsoft-update-catalog-site"></a>Para acessar o Site do catálogo do Microsoft Update

1.  No console administrativo do WSUS, selecione o nó superior do servidor ou **atualizações**e, nas **ações** painel clique **importar atualizações**. Uma janela do navegador será aberta no site da Web de catálogo do Microsoft Update.

2.  Para acessar as atualizações desse site, você deve instalar o controle activeX de catálogo do Microsoft Update.

3.  Você pode procurar esse site para drivers de hardware e os hotfixes do Windows. Depois de encontrar os ambientes desejados, adicioná-los ao seu carrinho de compras.

4.  Quando você tiver terminado de navegação, vá ao carrinho de compras e clique em Importar para importar as atualizações. Para baixar as atualizações sem importá-los, desmarque a **importar diretamente para o Windows Server Update Services** caixa de seleção.

Atualizações aprovadas importadas do Site do catálogo do Microsoft Update são baixadas na próxima vez em que o servidor do WSUS sincroniza. Eles não são baixados no momento da importação do Site do catálogo do Microsoft Update.

Observe que você deve acessar o Site do catálogo do Microsoft Update no entanto o console do WSUS para garantir que as atualizações sejam importadas em um formato compatível com o WSUS. Se você acessar o site do catálogo do Microsoft Update manualmente, todas as atualizações que você baixe não são importadas para o servidor do WSUS, mas em vez disso, são baixadas como indivíduo *. Arquivos MSU. WSUS atualmente não tem um mecanismo com suporte para importação de arquivos no \*. Formato MSU.

Se você executar o Assistente de limpeza do servidor, as atualizações importadas do catálogo do Microsoft Update são definidas como não aprovada ou recusada podem ser removidas do servidor do WSUS. Se eles forem removidos, eles podem ser importados novamente do catálogo do Microsoft Update.

> [!NOTE]
> Você pode remover as atualizações que são importadas do catálogo do Microsoft Update que são definidas como não aprovado ou recusado, executando a Assistente de limpeza de servidor do WSUS. Você pode importadas novamente as atualizações que foram previamente removidas de seus sistemas WSUS por meio do catálogo do Microsoft Update.

## <a name="restricting-access-to-hotfixes"></a>Restringir o acesso a hotfixes
Administradores do WSUS podem considerar a restringir o acesso para os hotfixes que baixou do Site de catálogo do Microsoft Update. Para fazer com que essa restrição siga as etapas abaixo:

#### <a name="to-restrict-access-to-hotfixes"></a>Para restringir o acesso a hotfixes

1.  Habilite a autenticação do Windows sobre o vroot IIS conteúdo.

    -   Inicie o Gerenciador do IIS.

    -   Navegue até o nó de conteúdo no site de administração do WSUS.

    -   Sobre o **conteúdo inicial** painel, clique duas vezes no **autenticação** opção.

    -   Selecione **autenticação anônima** e clique em **desabilitar** no **ações** painel à direita.

    -   Selecione **autenticação do Windows** e clique em **habilitar** no **ações** painel à direita.

2.  Criar um grupo de destino do WSUS para os computadores que precisam de hotfix e adicioná-los ao grupo. Para obter mais informações sobre computadores e grupos, consulte [computadores de gerenciamento de cliente do WSUS e grupos de computadores do WSUS](managing-wsus-client-computers-and-wsus-computer-groups.md) neste guia e seção [3.3. Configurar grupos de computadores do WSUS](../deploy/2-configure-wsus.md#BKMK_ConfigcomputerGroups) da etapa 3: Configure o WSUS, no guia de implantação do WSUS.

3.  Baixe os arquivos do hotfix.

4.  Defina as permissões desses arquivos para que somente as contas de computador desses computadores possam lê-los. Você também precisará permitir o acesso completo de conta de serviço de rede aos arquivos.

5.  Aprove o hotfix para o grupo de destino do WSUS criado na etapa 2.

## <a name="importing-updates-in-different-languages"></a>Importando atualizações em idiomas diferentes
O site de catálogo do Microsoft Update inclui atualizações que dão suporte a vários idiomas. É muito **importante** para corresponder os idiomas com suporte pelo servidor do WSUS com as linguagens compatíveis com essas atualizações. Se o servidor do WSUS não oferece suporte a todos os idiomas incluídos na atualização, a atualização não será implantada nos computadores cliente. Da mesma forma, se uma atualização que dão suporte a vários idiomas foi baixada para o servidor do WSUS, mas ainda não foram implantada nos computadores cliente e anula a seleção de um administrador de um dos idiomas incluídos a atualização, a atualização não será implantada para os clientes.
