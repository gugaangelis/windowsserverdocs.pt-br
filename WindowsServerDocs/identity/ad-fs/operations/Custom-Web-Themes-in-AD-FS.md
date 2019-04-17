---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: Temas da Web personalizado no AD FS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 663623cf34fcfe715080945ebcc135587b5ddba1
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="custom-web-themes-in-ad-fs"></a>Temas da Web personalizado no AD FS 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

O tema é enviado out\-of\-the\-box é chamado de padrão. Você pode exportar o tema padrão e usá-lo para que você possa começar rapidamente. Você pode personalizar a aparência e comportamento, que inclui o layout modificando o arquivo. CSS, importar e aplicar esse novo tema e, em seguida, você pode usar a aparência personalizada e comportamento. Usar o arquivo. CSS também torna mais fácil trabalhar com seus designers da web.  
  
O seguinte cmdlet cria um tema personalizado na web, que duplica o tema da web padrão.  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
Você pode modificar o arquivo. CSS e configurar o novo tema da web usando o novo arquivo. CSS. Para exportar um tema da web, use o seguinte cmdlet.  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
Para aplicar o arquivo. CSS para o novo tema, use o seguinte cmdlet.  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path=”c:\NewTheme.css”}  
  
  
O seguinte cmdlet cria um tema personalizado da web de uma nova folha de estilos.  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path=”c:\NewTheme.css”} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
Para aplicar o tema da web personalizado para o AD FS, use o seguinte cmdlet.  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
Para adicionar JavaScript ao AD FS, use o seguinte cmdlet.  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’ /adfs/portal/script/onload.js’;path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
