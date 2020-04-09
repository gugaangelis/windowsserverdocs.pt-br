---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: Adicione o link da página inicial
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f4a6210b1b7475a4ec34bbe0575915f376381fe1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859379"
---
# <a name="add-home-link"></a>Adicione o link da página inicial 

Para adicionar o link inicial exibido na página\-de entrada, use o cmdlet e a sintaxe do Windows PowerShell a seguir. 


![Adicionar link inicial](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> O parâmetro `linkText` nesse cmdlet não é necessário, a menos que você use outro valor diferente do padrão, que é *Página Inicial*. A vantagem de usar o padrão é que ele é localizado para todas as localidades de cliente. Depois que o sinal\-na página é personalizado, a personalização tem precedência; Portanto, você deve personalizar o para todos os idiomas aos quais deseja dar suporte.

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
