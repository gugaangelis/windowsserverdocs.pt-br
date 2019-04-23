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
ms.openlocfilehash: 1654add6a81169b3d4831d6ebba320402e0734c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849857"
---
# <a name="add-help-desk-link"></a>Adicione link de suporte técnico 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2


## <a name="to-add-a-help-desk-link"></a>Para adicionar um Link de suporte técnico  
Para adicionar o link de suporte técnico que é exibido no sinal de\-na página, use o seguinte cmdlet do Windows PowerShell e a sintaxe.  

![Adicionar o suporte técnico](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  

`Set-AdfsGlobalWebContent -HelpDeskLink https://fs1.contoso.com/help/ -HelpDeskLinkText Help`  
 
  
> [!IMPORTANT]  
> O parâmetro `linkText` nesse cmdlet não é necessário, a menos que você use outro valor diferente do padrão, que é *Ajuda*. A vantagem de usar o padrão é que ele é localizado para todas as localidades de cliente. Depois de personalizar a página, a personalização terá prioridade; portanto, você deve personalizar todos os idiomas aos quais deseja dar suporte.  


## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
