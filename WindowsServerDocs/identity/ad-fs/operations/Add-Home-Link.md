---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: Adicione o link da página inicial
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a9a043390f5bfb412e549779ed4a9048d1c8a0b5
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190184"
---
# <a name="add-home-link"></a>Adicione o link da página inicial 

Para adicionar o link da página inicial que é exibido no sinal de\-na página, use o seguinte cmdlet do Windows PowerShell e a sintaxe. 


![Adicionar o link da página inicial](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> O parâmetro `linkText` nesse cmdlet não é necessário, a menos que você use outro valor diferente do padrão, que é *Página Inicial*. A vantagem de usar o padrão é que ele é localizado para todas as localidades de cliente. Após o sinal\-na página é personalizado, a personalização terá prioridade; portanto, você deve personalizar para todos os idiomas que você deseja dar suporte.

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
