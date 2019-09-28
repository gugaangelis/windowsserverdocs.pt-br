---
title: Controles de autenticação de dispositivo no AD FS
description: Este documento descreve como habilitar a autenticação de dispositivo no AD FS para Windows Server 2016 e 2012 R2
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 87c011b18ad4a1d464072c1ea90b09a44e831378
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407360"
---
# <a name="device-authentication-controls-in-ad-fs"></a>Controles de autenticação de dispositivo no AD FS
O documento a seguir mostra como habilitar os controles de autenticação de dispositivo no Windows Server 2016 e 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Controles de autenticação de dispositivo no AD FS 2012 R2
Originalmente no AD FS 2012 R2, havia uma propriedade de autenticação global chamada `DeviceAuthenticationEnabled` que controlava a autenticação do dispositivo.

Para definir a configuração, o cmdlet `Set-AdfsGlobalAuthenticationPolicy` foi usado como mostrado abaixo:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Para desabilitar a autenticação do dispositivo, o mesmo cmdlet foi usado para definir o valor como $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Controles de autenticação de dispositivo no AD FS 2016
O único tipo de autenticação de dispositivo com suporte no 2012 R2 foi clientTLS.  No AD FS 2016, além de clientTLS, há dois novos tipos de autenticação de dispositivo para a autenticação de dispositivos modernos.  Elas são:
- PKeyAuth
- PRT

Para controlar o novo comportamento, a propriedade `DeviceAuthenticationEnabled` é usada em combinação com uma nova propriedade chamada `DeviceAuthenticationMethod`.  

O método de autenticação de dispositivo determina o tipo de autenticação de dispositivo que será feito: PRT, PKeyAuth, clientTLS ou alguma combinação.
Ele tem os seguintes valores:
 - SignedToken: Somente PRT
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS: PRT + clientTLS
 - All: Todos os anteriores

Como você pode ver, o PRT faz parte de todos os métodos de autenticação de dispositivo, fazendo com que ele esteja em vigor no método padrão que está sempre habilitado quando `DeviceAuthenticationEnabled` é definido como `$true`.

Exemplo: Para configurar os métodos, use o cmdlet DeviceAuthenticationEnabled como acima, juntamente com a nova propriedade:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```

>[!NOTE]
> No ADFS 2019, `DeviceAuthenticationMethod` pode ser usado com o comando `Set-AdfsRelyingPartyTrust`.

``` powershell
PS:\>Set-AdfsRelyingPartyTrust -DeviceAuthenticationMethod ClientTLS
```

>[!NOTE]
> Habilitar a autenticação de dispositivo (definir `DeviceAuthenticationEnabled` para `$true`) significa que o `DeviceAuthenticationMethod` está implicitamente definido como `SignedToken`, que equivale a **PRT**.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
> [!NOTE]
> O método de autenticação de dispositivo padrão é `SignedToken`.  Outros valores são **PKeyAuth,** <strong>ClientTLS</strong> e **All**.

Os significados dos valores `DeviceAuthenticationMethod` foram ligeiramente alterados desde que AD FS 2016 foi lançado.  Consulte a tabela abaixo para saber o significado de cada valor, dependendo do nível de atualização:


|Versão do AD FS|Valor de DeviceAuthenticationMethod|Maneira|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Todas|PRT + PkeyAuth + clientTLS|
|2016 RTM + atualizado com Windows Update|SignedToken (significado alterado)|PRT (somente)|
||PkeyAuth (novo)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Todas|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>Consulte também
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
