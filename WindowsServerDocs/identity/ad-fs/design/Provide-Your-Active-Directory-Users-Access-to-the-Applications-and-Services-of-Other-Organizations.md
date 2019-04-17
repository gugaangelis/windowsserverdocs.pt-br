---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: "Fornecer o acesso de usuários do Active Directory para os aplicativos e serviços de outras organizações"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d50d26c5c654e5c91b82f6f209e21f257221c12d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Fornecer o acesso de usuários do Active Directory para os aplicativos e serviços de outras organizações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Essa meta de implantação do serviços de Federação do Active Directory \(AD FS\) complementa o objetivo de [fornecer o Active Directory aos usuários acesso a seus aplicativos com reconhecimento de declarações e serviços](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Quando você for um administrador na organização do parceiro de conta e ter uma meta de implantação para fornecer acesso federado para os funcionários hospedado recursos em outra organização:  
  
-   Os funcionários estejam conectados a um domínio do Active Directory na rede da empresa podem usar sign\ single\ em \(SSO\) funcionalidade para acessar vários aplicativos baseados em Web\ ou serviços, que são protegidos pelo AD FS, quando os aplicativos ou serviços estão em uma organização diferente. Para obter mais informações, consulte [Design de SSO da Web federado](Federated-Web-SSO-Design.md).  
  
    Por exemplo, Fabrikam pode querer funcionários tenham acesso a serviços Web que são hospedados na Contoso federado da rede corporativa.  
  
-   Funcionários remotos que estejam conectados a um domínio do Active Directory podem obter tokens do AD FS do servidor de Federação em sua organização acessem federado AD FS – protegidas com base em Web\ aplicativos ou serviços que são hospedados em outra organização.  
  
    Por exemplo, Fabrikam pode querer seus funcionários tenham acesso aos Serviços AD FS – protegidas que são hospedados na Contoso, sem a necessidade dos funcionários da Fabrikam seja na rede corporativa Fabrikam federado remotos.  
  
Além dos componentes básicos descritos nos [fornecer o Active Directory aos usuários acesso a seus aplicativos com reconhecimento de declarações e serviços](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) e que são sombreado na ilustração a seguir, os seguintes componentes são necessários para essa meta de implantação:  
  
-   **Proxy do servidor de federação de parceiro da conta:** funcionários que acessam o serviço federado ou aplicativo da Internet podem usar esse componente AD FS para realizar autenticação. Por padrão, esse componente faz a autenticação de formulários, mas também pode executar a autenticação básica. Você também pode configurar esse componente para realizar autenticação de cliente Secure Sockets Layer \(SSL\) se os funcionários em sua organização tem certificados para apresentar. Para obter mais informações, consulte [onde colocar um Proxy de servidor de Federação](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **Perímetro DNS:** esta implementação de Domain Name System \(DNS\) fornece os nomes de host para a rede do perímetro. Para obter mais informações sobre como configurar o perímetro DNS para um proxy de servidor de federação, consulte [requisitos de resolução de nome de Proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Funcionário remoto:** o funcionário remoto acessa um aplicativo baseado em Web\ \ (por meio de um browser\ da Web com suporte) ou um serviço baseado em Web\ \ (por meio de um application\), usando credenciais válidas da rede corporativa, enquanto o funcionário é externo usando a Internet. O computador do funcionário cliente no local remoto se comunica diretamente com o proxy do servidor de federação para gerar um token e autenticar para o aplicativo ou serviço.  
  
Após analisar as informações nos tópicos vinculados, você pode começar a implantar essa meta seguindo as etapas em [lista de verificação: Implementando um Design de SSO da Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
A ilustração a seguir mostra cada um dos componentes necessários para essa meta de implantação do AD FS.  
  
![Acesso a seus aplicativos](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
