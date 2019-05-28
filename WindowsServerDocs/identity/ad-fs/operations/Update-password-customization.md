---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: Atualizar senha personalização
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e67c04c98a53f4f1db36e6586fa77bcf181a8d5a
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188967"
---
# <a name="update-password-customization"></a>Atualizar senha personalização 


Em algumas instâncias, os usuários, às vezes, não podem se conectar à rede corporativa para alterarem a senha de sua conta. Esse fator pode ser especialmente problemático para funcionários remotos que moram longe do escritório corporativo mais próximo. Para esses casos específicos, a página de atualização de senha pode ser usada somente para se conectar à Internet.  
  
Você pode personalizar a página de atualização de senha fornecendo sua própria descrição para a página.  
  
> Para habilitar a página de atualização de senha, acesse o Gerenciamento de AD FS sob Pontos de Extremidade. O ponto de extremidade para atualização de senha está localizado na parte inferior, sob Outros - /adfs/portal/updatepassword/. Depois de ter habilitado o ponto de extremidade, você deve reiniciar o serviço AD FS. Isso deve ser feito manualmente. Você pode, então, navegar para https://<fqdn>/adfs/portal/updatepassword/ em um dispositivo integrado em local de trabalho e deverá ver a página de atualização de senha.  
  
![atualizar](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personalize a descrição da página de Atualização de senha  
Para personalizar a descrição da página atualização de senha, use o seguinte cmdlet do Windows PowerShell e a sintaxe.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
