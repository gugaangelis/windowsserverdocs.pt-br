---
title: Excluir registros de recurso do DNS
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
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9ebfeca1da9e36cd00272113f2e86c33174074b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="delete-dns-resource-records"></a>Excluir registros de recurso do DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para excluir um ou mais registros de recurso DNS usando o console do cliente IPAM.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-delete-dns-resource-records"></a>Para excluir registros de recurso do DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. O console de cliente IPAM aparece.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  Divide o painel de navegação em um painel de navegação superior e um painel de navegação inferior.  
  
3.  Clique para expandir **pesquisa direta** e o domínio onde estão localizados os registros de zona e recursos que você deseja excluir. Clique na região e, no painel de exibição, clique em **exibição atual**. Clique em **registros de recurso**.  
  
4.  No painel de exibição, localize e selecione os registros de recurso que você deseja excluir.  
  
    ![Selecione os registros de recurso para excluir](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Clique com botão direito os registros selecionados e, em seguida, clique em **registro de recurso DNS excluir**.  
  
    ![Excluir os registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  O **excluir registro de recurso DNS** caixa de diálogo é aberta. Verifique se o servidor DNS correto está selecionado. Se não estiver, clique em **servidor DNS** e selecione o servidor do qual você deseja excluir os registros de recurso. Clique em **Okey**. IPAM exclui os registros de recurso do servidor DNS.  
  
    ![Verifique se o servidor DNS correto está selecionado e excluir registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de registros de recurso do DNS](DNS-Resource-Record-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


