---
title: Recuperação de floresta do AD-redefinindo a conta do computador no controlador de domínio
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.openlocfilehash: 54d3233db2e4cba388abb35e91053413072bb1c1
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941586"
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
