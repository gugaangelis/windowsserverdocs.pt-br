---
title: Registros de recurso do modo de exibição DNS para uma zona DNS
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
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 786c1ee8fd673bd17465ab9586dd1e0bcfd7971c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Registros de recurso do modo de exibição DNS para uma zona DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para exibir os registros de recurso DNS para uma zona DNS no console do cliente IPAM.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>Para exibir os registros de recurso DNS para uma zona  
  
1.  No Gerenciador do servidor, clique em **IPAM**. O console de cliente IPAM aparece.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  Divide o painel de navegação em um painel de navegação superior e um painel de navegação inferior.  
  
3.  No painel de navegação inferior, clique em **pesquisa direta**e, em seguida, expanda a lista de domínio e zona para localizar e selecionar a zona que você deseja exibir. Por exemplo, se você tiver uma região nomeada dublin, clique em **dublin**.  
  
    ![Selecione a zona que você deseja exibir](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  No painel de exibição, o modo de exibição padrão é dos servidores DNS para a zona. Para alterar o modo de exibição, clique em **exibição atual**e clique em **registros de recurso**.  
  
    ![Alterar o modo de exibição para registros de recurso](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  Os registros de recurso DNS para a zona da são exibidos. Para filtrar os registros, digite o texto que deseja encontrar na **filtro**.  
  
    ![Digite um texto para filtrar os registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  Para filtrar os registros de recurso por tipo de registro, o escopo de acesso ou outros critérios, clique em **adicionar critérios de**e faz as seleções da lista critérios e clique em **adicionar**.  
  
    ![Use critérios para filtrar os registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de zona de DNS](DNS-Zone-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


