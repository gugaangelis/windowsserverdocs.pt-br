---
title: Recuperação de floresta do AD-redefinindo a conta do computador no controlador de domínio
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.openlocfilehash: 3d779e989c4414629c9a7414adf41c96525368c4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87969833"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Recuperação de floresta do AD-redefinindo a conta do computador no controlador de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Use o procedimento a seguir para redefinir a senha da conta de computador do controlador de domínio.

## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Para redefinir a senha da conta de computador do controlador de domínio

1. Em um prompt de comando, digite o seguinte comando e pressione ENTER:

   ```
   netdom help resetpwd
   ```

2. Use a sintaxe que esse comando fornece para usar a ferramenta de linha de comando netdom para redefinir a senha da conta do computador, por exemplo:

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*
   ```

    Onde o *nome do controlador de domínio* é o DC local que você está recuperando.

   > [!NOTE]
   > Você deve executar esse comando duas vezes.

## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
