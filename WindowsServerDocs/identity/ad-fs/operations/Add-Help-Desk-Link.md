---
ms.assetid: 2bac7744-9de3-491a-b0a2-4e843cec7344
title: Adicione link de suporte técnico
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb186c3ba5cfb3acb9bfd0c3139b09b992fb8863
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190213"
---
# <a name="add-help-desk-link"></a>Adicione link de suporte técnico 


## <a name="to-add-a-help-desk-link"></a>Para adicionar um Link de suporte técnico  
Para adicionar o link de suporte técnico que é exibido no sinal de\-na página, use o seguinte cmdlet do Windows PowerShell e a sintaxe.  

![Adicionar o suporte técnico](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> O parâmetro `linkText` nesse cmdlet não é necessário, a menos que você use outro valor diferente do padrão, que é *Ajuda*. A vantagem de usar o padrão é que ele é localizado para todas as localidades de cliente. Depois de personalizar a página, a personalização terá prioridade; portanto, você deve personalizar todos os idiomas aos quais deseja dar suporte.  


## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
