---
title: "Recuperação de floresta do AD - fazer backup de um servidor completo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adfs
ms.openlocfilehash: 51b53e7348ae00ed2fc63711b9c3297279ebe033
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Redefinir uma senha de confiança em um lado da relação de confiança  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Se a recuperação de floresta está relacionada a uma violação de segurança, use o procedimento a seguir para redefinir a senha de confiança em um lado da relação de confiança. Isso inclui implícitas relações de confiança entre o filho e domínios pai, bem como as relações de confiança explícitas entre neste domínio (o domínio confiante) e outro domínio (o domínio confiável).  
  
 Redefina a senha apenas o lado domínio confiante de confiança, também conhecido como a confiança de entrada (o lado em que este domínio pertence). Em seguida, use a mesma senha do lado do domínio confiável de confiança, também conhecido como a confiança de saída. Redefina a senha da relação de confiança saída quando você restaurar o primeiro controlador de domínio em cada um dos outros domínios (confiáveis).  
  
 Redefinir a senha de confiança garante que o controlador de domínio não replicar com controladores de domínio potencialmente ruins fora de seu domínio. Definindo a mesma senha de confiança ao restaurar o primeiro controlador de domínio em cada um dos domínios, você garantir que esse DC replica com cada um dos controladores de domínio recuperados. Subsequentes controladores de domínio no domínio que são recuperadas instalando o AD DS automaticamente replicará esses novas senhas durante o processo de instalação.  
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Para redefinir uma senha de confiança em um lado da relação de confiança  
  
1.  Em um prompt de comando, digite o seguinte comando e pressione ENTER:  
  
    ```  
    netdom experthelp trust  
    ```  
  
2.  Use a sintaxe que esse comando fornece para usar a ferramenta NetDom para redefinir a senha de confiança.  
  
     Por exemplo, se houver duas domínios na floresta — pai e filho e você estiver executando esse comando no controlador de domínio restaurado no domínio pai, use a seguinte sintaxe de comando:  
  
    ```  
    netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
    ```  
  
     Quando você executar esse comando no domínio filho, use a seguinte sintaxe de comando:  
  
    ```  
    netdom trust child domain name /domain:parent domain name /resetOneSide /password:password /userO:administrator /passwordO:*  
    ```  
  
    > [!NOTE]
    >  **passwordT** deve ser o mesmo valor em ambos os lados da relação de confiança. Execute este comando apenas uma vez (ao contrário do **netdom resetpwd** comando) porque ele automaticamente redefine a senha duas vezes.  
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação de floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
