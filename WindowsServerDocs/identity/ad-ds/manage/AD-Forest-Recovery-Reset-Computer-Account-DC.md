---
title: Recuperação de floresta do AD - redefinir a conta de computador no controlador de domínio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adds
ms.openlocfilehash: 388b460196888d4ca0cd12218972197afb6d49c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852757"
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Recuperação de floresta do AD - redefinir a conta de computador no controlador de domínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Use o procedimento a seguir para redefinir a senha da conta de computador do controlador de domínio. 
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Para redefinir a senha da conta de computador do controlador de domínio  

1. No prompt de comando, digite o seguinte comando e pressione ENTER:  

   ```
   netdom help resetpwd  
   ```
  
2. Use a sintaxe que esse comando fornece para usar a ferramenta de linha de comando Netdom para redefinir a senha da conta de computador, por exemplo:  

   ```
   netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
   ```  
  
    Em que *nome do controlador de domínio* é o controlador de domínio local que você está recuperando. 
  
   > [!NOTE]
   > Você deve executar esse comando duas vezes.
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
