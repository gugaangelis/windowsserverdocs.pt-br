---
title: 'Etapa 5: Habilitar o redirecionamento de pasta na migração de servidor de destino para o Windows Server Essentials'
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815377"
---
# <a name="step-5-enable-folder-redirection-on-the-destination-server-for-windows-server-essentials-migration"></a>Etapa 5: Habilitar o redirecionamento de pasta na migração de servidor de destino para o Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Caso o redirecionamento de pastas esteja habilitado no servidor de origem, você poderá habilitá-lo no servidor de destino e excluir a antiga configuração de Política de Grupo de Redirecionamento de Pastas.  
  
 Primeiro, use o painel do Windows Server Essentials para habilitar o redirecionamento de pasta no servidor de destino. Então, exclua a antiga configuração da Política de Grupo de Redirecionamento de Pasta.  
  
### <a name="to-enable-folder-redirection-on-the-destination-server"></a>Para habilitar o redirecionamento de pasta no servidor de destino  
  
1.  No servidor de destino, abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **DISPOSITIVOS**.  
  
3.  No painel **Tarefas de Dispositivos**, clique em **Implementar Política de Grupo**.  
  
4.  Na página **Habilitar Política de Grupo de Redirecionamento de Pasta**, selecione as pastas a serem redirecionadas e, em seguida, clique em **Avançar**.  
  
5.  Na página **Habilitar Configurações de Política de Segurança** , clique em **Concluir**.  
  
### <a name="to-delete-the-old-folder-redirection-group-policy-setting"></a>Para excluir a antiga configuração da Política de Grupo de Redirecionamento de Pasta  
  
1.  No servidor de destino, abra a ferramenta administrativa **Gerenciamento de Política de Grupo**.  
  
2.  Na **gerenciamento de política de grupo**, expanda **floresta:***YourNetworkDomainName*, expanda **domínios**, expanda *YourNetworkDomainName* e, em seguida, expanda **objetos de diretiva de grupo**.  
  
3.  Clique com o botão direito na política que você deseja excluir e, em seguida, clique em **Excluir**.  
  
4.  Leia o aviso e depois clique em **Sim**.  
  
5.  Feche o **Gerenciamento de Política de Grupo**.  
  
 Para aplicar a alteração no redirecionamento de pasta, os usuários da rede devem fazer logoff de seus computadores e depois fazer logon novamente. Isso garante a transferência de todas as pastas redirecionadas para o servidor de destino.  
  
## <a name="next-steps"></a>Próximas etapas  
 Você ativou o redirecionamento de pasta no servidor de destino. Agora vá para [etapa 6: Rebaixar e remover o servidor de origem da nova rede do Windows Server Essentials](Step-6--Demote-and-remove-the-Source-Server-from-the-new-Windows-Server-Essentials-network.md).  
  

Para exibir todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

