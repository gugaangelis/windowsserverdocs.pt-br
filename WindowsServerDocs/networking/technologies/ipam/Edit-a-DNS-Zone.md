---
title: Editar uma zona de DNS
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
ms.assetid: a35164e1-11ad-47c8-9843-580d30c70d07
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3e7cc75017c2b59293a042d4af0a677d3eda46c0
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="edit-a-dns-zone"></a>Editar uma zona de DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para editar uma zona DNS no console do cliente IPAM.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-edit-a-dns-zone"></a>Para editar uma zona DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. O console de cliente IPAM aparece.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**. Divide o painel de navegação em um painel de navegação superior e um painel de navegação inferior.  
  
3.  No painel de navegação inferior, fazer uma das seguintes opções:  
  
    -   Pesquisa direta  
  
    -   Pesquisa inversa IPv4  
  
    -   Pesquisa inversa IPv6  
  
4.  Por exemplo, selecione IPv4 Inverter pesquisa.  
  
    ![Selecione um tipo de zona](../../media/Edit-a-DNS-Zone/ipam_EditZone_01.jpg)  
  
5.  No painel de exibição, clique com botão direito na zona que você deseja editar e, em seguida, clique em **Editar DNS zona**.  
  
    ![Zona de DNS editar](../../media/Edit-a-DNS-Zone/ipam_EditZone_02.jpg)  
  
6.  O **Editar DNS zona** caixa de diálogo Abrir com o **geral** página selecionada. Se necessário, editar as propriedades de zona gerais: **servidor DNS**, **categoria de zona**, e **zona tipo**e clique em **aplicar** ou, se as edições são concluídas, **Okey**.  
  
    ![Propriedades de zona de editar e salvar](../../media/Edit-a-DNS-Zone/ipam_EditZone_03a.jpg)  
  
7.  No **Editar DNS zona** caixa de diálogo, clique em **avançado**. O **avançado** propriedades da zona página for aberta. Se necessário, editar as propriedades que você deseja alterar e, em seguida, clique em **aplicar** ou, se as edições são concluídas, **Okey**.  
  
    ![Editar as propriedades de zona avançado](../../media/Edit-a-DNS-Zone/ipam_EditZone_04a.jpg)  
  
8.  Se necessário, selecione os nomes de página de propriedades de zona adicionais (servidores de nomes, SOA, transferências de zona), faça edições e clique em **aplicar** ou **Okey**. Para examinar todas as suas edições da zona, clique em **resumo**e clique em **Okey**.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de zona de DNS](DNS-Zone-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


