---
title: Configurar o AD FS proibidos endereços IP
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 01ef992554a1e0961d8d795e9baa7730a1a1d682
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189892"
---
# <a name="ad-fs-and-banned-ip-addresses"></a>O AD FS e os endereços IP proibidos


Em junho de 2018, o AD FS no Windows Server 2016 introduziu **IPs proibidos** com o AD FS atualização de junho de 2018.  Essa atualização permite que você configure um conjunto de endereços IP globalmente no AD FS, para que as solicitações provenientes de endereços IP, ou que tenham esses endereços IP **x-forwarded-for** ou **x-ms-forwarded-client-ip** cabeçalhos, serão bloqueados pelo AD FS.

## <a name="adding-banned-ips"></a>Adicionando IPs proibidos
Para adicionar IPs proibidos à lista global, use o cmdlet do Powershell abaixo:

``` powershell
PS C:\ >Set-AdfsProperties -AddBannedIps "1.2.3.4", "::3", "1.2.3.4/16"
```

Formatos permitidos

1.  IPv4
2.  IPv6
3.  Formato CIDR com IPv4 ou v6

Há um limite de 300 entradas para os endereços IP proibidos. Você pode usar o formato CIDR ou o intervalo para negar a um grande bloco de entradas com uma única entrada.

## <a name="removing-banned-ips"></a>Removendo IPs proibidos
Para remover banidas IPs da lista global, use o cmdlet do Powershell abaixo:

``` powershell
PS C:\ >Set-AdfsProperties -RemoveBannedIps "1.2.3.4"
```

#### <a name="read-banned-ips"></a>IPs proibidos de leitura
Para ler o conjunto atual de endereços IP proibidos, use o cmdlet do Powershell abaixo:

``` powershell
PS C:\ >Get-AdfsProperties 
```

Exemplo de saída:

```
BannedIpList                   : {1.2.3.4, ::3,1.2.3.4/16}
```



## <a name="additional-references"></a>Referências adicionais  
[Práticas recomendadas para proteger os serviços de Federação do Active Directory](../../ad-fs/deployment/best-practices-securing-ad-fs.md)

[Set-AdfsProperties](https://technet.microsoft.com/itpro/powershell/windows/adfs/set-adfsproperties)

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
