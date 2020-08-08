---
title: Configurar AD FS endereços IP proibidos
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.openlocfilehash: e5cb939fcf862d16837c2f9e05548644faf3bf71
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956483"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>AD FS e endereços IP proibidos


Em junho de 2018, AD FS no Windows Server 2016 introduziu **IPs Banidos** com o AD FS atualização de junho de 2018.  Essa atualização permite que você configure um conjunto de endereços IP globalmente no AD FS, de forma que as solicitações provenientes desses endereços IP ou que tenham esses endereços IP nos cabeçalhos **x-Forwarded-for** ou **x-MS-Forwarded-Client-IP** sejam bloqueadas pelo AD FS.

## <a name="adding-banned-ips"></a>Adicionando IPs proibidos
Para adicionar IPs Banidos à lista global, use o cmdlet do PowerShell abaixo:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formatos permitidos

1.  IPv4
2.  IPv6
3.  Formato CIDR com IPv4 ou V6

Há um limite de 300 entradas para endereços IP proibidos. Você pode usar o formato CIDR ou Range para negar um grande bloco de entradas com uma única entrada.

## <a name="removing-banned-ips"></a>Removendo IPs Banidos
Para remover IPs Banidos da lista global, use o cmdlet do PowerShell abaixo:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>Ler IPs proibidos
Para ler o conjunto atual de endereços IP proibidos, use o cmdlet do PowerShell abaixo:

``` powershell
PS C:\ >Get-AdfsProperties
```

Saída de exemplo:

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Referências adicionais
[Práticas recomendadas para proteger Serviços de Federação do Active Directory (AD FS)](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-Adfsproperties](/powershell/module/adfs/set-adfsproperties?view=win10-ps)

[Operações do AD FS](../ad-fs-operations.md)
