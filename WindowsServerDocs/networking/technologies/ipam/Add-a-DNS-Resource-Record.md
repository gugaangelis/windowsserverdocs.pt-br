---
title: Adicionar um registro de recurso DNS
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
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e14a59e9f172b20e85a34d2299e3733a796adafc
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="add-a-dns-resource-record"></a>Adicionar um registro de recurso DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para adicionar um ou mais registros de recurso DNS novo usando o console do cliente IPAM.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-add-a-dns-resource-record"></a>Para adicionar um registro de recurso DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. O console de cliente IPAM aparece.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  Divide o painel de navegação em um painel de navegação superior e um painel de navegação inferior.  
  
3.  No painel de navegação inferior, clique em **pesquisa direta**. Todas as regiões gerenciado IPAM DNS para a frente pesquisa são exibidas nos resultados da pesquisa de painel de exibição. Clique com botão direito a zona em que você deseja adicionar um registro de recurso e, em seguida, clique em **registro de recurso DNS adicionar**.  
  
    ![Adicione o registro de recurso DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  O **adicionar registros de recurso DNS** caixa de diálogo é aberta. Em **propriedades de registro de recurso**, clique em **servidor DNS** e selecione o servidor DNS onde você deseja adicionar um ou mais registros de recurso de novo. Em **registros de recurso de configurar DNS**, clique em **nova**.  
  
    ![Configurar os registros de recurso do DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  A caixa de diálogo possa ser expandido para revelar **novo registro de recurso**. Clique em **tipo de registro de recurso**.  
  
    ![Tipo de registro de recurso](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  A lista de tipos de registro de recursos é exibida. Clique no tipo de registro de recurso que você deseja adicionar.  
  
    ![Selecione o tipo de registro para adicionar](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  Em **novo registro de recurso,** em **nome**, digite um nome de registro de recurso. Em **endereço IP**, digite um endereço IP e, em seguida, selecione as propriedades de registro de recurso que são apropriadas para a implantação. Clique em **adicionar registro de recurso**.  
  
    ![Adicione o registro de recurso](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  Se você não quiser criar novos registros de recurso adicional, clique em **Okey**. Se você quiser criar novos registros de recurso adicional, clique em **nova**.  
  
    ![Clique em novo ou Okey](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. A caixa de diálogo possa ser expandido para revelar **novo registro de recurso**. Clique em **tipo de registro de recurso**. A lista de tipos de registro de recursos é exibida. Clique no tipo de registro de recurso que você deseja adicionar.  
  
10. Em **novo registro de recurso,** em **nome**, digite um nome de registro de recurso. Em **endereço IP**, digite um endereço IP e, em seguida, selecione as propriedades de registro de recurso que são apropriadas para a implantação. Clique em **adicionar registro de recurso**.  
  
    ![Adicione o registro de recurso](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. Se você quiser adicionar mais registros de recurso, repita o processo de criação de registros. Quando terminar de criar novos registros de recurso, clique em **aplicar**.  
  
    ![Criação de registro de recurso completo](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. O **registro de recurso Adicionar** caixa de diálogo exibe um resumo de registros de recurso enquanto IPAM cria os registros de recurso no servidor DNS que você especificou. Quando os registros são criados com êxito, o **Status** do registro é **sucesso**.  
  
    ![Status de adição do registro](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. Clique em **Okey**.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de registros de recurso do DNS](DNS-Resource-Record-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


