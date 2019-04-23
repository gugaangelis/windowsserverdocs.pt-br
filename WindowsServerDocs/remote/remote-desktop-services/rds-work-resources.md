---
title: Personalizar o RDS título "recursos de trabalho" usando o PowerShell no Windows Server
description: Fornece a descrição de como alterar o nome do espaço de trabalho do padrão no Windows Server.
ms.prod: windows-server-threshold
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 10/26/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 43837826a6cddc2c3c4c7c1af874334718a3a067
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826707"
---
# <a name="customize-the-rds-title-work-resources-using-powershell-on-windows-server"></a>Personalizar o RDS título "recursos de trabalho" usando o PowerShell no Windows Server

Ao usar o Windows Server para acessar aplicativos remotos ou áreas de trabalho por meio do acesso Web da área de trabalho remota ou o novo aplicativo de área de trabalho remota, você talvez tenha observado que o espaço de trabalho é intitulado "Recursos de trabalho" por padrão.  Você pode alterar facilmente o título usando cmdlets do PowerShell.

Para alterar o título, abra uma nova janela do PowerShell no servidor do agente de conexão e importe o módulo de área de trabalho remota com o comando a seguir.

```powershell
    Import-Module RemoteDesktop
```

Em seguida, use o comando Set-RDWorkspace para alterar o nome do espaço de trabalho.

```powershell
    Set-RDWorkspace [-Name] <string> [-ConnectionBroker <string>]  [<CommonParameters>]
```   

Por exemplo, você pode usar o comando a seguir para alterar o nome do espaço de trabalho para "Contoso RemoteApps":

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker broker01.contoso.com
```

Se você estiver executando vários agentes de Conexão no modo de alta disponibilidade, você deve executá-lo contra o broker ativo. Você pode usar este comando:

```powershell
    Set-RDWorkspace -Name "Contoso RemoteApps" -ConnectionBroker (Get-RDConnectionBrokerHighAvailability).ActiveManagementServer
```

Para obter mais informações sobre o cmdlet Set-RDWorkspace, consulte o [RDSWorkspace conjunto](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdworkspace?view=win10-ps) referência.