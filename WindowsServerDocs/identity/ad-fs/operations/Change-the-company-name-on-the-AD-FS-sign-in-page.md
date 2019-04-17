---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: "Alterar o nome da empresa no AD FS-na página de entrada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8e99f6fed8922ed15bf78a98b207b6f46767763a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Alterar o nome da empresa no AD FS-na página de entrada

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2
 
Para alterar o nome da empresa que é exibido na página de sign\, use a sintaxe e o seguinte cmdlet do PowerShell do Windows PowerShell. Esse valor é definido por padrão usando o valor do nome de exibição de serviço de federação que você inseriu durante a instalação.  

![alterar o nome](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> Você também pode usar o Windows PowerShell Integrated Scripting ambiente \(ISE\) para alterar o nome da empresa. Usando o Windows PowerShell ISE, você pode exibir o conteúdo em um ambiente Unicode\ compatível. Para obter informações adicionais, consulte [apresentando o Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
  
