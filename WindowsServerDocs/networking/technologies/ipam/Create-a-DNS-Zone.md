---
title: Crie uma zona de DNS
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
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e532837e6c98694fa040a6d47a8e536eecb4c3da
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-dns-zone"></a>Crie uma zona de DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para criar uma zona DNS usando o console do cliente IPAM.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-create-a-dns-zone"></a>Para criar uma zona DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. O console de cliente IPAM aparece.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **servidores DHCP e DNS **. No painel de exibição, clique em **tipo de servidor**e clique em **DNS **. Todos os servidores DNS que são gerenciados por IPAM estão listados nos resultados da pesquisa.  
  
3.  Localize o servidor em que você deseja adicionar uma zona e clique com botão direito do servidor.  Clique em **zona DNS criar **.  
  
    ![Criar zona DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  O **criar DNS zona** caixa de diálogo é aberta. Em **propriedades gerais**, selecione uma categoria de zona, um tipo de zona e insira um nome na **nome do fuso **. Selecione também os valores apropriados para a implantação em **propriedades avançadas**e clique em **Okey **.  
  
    ![Propriedades avançadas](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de zona de DNS](DNS-Zone-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


