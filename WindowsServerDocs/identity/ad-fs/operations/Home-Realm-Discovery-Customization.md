---
ms.assetid: 626e33fc-4ac2-4d38-9ac9-addaa4c8d9bb
title: "Personalização de descoberta de território primário"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 14b19e2774cf1eac2f5f23ea6737219611886b4c
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="home-realm-discovery-customization"></a>Personalização de descoberta de território primário

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Quando o cliente do AD FS primeiro solicita um recurso, o servidor de Federação do recurso não tem nenhuma informação sobre o território do cliente. O servidor de Federação do recurso responde ao cliente AD FS com um **cliente território descoberta** página, onde o usuário seleciona o território doméstico em uma lista. Os valores de lista são preenchidos da propriedade display nome no provedor de declarações confie. Use os seguintes cmdlets do Windows PowerShell para modificar e personalizar a experiência de descoberta do AD FS Home território.  
  
![Território primário](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom4.png)  
  
> [!WARNING]  
> Lembre-se de que o nome do provedor de declarações mostrado para o Active Directory local é o nome de exibição do serviço de Federação.  
  
## <a name="configure-identity-provider-to-use-certain-email-suffixes"></a>Configurar provedor de identidade para usar determinados sufixos de email  
Uma organização pode interagir com vários provedores de declarações. AD FS agora oferece a funcionalidade de caixa in\ para os administradores para listar os sufixos, por exemplo, @us.contoso.com, @eu.contoso.com, que é compatível com um provedor de declarações e habilitá-lo com base em suffix\ descoberta. Com essa configuração, os usuários finais podem digitar em sua conta organizacional e AD FS selecionará automaticamente o provedor de declarações correspondente.  
  
Para configurar um provedor de identidade \(IDP\), como `fabrikam`, para usar determinados sufixos de email, use o cmdlet do Windows PowerShell e a sintaxe a seguir.  
  

`Set-AdfsClaimsProviderTrust -TargetName fabrikam -OrganizationalAccountSuffix @("fabrikam.com";"fabrikam2.com") ` 
 
  
## <a name="configure-an-identity-provider-list-per-relying-party"></a>Configurar uma lista de provedores de identidade por confiar terceiros  
Para alguns cenários, um de organizações talvez queiram os usuários finais para ver somente os provedores de declarações que são específicos para um aplicativo para que somente um subconjunto de provedor de declarações são exibidos na página de descoberta de território primário.  
  
Configure uma lista de IDP por confiar \(RP\) de terceiros, use a sintaxe e o seguinte cmdlet do Windows PowerShell.  
  
 
`Set-AdfsRelyingPartyTrust -TargetName claimapp -ClaimsProviderName @("Fabrikam","Active Directory") ` 

  
## <a name="bypass-home-realm-discovery-for-the-intranet"></a>Ignorar a descoberta de território Home da intranet  
A maioria das organizações suporte somente o Active Directory local para qualquer usuário que acessa a partir de dentro de seu firewall. Nesses casos, os administradores podem configurar o AD FS para ignorar a descoberta de território primário para a intranet.  
  
Para ignorar HRD para a intranet, use a sintaxe e o seguinte cmdlet do Windows PowerShell.  
  

`Set-AdfsProperties -IntranetUseLocalClaimsProvider $true ` 
 
  
> [!IMPORTANT]  
> Por favor, observe que se uma lista de provedores de identidade para uma dependência terceiros tem sido configurado, mesmo que a configuração anterior foi habilitada e os acessos de usuário da intranet, o AD FS ainda mostra a página \(HRD\) de descoberta de território primário. Para ignorar HRD nesse caso, você precisa garantir que "Active Directory" também é adicionado à lista IDP para esse terceiro.  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
