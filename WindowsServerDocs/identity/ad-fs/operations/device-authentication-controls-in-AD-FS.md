---
title: Controles de autenticação de dispositivo no AD FS
description: Este documento descreve como habilitar a autenticação de dispositivo no AD FS para o Windows Server 2016 e 2012 R2
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d66cfde20060229844c34abeea85dd83b802ddad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822817"
---
# <a name="device-authentication-controls-in-ad-fs"></a>Controles de autenticação de dispositivo no AD FS
O documento a seguir mostra como habilitar os controles de autenticação de dispositivo no Windows Server 2016 e 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Controles de autenticação de dispositivo no AD FS 2012 R2
Originalmente no AD FS 2012 R2, havia uma propriedade de autenticação global chamada `DeviceAuthenticationEnabled` que autenticação de dispositivo controlado.

Para definir a configuração, o `Set-AdfsGlobalAuthenticationPolicy` cmdlet foi usado, conforme mostrado abaixo:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Para desabilitar a autenticação de dispositivo, o mesmo cmdlet foi usado para definir o valor como $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Controles de autenticação de dispositivo no AD FS 2016
O único tipo de autenticação de dispositivo com suporte no 2012 R2 foi clientTLS.  No AD FS 2016, além de clientTLS há dois novos tipos de autenticação de dispositivo para a autenticação de dispositivos modernos.  Elas são:
- PKeyAuth
- PRT

Para controlar o novo comportamento, o `DeviceAuthenticationEnabled` propriedade é usada em combinação com uma nova propriedade chamada `DeviceAuthenticationMethod`.  

O método de autenticação de dispositivo determina o tipo de autenticação de dispositivo que será feita: PRT, PKeyAuth, clientTLS ou alguma combinação.
Ele tem os seguintes valores:
 - SignedToken: Somente PRT
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS: PRT + clientTLS 
 - All: Todos os anteriores

Como você pode ver, PRT faz parte de todos os métodos de autenticação de dispositivo, tornando-o método padrão que é sempre em vigor habilitada quando `DeviceAuthenticationEnabled` é definido como `$true`.

Exemplo: Para configurar o método (s), use o cmdlet DeviceAuthenticationEnabled como acima, juntamente com a nova propriedade:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```
>[!NOTE]
> Habilitar a autenticação de dispositivo (configuração `DeviceAuthenticationEnabled` à `$true`) significa que o `DeviceAuthenticationMethod` é definido implicitamente como `SignedToken`, que é igual a **PRT**.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
>[!NOTE]
>O método de autenticação de dispositivo padrão é `SignedToken`.  Outros valores são **PKeyAuth, * * * ClientTLS,** e **todos os**.

Os significados do `DeviceAuthenticationMethod` valores foram ligeiramente alterados desde o lançamento do AD FS 2016.  Consulte a tabela abaixo para o significado de cada valor, dependendo do nível de atualização:


|Versão do AD FS|Valor de DeviceAuthenticationMethod|Significa|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Todas|PRT + PkeyAuth + clientTLS|
|2016 RTM + até data com o Windows Update|SignedToken (significado alterado)|PRT (somente)|
||PkeyAuth (novo)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Todas|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>Consulte também
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
