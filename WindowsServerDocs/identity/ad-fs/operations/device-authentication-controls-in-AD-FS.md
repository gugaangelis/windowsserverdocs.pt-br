---
title: "Controles de autenticação de dispositivo no AD FS"
description: "Este documento descreve como habilitar a autenticação de dispositivo no AD FS para o Windows Server 2016 e 2012 R2"
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 11/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d66cfde20060229844c34abeea85dd83b802ddad
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="device-authentication-controls-in-ad-fs"></a>Controles de autenticação de dispositivo no AD FS
O documento a seguir mostra como habilitar os controles de autenticação de dispositivo no Windows Server 2016 e 2012 R2.

## <a name="device-authentication-controls-in-ad-fs-2012-r2"></a>Controles de autenticação de dispositivo no AD FS 2012 R2
Originalmente no AD FS 2012 R2, havia uma propriedade global autenticação chamada `DeviceAuthenticationEnabled`essa autenticação de dispositivo controlado.

Para definir a configuração, o `Set-AdfsGlobalAuthenticationPolicy`cmdlet foi usado conforme mostrado abaixo:


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```



Para desativar a autenticação de dispositivo, o cmdlet mesmo foi usado para definir o valor como $false.

## <a name="device-authentication-controls-in-ad-fs-2016"></a>Controles de autenticação de dispositivo no AD FS 2016
O único tipo de autenticação de dispositivo tem suportado no 2012 R2 foi clientTLS.  No AD FS 2016, além de clientTLS há dois novos tipos de autenticação de dispositivo para autenticação de dispositivos modernos.  Estes são:
- PKeyAuth
- PRT

Para controlar o comportamento de novo, o `DeviceAuthenticationEnabled`propriedade é usada em combinação com uma nova propriedade chamada `DeviceAuthenticationMethod`.  

O método de autenticação de dispositivo determina o tipo de autenticação de dispositivo que será feito: PRT, PKeyAuth, clientTLS, ou alguma combinação.
Ele possui os seguintes valores:
 - SignedToken: PRT somente
 - PKeyAuth: PRT + PKeyAuth
 - ClientTLS: PRT + clientTLS 
 - Tudo: Todos os itens acima

Como você pode ver, PRT faz parte de todos os métodos de autenticação de dispositivo, tornando-o em vigor o método padrão que está sempre habilitado quando `DeviceAuthenticationEnabled`é definido como `$true`.

Exemplo: Para configurar os métodos, use o cmdlet DeviceAuthenticationEnabled como acima, juntamente com a nova propriedade:

``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationEnabled $true
```
>[!NOTE]
> Habilitando autenticação de dispositivo (configuração `DeviceAuthenticationEnabled`para `$true`) significa que o `DeviceAuthenticationMethod`implicitamente é definida como `SignedToken`, que equivale à **PRT **.


``` powershell
PS:\>Set-AdfsGlobalAuthenticationPolicy –DeviceAuthenticationMethod All
```
>[!NOTE]
>O método de autenticação de dispositivo padrão é `SignedToken`.  Outros valores são **PKeyAuth, * * * ClientTLS,** e **todos os **.

Os significados do `DeviceAuthenticationMethod`valores foram ligeiramente modificados desde o lançamento do AD FS 2016.  Consulte a tabela abaixo para o significado de cada valor, dependendo do nível de atualização:


|Versão do AD FS|Valor de DeviceAuthenticationMethod|Significa|
| ----- | ----- | ----- |
|2016 RTM|SignedToken|PRT + PkeyAuth|
||clientTLS|clientTLS|
||Todos os|PRT + PkeyAuth + clientTLS|
|RTM 2016 + atualizado com o Windows Update|SignedToken (significado alterado)|PRT (somente)|
||PkeyAuth (novo)|PRT + PkeyAuth|
||clientTLS|PRT + clientTLS|
||Todos os|PRT + PkeyAuth + clientTLS|

## <a name="see-also"></a>Consulte também
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
