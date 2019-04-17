---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: Remover os direitos autorais Microsoft
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c2e6f9445e53a5b5867a763d58ad4a6ca3600cbe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="remove-the-microsoft-copyright"></a>Remover os direitos autorais Microsoft 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2
 
Por padrão, as páginas do AD FS contêm os direitos autorais Microsoft. Para remover esse direito autoral de suas páginas personalizadas, você pode usar o procedimento a seguir. 

![Remover direitos autorais](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>Para remover os direitos autorais Microsoft  
  
1.  Crie um tema personalizado que é baseado no padrão.  
  

    `New-AdfsWebTheme –Name custom –SourceName default ` 
 
  
2.  Exporte o tema, especificando a pasta de saída.  

    `Export-AdfsWebTheme -Name custom -DirectoryPath C:\customWebTheme ` 

  
3.  Localize o arquivo Style.css que está localizado na pasta de saída. Usando o exemplo anterior, o caminho seria C:\\CustomWebTheme\\Css\\Style.css.  
  
4.  Abra o arquivo Style.css com um editor, como o bloco de notas.  
  
5.  Localize o `#copyright` parte e altere-o para o seguinte:  
  `#copyright {color:#696969; display:none;} ` 
 
6.  Crie um tema personalizado que é baseado no novo arquivo Style.css.  
  
    `Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}  `

7.  Ative o novo tema.  
  

    `Set-AdfsWebConfig -ActiveThemeName custom ` 


Agora, você não deverá mais ver os direitos autorais na parte inferior da página de entrada.

![Remover direitos autorais](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md) 
