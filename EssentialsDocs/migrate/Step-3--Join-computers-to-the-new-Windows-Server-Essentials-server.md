---
title: 'Etapa 3: Ingressar computadores para o novo servidor Windows Server Essentials'
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a0e07d1a-8409-429b-87d7-0f4a7e14d668
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f71ac280e2de0b7d945f2d979fe52d173f7c3323
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>Etapa 3: Ingressar computadores para o novo servidor Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

A próxima etapa no processo de migração é para se conectar a computadores cliente para o novo servidor executando o Windows Server Essentials.  
  
> [!NOTE]
>  Você pode pular esta etapa para computadores que executam os sistemas operacionais Windows Vista ou Windows XP. O software do Windows Server Connector não oferece suporte a computadores que executam o Windows XP ou Windows Vista.  
  
 Antes de você pode ingressar um computador cliente para o novo servidor Windows Server Essentials, é necessário desconectá-lo do servidor de origem Desinstalando o software do Windows Server Connector no computador cliente.  
  
### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>Para desinstalar o conector do Windows Server em um computador cliente  
  
1.  Em um computador cliente, abra o painel de controle e, em seguida, abra **programas e recursos**.  
  
2.  Na lista de programas, clique com botão direito do aplicativo conector que é executado em seu computador.  
  
    > [!NOTE]
    >  O aplicativo conector pode ser **conector Windows Small Business Server 2011 Essentials**, ou **conector do Windows Server Essentials**, dependendo de qual versão do Windows Server Essentials o computador cliente foi conectado ao.  
  
3.  Clique em **desinstalar**.  
  
### <a name="to-reconnect-a-client-computer-to-the-server"></a>Um computador cliente para o servidor reconectá-lo  
  
1.  Entrar no computador em que você deseja se conectar ao servidor.  
  
    > [!NOTE]
    >  Se este computador tiver várias contas de usuário, entre usando a conta de usuário que possui documentos, fotos e preferências pessoais que você deseja manter, depois que você conecte o computador para o servidor.  
  
2.  Abra um navegador da Internet, como o Internet Explorer.  
  
3.  Na barra de endereços, digite **http://<servername\>/Connect**, e pressione ENTER.  
  
4.  Siga as instruções na tela para ingressar o computador cliente para o novo servidor Windows Server Essentials.  
  
## <a name="next-steps"></a>Próximas etapas  
 Você ingressaram computadores cliente para o novo servidor executando o Windows Server Essentials. Acesse agora [etapa 4: mova as configurações e dados para a migração do servidor de destino para o Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Para ver todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

