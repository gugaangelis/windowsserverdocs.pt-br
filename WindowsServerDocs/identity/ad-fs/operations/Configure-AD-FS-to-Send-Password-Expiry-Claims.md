---
ms.assetid: 03c82f43-ae2d-4038-b286-ae3858aed35a
title: Configurar o AD FS para enviar solicitações de expiração de senha
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 080e8cc81949df3bf74ae846eee7f32c5e145f53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834357"
---
# <a name="configure-ad-fs-to-send-password-expiry-claims"></a>Configurar o AD FS para enviar solicitações de expiração de senha

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Você pode configurar os serviços de Federação do Active Directory (AD FS) para enviar solicitações de expiração de senha para a terceira parte (aplicativos) que é protegidas pelo AD FS. Como essas declarações são usadas depende do aplicativo. Por exemplo, com o Office 365, como a terceira, as atualizações foram implementadas ao Exchange e Outlook para notificar os usuários federados das suas senhas estar expirado em breve.

Para configurar o AD FS para enviar a senha de expiração de declarações para uma terceira parte confiável, você deve adicionar as seguintes regras de declaração para essa terceira parte confiável:

```
@RuleName = "Issue Password Expiry Claims"
c1:[Type == "http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime"]
 => issue(store = "_PasswordExpiryStore", types = ("http://schemas.microsoft.com/ws/2012/01/passwordexpirationtime", "http://schemas.microsoft.com/ws/2012/01/passwordexpirationdays", "http://schemas.microsoft.com/ws/2012/01/passwordchangeurl"), query = "{0};", param = c1.Value);
```

> [!NOTE]
> Declarações de expiração de senha só estão disponíveis para o nome de usuário e senha e o Microsoft Passport para tipos de autenticação do trabalho.  Se o usuário é autenticado usando a autenticação integrada do Windows e do Passport não está configurado, as declarações não estará disponíveis e os usuários não verão as notificações de expiração de senha.

> [!NOTE]
> Há uma janela de 14 dias para que as declarações enviadas serão preenchidas somente se a senha expirará dentro de 14 dias.

## <a name="see-also"></a>Consulte também
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
