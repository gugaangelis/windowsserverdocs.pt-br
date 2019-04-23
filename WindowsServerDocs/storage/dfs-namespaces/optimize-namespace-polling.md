---
title: Otimizar sondagem de namespace
description: Este artigo descreve como otimizar a sondagem para manter um namespace baseado em domínio consistente em todos os servidores de namespace
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 995b01604b680746c4b0d6502b3b3968503d4210
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878267"
---
# <a name="optimize-namespace-polling"></a>Otimizar sondagem de namespace

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Para manter um namespace baseado em domínio consistente em todos os servidores, é necessário que os servidores de namespace realizem chamada seletiva periodicamente nos Serviços de Domínio Active Directory (AD DS) para obter os dados mais atuais do namespace. 

## <a name="to-optimize-namespace-polling"></a>Para otimizar a chamada seletiva do namespace

Use o procedimento a seguir para otimizar como essa sondagem de namespace ocorre:

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique com o botão direito do mouse em um namespace baseado em domínio e, em seguida, clique em **Propriedades**.

3.  Na guia **Avançado**, selecione se você deseja que o namespace seja otimizado para consistência ou escalabilidade.

    -   Escolha **Otimizar para consistência** se houver 16 ou menos servidores de namespace hospedando o namespace.
    -   Escolha **Otimizar para escalabilidade** se houver mais de 16 servidores de namespace. Isso reduz a carga no emulador PDC (Primary Domain Controller, controlador de domínio primário), mas aumenta o tempo necessário para que as alterações no namespace sejam replicadas para todos os servidores de namespace. Até que as alterações sejam replicadas em todos os servidores, os usuários podem ter uma visão inconsistente do namespace.

> [!NOTE]
> Para definir o modo de chamada seletiva do namespace usando o Windows PowerShell, use o [EnableRootScalability DfsnRoot conjunto](https://technet.microsoft.com/library/jj884281.aspx) cmdlet, que foi introduzido no Windows Server 2012.

## <a name="see-also"></a>Consulte também

-   [Ajuste os Namespaces do DFS](tuning-dfs-namespaces.md)
-   [Delegar permissões de gerenciamento para Namespaces do DFS](delegate-management-permissions-for-dfs-namespaces.md)