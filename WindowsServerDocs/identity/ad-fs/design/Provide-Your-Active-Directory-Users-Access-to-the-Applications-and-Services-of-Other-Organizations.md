---
ms.assetid: 2d62386c-b466-4a54-b6fa-5d16cda120d8
title: Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 0d1bf60c276932a77573acff2e4e011831e65a05
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87967563"
---
# <a name="provide-your-active-directory-users-access-to-the-applications-and-services-of-other-organizations"></a>Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações

Esse \( objetivo de implantação de AD FS serviços de Federação do Active Directory (AD FS) \) se baseia no objetivo de [fornecer aos seus Active Directory usuários acesso aos seus aplicativos e serviços com reconhecimento de declaração](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).

Quando você é um administrador na organização do parceiro de conta e tem uma meta de implantação de fornecer acesso federado para funcionários a recursos hospedados em outra organização:

-   Os funcionários que fizeram logon em um domínio Active Directory na rede corporativa podem usar a \- funcionalidade SSO de logon único \- \( \) para acessar vários \- aplicativos ou serviços baseados na Web, que são protegidos pelo AD FS, quando os aplicativos ou serviços estão em uma organização diferente. Para obter mais informações, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).

    Por exemplo, Fabrikam pode querer que os funcionários da rede corporativa tenham acesso federado a serviços Web hospedados na Contoso.

-   Os funcionários remotos que fizeram logon em um domínio de Active Directory podem obter AD FS tokens do servidor de Federação em sua organização para obter acesso federado a aplicativos baseados na Web protegidos por AD FS \- ou serviços hospedados em outra organização.

    Por exemplo, a Fabrikam pode querer que seus funcionários remotos tenham acesso federado a serviços protegidos por AD FS que estão hospedados na contoso, sem exigir que os funcionários da Fabrikam estejam na rede corporativa da Fabrikam.

Além dos componentes básicos descritos em [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md) e que estão sombreados na ilustração a seguir, estes componentes são necessários para essa meta de implantação:

-   **Proxy do servidor de Federação do parceiro de conta:** Os funcionários que acessam o serviço federado ou o aplicativo da Internet podem usar esse AD FS componente para realizar a autenticação. Por padrão, esse componente executa a autenticação de formulários, mas também pode executar a autenticação básica. Você também pode configurar esse componente para executar protocolo SSL \( \) autenticação de cliente SSL se os funcionários da sua organização tiverem certificados para apresentar. Para obter mais informações, consulte [Onde colocar um Proxy do servidor de Federação](Where-to-Place-a-Federation-Server-Proxy.md).

-   **DNS de perímetro:** Essa implementação do DNS do sistema de nomes de domínio \( \) fornece os nomes de host para a rede de perímetro. Para obter mais informações sobre como configurar o DNS de perímetro para um proxy de servidor de Federação, consulte [requisitos de resolução de nomes para proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).

-   **Funcionário remoto:** O funcionário remoto acessa um \- aplicativo baseado na Web \( por meio de um navegador da Web com suporte \) ou um \- serviço baseado na Web \( por meio de um aplicativo \) , usando credenciais válidas da rede corporativa, enquanto o funcionário é externo usando a Internet. O computador cliente do funcionário no local remoto se comunica diretamente com o proxy do servidor de Federação para gerar um token e autenticar para o aplicativo ou serviço.

Depois de revisar as informações nos tópicos vinculados, você pode começar a implantar essa meta, seguindo as etapas em [Checklist: Implementing a Federated Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).

A ilustração a seguir mostra cada um dos componentes necessários para essa AD FS meta de implantação.

![acesso aos seus aplicativos](media/50af4837-31e0-451f-a942-e705c2300065.gif)

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
