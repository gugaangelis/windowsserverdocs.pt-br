---
title: Gerenciar baseado em função acessar controle com o Windows PowerShell
description: Este tópico faz parte do guia de gerenciamento de gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4f13f78e-0114-4e41-9a28-82a4feccecfc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: df6fa423a4ec891f1ad3faefad6c6054519542c4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Gerenciar baseado em função acessar controle com o Windows PowerShell

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como usar IPAM para gerenciar o controle de acesso baseado na função com o Windows PowerShell.  
  
>[!NOTE]
>Para a referência de comando IPAM Windows PowerShell, consulte [Cmdlets de servidor de gerenciamento de endereço IP (IPAM) no Windows PowerShell](https://technet.microsoft.com/library/jj553807.aspx).  
  
Os novos comandos do Windows PowerShell IPAM fornecem a capacidade de recuperar e alterar os escopos de acesso de objetos DNS e DHCP. A tabela a seguir ilustra o comando correto para usar para cada objeto IPAM.  
  
|Objeto IPAM|Comando|Descrição|  
|---------------|-----------|---------------|  
|Servidor DNS|Get-IpamDnsServer|Este cmdlet retorna o objeto de servidor DNS IPAM|  
|Zona de DNS|Get-IpamDnsZone|Este cmdlet retorna o objeto de zona DNS IPAM|  
|Registro de recurso DNS|Get-IpamResourceRecord|Este cmdlet retorna o objeto de registro de recurso DNS IPAM|  
|Encaminhador condicional DNS|Get-IpamDnsConditionalForwarder|Este cmdlet retorna o objeto de encaminhador condicional DNS IPAM|  
|Servidor DHCP|Get-IpamDhcpServer|Este cmdlet retorna o objeto de servidor DHCP no IPAM|  
|Superescopo DHCP|Get-IpamDhcpSuperscope|Este cmdlet retorna o objeto de superescopo DHCP IPAM|  
|Escopo DHCP|Get-IpamDhcpScope|Este cmdlet retorna o objeto de escopo DHCP IPAM|  
  
No exemplo a seguir da saída do comando, o `Get-IpamDnsZone` cmdlet recupera o **dublin.contoso.com** zona DNS.  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>Definição de escopos de acesso em objetos IPAM  
Você pode definir escopos de acesso em objetos IPAM usando o `Set-IpamAccessScope` comando. Você pode usar esse comando para definir o escopo do acesso a um valor específico de um objeto ou para fazer com que os objetos herde escopo do acesso a objetos pai. A seguir estão os objetos que você pode configurar com esse comando.  
  
-   Escopo DHCP  
  
-   Servidor DHCP  
  
-   Superescopo DHCP  
  
-   Encaminhador condicional DNS  
  
-   Registros de recurso do DNS  
  
-   Servidor DNS  
  
-   Zona de DNS  
  
-   Bloco de endereço IP  
  
-   Intervalo de endereços IP  
  
-   Espaço de endereço IP  
  
-   Sub-rede de endereço IP  
  
A seguir está a sintaxe para a `Set-IpamAccessScope` comando.  
  
```  
NAME  
    Set-IpamAccessScope  
  
SYNTAX  
    Set-IpamAccessScope [-IpamRange] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpServer] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpSuperscope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDhcpScope] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsConditionalForwarder] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsResourceRecord] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamDnsZone] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamAddressSpace] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  
    [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamSubnet] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
  
    Set-IpamAccessScope [-IpamBlock] -InputObject <ciminstance[]> [-AccessScopePath <string>] [-IsInheritedAccessScope] [-PassThru] [-CimSession <CimSession[]>] [-ThrottleLimit <int>] [-AsJob] [-WhatIf] [-Confirm]  [<CommonParameters>]  
```  
  
No exemplo a seguir, o escopo de acesso da zona DNS **dublin.contoso.com** é alterada de **Dublin** para **Europa**.  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
  
PS C:\Users\Administrator.CONTOSO> $a = Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
PS C:\Users\Administrator.CONTOSO> Set-IpamAccessScope -IpamDnsZone -InputObject $a -AccessScopePath \Global\Europe -PassThru  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Europe  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  


