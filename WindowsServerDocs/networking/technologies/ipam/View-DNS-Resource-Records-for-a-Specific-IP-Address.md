---
title: Exibir registros de recurso de DNS para um endereço IP específico
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: f590fb86-4195-4f90-98cb-e90459d4c1e3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 211dcfdc17cfa81aad7adb1a424a9fadc0c932c6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944597"
---
# <a name="view-dns-resource-records-for-a-specific-ip-address"></a>Exibir registros de recurso de DNS para um endereço IP específico

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para exibir os registros de recursos de DNS que estão associados ao endereço IP que você escolher.

A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.

### <a name="to-view-resource-records-for-an-ip-address"></a>Para exibir registros de recursos para um endereço IP

1.  Em Gerenciador do Servidor, clique em **IPAM**. O console do cliente IPAM é exibido.

2.  No painel de navegação, em **espaço de endereço IP**, clique em **inventário de endereço IP**. No painel de navegação inferior, clique em **IPv4** ou **IPv6**. O inventário de endereço IP aparece na exibição de pesquisa do painel de exibição. Localize e selecione o endereço IP cujos registros de recursos DNS você deseja exibir.

    ![Exibir o inventário de endereços IP](../../media/View-DNS-Resource-Records-for-a-Specific-IP-Address/ipam_IPInventory_01.jpg)

3.  No modo de exibição **detalhes**do painel de exibição, clique em **registros de recursos DNS**. Os registros de recursos associados ao endereço IP selecionado são exibidos.

    ![Exibir registros de recursos de DNS](../../media/View-DNS-Resource-Records-for-a-Specific-IP-Address/ipam_IPInventory_02.jpg)

## <a name="see-also"></a>Consulte Também
Gerenciamento de registros de [recursos de DNS](DNS-Resource-Record-Management.md) 
 [Gerenciar IPAM](Manage-IPAM.md)



