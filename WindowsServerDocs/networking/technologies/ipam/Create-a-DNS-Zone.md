---
title: Criar uma zona DNS
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a030ff51-a815-4fc4-b26d-aae41c3e4ce5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbd46ad129a4b3e5bbbe55f584f1bae43bd077c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355419"
---
# <a name="create-a-dns-zone"></a>Criar uma zona DNS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para criar uma zona DNS usando o console do cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-create-a-dns-zone"></a>Para criar uma zona DNS  
  
1.  Em Gerenciador do Servidor, clique em **IPAM**. O console do cliente IPAM é exibido.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **servidores DNS e DHCP**. No painel de exibição, clique em **tipo de servidor**e, em seguida, clique em **DNS**. Todos os servidores DNS gerenciados pelo IPAM são listados nos resultados da pesquisa.  
  
3.  Localize o servidor no qual você deseja adicionar uma zona e clique com o botão direito do mouse no servidor.  Clique em **criar zona DNS**.  
  
    ![Criar zona DNS](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_01a.jpg)  
  
4.  A caixa de diálogo **criar zona DNS** é aberta. Em **Propriedades gerais**, selecione uma categoria de zona, um tipo de zona e insira um nome em **nome da zona**. Selecione também os valores apropriados para sua implantação em **Propriedades avançadas**e clique em **OK**.  
  
    ![Propriedades avançadas](../../media/Create-a-DNS-Zone/ipam_CreateDNSZone_02a.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de zona DNS](DNS-Zone-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


