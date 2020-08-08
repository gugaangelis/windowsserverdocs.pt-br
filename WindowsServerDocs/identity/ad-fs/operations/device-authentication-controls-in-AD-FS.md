---
title: Controles de autenticação de dispositivo no AD FS
description: Este documento descreve como habilitar a autenticação de dispositivo no AD FS para Windows Server 2016 e 2012 R2
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.openlocfilehash: 976a8db89d8ffdb08f5f453619b7a341fb4dcdb4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949690"
---
# <a name="device-authentication-controls-in-ad-fs"></a>Controles de autenticação de dispositivo no AD FS
O documento a seguir mostra como habilitar os controles de autenticação de dispositivo no Windows Server 2016 e 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Controles de autenticação de dispositivo no AD FS 2012 R2
Originalmente no AD FS 2012 R2, havia uma propriedade de autenticação global chamada `DeviceAuthenticationEnabled` essa autenticação de dispositivo controlada.

Para definir a configuração, o `Set-AdfsGlobalAuthenticationPolicy` cmdlet foi usado como mostrado abaixo:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Para desabilitar a autenticação do dispositivo, o mesmo cmdlet foi usado para definir o valor como $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Controles de autenticação de dispositivo no AD FS 2016
O único tipo de autenticação de dispositivo com suporte no 2012 R2 foi clientTLS.  No AD FS 2016, além de clientTLS, há dois novos tipos de autenticação de dispositivo para a autenticação de dispositivos modernos.  Eles são:
- PKeyAuth
- PRT

Para controlar o novo comportamento, a `DeviceAuthenticationEnabled` propriedade é usada em combinação com uma nova propriedade chamada `DeviceAuthenticationMethod` .

O método de autenticação de dispositivo determina o tipo de autenticação de dispositivo que será feito: PRT, PKeyAuth, clientTLS ou alguma combinação.
Ele tem os seguintes valores:
 - SignedToken: somente PRT
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS: PRT + clientTLS
 - Todos: todos os itens acima

Como você pode ver, o PRT faz parte de todos os métodos de autenticação de dispositivo, fazendo com que ele esteja em vigor no método padrão sempre habilitado quando `DeviceAuthenticationEnabled` é definido como `$true` .

Exemplo: para configurar os métodos, use o cmdlet DeviceAuthenticationEnabled como acima, juntamente com a nova propriedade:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> No ADFS 2019, `DeviceAuthenticationMethod` o pode ser usado com o `Set-AdfsRelyingPartyTrust` comando.

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> Habilitar a autenticação de dispositivo (definindo `DeviceAuthenticationEnabled` como `$true` ) significa que o `DeviceAuthenticationMethod` está definido implicitamente como `SignedToken` , que é igual a **PRT**.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> O método de autenticação de dispositivo padrão é `SignedToken` .  Outros valores são **PKeyAuth,**<strong>ClientTLS</strong> e **All**.

Os significados dos `DeviceAuthenticationMethod` valores mudaram um pouco desde que AD FS 2016 foi lançado.  Consulte a tabela abaixo para saber o significado de cada valor, dependendo do nível de atualização:


|Versão do AD FS|Valor de DeviceAuthenticationMethod|Maneira|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Todos|PRT + PkeyAuth + clientTLS|
|2016 RTM + atualizado com Windows Update|SignedToken (significado alterado)|PRT (somente)|
||PkeyAuth (novo)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Todos|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>Consulte Também
[Operações do AD FS](../ad-fs-operations.md)
