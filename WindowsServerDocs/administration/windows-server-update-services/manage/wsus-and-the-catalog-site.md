---
title: WSUS e o site do catálogo
description: Tópico Windows Server Update Service (WSUS)-como importar hotfixes para o WSUS acessando o site do catálogo Microsoft Update
ms.prod: windows-server
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
ms.openlocfilehash: c30fad5f38a1b3088c6b4d12ac92d8b8a15aee83
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361455"
---
# <a name="wsus-and-the-catalog-site"></a>WSUS e o site do catálogo

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O site do catálogo é o local da Microsoft no qual você pode importar hotfixes e drivers de hardware.

## <a name="the-microsoft-update-catalog-site"></a>O site do catálogo Microsoft Update
Para importar hotfixes para o WSUS, você deve acessar o site de catálogo Microsoft Update de um computador do WSUS. Qualquer computador que tenha o console administrativo do WSUS instalado, seja ele um servidor do WSUS, pode ser usado para importar hotfixes do site do catálogo. Você deve estar conectado ao computador como administrador para importar os hotfixes.

#### <a name="to-access-the-microsoft-update-catalog-site"></a>Para acessar o site de catálogo Microsoft Update

1.  No console administrativo do WSUS, selecione o nó de servidor ou **as atualizações**superiores e, no painel **ações** , clique em **importar atualizações**. Uma janela do navegador será aberta no site do Microsoft Update Catalog.

2.  Para acessar as atualizações neste site, você deve instalar o controle activeX de catálogo Microsoft Update.

3.  Você pode procurar por hotfixes do Windows e drivers de hardware no site. Quando você encontrar os que deseja, adicione-os à sua cesta.

4.  Quando você terminar de navegar, vá para a cesta e clique em importar para importar suas atualizações. Para baixar as atualizações sem importá-las, desmarque a caixa de seleção **importar diretamente para Windows Server Update Services** .

As atualizações aprovadas importadas do site do catálogo Microsoft Update são baixadas na próxima vez que o servidor do WSUS for sincronizado. Eles não são baixados no momento da importação do site do catálogo Microsoft Update.

Observe que você deve acessar o site do catálogo do Microsoft Update no console do WSUS para garantir que as atualizações sejam importadas em um formato compatível com o WSUS. Se você acessar o site do catálogo do Microsoft Update manualmente, as atualizações baixadas não serão importadas para o servidor do WSUS, mas, em vez disso, serão baixadas como individuais *. Arquivos MSU. No momento, o WSUS não tem um mecanismo com suporte para importar arquivos no \*. Formato MSU.

Se você executar o assistente de limpeza do servidor, as atualizações importadas do catálogo de Microsoft Update definidas como não aprovadas ou como recusadas poderão ser removidas do servidor do WSUS. Se eles forem removidos, eles poderão ser importados novamente do catálogo de Microsoft Update.

> [!NOTE]
> Você pode remover as atualizações importadas do catálogo de Microsoft Update que estão definidas como não aprovadas ou recusadas, executando o assistente de limpeza do servidor do WSUS. Você pode importar novamente as atualizações que foram previamente removidas dos sistemas WSUS por meio do catálogo Microsoft Update.

## <a name="restricting-access-to-hotfixes"></a>Restringindo o acesso a hotfixes
Os administradores do WSUS podem considerar a restrição de acesso aos hotfixes baixados do site Microsoft Update Catalog. Para fazer essa restrição, siga as etapas abaixo:

#### <a name="to-restrict-access-to-hotfixes"></a>Para restringir o acesso a hotfixes

1.  Habilite a autenticação do Windows no conteúdo do IIS vroot.

    -   Inicie o Gerenciador do IIS.

    -   Navegue até o nó de conteúdo no site de administração do WSUS.

    -   No painel **início do conteúdo** , clique duas vezes na opção **autenticação** .

    -   Selecione **autenticação anônima** e clique em **desabilitar** no painel **ações** à direita.

    -   Selecione **autenticação do Windows** e clique em **habilitar** no painel **ações** à direita.

2.  Crie um grupo de destino do WSUS para os computadores que precisam do hotfix e adicione-os ao grupo. Para obter mais informações sobre computadores e grupos, consulte [Gerenciando computadores cliente do WSUS e grupos de computadores do WSUS](managing-wsus-client-computers-and-wsus-computer-groups.md) neste guia e a seção [3,3. Configurar grupos de computadores do WSUS](../deploy/2-configure-wsus.md#23-configure-wsus-computer-groups) da etapa 3: configurar o WSUS, no guia de implantação do WSUS.

3.  Baixe os arquivos para o hotfix.

4.  Defina as permissões desses arquivos para que apenas as contas de computador dessas máquinas possam lê-los. Você também precisará permitir que a conta serviço de rede tenha acesso completo aos arquivos.

5.  Aprove o hotfix para o grupo de destino do WSUS criado na etapa 2.

## <a name="importing-updates-in-different-languages"></a>Importando atualizações em idiomas diferentes
O site do catálogo de Microsoft Update inclui atualizações que dão suporte a vários idiomas. É muito **importante** corresponder aos idiomas com suporte do servidor do WSUS com os idiomas com suporte dessas atualizações. Se o servidor do WSUS não oferecer suporte a todos os idiomas incluídos na atualização, a atualização não será implantada nos computadores cliente. Da mesma forma, se uma atualização com suporte a vários idiomas tiver sido baixada para o servidor WSUS, mas ainda não tiver sido implantada em computadores cliente, e um administrador desmarcar uma das linguagens incluídas na atualização, a atualização não será implantada nos clientes.
