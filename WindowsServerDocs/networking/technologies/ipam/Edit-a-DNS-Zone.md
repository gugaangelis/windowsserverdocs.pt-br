---
title: Editar uma zona DNS
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b2fa8ff6742dc393a5a2cc962c7f849049b5ef31
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312426"
---
# <a name="edit-a-dns-zone"></a>Editar uma zona DNS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para editar uma zona DNS no console do cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-edit-a-dns-zone"></a>Para editar uma zona DNS  
  
1.  Em Gerenciador do Servidor, clique em **IPAM**. O console do cliente IPAM é exibido.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**. O painel de navegação divide-se em um painel de navegação superior e em um painel de navegação inferior.  
  
3.  No painel de navegação inferior, faça uma das seguintes seleções:  
  
    -   Pesquisa direta  
  
    -   Pesquisa inversa de IPv4  
  
    -   Pesquisa inversa de IPv6  
  
4.  Por exemplo, selecione pesquisa inversa de IPv4.  
  
    ![Selecionar um tipo de zona](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  No painel de exibição, clique com o botão direito do mouse na zona que você deseja editar e clique em **Editar zona DNS**.  
  
    ![Editar zona DNS](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  A caixa de diálogo **Editar zona DNS** é aberta com a página **geral** selecionada. Se necessário, edite as propriedades de zona geral: **servidor DNS**, **categoria de zona**e **tipo de zona**e, em seguida, clique em **aplicar** ou, se as edições estiverem concluídas, **OK**.  
  
    ![Editar propriedades da zona e salvar](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  Na caixa de diálogo **Editar zona DNS** , clique em **avançado**. A página Propriedades **avançadas** de zona é aberta. Se necessário, edite as propriedades que você deseja alterar e, em seguida, clique em **aplicar** ou, se suas edições estiverem concluídas, **OK**.  
  
    ![Editar propriedades avançadas de zona](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Se necessário, selecione os nomes de página de propriedades de zona adicionais (servidores de nomes, SOA, transferências de zona), faça suas edições e clique em **aplicar** ou em **OK**. Para revisar todas as edições de zona, clique em **Resumo**e em **OK**.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de zona DNS](DNS-Zone-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


