---
title: Excluir registros de recursos de DNS
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 366e6fd5-d563-4de3-9551-5614cbb8f2cb
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8c93af6a5d43086320ab27982f7a0c0308ee551d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312460"
---
# <a name="delete-dns-resource-records"></a>Excluir registros de recursos de DNS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para excluir um ou mais registros de recursos DNS usando o console do cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-delete-dns-resource-records"></a>Para excluir registros de recursos de DNS  
  
1.  Em Gerenciador do Servidor, clique em **IPAM**. O console do cliente IPAM é exibido.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  O painel de navegação divide-se em um painel de navegação superior e em um painel de navegação inferior.  
  
3.  Clique para expandir a **pesquisa direta** e o domínio no qual os registros de zona e de recursos que você deseja excluir estão localizados. Clique na zona e, no painel de exibição, clique em **exibição atual**. Clique em **registros de recursos**.  
  
4.  No painel de exibição, localize e selecione os registros de recursos que você deseja excluir.  
  
    ![Selecionar registros de recursos a serem excluídos](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Clique com o botão direito do mouse nos registros selecionados e clique em **Excluir registro de recurso DNS**.  
  
    ![Excluir os registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  A caixa de diálogo **Excluir registro de recurso DNS** é aberta. Verifique se o servidor DNS correto está selecionado. Se não estiver, clique em **servidor DNS** e selecione o servidor do qual você deseja excluir os registros de recursos. Clique em **OK**. O IPAM exclui os registros de recursos do servidor DNS.  
  
    ![Verifique se o servidor DNS correto está selecionado e exclua os registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de registros de recursos de DNS](DNS-Resource-Record-Management.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


