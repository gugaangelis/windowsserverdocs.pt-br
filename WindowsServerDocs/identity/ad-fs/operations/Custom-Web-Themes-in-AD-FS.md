---
ms.assetid: 0379abc3-25c7-46ab-9a6b-80a5152365b0
title: Temas Web personalizados no AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 69255eeaecd3e5198054242c1ab6dd1d0a58ce33
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816419"
---
# <a name="custom-web-themes-in-ad-fs"></a>Temas Web personalizados no AD FS 

O tema que é enviado\-de\-caixa de\-é chamado de padrão. Você pode exportar o tema padrão e usá-lo de forma a poder iniciar rapidamente. Você pode personalizar a aparência e o comportamento, que incluem o layout, modificando o arquivo .css, importando e aplicando esse novo tema, depois você pode usar a aparência e o comportamento personalizados. Usar o arquivo .css também facilita o trabalho com seus web designers.  
  
O cmdlet a seguir cria um tema da Web personalizado, que duplica o tema da Web padrão.  
  
  
`New-AdfsWebTheme –Name custom –SourceName default ` 

  
Você pode modificar o arquivo .css e configurar o novo tema da Web usando o novo arquivo .css. Para exportar um tema da Web, use o seguinte cmdlet.  
  

    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  

  
Para aplicar o arquivo .css para o novo tema, use o seguinte cmdlet.  
  

    Set-AdfsWebTheme –TargetName custom –StyleSheet @{path="c:\NewTheme.css"}  
  
  
O seguinte cmdlet cria um tema da Web personalizado para uma nova folha de estilo.  
  
  
`New-AdfsWebTheme –Name custom –StyleSheet @{path="c:\NewTheme.css"} –RTLStyleSheetPath c:\NewRtlTheme.css ` 
  
  
  
Para aplicar o tema da Web personalizado a AD FS, use o cmdlet a seguir.  
  

`Set-AdfsWebConfig -ActiveThemeName custom`  

  
Para adicionar JavaScript ao AD FS, use o cmdlet a seguir.  
  
 
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=' /adfs/portal/script/onload.js';path="D:\inetpub\adfsassets\script\onload.js"}  


## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
