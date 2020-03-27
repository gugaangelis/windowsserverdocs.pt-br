---
title: Visão geral da Barra Inicial no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 198d16cb-3d07-4706-be89-ad14a5f7dc47
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 63a161057f7068dcb9e02faa353270f0150200b4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310659"
---
# <a name="overview-of-the-launchpad-in-windows-server-essentials"></a>Visão geral da Barra Inicial no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

A Barra Inicial do Windows Server Essentials é um pequeno aplicativo que é instalado em um computador na primeira vez em que o computador se conecta ao servidor. A Barra Inicial fornece aos usuários autenticados acesso aos principais recursos do Windows Server Essentials, incluindo backups de computador, arquivos compartilhados e mídia e o site de Acesso Remoto via Web. Os usuários podem acessar esses recursos a partir de computadores que ingressaram no domínio ou computadores associados fora do domínio. A Barra Inicial fornece também notificações e informações em tempo real sobre a integridade do computador. Os administradores podem usar a Barra Inicial para acessar o Painel do servidor, mesmo se o computador não estiver conectado à rede.  
  
 Os OEMs e fornecedores independentes de Software (ISVs) que desenvolvem suplementos para o Windows Server Essentials podem usar a Barra Inicial para estender a funcionalidade de suplemento para computadores na rede.  
  
 Os seguintes sistemas operacionais do Windows oferecem suporte ao uso da Barra Inicial do Windows Server Essentials:  
  
- **Windows 8**: Todas as edições.  
  
- **Windows 7**: Todas as edições.  
- **Windows 10**: todas as edições. 
  
  Os seguintes sistemas operacionais não têm suporte para o uso da Barra Inicial do Windows Server Essentials:  
  
- **Servidores adicionais**: Você não pode executar a Barra Inicial do Windows Server Essentials em quaisquer outros computadores que executam um sistema operacional Windows Server.  
  
  Neste tópico:  
  
- [Usar o Launchpad](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Launchpad)  
  
- [Usar o Launchpad com um computador Mac](Overview-of-the-Launchpad-in-Windows-Server-Essentials.md#BKMK_Mac)  
  
##  <a name="use-the-launchpad"></a><a name="BKMK_Launchpad"></a>Usar o Launchpad  
 Os links e informações a seguir estão disponíveis na Barra Inicial do Windows Server Essentials.  
  
### <a name="backup"></a>Backup  
 Clique em **Backup** para abrir as **Propriedades de Backup** do computador. Na página **Propriedades de Backup**, você pode:  
  
- Iniciar ou interromper um backup.  
  
- Exibir o status e os detalhes do backup mais recente.  
  
- Especificar como gerenciar a energia do computador quando o backup é executado.  
  
  Para obter informações sobre como usar o Launchpad para fazer backup de seu computador, consulte [gerenciar o backup do cliente](Manage-Client-Computer-Backup-in-Windows-Server-Essentials.md).  
  
### <a name="remote-web-access"></a>Acesso Web remoto  
 Clique em **Acesso via Web remoto** para abrir o navegador da Web no site do Acesso via Web remoto. O site de Acesso via Web remoto permite que você se conecte a outros computadores e acesse alguns dos recursos de rede de dentro do escritório ou de qualquer local remoto com um computador conectado à Internet. Para obter mais informações sobre Acesso via Web remotos, consulte [gerenciar acesso via Web remoto](Manage-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
### <a name="shared-folders"></a>Pastas Compartilhadas  
 Clique em **Pastas compartilhadas** para abrir o Windows Explorer no local das pastas compartilhadas no servidor. Para obter informações sobre como compartilhar arquivos e pastas, consulte o tópico [gerenciar pastas de servidor](Manage-Server-Folders-in-Windows-Server-Essentials.md).  
  
### <a name="dashboard"></a>Painel  
 Clique em  **Painel** para abrir a página **Entrar** para acessar o Painel do Windows Server Essentials. Após você entrar, uma conexão de área de trabalho remota com o Painel do servidor é aberta. Para obter mais informações sobre o painel, consulte [visão geral do painel](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).  
  
> [!NOTE]
>  Para usar esse recurso, você deve ter o acesso ou permissões apropriados para fazer logon no servidor.  
  
### <a name="microsoft-office-365"></a>Microsoft Office 365  
 O link para o **Microsoft Office 365** só aparece na Barra Inicial se o usuário tiver uma conta do Office 365. Clique em  **Microsoft Office 365** para acessar links adicionais para os recursos do Office 365. Para obter mais informações, consulte [Guia de início rápido usar Microsoft Office 365](../use/Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).  
  
### <a name="computer-health-alerts"></a>Alertas de integridade do computador  
 Os alertas que aparecem na Barra Inicial fornecem um status rápido da integridade imediata do computador. Para exibir informações sobre um alerta de integridade, clique em um indicador de alerta para abrir o visualizador de alertas. Alertas de integridade são exibidos no visualizador com base no nível de gravidade. Os alertas mais graves aparecem primeiro na lista; alertas menos graves aparecem posteriormente na lista. Para obter mais informações sobre alertas de integridade do computador, consulte [gerenciar a integridade do sistema](Manage-System-Health-in-Windows-Server-Essentials.md).  
  
##  <a name="use-the-launchpad-with-a-mac-computer"></a><a name="BKMK_Mac"></a>Usar o Launchpad com um computador Mac  
 Você pode conectar um computador Mac® executando o Mac OS X® 10,5 ou posterior ao Windows Server Essentials, Windows Server Essentials ou Windows Server 2012 R2 ou baixando e instalando o software do conector. Quando terminar de instalar o software do conector, você pode optar por iniciar automaticamente a Barra Inicial durante a inicialização.  
  
 A Barra Inicial é um aplicativo pequeno que fornece aos usuários autenticados acesso aos principais recursos do servidor, incluindo mídias e arquivos compartilhados, suplementos e o Acesso via Web remoto. A Barra Inicial fornece também notificações e informações em tempo real sobre a integridade do computador.  
  
> [!NOTE]
>  Os administradores do servidor não podem usar a Barra Inicial ou o Acesso via Web remoto em um computador Mac para abrir o Painel do servidor e gerenciar o servidor.  
  
### <a name="backup"></a>Backup  
 Clique em **Backup** para configurar a Máquina do Tempo para fazer backup do computador e para alterar as configurações de Máquina do Tempo. Para obter mais informações sobre a Máquina do Tempo, consulte a documentação do fabricante do computador.  
  
### <a name="remote-web-access"></a>Acesso Web remoto  
 Clique em **acesso via Web remota** para abrir o navegador da Web no site do acesso via Web remoto. O Acesso via Web remoto permite que você acesse os arquivos e as pastas compartilhadas no servidor de qualquer local remoto com um computador habilitado para Internet. Você pode carregar arquivos, reproduzir músicas e vídeos em um reprodutor de mídia baseado na web, além de exibir imagens e reproduzir apresentações. Para obter mais informações, consulte [usar o acesso via Web remoto](../use/Use-Remote-Web-Access-in-Windows-Server-Essentials.md).  
  
### <a name="shared-folders"></a>Pastas Compartilhadas  
 Clique em **Pastas compartilhadas** para abrir o Localizador no local das pastas compartilhadas no servidor. Para obter informações sobre como compartilhar arquivos e pastas, consulte [usar pastas compartilhadas](../use/Use-Shared-Folders-in-Windows-Server-Essentials.md).  
  
### <a name="computer-health-alerts"></a>Alertas de integridade do computador  
 Os alertas que aparecem na Barra Inicial fornecem um status rápido sobre a integridade imediata do computador. Para exibir informações sobre um alerta de integridade, clique em um indicador de alerta para abrir o visualizador de alertas. Alertas de integridade são exibidos no visualizador com base no nível de gravidade. Os alertas mais graves aparecem primeiro na lista. Alertas menos graves aparecem posteriormente na lista.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Conecte-se](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Utilizar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
