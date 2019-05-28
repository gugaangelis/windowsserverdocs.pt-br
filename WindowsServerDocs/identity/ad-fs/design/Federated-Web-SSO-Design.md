---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Design SSO da Web Federado
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f7454279f234f65136b9fe6649a6e96ea53e5d51
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191513"
---
# <a name="federated-web-sso-design"></a>Design SSO da Web Federado

Único de Web federado\-sinal\-nos \(SSO\) design nos serviços de Federação do Active Directory \(AD FS\) envolve comunicação segura que abrange vários firewalls, perímetro redes e o nome\-servidores de resolução — além da infraestrutura de roteamento de Internet inteira.  
  
Normalmente, esse design é usado quando duas organizações concordam em criar uma relação de confiança de federação para permitir que os usuários em uma organização \(organização do parceiro de conta\) para acessar a Web\-com base em aplicativos ou serviços , que é protegido pelo AD FS, na outra organização \(organização do parceiro de recurso\).  
  
Em outras palavras, uma relação de confiança de Federação é a incorporação de uma empresa\-contrato de nível ou uma parceria entre duas organizações. Conforme mostrado na ilustração a seguir, você pode estabelecer uma relação de confiança de federação entre duas empresas, o que resulta em um término\-para\-cenário de federação de ponta.  
  
![sso da web federado](media/adfs2_FederatedWebSSODesign.gif)  
  
Aquele\-setas na ilustração indica a direção da Federação confia, que — assim como a direção das relações de confiança do Windows — sempre aponta para o lado da conta da floresta. Isso significa que a autenticação flui da organização do parceiro de conta para a organização do parceiro de recurso.  
  
Nesse design de SSO da Web federado, dois servidores de Federação \(um na Fabrikam e outro na Contoso\) rotear solicitações de autenticação de contas de usuário na Fabrikam Web\-com base em aplicativos ou serviços na Contoso.  
  
> [!NOTE]  
> Para obter segurança adicional, você pode usar proxies do servidor de federação para retransmitir solicitações para servidores de federação que não são diretamente acessíveis pela Internet.  
  
Neste exemplo, a Fabrikam é o provedor de identidade ou conta. A parte da Fabrikam do design SSO da Web federado usa a seguinte meta de implantação do AD FS:  
  
-   [Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
Contoso é o provedor de recursos. A parte da Contoso do design SSO da Web federado atinge as seguintes metas de implantação do AD FS:  
  
-   [Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços com reconhecimento de declarações](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar o design de SSO da Web federado, consulte [lista de verificação: Implementando um Design SSO da Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
