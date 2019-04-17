---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: "Alterando o logotipo da empresa no AD FS-na página de entrada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ca54abe7fe852b22f2f4d9a717e38d219fa50694
ms.sourcegitcommit: a00fc4426dc4ad0098257f01f0124d6c733d1aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2018
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Alterando o logotipo da empresa no AD FS-na página de entrada

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

#### <a name="change-company-logo"></a>Mudar o logotipo de empresa  
Para alterar o logotipo da empresa que é exibido na página de sign\, use a sintaxe e o seguinte cmdlet do PowerShell do Windows PowerShell.  

![Alterar logotipo](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Recomendamos que as dimensões para o logotipo seja 260 x 350 @ 96 dpi com um tamanho de arquivo não maior que 10 KB.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> O `TargetName` parâmetro será obrigatório. O tema padrão que foi lançado com o AD FS é denominado *padrão*.  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
