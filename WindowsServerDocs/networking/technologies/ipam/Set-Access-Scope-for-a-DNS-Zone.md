---
title: Definir escopo de acesso para uma zona DNS
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a211dde-80eb-4888-b5bb-4e28fe8dc7df
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 67e6e16d361d6d975c4cf900dc9c6b9e7abd3f20
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282199"
---
# <a name="set-access-scope-for-a-dns-zone"></a>Definir escopo de acesso para uma zona DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para definir o escopo de acesso para uma zona DNS usando o console de cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-set-the-access-scope-for-a-dns-zone"></a>Para definir o escopo de acesso para uma zona DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. Console de cliente IPAM é exibida.  
  
2.  No painel de navegação, clique em **zonas DNS**. No painel de exibição, clique com botão direito a zona DNS para o qual você deseja alterar o escopo de acesso e, em seguida, clique em **definir escopo de acesso**.  
  
    ![Definir Escopo de Acesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_02.jpg)  
  
3.  O **definir escopo de acesso** caixa de diálogo é aberta. Se for necessário para sua implantação, clique para desmarcar **escopo de acesso herdado do pai**. Na **selecionar o escopo de acesso**, selecione um item e, em seguida, clique em **Okey**.  
  
    ![Selecione o escopo de acesso](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_03.jpg)  
  
4.  No painel de exibição do console de cliente IPAM, verifique se que o escopo de acesso para a zona é alterado.  
  
    ![Verifique se o escopo de acesso para a zona é alterado](../../media/Set-Access-Scope-for-a-DNS-Zone/ipam_SetAccessScopeOfZone_04.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso baseado em função](Role-based-Access-Control.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


