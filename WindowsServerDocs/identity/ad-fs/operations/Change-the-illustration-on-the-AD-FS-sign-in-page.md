---
ms.assetid: a4526500-24b3-423d-805c-24b0d8061aba
title: Alterar a ilustração na página de entrada AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 91ba9ca9068ccf2d619c8f98e1f678df7194e1a6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956513"
---
# <a name="change-the-illustration-on-the-ad-fs-sign-in-page"></a>Alterar a ilustração na página de entrada AD FS

## <a name="change-the-illustration"></a>Alterar a ilustração

Para alterar a ilustração, o gráfico à esquerda, que é exibido na página de entrada \- , use o seguinte cmdlet e sintaxe do Windows PowerShell.

![alterar ilustração](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

> [!IMPORTANT]
> Recomendamos que as dimensões para a ilustração sejam de 1420x1080 pixels a 96 DPI com um arquivo que não exceda o tamanho de 200 KB.

```powershell
Set-AdfsWebTheme -TargetName default -Illustration @{path="c:\Contoso\illustration.png"}
```

## <a name="additional-references"></a>Referências adicionais

[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)
