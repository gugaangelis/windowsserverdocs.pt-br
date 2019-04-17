---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: "Adicionar o Link de suporte técnico da Ajuda"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6d16cc0a75bfe636c29b44687b669e87f31b69ce
ms.sourcegitcommit: 76e57a5453d6ee9a04dcff6a8cca087132cb1d5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/20/2018
---
# <a name="add-help-desk-link"></a>Adicionar o Link de suporte técnico da Ajuda 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2


## <a name="to-add-a-help-desk-link"></a>Para adicionar um Link de suporte técnico ajuda  
Para adicionar o link de suporte técnico de Ajuda que é exibido na página de sign\, use a sintaxe e o seguinte cmdlet do PowerShell do Windows PowerShell.  

![Adicionar suporte técnico](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> O `linkText`parâmetro neste cmdlet não é necessário, a menos que você usa outro valor que o padrão, que é *ajudar*. A vantagem de usar o padrão é que eles estão localizados em todas as localidades de cliente. Depois que a página é personalizada, a personalização tem precedência; Portanto, você deve personalizar para todos os idiomas que você deseja dar suporte.  


## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
