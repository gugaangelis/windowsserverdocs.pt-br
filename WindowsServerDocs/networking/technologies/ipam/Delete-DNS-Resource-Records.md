---
title: Excluir registros de recursos de DNS
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
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
ms.openlocfilehash: 2d3c5f4f02cc1a8386bf12fe634620ba98f2f23a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883217"
---
# <a name="delete-dns-resource-records"></a>Excluir registros de recursos de DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para excluir um ou mais registros de recursos DNS usando o console de cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
### <a name="to-delete-dns-resource-records"></a>Para excluir registros de recurso DNS  
  
1.  No Gerenciador do servidor, clique em **IPAM**. Console de cliente IPAM é exibida.  
  
2.  No painel de navegação, em **monitorar e gerenciar**, clique em **zonas DNS**.  Divide o painel de navegação em um painel de navegação superior e um painel de navegação inferior.  
  
3.  Clique para expandir **pesquisa direta** e o domínio onde se encontram os registros de zona e recursos que você deseja excluir. Clique na região e no painel de exibição, clique em **modo de exibição atual**. Clique em **registros de recurso**.  
  
4.  No painel de exibição, localize e selecione os registros de recursos que você deseja excluir.  
  
    ![Selecione os registros de recurso para excluir](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_01.jpg)  
  
5.  Os registros selecionados com o botão direito e, em seguida, clique em **registro de recurso DNS excluir**.  
  
    ![Excluir os registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_02.jpg)  
  
6.  O **excluir registro de recurso DNS** caixa de diálogo é aberta. Verifique se o servidor DNS correto está selecionado. Se não estiver, clique em **servidor DNS** e selecione o servidor do qual você deseja excluir os registros de recursos. Clique em **OK**. IPAM exclui os registros de recursos do servidor DNS.  
  
    ![Verificar se o servidor DNS correto está selecionado e excluir registros](../../media/Delete-DNS-Resource-Records/ipam_DeleteRR_03.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Gerenciamento de registro de recurso DNS](DNS-Resource-Record-Management.md)  
[Gerenciar o IPAM](Manage-IPAM.md)  
  


