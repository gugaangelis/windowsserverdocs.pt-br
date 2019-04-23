---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d50d26c5c654e5c91b82f6f209e21f257221c12d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843577"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esse serviços de Federação do Active Directory \(do AD FS\) meta de implantação se baseia na meta no [fornecer Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Quando você é um administrador na organização do parceiro de conta e tem uma meta de implantação de fornecer acesso federado para funcionários a recursos hospedados em outra organização:  
  
-   Os funcionários que fizerem logon em um domínio do Active Directory na rede corporativa podem usar único\-sinal\-nos \(SSO\) funcionalidade para acessar vários Web\-com base em aplicativos ou serviços, que são protegidos pelo AD FS, quando os aplicativos ou serviços estão em uma organização diferente. Para obter mais informações, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Por exemplo, Fabrikam pode querer que os funcionários da rede corporativa tenham acesso federado a serviços Web hospedados na Contoso.  
  
-   Funcionários remotos que fizerem logon em um domínio do Active Directory podem obter tokens do AD FS do servidor de federação na sua organização para ganhar acesso federado ao AD FS – protegido Web\-com base em aplicativos ou serviços que são hospedados em outro organização.  
  
    Por exemplo, Fabrikam pode querer que seus funcionários remotos tenham acesso federado a serviços do AD FS – protegido que são hospedados na Contoso, sem a necessidade dos funcionários da Fabrikam estejam na rede corporativa da Fabrikam.  
  
Além dos componentes básicos descritos em [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) e que estão sombreados na ilustração a seguir, estes componentes são necessários para essa meta de implantação:  
  
-   **Proxy do servidor de federação de parceiro de conta:** Os funcionários que acessam o aplicativo ou serviço federado da Internet podem usar esse componente do AD FS para realizar a autenticação. Por padrão, esse componente executa a autenticação de formulários, mas também pode executar a autenticação básica. Você também pode configurar esse componente para realizar o Secure Sockets Layer \(SSL\) se os funcionários da sua organização tiverem certificados para apresentar a autenticação do cliente. Para obter mais informações, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **DNS de perímetro:** Essa implementação do sistema de nomes de domínio \(DNS\) fornece os nomes de host para a rede de perímetro. Para obter mais informações sobre como configurar o DNS de perímetro para um proxy do servidor de federação, consulte [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Funcionário remoto:** O funcionário remoto acessa um Web\-aplicativo baseado em \(por meio de um navegador da Web com suporte\) ou uma Web\-serviço baseado no \(por meio de um aplicativo\), usando credenciais válidas da a rede corporativa, enquanto o funcionário está fora do local usando a Internet. Computador de cliente do funcionário no local remoto se comunica diretamente com o proxy do servidor de federação para gerar um token e autenticar para o aplicativo ou serviço.  
  
Depois de revisar as informações nos tópicos vinculados, você pode começar a implantar essa meta, seguindo as etapas em [lista de verificação: Implementando um Design SSO da Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
A ilustração a seguir mostra cada um dos componentes necessários para essa meta de implantação do AD FS.  
  
![acesso a seus aplicativos](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
