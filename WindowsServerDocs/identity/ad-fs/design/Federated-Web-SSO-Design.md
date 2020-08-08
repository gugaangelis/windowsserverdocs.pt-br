---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Design SSO da Web Federado
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: f3f34aca7f7a316ff714b88209e3bf81de420ecb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942760"
---
# <a name="federated-web-sso-design"></a>Design SSO da Web Federado

O design de SSO de logon único da Web federada \- \- \( \) no serviços de Federação do Active Directory (AD FS) \( AD FS \) envolve a comunicação segura que abrange vários firewalls, redes de perímetro e servidores de resolução de nomes, \- além de toda a infraestrutura de roteamento da Internet.

Normalmente, esse design é usado quando duas organizações concordam em criar uma relação de confiança de Federação para permitir que os usuários em uma organização \( a organização do parceiro de conta \) acessem \- aplicativos ou serviços baseados na Web, que são protegidos pelo AD FS, na outra organização \( da organização do parceiro de recursos \) .

Em outras palavras, uma relação de confiança de Federação é a Embodiment de um contrato de nível de negócios \- ou parceria entre duas organizações. Conforme mostrado na ilustração a seguir, você pode estabelecer uma relação de confiança de federação entre duas empresas, o que resulta em um cenário de Federação de ponta \- a \- ponta.

![SSO da Web federado](media/adfs2_FederatedWebSSODesign.gif)

A \- seta unidirecional na ilustração significa a direção da confiança da Federação, que, como a direção das relações de confiança do Windows, sempre aponta para o lado da conta da floresta. Isso significa que a autenticação flui da organização do parceiro de conta para a organização do parceiro de recurso.

Nesse design de SSO da Web federado, dois servidores de Federação \( um na Fabrikam e o outro na Contoso \) roteiam solicitações de autenticação de contas de usuário na Fabrikam para \- aplicativos baseados na Web ou serviços na contoso.

> [!NOTE]
> Para obter mais segurança, você pode usar proxies de servidor de Federação para retransmitir solicitações para servidores de Federação que não estão diretamente acessíveis pela Internet.

Neste exemplo, a Fabrikam é o provedor de identidade ou conta. A parte da Fabrikam do design de SSO da Web federado usa o seguinte objetivo de implantação de AD FS:

-   [Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)

Contoso é o provedor de recursos. A parte contoso do design de SSO da Web federado atinge os seguintes objetivos de implantação AD FS:

-   [Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)

-   [Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços com reconhecimento de declarações](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)

Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar o design de SSO da Web federado, consulte [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
