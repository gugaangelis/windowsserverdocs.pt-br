---
ms.assetid: 09f335bb-896a-45dd-adc2-f215b8fba828
title: Design SSO da Web federado
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b85f49ac0556bf9b3542a23514d7fcbf82d2d88e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="federated-web-sso-design"></a>Design SSO da Web federado

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O design federados Single\-Sign\-On Web \(SSO\) em serviços de Federação do Active Directory \(AD FS\) envolve a comunicação segura que abrange vários firewalls, redes de perímetro e servidores de resolução de name\ — além a infraestrutura de roteamento de Internet inteira.  
  
Normalmente, esse design é usado quando as duas organizações concordam criar uma relação de confiança de federação para permitir que os usuários em uma organização \ (a conta parceiro organization\) para acessar serviços, que são protegidos pelo AD FS, em outra organização ou aplicativos baseados em Web\ \ (o organization\ do parceiro de recurso).  
  
Em outras palavras, uma relação de confiança de Federação é a incorporação de um contrato de nível de business\ ou parceria entre duas organizações. Conforme mostrado na ilustração a seguir, você pode estabelecer uma relação de confiança de federação entre duas empresas, o que resulta em um cenário de Federação end\ to\ ponta.  
  
![Sso da web federado](media/adfs2_FederatedWebSSODesign.gif)  
  
A seta one\ vias na ilustração significa a direção da Federação confia, que — assim como a direção de relações de confiança do Windows — sempre aponta para o lado da conta da floresta. Isso significa que a autenticação flui da organização de parceiro de conta para a organização de parceiro de recurso.  
  
Neste design de SSO da Web federado, dois servidores de Federação \ (na Fabrikam e o outro em Contoso\) roteiam solicitações de autenticação de contas de usuário na Fabrikam para aplicativos baseados em Web\ ou serviços na Contoso.  
  
> [!NOTE]  
> Para ter mais segurança, você pode usar proxies de servidor de federação para solicitações de retransmissão para servidores de federação que não são acessadas diretamente da Internet.  
  
Neste exemplo, a Fabrikam é o provedor de identidade ou conta. A parte da Fabrikam do design SSO da Web federado usa a seguinte meta de implantação do AD FS:  
  
-   [Fornecer o acesso de usuários do Active Directory para os aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
A Contoso é o provedor de recursos. A parte da Contoso do design SSO da Web federado realiza as seguintes metas de implantação do AD FS:  
  
-   [Fornecer aos usuários em outra organização acesso aos seus aplicativos com reconhecimento de declarações e serviços](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornecer seu do Active Directory aos usuários acesso aos seus aplicativos com reconhecimento de declarações e serviços](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar o design de SSO da Web federado, consulte [lista de verificação: Implementando um Design de SSO da Web federado ](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
