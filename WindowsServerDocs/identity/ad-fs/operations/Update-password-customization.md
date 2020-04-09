---
ms.assetid: 7e804590-6d6c-4cca-ac14-02d4dff06cec
title: Atualizar personalização de senha
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cb5fd0ff432e441900e379d3fe798dbe6aef855f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816099"
---
# <a name="update-password-customization"></a>Atualizar personalização de senha 


Em algumas instâncias, os usuários, às vezes, não podem se conectar à rede corporativa para alterarem a senha de sua conta. Esse fator pode ser especialmente problemático para funcionários remotos que moram longe do escritório corporativo mais próximo. Para esses casos específicos, a página de atualização de senha pode ser usada somente para se conectar à Internet.  
  
Você pode personalizar a página de atualização de senha fornecendo sua própria descrição para a página.  
  
> Para habilitar a página de atualização de senha, acesse o Gerenciamento de AD FS sob Pontos de Extremidade. O ponto de extremidade para atualização de senha está localizado na parte inferior, sob Outros - /adfs/portal/updatepassword/. Depois de ter habilitado o ponto de extremidade, você deve reiniciar o serviço AD FS. Isso deve ser feito manualmente. Você pode, então, navegar para https://<fqdn>/adfs/portal/updatepassword/ em um dispositivo integrado em local de trabalho e deverá ver a página de atualização de senha.  
  
![{1&gt;update&lt;1}](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom5.png)  
  
## <a name="customize-the-update-password-page-description"></a>Personalize a descrição da página de Atualização de senha  
Para personalizar a descrição da página de senha de atualização, use o seguinte cmdlet e sintaxe do Windows PowerShell.  
  

    Set-AdfsGlobalWebContent -UpdatePasswordPageDescriptionText "This is the Contoso Update Password page."  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
