---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: Configurar o AD FS para enviar solicitações de expiração de senha
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 34a0544946da429f4e5b54a27b73c8bf139f5a8d
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948566"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configurar o AD FS para enviar solicitações de expiração de senha


Você pode configurar Serviços de Federação do Active Directory (AD FS) (AD FS) para enviar declarações de expiração de senha para os aplicativos de confiança de terceira parte confiável que são protegidos pelo ADFS. A forma como essas declarações são usadas depende do aplicativo. Por exemplo, com o Office 365 como sua terceira parte confiável, as atualizações foram implementadas para o Exchange e o Outlook para notificar os usuários federados sobre suas senhas em breve.

Para configurar AD FS para enviar declarações de expiração de senha a uma relação de confiança de terceira parte confiável, você deve adicionar as seguintes regras de declaração a esta relação de confiança de terceira parte confiável:

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("https://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "https://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "https://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> As declarações de expiração de senha só estão disponíveis para nome de usuário e senha e Microsoft Passport for Work tipos de autenticação.  Se o usuário autenticar usando a autenticação integrada do Windows e o Passport não estiver configurado, as declarações não estarão disponíveis e os usuários não verão as notificações de expiração de senha.

> [!NOTE]
> Há uma janela de 14 dias para que as declarações enviadas só sejam populadas se a senha estiver expirando em 14 dias.

## <a name="see-also"></a>Veja também
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
