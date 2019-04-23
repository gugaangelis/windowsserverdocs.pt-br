---
title: Recuperação de floresta do AD - fazer backup de um servidor completo
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 398918dc-c8ab-41a6-a377-95681ec0b543
ms.technology: identity-adds
ms.openlocfilehash: 455c930a90cd443cf1fe62f436abca88384f79c1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865557"
---
# <a name="resetting-a-trust-password-on-one-side-of-the-trust"></a>Redefinir uma senha de relação de confiança no um lado da relação de confiança  

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

 Se a recuperação de floresta estiver relacionada a uma violação de segurança, use o procedimento a seguir para redefinir uma senha de relação de confiança no um lado da relação de confiança. Isso inclui as relações de confiança implícitas entre filho e domínios pai, bem como as relações de confiança explícitas entre este domínio (o domínio confiante) e o outro domínio (o domínio confiável). 
  
 Redefina a senha somente o lado domínio confiante da relação de confiança, também conhecido como a confiança de entrada (o lado que pertence a este domínio). Em seguida, use a mesma senha no lado do domínio confiável da relação de confiança, também conhecido como a confiança de saída. Redefina a senha da relação de confiança de saída quando você restaura o primeiro controlador de domínio em cada um dos outros domínios (confiáveis). 
  
 Redefinir a senha de relação de confiança garante que o controlador de domínio não replica com controladores de domínio potencialmente inválidos fora de seu domínio. Definindo a mesma senha de relação de confiança ao restaurar o primeiro controlador de domínio em cada um dos domínios, verifique se que esse controlador de domínio replica com cada um dos controladores de domínio recuperados. Os DCs subsequentes no domínio que são recuperados por meio da instalação do AD DS serão replicadas automaticamente essas novas senhas durante o processo de instalação. 
  
## <a name="to-reset-a-trust-password-on-one-side-of-the-trust"></a>Para redefinir uma senha de relação de confiança no um lado da relação de confiança  
  
1. No prompt de comando, digite o seguinte comando e pressione ENTER:  

   ```  
   netdom experthelp trust  
   ```  
  
2. Use a sintaxe que esse comando fornece para usar a ferramenta NetDom para redefinir a senha de relação de confiança.
   Por exemplo, se houver dois domínios na floresta — pai e filho — e você estiver executando este comando no controlador de domínio restaurado no domínio pai, use a seguinte sintaxe de comando:  

   ```  
   netdom trust parent domain name /domain:child domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   Quando você executar esse comando no domínio filho, use a seguinte sintaxe de comando:  

   ```  
   netdom trust child domain name /domain:parent domain name /resetOneSide /passwordT:password /userO:administrator /passwordO:*  
   ```  

   > [!NOTE]
   > **passwordT** deve ser o mesmo valor em ambos os lados da relação de confiança. Execute este comando apenas uma vez (ao contrário do **netdom resetpwd** comando) porque ele redefine automaticamente a senha duas vezes. 
  
## <a name="next-steps"></a>Próximas etapas

- [Guia de recuperação da floresta do AD](AD-Forest-Recovery-Guide.md)
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)
