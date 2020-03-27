---
title: Adicionar um registro de recursos de DNS
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5379373f-a3d9-4f51-b6fc-bf0f6df1d244
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b89502a7ba5ceceea10e1f7cfae9e91f6a7ead59
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316902"
---
# <a name="add-a-dns-resource-record"></a>Adicionar um registro de recursos de DNS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para adicionar um ou mais novos registros de recursos DNS usando o console do cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-add-a-dns-resource-record"></a>Para adicionar um registro de recurso DNS  
  
1.  Em Gerenciador do Servidor, clique em **IPAM**. O console do cliente IPAM é exibido.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  O painel de navegação divide-se em um painel de navegação superior e em um painel de navegação inferior.  
  
3.  No painel de navegação inferior, clique em **pesquisa direta**. Todas as zonas de pesquisa direta do DNS gerenciadas pelo IPAM são exibidas nos resultados da pesquisa do painel de exibição. Clique com o botão direito do mouse na zona em que você deseja adicionar um registro de recurso e clique em **adicionar registro de recurso DNS**.  
  
    ![Adicionar registro de recurso DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_01.jpg)
  
4.  A caixa de diálogo **adicionar registros de recurso DNS** é aberta. Em **Propriedades do registro de recurso**, clique em **servidor DNS** e selecione o servidor DNS ao qual você deseja adicionar um ou mais novos registros de recursos. Em **Configurar registros de recursos de DNS**, clique em **novo**.  
  
    ![Configurar registros de recursos de DNS](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_02.jpg)  
  
5.  A caixa de diálogo é expandida para revelar **novo registro de recurso**. Clique em **tipo de registro de recurso**.  
  
    ![Tipo de registro de recurso](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_03.jpg)  
  
6.  A lista de tipos de registros de recursos é exibida. Clique no tipo de registro de recurso que você deseja adicionar.  
  
    ![Selecione o tipo de registro a ser adicionado](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_04.jpg)  
  
7.  Em **novo registro de recurso,** em **nome**, digite um nome de registro de recurso. Em **endereço IP**, digite um endereço IP e, em seguida, selecione as propriedades de registro de recurso que são apropriadas para sua implantação. Clique em **adicionar registro de recurso**.  
  
    ![Adicionar registro de recurso](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_06.jpg)  
  
8.  Se você não quiser criar novos registros de recursos adicionais, clique em **OK**. Se você quiser criar novos registros de recursos adicionais, clique em **novo**.  
  
    ![Clique em OK ou em novo](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_01.jpg)
  
9. A caixa de diálogo é expandida para revelar **novo registro de recurso**. Clique em **tipo de registro de recurso**. A lista de tipos de registros de recursos é exibida. Clique no tipo de registro de recurso que você deseja adicionar.  
  
10. Em **novo registro de recurso,** em **nome**, digite um nome de registro de recurso. Em **endereço IP**, digite um endereço IP e, em seguida, selecione as propriedades de registro de recurso que são apropriadas para sua implantação. Clique em **adicionar registro de recurso**.  
  
    ![Adicionar registro de recurso](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_02.jpg)  
  
11. Se você quiser adicionar mais registros de recursos, repita o processo para criar registros. Quando terminar de criar novos registros de recursos, clique em **aplicar**.  
  
    ![Concluir a criação do registro de recurso](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_03.jpg)  
  
12. A caixa de diálogo **adicionar registro de recurso** exibe um resumo de registros de recursos enquanto o IPAM cria os registros de recursos no servidor DNS que você especificou. Quando os registros são criados com êxito, o **status** do registro é **êxito**.  
  
    ![Registrar status de adição](../../media/Add-a-DNS-Resource-Record/ipam_DNSrr_r2_04.jpg)  
  
13. Clique em **OK**.  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de registros de recursos de DNS](DNS-Resource-Record-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


