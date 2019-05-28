---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Adicione link de privacidade
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3617335a179ab419982ab57343999ad4fcaf522a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190156"
---
# <a name="add-privacy-link"></a>Adicione link de privacidade 


Para adicionar o link de privacidade que é exibido no sinal de\-na página, use o seguinte cmdlet do Windows PowerShell e a sintaxe.  

![Adicionar link de privacidade](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> O parâmetro `linkText` nesse cmdlet não é necessário, a menos que você use outro valor diferente do padrão, que é *Privacidade*. A vantagem de usar o padrão é que as páginas são localizadas para todas as localidades de cliente. Após o sinal\-na página é personalizado, a personalização terá prioridade; portanto, você deve personalizar para todos os idiomas que você deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Quando você configura o conteúdo localizado, você deve configurá-lo com um país\-menos localidade primeiro, por exemplo, "en", antes de configurar o país e região\-localidade específica, como "en\-us".  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
