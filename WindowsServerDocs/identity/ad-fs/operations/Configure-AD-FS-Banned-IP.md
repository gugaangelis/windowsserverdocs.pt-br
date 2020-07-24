---
title: Configurar AD FS endereços IP proibidos
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9e38105bafc92efc4d9e62e4815cdb24c3c25512
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965608"
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
