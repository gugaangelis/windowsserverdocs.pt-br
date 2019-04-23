---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: Personalização de descoberta de Realm inicial
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1198d8b76f2ecdad728e2de6ce7a5c0d053f779f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868927"
---
# <a name="home-realm-discovery-customization"></a>Personalização de descoberta de Realm inicial

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Quando o cliente do AD FS primeiro solicita um recurso, o servidor de Federação do recurso não possui informações sobre o realm do cliente. O servidor de Federação do recurso responde ao cliente do AD FS com um **descoberta de Realm do cliente** página, em que o usuário seleciona o realm inicial de uma lista. Os valores da lista são preenchidos a partir da propriedade de nome de exibição nas Relações de Confiança do Provedor de Declarações. Use os seguintes cmdlets do Windows PowerShell para modificar e personalizar a experiência do AD FS Home Realm Discovery.  
  
![realm de início](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> Fique atento ao nome do Provedor de Declarações que aparece para o Active Directory local, se ele é o nome de exibição do serviço de federação.  
  



## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>Configure o Provedor de identidade para usar determinados sufixos de email  
Uma organização pode federar com vários provedores de declarações. O AD FS agora fornece no\-caixa funcionalidade para administradores listarem os sufixos, por exemplo, @us.contoso.com, @eu.contoso.com, que é aceito por um provedor de declarações e habilitá-la para o sufixo\-com base em descoberta. Com essa configuração, os usuários finais podem digitar sua conta institucional e AD FS selecionará automaticamente o provedor de declarações correspondente.  
  
Para configurar um provedor de identidade \(IDP\), como `fabrikam`, para usar certos sufixos de email, use o cmdlet do Windows PowerShell e a sintaxe a seguir.  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
>[!NOTE]
> Ao estabelecer uma federação entre dois servidores do AD FS, defina a propriedade de PromptLoginFederation sobre a relação de confiança do provedor de declarações para ForwardPromptAndHintsOverWsFederation.  Isso é para que o AD FS para encaminhar o login_hint e o parâmetro de prompt para o IDP.  Isso pode ser feito executando o seguinte cmdlet do PowerShell:
>
>`Set-AdfsclaimsProviderTrust -PromptLoginFederation ForwardPromptAndHintsOverWsFederation`

## <a name="configure-an-identity-provider-list-per-relying-party"></a>Configurar uma lista de provedores de identidade por terceira parte confiável.  
Para alguns cenários, uma organização pode desejar que os usuários finais vejam somente os provedores de declarações específicos para um aplicativo, de forma que somente um subconjunto de provedores de declarações é exibido na página de descoberta de realm inicial.  
  
Para configurar uma lista de IDP por terceira parte confiável participante \(RP\), use o seguinte cmdlet do Windows PowerShell e a sintaxe.  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>Ignore a descoberta de realm inicial para Intranet.  
A maior parte das organizações dá suporte somente ao seu Active Directory local para qualquer usuário que acessa de dentro de seu firewall. Nesses casos, os administradores podem configurar o AD FS para ignorar a descoberta de realm inicial para intranet.  
  
Para ignorar a HRD para intranet, use o seguinte cmdlet do Windows PowerShell e a sintaxe.  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> Observe que se uma lista de provedor de identidade para uma terceira parte confiável tiver sido configurada, mesmo se a configuração anterior tiver sido habilitada e o usuário acessar da intranet, o AD FS ainda mostrará a descoberta de realm inicial \(HRD\) página. Para ignorar a HRD nesse caso, você precisa assegurar que o "Active Directory" também tenha sido adicionado à lista de IDP para essa terceira parte confiável.  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
