---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: Alterar a ilustração na página de entrada do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4f1cba9862766092c2beadb894cbac092d146887
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858237"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Alterar a ilustração na página de entrada do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

## <a name="change-the-illustration"></a>Alterar a ilustração  


Para alterar a ilustração, o gráfico à esquerda, que é exibido no sinal de\-na página, use o seguinte cmdlet do Windows PowerShell e a sintaxe.  

![alterar a ilustração](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Recomendamos que as dimensões para a ilustração sejam de 1420x1080 pixels a 96 DPI com um arquivo que não exceda o tamanho de 200 KB.  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
  
  
