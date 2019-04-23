---
title: Gerenciar controle de acesso baseado em função com o Windows PowerShell
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
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
ms.openlocfilehash: e0318db1b2b1b2730ee6dc57b7b9df6d16fe57e8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841467"
---
# <a name="manage-role-based-access-control-with-windows-powershell"></a>Gerenciar controle de acesso baseado em função com o Windows PowerShell

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender a usar o IPAM para gerenciar o controle de acesso baseado em função com o Windows PowerShell.  
  
>[!NOTE]
>Para a referência de comandos do PowerShell do Windows de IPAM, consulte o [IpamServer cmdlets no Windows PowerShell](https://docs.microsoft.com/powershell/module/ipamserver/?view=win10-ps).  
  
Os novos comandos de IPAM do Windows PowerShell fornecem a capacidade de recuperar e alterar os escopos de acesso de objetos DNS e DHCP. A tabela a seguir ilustra o comando correto a ser usado para cada objeto IPAM.  
  
|Objeto IPAM|Comando|Descrição|  
|---------------|-----------|---------------|  
|Servidor DNS|Get-IpamDnsServer|Esse cmdlet retorna o objeto de servidor DNS no IPAM|  
|Zona do DNS|Get-IpamDnsZone|Esse cmdlet retorna o objeto de zona DNS no IPAM|  
|Registro de recurso DNS|Get-IpamResourceRecord|Esse cmdlet retorna o objeto de registro de recurso DNS no IPAM|  
|Encaminhador condicional DNS|Get-IpamDnsConditionalForwarder|Esse cmdlet retorna o objeto de encaminhador condicional DNS no IPAM|  
|Servidor DHCP|Get-IpamDhcpServer|Esse cmdlet retorna o objeto de servidor DHCP no IPAM|  
|Superescopo do DHCP|Get-IpamDhcpSuperscope|Esse cmdlet retorna o objeto de superescopo do DHCP no IPAM|  
|Escopo do DHCP|Get-IpamDhcpScope|Esse cmdlet retorna o objeto de escopo do DHCP no IPAM|  
  
No exemplo a seguir da saída do comando, o `Get-IpamDnsZone` cmdlet recupera as **dublin.contoso.com** zona DNS.  
  
```  
PS C:\Users\Administrator.CONTOSO> Get-IpamDnsZone -ZoneType Forward -ZoneName dublin.contoso.com  
  
ZoneName             : dublin.contoso.com  
ZoneType             : Forward  
AccessScopePath      : \Global\Dublin  
IsSigned             : False  
DynamicUpdateStatus  : None  
ScavengeStaleRecords : False  
```  
  
## <a name="setting-access-scopes-on-ipam-objects"></a>A definição de escopos de acesso em objetos IPAM  
Você pode definir escopos de acesso nos objetos IPAM usando o `Set-IpamAccessScope` comando. Você pode usar esse comando para definir o escopo de acesso para um valor específico para um objeto ou para fazer com que os objetos herdar o escopo de acesso de objetos pai. A seguir estão os objetos que podem ser configuradas com este comando.  
  
-   Escopo do DHCP  
  
-   Servidor DHCP  
  
-   Superescopo do DHCP  
  
-   Encaminhador condicional DNS  
  
-   Registros de recurso DNS  
  
-   Servidor DNS  
  
-   Zona do DNS  
  
-   Bloco de endereço IP  
  
-   Intervalo de endereços IP  
  
-   Espaço de endereço IP  
  
-   Subrede de endereço IP  
  
Esta é a sintaxe para o `Set-IpamAccessScope` comando.  
  
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
  
No exemplo a seguir, o escopo de acesso da zona DNS **dublin.contoso.com** é alterado de **Dublin** para **Europa**.  
  
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
  


