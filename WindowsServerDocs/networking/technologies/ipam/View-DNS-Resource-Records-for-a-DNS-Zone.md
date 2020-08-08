---
title: Exibir registros de recurso de DNS para uma zona DNS
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 375feefc-949e-47c3-9e61-35b79e021966
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0395033fe009a6bb787761ab3ec47604c94ba6b1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944638"
---
# <a name="view-dns-resource-records-for-a-dns-zone"></a>Exibir registros de recurso de DNS para uma zona DNS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para exibir registros de recursos de DNS para uma zona DNS no console do cliente IPAM.

A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.

### <a name="to-view-dns-resource-records-for-a-zone"></a>Para exibir registros de recursos de DNS para uma zona

1.  Em Gerenciador do Servidor, clique em **IPAM**. O console do cliente IPAM é exibido.

2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  O painel de navegação divide-se em um painel de navegação superior e em um painel de navegação inferior.

3.  No painel de navegação inferior, clique em **pesquisa direta**e, em seguida, expanda a lista domínio e zona para localizar e selecionar a zona que você deseja exibir. Por exemplo, se você tiver uma zona chamada Dublin, clique em **Dublin**.

    ![Selecione a zona que você deseja exibir](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01a.jpg)


4.  No painel de exibição, a exibição padrão é dos servidores DNS da zona. Para alterar a exibição, clique em **modo de exibição atual**e em **registros de recursos**.

    ![Alterar a exibição para registros de recursos](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_Zone_RR_02.jpg)

5.  Os registros de recursos de DNS para a zona são exibidos. Para filtrar os registros, digite o texto que você deseja localizar no **filtro**.

    ![Digite o texto para filtrar os registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01c.jpg)

6.  Para filtrar os registros de recursos por tipo de registro, escopo de acesso ou outros critérios, clique em **Adicionar critérios**e faça seleções na lista critérios e clique em **Adicionar**.

    ![Usar critérios para filtrar registros](../../media/View-DNS-Resource-Records-for-a-DNS-Zone/ipam_DNSzones_01d.jpg)

## <a name="see-also"></a>Consulte Também
Gerenciamento de zona [DNS](DNS-Zone-Management.md) 
 [Gerenciar IPAM](Manage-IPAM.md)



