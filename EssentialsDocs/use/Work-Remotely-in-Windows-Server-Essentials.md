---
title: Trabalhar remotamente no Windows Server Essentials [SBS8]
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 8b183f8f-1279-4fdf-a495-c7c801563cb0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 0530833b8006302791cbe62f0156c1efb51cbc2f
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85469831"
---
# <a name="work-remotely-in-windows-server-essentials"></a>Trabalhar remotamente no Windows Server Essentials [SBS8]

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Há várias maneiras de acessar os recursos que estão localizados no servidor quando você estiver ausente da rede, se funcionalidades de Acesso em qualquer local (acesso remoto via Web, rede privada virtual e DirectAccess) estiverem configuradas no servidor.

 Os tópicos a seguir podem ajudá-lo a acessar os recursos do servidor remotamente:


-   [Utilizar o Acesso Remoto via Web no Windows Server Essentials](Work-Remotely-in-Windows-Server-Essentials.md#BKMA_RWA)

-   [Usar VPN para se conectar ao Windows Server Essentials](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_3)

-   [Usar o aplicativo meu servidor para se conectar ao Windows Server Essentials](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_App)

-   [Aplicativo My Server para Windows Phone.](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_2)

-   [Usar o Microsoft Office 365 com o Windows Server Essentials](Work-Remotely-in-Windows-Server-Essentials.md#BKMK_O365)

> [!NOTE]
>  Para obter informações sobre como configurar o acesso em qualquer local em seu servidor, consulte [gerenciar o acesso em qualquer local](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md).

##  <a name="use-remote-web-access-in-windows-server-essentials"></a><a name="BKMA_RWA"></a>Usar Acesso via Web remotos no Windows Server Essentials

 O Acesso via Web remoto  ajuda você a permanecer conectado à sua rede do Windows Server Essentials quando estiver ausente. Para obter mais informações, consulte o tópico [usar acesso via Web remoto](Use-Remote-Web-Access-in-Windows-Server-Essentials.md).


##  <a name="use-vpn-to-connect-to-windows-server-essentials"></a><a name="BKMK_3"></a>Usar VPN para se conectar ao Windows Server Essentials
 Se você tiver um computador cliente que está configurado com contas de rede que podem ser usadas para se conectar a um servidor hospedado executando o Windows Server Essentials por meio de uma conexão VPN, todas as contas de usuário recém-criadas no servidor hospedado devem usar VPN para fazer logon no computador cliente pela primeira vez. Conclua o procedimento a seguir no computador cliente conectado ao servidor.

#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>Para usar VPN para acessar remotamente os recursos do servidor

1.  Pressione Ctrl + Alt + Delete no computador cliente.

2.  Clique em **Trocar Usuário** na tela de logon.

3.  Clique no ícone de logon de rede no canto inferior direito da tela.

4.  Fazer logon na rede do Windows Server Essentials usando seu nome de usuário e senha.

##  <a name="use-the-my-server-app-to-connect-to-windows-server-essentials"></a><a name="BKMK_App"></a>Usar o aplicativo meu servidor para se conectar ao Windows Server Essentials
 O aplicativo meu servidor permite que você se conecte a recursos e execute tarefas administrativas leves no servidor do Windows Server Essentials a partir de seu PC, laptop ou dispositivo de superfície baseado no Windows. Se o servidor estiver executando o Windows Server 2012, baixe o aplicativo original meu servidor de [aplicativos para Windows](https://windows.microsoft.com/windows-8/apps). Se o servidor estiver executando o Windows Server Essentials, você deverá baixar o aplicativo meu servidor 2012 R2.

 Com o aplicativo My Server 2012 R2 expandido, você pode se conectar ao servidor ou aos computadores cliente usando a área de trabalho remota. Se o servidor do Windows Server Essentials estiver integrado ao Office 365 e sua assinatura incluir o SharePoint Online, você também poderá trabalhar com documentos em suas bibliotecas do SharePoint Online e abrir seus sites de equipe do SharePoint a partir do meu servidor 2012 R2.


 Para obter informações sobre como instalar e usar esses aplicativos, consulte [usar o aplicativo de servidor](Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).

 Para obter informações sobre como instalar e usar esses aplicativos, consulte [usar o aplicativo de servidor](../use/Use-the-My-Server-App-to-Connect-to-Windows-Server-Essentials.md).


##  <a name="use-the-my-server-app-for-windows-phone"></a><a name="BKMK_2"></a>Usar o aplicativo meu servidor para Windows Phone
 O aplicativo do Windows do meu servidor para Windows Phone (para Windows Server 2012) e o aplicativo meu servidor 2012 R2 para Windows Phone (para Windows Server Essentials) foram criados para ajudá-lo a se conectar diretamente com seus servidores por meio de telefones inteligentes enquanto trabalha em locais remotos. Essa é uma das várias maneiras de acessar o Windows Server Essentials depois de configurar o servidor para acesso remoto.

 Você pode baixar o aplicativo de armazenamento do Windows Phone:

- [Instalar o My Server para Windows Phone](http://www.windowsphone.com/store/app/my-server/6c2f98d5-6fcf-4e1d-b8b1-cde62ea1a94a)

- [Instalar o My Server 2012 R2 para Windows Phone](http://www.windowsphone.com/store/app/my-server-2012-r2/44f596b5-0477-4096-b96e-ddd6ef64ad6b)

  Para obter mais informações sobre o meu aplicativo de telefone de servidor, consulte a entrada de blog [meu aplicativo de telefone do servidor para Windows Server Essentials](https://blogs.technet.com/b/sbs/archive/2012/09/18/my-server-phone-app-for-windows-server-2012-essentials.aspx). Para obter mais informações sobre o aplicativo de telefone My Server 2012 R2, consulte a entrada no blog [Aplicativos My Server 2012 R2 de Windows e Windows Phone](https://blogs.technet.com/b/sbs/archive/2013/11/19/my-server-2012-r2-windows-and-windows-phone-apps.aspx).

##  <a name="use-microsoft-office-365-with-windows-server-essentials"></a><a name="BKMK_O365"></a>Usar o Microsoft Office 365 com o Windows Server Essentials

 Office 365 é um conjunto de fácil de usar ferramentas habilitados para a web que permite acessar seu email, documentos importantes, contatos e calendário de qualquer lugar e de qualquer dispositivo. Para obter mais informações, consulte o [Guia de início rápido para usar Microsoft Office 365](Quick-Start-Guide-to-Using-Microsoft-Office-365-with-Windows-Server-Essentials.md).


## <a name="see-also"></a>Veja também

-   [Gerenciar o Acesso em Qualquer Local](../manage/Manage-Anywhere-Access-in-Windows-Server-Essentials.md)

-   [Conecte-se](Get-Connected-in-Windows-Server-Essentials.md)

-   [Usar pastas compartilhadas](Use-Shared-Folders-in-Windows-Server-Essentials.md)

-   [Reproduzir mídia digital](Play-Digital-Media-in-Windows-Server-Essentials.md)

