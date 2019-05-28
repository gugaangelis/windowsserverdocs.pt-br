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
ms.openlocfilehash: a182b6243777d119394615008cee63b5e5a71ab8
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189954"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Alterar a ilustração na página de entrada do AD FS

## <a name="change-the-illustration"></a>Alterar a ilustração  


Para alterar a ilustração, o gráfico à esquerda, que é exibido no sinal de\-na página, use o seguinte cmdlet do Windows PowerShell e a sintaxe.  

![alterar a ilustração](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Recomendamos que as dimensões para a ilustração sejam de 1420x1080 pixels a 96 DPI com um arquivo que não exceda o tamanho de 200 KB.  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
  
  
