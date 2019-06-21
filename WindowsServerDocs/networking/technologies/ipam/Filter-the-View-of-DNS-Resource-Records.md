---
title: Filtrar a exibição de registros de recursos de DNS
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b80294a-7325-476b-84eb-69f0d051e8b2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: cc3f2b8ec6e7c5149ef6351639fbbf8f0def8be8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283941"
---
# <a name="filter-the-view-of-dns-resource-records"></a>Filtrar a exibição de registros de recursos de DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para filtrar a exibição DNS de registros de recursos no console do cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-filter-the-view-of-dns-resource-records"></a>Para filtrar a exibição DNS de registros de recursos  
  
1.  No Gerenciador do servidor, clique em **IPAM**. Console de cliente IPAM é exibida.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  Divide o painel de navegação em um painel de navegação superior e um painel de navegação inferior.  
  
3.  No painel de navegação inferior, clique em **pesquisa direta**. Todos os gerenciados pelo IPAM DNS zonas de pesquisa direta são exibidas nos resultados da pesquisa do painel exibição.  
  
4.  Clique na zona de cujos registros que você deseja exibir e filtrar.  
  
5.  No painel de exibição, clique em **modo de exibição atual**e, em seguida, clique em **registros de recurso**. Os registros de recursos para a zona são mostrados no painel de exibição.  
  
6.  No painel de exibição, clique em **adicionar critérios**.  
  
    ![Adicionar critérios](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_01.jpg)  
  
7.  Selecione um critério na lista suspensa. Por exemplo, se você quiser exibir um tipo de registro específico, clique em **tipo de registro**.  
  
    ![Selecionar um critério](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_02.jpg)  
  
8.  Clique em **Adicionar**.  
  
    ![Adicionar critérios](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_03.jpg)  
  
9. **Tipo de registro** é adicionado como um parâmetro de pesquisa. Insira texto para o tipo de registro que você deseja localizar. Por exemplo, se você quiser exibir apenas os registros SRV, digite **SRV**.  
  
    ![Especifique o tipo de registro que você deseja encontrar](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_04.jpg)  
  
10. Pressione ENTER. Os registros de recursos DNS são filtrados de acordo com os critérios e pesquisar a frase que você especificou.  
  
    ![Executar o filtro](../../media/Filter-the-View-of-DNS-Resource-Records/ipam_FilterRR_05.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de registro de recurso DNS](DNS-Resource-Record-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


