---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: Remover os direitos autorais da Microsoft
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aa27a014b0803712a20fdd23f075486dc35f33c2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820977"
---
# <a name="remove-the-microsoft-copyright"></a>Remover os direitos autorais da Microsoft 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2
 
Por padrão, as páginas do AD FS contêm os direitos autorais da Microsoft. Para remover esses direitos autorais de suas páginas personalizadas, execute o seguinte procedimento. 

![remover direitos autorais](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png) 
  
## <a name="to-remove-the-microsoft-copyright"></a>Para remover os direitos autorais da Microsoft  
  
1. Crie um tema personalizado que tenha por base o padrão.

   ```powershell
   New-AdfsWebTheme –Name custom –SourceName default
   ```

2. Exporte o tema especificando a pasta de saída.  

   ```powershell
   Export-AdfsWebTheme -Name custom -DirectoryPath C:\CustomWebTheme
   ```

3. Localize o `Style.css` arquivo está localizado na pasta de saída. Usando o exemplo anterior, o caminho seria `C:\CustomWebTheme\Css\Style.css.`
  
4. Abra o `Style.css` arquivo com um editor, como o bloco de notas.  
  
5. Localize a parte de `#copyright` e altere-a para o seguinte:  

   ```css
   #copyright {color:#696969; display:none;}
   ```

6. Criar um tema personalizado que é baseado no novo `Style.css` arquivo.  

   ```powershell
   Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}
   ```

7. Ative o novo tema.  

   ```powershell
   Set-AdfsWebConfig -ActiveThemeName custom
   ```

Agora, você não verá os direitos autorais na parte inferior da página de entrada.

![remover direitos autorais](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png) 

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md) 
