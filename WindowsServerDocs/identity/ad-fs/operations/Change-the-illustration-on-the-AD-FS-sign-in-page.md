---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: Alterar a ilustração na página de entrada AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: de0fc7af4c4eecad59b7af6b08426ef207da5db1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859949"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Alterar a ilustração na página de entrada AD FS

## <a name="change-the-illustration"></a>Alterar a ilustração  


Para alterar a ilustração, o gráfico à esquerda, que é exibido na página\-de entrada, use o seguinte cmdlet e sintaxe do Windows PowerShell.  

![alterar ilustração](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Recomendamos que as dimensões para a ilustração sejam de 1420x1080 pixels a 96 DPI com um arquivo que não exceda o tamanho de 200 KB.  
  
 
    Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
  
  
