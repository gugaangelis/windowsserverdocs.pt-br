---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: "Configurar o AD FS para enviar solicitações de expiração de senha"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 386a5ac921ba609c371121b8657351667628951b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configurar o AD FS para enviar solicitações de expiração de senha

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Você pode configurar serviços de Federação Active Directory (AD FS) para enviar as declarações de expiração de senha para as terceira parte relações de confiança (aplicativos) que são protegidas pelo AD FS. Como essas declarações são usadas dependem do aplicativo. Por exemplo, com o Office 365, como o terceiro, atualizações foram implementadas para Exchange e o Outlook para notificar os usuários federados das suas senhas ser expirou em breve.

Para configurar o AD FS para enviar senha declarações de expiração para uma terceira relação de confiança de terceiros, você deve adicionar as seguintes regras reivindicação nesta terceira relação de confiança de terceiros:

```
c1:[Type == "https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
=> issue(store = "_PasswordExpiryStore", types = ("https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "https://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "https://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Declarações de expiração de senha só estão disponíveis para o nome de usuário e senha e o Microsoft Passport para tipos de autenticação de trabalho.  Se o usuário se autentica usando autenticação integrada do Windows e do Passport não está configurada, as declarações não estarão disponíveis e os usuários não verá notificações de expiração de senha.

> [!NOTE]
> Há uma janela de 14 dias para que as declarações enviadas apenas serão populadas se a senha estiver expirando em 14 dias.

## <a name="see-also"></a>Consulte também
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
