---
title: Definir escopo de acesso para uma zona DNS
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 155e21bae4045f094696a8193966469f09a8b6ea
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944658"
---
# <a name="set-access-scope-for-a-dns-zone"></a>Definir escopo de acesso para uma zona DNS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para definir o escopo de acesso para uma zona DNS usando o console do cliente IPAM.

A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.

### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Para definir o escopo de acesso para uma zona DNS

1.  Em Gerenciador do Servidor, clique em **IPAM**. O console do cliente IPAM é exibido.

2.  No painel de navegação, clique em **zonas DNS**. No painel de exibição, clique com o botão direito do mouse na zona DNS para a qual você deseja alterar o escopo de acesso. e clique em **definir escopo de acesso**.

    ![Definir Escopo de Acesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)

3.  A caixa de diálogo **definir escopo de acesso** é aberta. Se necessário para sua implantação, clique para anular a seleção **de herdar o escopo de acesso do pai**. Em **selecionar o escopo de acesso**, selecione um item e clique em **OK**.

    ![Selecionar o escopo de acesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)

4.  No painel de exibição do console do cliente IPAM, verifique se o escopo de acesso da zona foi alterado.

    ![Verifique se o escopo de acesso da zona foi alterado](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)

## <a name="see-also"></a>Consulte Também
Controle de acesso [baseado em função](Role-based-Access-Control.md) 
 [Gerenciar IPAM](Manage-IPAM.md)



