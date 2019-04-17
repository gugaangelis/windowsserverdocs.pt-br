---
ms.assetid: da035189-e87f-4597-9933-49bf391a8d5d
title: "Adicionar o Link início"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fb903c62e717e36099934e64e1c939a502f691a3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="add-home-link"></a>Adicionar o Link início 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Para adicionar o link inicial que é exibido na página de sign\, use a sintaxe e o seguinte cmdlet do Windows PowerShell. 


![Adicionar o link Início](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  

`Set-AdfsGlobalWebContent -HomeLink https://fs1.contoso.com/home/ -HomeLinkText Home ` 
 
  
> [!IMPORTANT]  
> O `linkText`parâmetro neste cmdlet não é necessário, a menos que você usa outro valor que o padrão, que é *Home*. A vantagem de usar o padrão é que eles estão localizados em todas as localidades de cliente. Depois que a página de sign\ é personalizada, a personalização tem precedência; Portanto, você deve personalizar para todos os idiomas que você deseja dar suporte.

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
