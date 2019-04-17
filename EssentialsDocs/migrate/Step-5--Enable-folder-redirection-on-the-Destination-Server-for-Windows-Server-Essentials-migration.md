---
title: "Etapa 5: Habilitar o redirecionamento de pasta sobre a migração do servidor de destino para o Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3925f80-552d-431f-b2a6-2af202e50ca4
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 613ff4c80a80ed4f3207cb0c1ead6db12c723e85
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>Etapa 5: Habilitar o redirecionamento de pasta sobre a migração do servidor de destino para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Se o redirecionamento de pasta está habilitado no servidor de origem, você pode habilitar o redirecionamento de pasta no servidor de destino e, em seguida, excluir a configuração de política de grupo de redirecionamento de pasta antiga.  
  
 Primeiro, use o painel do Windows Server Essentials para habilitar o redirecionamento de pasta no servidor de destino. Em seguida, exclua a configuração de política de grupo de redirecionamento de pasta antigos.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Para habilitar o redirecionamento de pasta no servidor de destino  
  
1.  No servidor de destino, abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **dispositivos**.  
  
3.  No **dispositivos tarefas** painel, clique em **política de grupo de implementar**.  
  
4.  Sobre o **permitir política de grupo de redirecionamento de pasta** página, selecione as pastas para ser redirecionado e, em seguida, clique em **próxima**.  
  
5.  Sobre o **ativar configurações de política de segurança** página, clique em **concluir**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Para excluir a configuração de política de grupo de redirecionamento de pasta antiga  
  
1.  No servidor de destino, abra o **Group Policy Management** ferramenta administrativa.  
  
2.  Em **Group Policy Management**, expanda **floresta:***YourNetworkDomainName*, expanda **domínios**, expanda *YourNetworkDomainName*e, em seguida, expanda **objetos de política de grupo**.  
  
3.  Clique com botão direito a política que você deseja excluir e clique em **excluir**.  
  
4.  Leia o aviso e clique em **Sim**.  
  
5.  Fechar **gerenciamento de política de grupo**.  
  
 Para aplicar a mudança para o redirecionamento de pasta, os usuários da rede devem fazer logoff seus computadores e, em seguida, faça logon novamente. Isso garante a transferência de todas as pastas redirecionadas para o servidor de destino.  
  
## <a name="next-steps"></a>Próximas etapas  
 Você tiver habilitado o redirecionamento de pasta no servidor de destino. Acesse agora [etapa 6: rebaixar e remova o servidor de origem a nova rede do Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  
  

Para ver todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

