---
title: "Recuperação de floresta do AD - redefinir a conta de computador no controlador de domínio"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 4e1a6070-df0a-4dfe-8773-899a010bfabd
ms.technology: identity-adfs
ms.openlocfilehash: 588510a27f56abb4497b2f80fa0281a858a7e8eb
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
# <a name="ad-forest-recovery---resetting-the-computer-account-on-the-dc"></a>Recuperação de floresta do AD - redefinir a conta de computador no controlador de domínio 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Use o procedimento a seguir para redefinir a senha de conta de computador do controlador de domínio.  
  
## <a name="to-reset-the-computer-account-password-of-the-domain-controller"></a>Para redefinir a senha de conta de computador do controlador de domínio  
  
1.  Em um prompt de comando, digite o seguinte comando e pressione ENTER:  
  
    ```  
    netdom help resetpwd  
    ```  
  
2.  Use a sintaxe que esse comando fornece para usar a ferramenta de linha de comando Netdom para redefinir a senha de conta de computador, por exemplo:  
  
    ```  
    netdom resetpwd /server:domain controller name /userD:administrator /passwordd:*  
    ```  
  
     Onde *nome do controlador de domínio* é o controlador de domínio local que você está recuperando.  
  
    > [!NOTE]
    >  Você deve executar esse comando duas vezes.  
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
