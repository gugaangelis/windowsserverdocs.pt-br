---
title: 'Etapa 3: Fazer computadores ingressarem no novo servidor do Windows Server Essentials'
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861867"
---
# <a name="step-3-join-computers-to-the-new-windows-server-essentials-server"></a>Etapa 3: Fazer computadores ingressarem no novo servidor do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

A próxima etapa no processo de migração é conectar os computadores cliente para o novo servidor que executa o Windows Server Essentials.  
  
> [!NOTE]
>  Você pode pular esta etapa para computadores que executam os sistemas operacionais Windows Vista ou Windows XP. O software Connector do Windows Server não dá suporte a computadores que executam o Windows XP ou Windows Vista.  
  
 Antes de adicionar um computador cliente para o novo servidor do Windows Server Essentials, você deve desconectá-lo do servidor de origem Desinstalando o software Connector do Windows Server no computador cliente.  
  
### <a name="to-uninstall-windows-server-connector-on-a-client-computer"></a>Para desinstalar o Windows Server Connector de um computador cliente  
  
1.  Em um computador cliente, abra o painel de controle e, em seguida, abra **Programas e recursos**.  
  
2.  Na lista de programas, clique no aplicativo Connector que está em execução no seu computador.  
  
    > [!NOTE]
    >  O aplicativo Connector pode ser **Windows Small Business Server 2011 Essentials Connector**, ou **Windows Server Essentials Connector**, dependendo de qual versão do Windows Server Essentials a computador cliente foi conectado.  
  
3.  Clique em **Desinstalar**.  
  
### <a name="to-reconnect-a-client-computer-to-the-server"></a>Para reconectar um computador cliente ao servidor  
  
1.  Autentique-se no computador que você deseja conectar ao servidor.  
  
    > [!NOTE]
    >  Caso esse computador tenha várias contas de usuário, autentique-se com a conta de usuário cujos documentos, imagens e preferências pessoais você deseja manter depois de conectar o computador ao servidor.  
  
2.  Abra um navegador de Internet, como o Internet Explorer.  
  
3.  Na barra de endereços, digite **http://<servername\>/Connect**, e pressione ENTER.  
  
4.  Siga as instruções na tela para ingressar o computador cliente para o novo servidor do Windows Server Essentials.  
  
## <a name="next-steps"></a>Próximas etapas  
 Você uniu seus computadores cliente para o novo servidor que executa o Windows Server Essentials. Agora vá para [etapa 4: Mover configurações e dados para a migração do servidor de destino para o Windows Server Essentials](Step-4--Move-settings-and-data-to-the-Destination-Server-for-Windows-Server-Essentials-migration.md).  
  

Para exibir todas as etapas, consulte [migrar para o Windows Server Essentials](Migrate-from-Previous-Versions-to-Windows-Server-Essentials-or-Windows-Server-Essentials-Experience.md).

