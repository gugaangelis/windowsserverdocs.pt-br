---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Design SSO da Web Federado
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a3e7eb6c42c8190da799c88c1e947e6aef1c29f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408105"
---
# <a name="federated-web-sso-design"></a>Design SSO da Web Federado

O design da Web federado @ no__t-0Sign @ no__t-1On \(SSO @ no__t-3 no Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-5 envolve uma comunicação segura que abrange vários firewalls, redes de perímetro e Name @ no__t-6resolution servidores — além de toda a infraestrutura de roteamento da Internet.  
  
Normalmente, esse design é usado quando duas organizações concordam em criar uma relação de confiança de Federação para permitir que os usuários em uma organização @no__t a organização do parceiro de conta @ no__t-1 Acesse os aplicativos ou serviços Web @ no__t-2based, que são protegidos pelo AD FS, na outra organização \(The a organização do parceiro de recurso @ no__t-4.  
  
Em outras palavras, uma relação de confiança de Federação é a Embodiment de um contrato Business @ no__t-0level ou parceria entre duas organizações. Conforme mostrado na ilustração a seguir, você pode estabelecer uma relação de confiança de federação entre duas empresas, o que resulta em um cenário de Federação @ no__t-0to @ no__t-1Fim Federation.  
  
![SSO da Web federado](media/adfs2_FederatedWebSSODesign.gif)  
  
A seta de um @ no__t-0way na ilustração significa a direção da confiança da Federação, que, como a direção das relações de confiança do Windows, sempre aponta para o lado da conta da floresta. Isso significa que a autenticação flui da organização do parceiro de conta para a organização do parceiro de recurso.  
  
Nesse design de SSO da Web federado, dois servidores de Federação \(one na Fabrikam e o outro no contoso @ no__t-1 roteiam solicitações de autenticação de contas de usuário na Fabrikam para aplicativos Web @ no__t-2based ou serviços na contoso.  
  
> [!NOTE]  
> Para obter mais segurança, você pode usar proxies de servidor de Federação para retransmitir solicitações para servidores de Federação que não estão diretamente acessíveis pela Internet.  
  
Neste exemplo, a Fabrikam é o provedor de identidade ou conta. A parte da Fabrikam do design de SSO da Web federado usa o seguinte objetivo de implantação de AD FS:  
  
-   [Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso é o provedor de recursos. A parte contoso do design de SSO da Web federado atinge os seguintes objetivos de implantação AD FS:  
  
-   [Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços com reconhecimento de declarações](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar o design de SSO da Web federado, consulte [Checklist: Implementando um design de SSO da Web federado @ no__t-0.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
