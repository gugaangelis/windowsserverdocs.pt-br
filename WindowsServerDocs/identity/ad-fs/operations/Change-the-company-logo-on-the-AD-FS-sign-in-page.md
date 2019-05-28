---
ms.assetid: f7f6bac2-1100-4b00-a248-4ca3eb3cdbe9
title: Alterar o logotipo da empresa na página de entrada do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/08/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fe5c138466ea288b5dfb8c7c284603150ab9d874
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190032"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Alterar o logotipo da empresa na página de entrada do AD FS

#### <a name="change-company-logo"></a>Altere o logotipo da empresa  
Para alterar o logotipo da empresa que é exibido no sinal de\-na página, use o seguinte cmdlet do PowerShell do Windows PowerShell e a sintaxe.  

![Alterar logotipo](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)
  
> [!IMPORTANT]  
> Recomendamos que as dimensões para o logotipo sejam de 260x35 a 96 DPI com um arquivo que não exceda o tamanho de 10 KB.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.png"}  

  
> [!NOTE]  
> O parâmetro `TargetName` é necessário. O tema padrão liberado com o AD FS é denominado *padrão*.  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
