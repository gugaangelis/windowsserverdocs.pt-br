---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Adicione link de privacidade
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: c0e65053529999b8463223f7654b357464b90642
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954272"
---
# <a name="add-privacy-link"></a>Adicione link de privacidade


Para adicionar o link de privacidade que é exibido na página de entrada \- , use o cmdlet e a sintaxe do Windows PowerShell a seguir.

![Adicionar link de privacidade](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)


`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`


> [!IMPORTANT]
> O parâmetro `linkText` nesse cmdlet não é necessário, a menos que você use outro valor diferente do padrão, que é *Privacidade*. A vantagem de usar o padrão é que as páginas são localizadas para todas as localidades de cliente. Depois que a \- página de entrada é personalizada, a personalização tem precedência; portanto, você deve personalizar para todos os idiomas aos quais você deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Ao configurar conteúdo localizado, você deve configurá-lo com um país \- menos localidade primeiro, por exemplo, "en", antes de configurar a localidade específica de país e região \- , como "en \- US".

## <a name="additional-references"></a>Referências adicionais
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)
