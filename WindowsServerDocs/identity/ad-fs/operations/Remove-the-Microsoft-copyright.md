---
ms.assetid: c89a977c-b09f-44ec-be42-41e76a6cf3ad
title: Remova os direitos autorais da Microsoft
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ec7d9cb02508fc046ce3e8f0378e63c82eecca8d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87949770"
---
# <a name="remove-the-microsoft-copyright"></a>Remova os direitos autorais da Microsoft



Por padrão, as páginas de AD FS contêm os direitos autorais da Microsoft. Para remover esses direitos autorais de suas páginas personalizadas, execute o seguinte procedimento.

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

3. Localize o `Style.css` arquivo localizado na pasta de saída. Usando o exemplo anterior, o caminho seria`C:\CustomWebTheme\Css\Style.css.`

4. Abra o `Style.css` arquivo com um editor, como o bloco de notas.

5. Localize a parte de `#copyright` e altere-a para o seguinte:

   ```css
   #copyright {color:#696969; display:none;}
   ```

6. Crie um tema personalizado baseado no novo `Style.css` arquivo.

   ```powershell
   Set-AdfsWebTheme -TargetName custom -StyleSheet @{locale="";path="C:\customWebTheme\css\style.css"}
   ```

7. Ative o novo tema.

   ```powershell
   Set-AdfsWebConfig -ActiveThemeName custom
   ```

Agora, você não deve mais ver os direitos autorais na parte inferior da página de entrada.

![remover direitos autorais](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1a.png)

## <a name="additional-references"></a>Referências adicionais
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)
