---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: "Personalização de senha de atualização"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4b06992bfb398b66988ad4882217a8a83738365e
ms.sourcegitcommit: 78d8839ccafa9530784cb9e38c3127ed2c215423
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/09/2018
---
# <a name="update-password-customization"></a>Personalização de senha de atualização 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Em alguns casos, os usuários não poderão se conectar à rede corporativa para alterar a senha da conta. Esse fator pode ser problemático especialmente para funcionários remotos que talvez vivem longe do mais próximo sede. Nesses casos específicos, a página de senha de atualização pode ser usada conectando-se somente à Internet.  
  
Você pode personalizar a página de senha de atualização, fornecendo sua própria descrição da página.  
  
> Para permitir que a página de atualização de senha, vá para o AD FS gerenciamento em pontos de extremidade. O ponto de extremidade para atualização senha está localizado na parte inferior em outros - / adfs/portal/updatepassword /. Depois que tiver habilitado o ponto de extremidade, você deve reiniciar o serviço do AD FS. Isso deve ser feito manualmente. Em seguida, você pode navegar para https://<fqdn>/adfs/portal/updatepassword/em um local de trabalho ingressar o dispositivo e você verá a página de senha de atualização.  
  
![Atualizar](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personalizar a descrição da página de senha de atualização  
Para personalizar a descrição de página da senha de atualização, use a sintaxe e o seguinte cmdlet do Windows PowerShell.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
