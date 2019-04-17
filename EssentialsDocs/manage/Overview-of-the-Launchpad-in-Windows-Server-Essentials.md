---
title: "Visão geral da barra inicial no Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 198d16cb-3d07-4706-be89-ad14a5f7dc47
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 9c240320d990652a4669499d99c1fc3eba9e06fa
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="overview-of-the-launchpad-in-windows-server-essentials"></a>Visão geral da barra inicial no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

O Windows Server Essentials Launchpad é um pequeno aplicativo que é instalado em um computador na primeira vez que o computador se conecta ao servidor. Barra inicial fornece aos usuários autenticados acessem os principais recursos do Windows Server Essentials, incluindo backups do computador, arquivos compartilhados e mídia e o site de acesso via Web remoto. Os usuários podem acessar esses recursos de computadores de domínio ou computadores conectados sem domínio. Barra inicial também fornece informações em tempo real e notificações sobre a integridade do computador. Os administradores podem usar a barra inicial para acessar o servidor Dashboard, mesmo se o computador não está conectado à rede.  
  
 Os OEMs e fornecedores independentes de Software (ISVs) que desenvolvem suplementos para o Windows Server Essentials podem usar a barra inicial para estender a funcionalidade de suplementos para computadores na rede.  
  
 Os seguintes sistemas operacionais Windows suporta o uso do Windows Server Essentials Launchpad:  
  
-   **Windows 8**: todas as edições.  
  
-   **Windows 7**: todas as edições.  
-   **Windows 10**: todas as edições. 
  
 Os seguintes sistemas operacionais não suporta o uso do Windows Server Essentials Launchpad:  
  
-   **Servidores adicionais**: você não pode executar o Windows Server Essentials Launchpad em quaisquer outros computadores que executam um sistema operacional Windows Server.  
  
 Neste tópico:  
  
-   [Uso da barra inicial](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Launchpad)  
  
-   [Use a barra inicial com um computador Mac](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  
  
##  <a name="BKMK_Launchpad"></a>Uso da barra inicial  
 Os links e as informações a seguir estão disponíveis sobre o Windows Server Essentials Launchpad.  
  
### <a name="backup"></a>Backup  
 Clique em **Backup** para abrir o **Backup Properties** para o computador. Sobre o **Backup Properties** página, você pode:  
  
-   Iniciar ou parar um backup.  
  
-   Exiba o status e os detalhes para o backup mais recente.  
  
-   Especifica como gerenciar o consumo de energia do computador quando o backup é executado.  
  
 Para obter informações sobre como usar a barra inicial para fazer backup do computador, consulte [gerenciar o Backup do cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  
  
### <a name="remote-web-access"></a>Acesso via Web remoto  
 Clique em **acesso via Web remoto** para abrir o navegador da web para o site de acesso via Web remoto. O site de acesso via Web remoto permite que você se conectar a outros computadores e acessar alguns dos recursos de rede de dentro do escritório ou de qualquer local remoto com um computador habilitado para Internet. Para obter mais informações sobre o acesso via Web remoto, consulte [gerenciar o acesso remoto via Web](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
### <a name="shared-folders"></a>Pastas compartilhadas  
 Clique em **pastas compartilhadas** para abrir o Windows Explorer para o local das pastas compartilhadas no servidor. Para obter informações sobre como compartilhar arquivos e pastas, consulte o tópico [gerenciar pastas de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md).  
  
### <a name="dashboard"></a>Painel  
 Clique em **painel** para abrir o **login** página para acessar o painel do Windows Server Essentials. Depois de se conectar, abre uma conexão de área de trabalho remota para o servidor de painel. Para obter mais informações sobre o painel, veja [visão geral do painel](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Para usar esse recurso, você deve ter o acesso apropriado ou as permissões para fazer logon no servidor.  
  
### <a name="microsoft-office-365"></a>Microsoft Office 365  
 O **Microsoft Office 365** link só aparece na barra inicial, se o usuário tiver uma conta do Office 365. Clique em **Microsoft Office 365** para acessar links adicionais para os recursos do Office 365. Para obter mais informações, consulte [guia de início rápido usando o Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
### <a name="computer-health-alerts"></a>Alertas de integridade do computador  
 Os alertas que aparecem na barra inicial fornecem um status rápido sobre a integridade imediata do computador. Para exibir informações sobre um alerta de saúde, clique em um indicador de alerta para abrir o Visualizador de alerta. Alertas de integridade aparecem no Visualizador de com base no nível de gravidade. Os alertas mais graves aparecem primeiros na lista; alertas menos graves aparecem mais tarde na lista. Para saber mais sobre alertas de integridade do computador, consulte [gerenciar a integridade do sistema](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
##  <a name="BKMK_Mac"></a>Use a barra inicial com um computador Mac  
 Você pode se conectar a um computador Mac® executando Mac OS X® 10.5 ou posterior para Windows Server 2012 R2, Windows Server Essentials ou Windows Server Essentials ou baixando e instalando o software connector. Quando você terminar de instalar o software do conector, você pode optar por iniciar automaticamente a barra inicial no momento da inicialização.  
  
 Barra inicial é um pequeno aplicativo que fornece aos usuários autenticados acessem os principais recursos do servidor, incluindo arquivos compartilhados e mídia, suplementos e acesso via Web remoto. Barra inicial também fornece informações em tempo real e notificações sobre a integridade do computador.  
  
> [!NOTE]
>  Os administradores de servidor não podem usar a barra inicial ou o acesso via Web remoto em um computador Mac para abrir o painel de servidor e gerenciar o servidor.  
  
### <a name="backup"></a>Backup  
 Clique em **Backup** para configurar a máquina do tempo para fazer backup do computador e alterar as configurações de máquina do tempo. Para obter mais informações sobre a máquina do tempo, consulte a documentação do fabricante do computador.  
  
### <a name="remote-web-access"></a>Acesso via Web remoto  
 Clique em **acesso via Web remoto** para abrir o navegador da web para o site de acesso via Web remoto. O acesso via Web remoto permite que você acesse os arquivos e pastas compartilhados no servidor de qualquer local remoto com um computador habilitado para Internet. Você pode fazer upload de arquivos, reproduzir músicas e vídeos no baseada na web mídia reproduzir, exibir imagens e reproduzir apresentações de slides. Para obter mais informações, consulte [acesso via Web remoto de uso](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
### <a name="shared-folders"></a>Pastas compartilhadas  
 Clique em **pastas compartilhadas** abrir Finder para o local das pastas compartilhadas no servidor. Para obter informações sobre como compartilhar arquivos e pastas, consulte [usar pastas compartilhadas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
### <a name="computer-health-alerts"></a>Alertas de integridade do computador  
 Os alertas que aparecem na barra inicial fornecem um status rápido sobre a integridade imediata do computador. Para exibir informações sobre um alerta de saúde, clique em um indicador de alerta para abrir o Visualizador de alerta. Alertas de integridade aparecem no Visualizador de com base no nível de gravidade. Os alertas mais graves aparecem primeiros na lista. Alertas menos graves aparecem mais tarde na lista.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Se conectar](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
