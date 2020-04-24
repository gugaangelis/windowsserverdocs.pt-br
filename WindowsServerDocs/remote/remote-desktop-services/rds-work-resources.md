---
title: Personalizar o título "Recursos de trabalho" do RDS usando o PowerShell no Windows Server
description: Fornece a descrição de como alterar o nome do workspace no Windows Server.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: ea7e1505f705c87e58145e7a306ed355ab3acf7f
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71387055"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>Personalizar o título "Recursos de trabalho" do RDS usando o PowerShell no Windows Server

Ao usar o Windows Server para acessar RemoteApps ou áreas de trabalho pelo WebAccess da Área de Trabalho Remota ou o novo aplicativo de Área de Trabalho Remota, você talvez tenha observado que o workspace se chama &quot;Recursos de Trabalho&quot; por padrão.  Você pode alterar facilmente o título usando cmdlets do PowerShell.

Para alterar o título, abra uma nova janela do PowerShell no servidor do agente de conexão e importe o módulo RemoteDesktop com o comando a seguir.

```powershell
    Import-Module RemoteDesktop
```

Use o comando Set-RDWorkspace para alterar o nome do espaço de trabalho.

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

Por exemplo, você pode usar o comando a seguir para alterar o nome do espaço de trabalho para "Contoso RemoteApps":

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

Se estiver executando vários Agentes de Conexão no modo de Alta Disponibilidade, você deverá executá-lo no agente ativo. Você pode usar este comando:

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

Para saber mais sobre o cmdlet Set-RDWorkspace, confira a referência [Set-RDSWorkspace](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps).