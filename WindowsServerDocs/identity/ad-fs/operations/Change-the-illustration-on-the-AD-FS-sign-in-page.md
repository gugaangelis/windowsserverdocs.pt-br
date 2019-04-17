---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: "Alterar a ilustração na página de entrada de AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac5e60aaad864248b58a3908e7aa9622165fbc14
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Alterar a ilustração na página de entrada de AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

## <a name="change-the-illustration"></a>Alterar a ilustração  


Para alterar a ilustração, o gráfico à esquerda, que é exibido na página de sign\, use a sintaxe e o seguinte cmdlet do PowerShell do Windows PowerShell.  

![Alterar ilustração](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Recomendamos que as dimensões para ilustração seja 1420 x 1080 pixels @ 96 DPI com um tamanho de arquivo de não mais que 200 KB.  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
  
  
