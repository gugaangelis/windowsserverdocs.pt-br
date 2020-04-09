---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2e47197a980c9bcb576d6634a0031a8ae13afbfd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858599"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações


Quando você é um administrador na organização do parceiro de recurso no Serviços de Federação do Active Directory (AD FS) \(AD FS\) e tem uma meta de implantação para fornecer acesso federado para usuários em outra organização \(a organização do parceiro de conta\) a um aplicativo de declarações\-com reconhecimento ou a um serviço Web\-que está localizado em sua organização \(a organização do parceiro de recurso\):  
  
-   Os usuários federados na sua organização e em organizações que configuraram uma relação de confiança de Federação com a sua organização \(organizações parceiras de contas\) podem acessar o aplicativo ou serviço protegido do AD FS que é hospedado pela sua organização. Para obter mais informações, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Por exemplo, Fabrikam pode querer que os funcionários da rede corporativa tenham acesso federado a serviços Web hospedados na Contoso.  
  
-   Os usuários federados que não têm nenhuma associação direta com uma organização confiável \(como clientes individuais\), que estão conectados a um repositório de atributos hospedado em sua rede de perímetro, podem acessar vários AD FS\-aplicativos protegidos, que também são hospedados em sua rede de perímetro, fazendo logon uma vez a partir de computadores cliente que estão localizados na Internet. Em outras palavras, quando você hospeda contas de clientes para permitir o acesso a aplicativos ou serviços na rede de perímetro, os clientes que você hospeda em um repositório de atributos podem acessar um ou mais aplicativos ou serviços na rede de perímetro simplesmente efetuando logon uma vez. Para obter mais informações, consulte [Web SSO Design](Web-SSO-Design.md).  
  
    Por exemplo, a Fabrikam pode querer que seus clientes tenham um único\-Sign\-em \(SSO\) acesso a vários aplicativos ou serviços hospedados em sua rede de perímetro.  
  
Os seguintes componentes são necessários para essa meta de implantação:  
  
-   **\)de AD DS de \(de Active Directory Domain Services:** O servidor de Federação do parceiro de recurso deve ser Unido a um domínio Active Directory.  
  
-   **DNS de perímetro:** O sistema de nomes de domínio \(\) DNS deve conter um host simples \(um registro de recurso de\) para que os computadores cliente possam localizar o servidor de Federação do parceiro de recurso e o servidor Web. O servidor DNS pode hospedar outros registros DNS que também são necessários na rede de perímetro. Para obter mais informações, consulte [Requisitos de resolução de nomes para servidores de federação](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de Federação do parceiro de recurso:** O servidor de Federação do parceiro de recurso valida AD FS tokens que os parceiros de conta enviam. A descoberta de parceiro de conta é executada por meio desse servidor de Federação. Para obter mais informações, consulte [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Servidor Web:** o servidor Web pode hospedar um aplicativo Web ou um serviço Web. O servidor Web confirma que recebe tokens do AD FS válidos de usuários federados antes de permitir acesso ao aplicativo Web protegido ou serviço Web.  
  
    Usando o Windows Identity Foundation \(o WIF\), você pode desenvolver seu aplicativo ou serviço da Web para que ele aceite solicitações de logon de usuário federado feitas com qualquer método de logon padrão, como nome de usuário e senha.  
  
Depois de examinar as informações nos tópicos vinculados, você pode começar a implantar essa meta seguindo as etapas em [lista de verificação: Implementando um design de SSO da Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) e uma [lista de verificação: Implementando um design de SSO da Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
A ilustração a seguir mostra cada um dos componentes necessários para essa AD FS meta de implantação.  
  
![acesso às suas declarações](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
