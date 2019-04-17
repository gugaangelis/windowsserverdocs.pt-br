---
title: Filtrar a exibição de registros de recurso do DNS
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
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 35c0e822daa9f2c8c49ae7e6f2f40ec0411cb6fa
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="filter-the-view-of-dns-resource-records"></a>Filtrar a exibição de registros de recurso do DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para filtrar a exibição dos registros de recurso do DNS no console do cliente IPAM.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>Para filtrar a exibição dos registros de recurso do DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. O console de cliente IPAM aparece.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  Divide o painel de navegação em um painel de navegação superior e um painel de navegação inferior.  
  
3.  No painel de navegação inferior, clique em **pesquisa direta**. Todas as regiões gerenciado IPAM DNS para a frente pesquisa são exibidas nos resultados da pesquisa de painel de exibição.  
  
4.  Clique na zona cujos registros que você deseja exibir e filtrar.  
  
5.  No painel de exibição, clique em **exibição atual**e clique em **registros de recurso**. Os registros de recurso para a zona são mostrados no painel de exibição.  
  
6.  No painel de exibição, clique em **adicionar critérios de**.  
  
    ![Adicionar critérios](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  Selecione um critério na lista suspensa. Por exemplo, se você quiser exibir um tipo de registro específico, clique em **tipo de registro**.  
  
    ![Selecione um critério](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  Clique em **adicionar**.  
  
    ![Adicionar os critérios](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **Tipo de registro** é adicionado como um parâmetro de pesquisa. Insira o texto para o tipo de registro que você quer localizar. Por exemplo, se você quiser exibir apenas os registros SRV, digite **SRV**.  
  
    ![Especifica o tipo de registro que você deseja localizar](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. Pressione ENTER. Os registros de recurso DNS são filtrados de acordo com os critérios e pesquisar frases que você especificou.  
  
    ![Execute o filtro](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de registros de recurso do DNS](DNS-Resource-Record-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


