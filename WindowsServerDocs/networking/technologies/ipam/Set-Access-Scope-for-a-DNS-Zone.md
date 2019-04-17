---
title: Defina o escopo de acesso para uma zona de DNS
description: Este tópico faz parte do guia de gerenciamento de gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a4e84f249e57df6bfd04203c8b825ff5d4d643b2
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="set-access-scope-for-a-dns-zone"></a>Defina o escopo de acesso para uma zona de DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para definir o escopo de acesso para uma zona DNS usando o console de cliente IPAM.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Para definir o escopo de acesso para uma zona DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. O console de cliente IPAM aparece.  
  
2.  No painel de navegação, clique em **zonas DNS**. No painel de exibição, clique com botão direito a zona DNS para o qual você deseja alterar o escopo de acesso e, em seguida, clique em **definir o escopo de acesso**.  
  
    ![Defina o escopo de acesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  O **definir o escopo de acesso** caixa de diálogo é aberta. Se necessário para a implantação, clique para desmarcar **Inherit escopo de acesso do pai**. Em **selecione o escopo do acesso**, selecione um item e, em seguida, clique em **Okey**.  
  
    ![Selecione o escopo de acesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  No painel de exibição de console IPAM cliente, verifique se que o escopo de acesso para a zona é alterado.  
  
    ![Verificar se o escopo de acesso para a zona é alterado](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso baseado em função](Role-based-Access-Control.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


