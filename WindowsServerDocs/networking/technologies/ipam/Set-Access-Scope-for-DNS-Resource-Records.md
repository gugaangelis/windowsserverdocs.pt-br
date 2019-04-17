---
title: Defina o escopo de acesso para registros de recurso do DNS
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
ms.assetid: a96a8752-5678-49c5-b069-d2cce8042a51
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 06c33a633975497e80863cc8d42b14a0f9ac8193
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="set-access-scope-for-dns-resource-records"></a>Defina o escopo de acesso para registros de recurso do DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para definir o escopo de acesso para registros de recurso DNS usando o console de cliente IPAM.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-set-access-scope-for-dns-resource-records"></a>Para definir o escopo de acesso para registros de recurso do DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. O console de cliente IPAM aparece.  
  
2.  No painel de navegação, clique em **zonas DNS**.  No painel de navegação inferior, expanda **pesquisa direta** e procure e selecione a zona que contém os registros de recurso escopo cujo acesso você deseja alterar.  
  
3.  No painel de exibição, localize e selecione os registros de recurso escopo cujo acesso você deseja alterar.  
  
    ![Selecione os registros de recurso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_02.jpg)  
  
4.  Clique com botão direito os registros de recurso DNS selecionados e clique em **definir o escopo de acesso**.  
  
    ![Defina o escopo de acesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_03.jpg)  
  
5.  O **definir o escopo de acesso** caixa de diálogo é aberta. Se necessário para a implantação, clique para desmarcar **Inherit escopo de acesso do pai**. Em **selecione o escopo do acesso**, selecione um item e, em seguida, clique em **Okey**.  
  
    ![Selecione o escopo de acesso](../../media/Set-Access-Scope-for-DNS-Resource-Records/ipam_RestrictUserToRRControl_04.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso baseado em função](Role-based-Access-Control.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


