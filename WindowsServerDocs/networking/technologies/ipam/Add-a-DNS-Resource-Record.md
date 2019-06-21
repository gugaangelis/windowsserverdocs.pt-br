---
title: Adicionar um registro de recursos de DNS
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 36773525187229e498b9addf4b1e6532fd413701
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282319"
---
# <a name="add-a-dns-resource-record"></a>Adicionar um registro de recursos de DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para adicionar um ou mais novos registros de recursos usando o console de cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-add-a-dns-resource-record"></a>Para adicionar um registro de recurso DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. Console de cliente IPAM é exibida.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  Divide o painel de navegação em um painel de navegação superior e um painel de navegação inferior.  
  
3.  No painel de navegação inferior, clique em **pesquisa direta**. Todos os gerenciados pelo IPAM DNS zonas de pesquisa direta são exibidas nos resultados da pesquisa do painel exibição. A zona em que você deseja adicionar um registro de recurso e, em seguida, clique com o botão direito **registro de recurso DNS adicionar**.  
  
    ![Adicionar um registro de recurso DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  O **adicionar registros de recursos DNS** caixa de diálogo é aberta. Na **propriedades de registro de recurso**, clique em **servidor DNS** e selecione o servidor DNS no qual você deseja adicionar um ou mais novos registros de recursos. Na **registros de recursos DNS Configure**, clique em **New**.  
  
    ![Configurar registros de recurso DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  A caixa de diálogo é expandida para revelar **novo registro de recurso**. Clique em **tipo de registro de recurso**.  
  
    ![Tipo de registro de recurso](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  A lista de tipos de registro de recurso é exibida. Clique no tipo de registro de recurso que você deseja adicionar.  
  
    ![Selecione o tipo de registro para adicionar](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  Na **novo registro de recurso** na **nome**, digite um nome de registro de recurso. Na **endereço IP**, digite um endereço IP e, em seguida, selecione as propriedades de registro de recurso que são apropriadas para sua implantação. Clique em **adicionar registro de recurso**.  
  
    ![Adicionar registro de recurso](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  Se você não quiser criar novos registros de recursos adicionais, clique em **Okey**. Se você quiser criar novos registros de recursos adicionais, clique em **New**.  
  
    ![Clique no botão Okey ou novo](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. A caixa de diálogo é expandida para revelar **novo registro de recurso**. Clique em **tipo de registro de recurso**. A lista de tipos de registro de recurso é exibida. Clique no tipo de registro de recurso que você deseja adicionar.  
  
10. Na **novo registro de recurso** na **nome**, digite um nome de registro de recurso. Na **endereço IP**, digite um endereço IP e, em seguida, selecione as propriedades de registro de recurso que são apropriadas para sua implantação. Clique em **adicionar registro de recurso**.  
  
    ![Adicionar registro de recurso](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. Se você quiser adicionar mais registros de recursos, repita o processo para a criação de registros. Quando você terminar de criar novos registros de recursos, clique em **aplicar**.  
  
    ![Criação de registro de recurso completo](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. O **registro de recurso de adicionar** caixa de diálogo exibe um resumo de registros de recursos, enquanto o IPAM cria os registros de recursos no servidor DNS que você especificou. Quando os registros são criados com êxito, o **Status** do registro é **sucesso**.  
  
    ![Status de registro de adição](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. Clique em **OK**.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de registro de recurso DNS](DNS-Resource-Record-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


