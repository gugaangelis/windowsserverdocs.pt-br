---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: Adicione o link da página inicial
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e5f1fad340629304fdf960139be05b8dbc2690e0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407776"
---
# <a name="add-home-link"></a>Adicione o link da página inicial 

Para adicionar o link inicial que é exibido na página assinar @ no__t-0in, use o seguinte cmdlet e sintaxe do Windows PowerShell. 


![Adicionar link inicial](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> O parâmetro `linkText` nesse cmdlet não é necessário, a menos que você use outro valor diferente do padrão, que é *Página Inicial*. A vantagem de usar o padrão é que ele é localizado para todas as localidades de cliente. Depois que a página Sign @ no__t-0in for personalizada, a personalização terá precedência; Portanto, você deve personalizar o para todos os idiomas aos quais deseja dar suporte.

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
