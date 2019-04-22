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
ms.openlocfilehash: 6e0271ae3d7ac120e510a2fb81fb55c8d10b3c87
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813577"
---
# <a name="changing-the-company-logo-on-the-ad-fs-sign-in-page"></a>Alterar o logotipo da empresa na página de entrada do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

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
