---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Design SSO da Web Federado
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9915a2942c9336d5aeb7776169d2e51491c22909
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853139"
---
# <a name="federated-web-sso-design"></a>Design SSO da Web Federado

O logon único\-da Web federado\-no design \(SSO\) no Serviços de Federação do Active Directory (AD FS) \(AD FS\) envolve a comunicação segura que abrange vários firewalls, redes de perímetro e servidores de resolução de nome\-, além de toda a infraestrutura de roteamento da Internet.  
  
Normalmente, esse design é usado quando duas organizações concordam em criar uma relação de confiança de Federação para permitir que usuários em uma organização \(a organização do parceiro de conta\) acessar aplicativos ou serviços baseados\-na Web, que são protegidos pelo AD FS, na outra organização \(\)da organização do parceiro de recursos.  
  
Em outras palavras, uma relação de confiança de Federação é a Embodiment de um contrato de nível de\-comercial ou parceria entre duas organizações. Conforme mostrado na ilustração a seguir, você pode estabelecer uma relação de confiança de federação entre duas empresas, o que resulta em um\-final para\-cenário de Federação final.  
  
![SSO da Web federado](media/adfs2_FederatedWebSSODesign.gif)  
  
A seta\-maneira na ilustração indica a direção da confiança da Federação, que, como a direção das relações de confiança do Windows, sempre aponta para o lado da conta da floresta. Isso significa que a autenticação flui da organização do parceiro de conta para a organização do parceiro de recurso.  
  
Nesse design de SSO da Web federado, dois servidores de Federação \(um na Fabrikam e o outro na contoso\) roteiam solicitações de autenticação de contas de usuário na Fabrikam para aplicativos ou serviços baseados na Web\-na contoso.  
  
> [!NOTE]  
> Para obter mais segurança, você pode usar proxies de servidor de Federação para retransmitir solicitações para servidores de Federação que não estão diretamente acessíveis pela Internet.  
  
Neste exemplo, a Fabrikam é o provedor de identidade ou conta. A parte da Fabrikam do design de SSO da Web federado usa o seguinte objetivo de implantação de AD FS:  
  
-   [Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso é o provedor de recursos. A parte contoso do design de SSO da Web federado atinge os seguintes objetivos de implantação AD FS:  
  
-   [Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços com reconhecimento de declarações](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar o design de SSO da Web federado, consulte [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
