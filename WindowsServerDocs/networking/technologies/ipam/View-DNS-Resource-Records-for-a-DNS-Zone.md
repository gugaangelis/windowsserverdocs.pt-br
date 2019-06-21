---
title: Exibir registros de recurso de DNS para uma zona DNS
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 44db34199257367e98279ccbcbc2d5041ee9884c
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283801"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Exibir registros de recurso de DNS para uma zona DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para exibir os registros de recurso DNS para uma zona DNS no console de cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-view-dns-resource-records-for-a-zone"></a>Para exibir os registros de recurso DNS para uma zona  
  
1.  No Gerenciador do servidor, clique em **IPAM**. Console de cliente IPAM é exibida.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  Divide o painel de navegação em um painel de navegação superior e um painel de navegação inferior.  
  
3.  No painel de navegação inferior, clique em **pesquisa direta**e, em seguida, expanda a lista de domínio e fuso para localizar e selecionar a zona que você deseja exibir. Por exemplo, se você tiver uma zona chamada dublin, clique em **dublin**.  
  
    ![Selecione a zona que você deseja exibir](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)  

  
4.  No painel de exibição, a exibição padrão é dos servidores DNS da zona. Para alterar o modo de exibição, clique em **modo de exibição atual**e, em seguida, clique em **registros de recurso**.  
  
    ![Alterar o modo de exibição para registros de recursos](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)  
  
5.  Os registros de recurso DNS para a zona são exibidos. Para filtrar os registros, digite o texto que você deseja localizar na **filtro**.  
  
    ![Digite o texto para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)  
  
6.  Para filtrar os registros de recurso por tipo de registro, o escopo de acesso ou outros critérios, clique em **adicionar critérios**e, em seguida, faça as seleções da lista de critérios e clique em **Add**.  
  
    ![Use critérios para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de zonas DNS](DNS-Zone-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


