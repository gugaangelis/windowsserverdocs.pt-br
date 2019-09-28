---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: bcf9b9ec91c1757ad060747a6aa1589012c1ec14
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359035"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações

Esse objetivo de implantação do Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 baseia-se no objetivo de [fornecer aos seus Active Directory usuários acesso aos seus aplicativos e serviços com reconhecimento de declaração](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Quando você é um administrador na organização do parceiro de conta e tem uma meta de implantação de fornecer acesso federado para funcionários a recursos hospedados em outra organização:  
  
-   Os funcionários que fizeram logon em um domínio Active Directory na rede corporativa podem usar a funcionalidade @ no__t-0sign @ no__t-1on \(SSO @ no__t-3 individual para acessar vários aplicativos ou serviços Web @ no__t-4based, que são protegidos por AD FS, quando o os aplicativos ou serviços estão em uma organização diferente. Para obter mais informações, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Por exemplo, Fabrikam pode querer que os funcionários da rede corporativa tenham acesso federado a serviços Web hospedados na Contoso.  
  
-   Os funcionários remotos que fizeram logon em um domínio de Active Directory podem obter AD FS tokens do servidor de Federação em sua organização para obter acesso federado a aplicativos Web @ no__t-0based ou serviços protegidos por AD FS, que são hospedados em outra organização.  
  
    Por exemplo, a Fabrikam pode querer que seus funcionários remotos tenham acesso federado a serviços protegidos por AD FS que estão hospedados na contoso, sem exigir que os funcionários da Fabrikam estejam na rede corporativa da Fabrikam.  
  
Além dos componentes básicos descritos em [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) e que estão sombreados na ilustração a seguir, estes componentes são necessários para essa meta de implantação:  
  
-   **Proxy do servidor de Federação do parceiro de conta:** Os funcionários que acessam o serviço federado ou o aplicativo da Internet podem usar esse AD FS componente para realizar a autenticação. Por padrão, esse componente executa a autenticação de formulários, mas também pode executar a autenticação básica. Você também pode configurar esse componente para executar a autenticação de cliente do protocolo SSL \(SSL @ no__t-1 se os funcionários da sua organização tiverem certificados para apresentar. Para obter mais informações, consulte [Where to Place a Federation Server Proxy](Where-to-Place-a-Federation-Server-Proxy.md).  
  
-   **DNS de perímetro:** Essa implementação do sistema de nomes de domínio \(DNS @ no__t-1 fornece os nomes de host para a rede de perímetro. Para obter mais informações sobre como configurar o DNS de perímetro para um proxy de servidor de Federação, consulte [requisitos de resolução de nomes para proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   **Funcionário remoto:** O funcionário remoto acessa um aplicativo Web @ no__t-0based \(through um navegador da Web com suporte @ no__t-2 ou um serviço Web @ no__t-3based \(through um aplicativo @ no__t-5, usando credenciais válidas da rede corporativa, enquanto o funcionário está externo usando a Internet. O computador cliente do funcionário no local remoto se comunica diretamente com o proxy do servidor de Federação para gerar um token e autenticar para o aplicativo ou serviço.  
  
Depois de examinar as informações nos tópicos vinculados, você pode começar a implantar essa meta seguindo as etapas em [Checklist: Implementando um design de SSO da Web federado @ no__t-0.  
  
A ilustração a seguir mostra cada um dos componentes necessários para essa AD FS meta de implantação.  
  
![acesso aos seus aplicativos](media/50af4837-31e0-451f-a942-e705c2300065.gif)  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
