---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a0b2429599036f2893f23df7921a11c8232d9f67
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359073"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações


Quando você é um administrador na organização do parceiro de recurso no Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 e tem uma meta de implantação para fornecer acesso federado para usuários em outra organização \(The parceiro de conta organização @ no__t-3 para um aplicativo Claims @ no__t-4aware ou um serviço Web @ no__t-5based que está localizado em sua organização \(ThE organização do parceiro de recurso @ no__t-7:  
  
-   Os usuários federados na sua organização e em organizações que configuraram uma relação de confiança de Federação com a sua organização \(account organizações parceiras @ no__t-1 podem acessar o aplicativo AD FS protegido ou o serviço hospedado pela sua organização. Para obter mais informações, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Por exemplo, Fabrikam pode querer que os funcionários da rede corporativa tenham acesso federado a serviços Web hospedados na Contoso.  
  
-   Os usuários federados que não têm nenhuma associação direta com uma organização confiável \(such como clientes individuais @ no__t-1, que estão conectados a um repositório de atributos que está hospedado em sua rede de perímetro, podem acessar vários aplicativos AD FS @ no__t-2secured, que também são hospedados em sua rede de perímetro, fazendo logon uma vez a partir de computadores cliente que estão localizados na Internet. Em outras palavras, quando você hospeda contas de clientes para permitir o acesso a aplicativos ou serviços na rede de perímetro, os clientes que você hospeda em um repositório de atributos podem acessar um ou mais aplicativos ou serviços na rede de perímetro simplesmente efetuando logon uma vez. Para obter mais informações, consulte [Web SSO Design](Web-SSO-Design.md).  
  
    Por exemplo, a Fabrikam pode querer que seus clientes tenham um único @ no__t-0sign @ no__t-1on \(SSO @ no__t-3 acesso a vários aplicativos ou serviços hospedados em sua rede de perímetro.  
  
Os seguintes componentes são necessários para essa meta de implantação:  
  
-   **Active Directory Domain Services \(AD DS @ no__t-2:** O servidor de Federação do parceiro de recurso deve ser Unido a um domínio Active Directory.  
  
-   **DNS de perímetro:** O sistema de nome de domínio \(DNS @ no__t-1 deve conter um registro de recurso de host simples \(A @ no__t-3 para que os computadores cliente possam localizar o servidor de Federação do parceiro de recurso e o servidor Web. O servidor DNS pode hospedar outros registros DNS que também são necessários na rede de perímetro. Para obter mais informações, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de Federação do parceiro de recurso:** O servidor de Federação do parceiro de recurso valida AD FS tokens que os parceiros de conta enviam. A descoberta de parceiro de conta é executada por meio desse servidor de Federação. Para obter mais informações, consulte [analisar a função do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Servidor Web:** O servidor Web pode hospedar um aplicativo Web ou um serviço Web. O servidor Web confirma que recebe tokens do AD FS válidos de usuários federados antes de permitir acesso ao aplicativo Web protegido ou serviço Web.  
  
    Usando o Windows Identity Foundation \(WIF @ no__t-1, você pode desenvolver seu aplicativo ou serviço Web para que ele aceite solicitações de logon de usuário federado feitas com qualquer método de logon padrão, como nome de usuário e senha.  
  
Depois de examinar as informações nos tópicos vinculados, você pode começar a implantar essa meta seguindo as etapas em [Checklist: Implementando um design de SSO da Web federado @ no__t-0 e [Checklist: Implementando um design de SSO da Web @ no__t-0.  
  
A ilustração a seguir mostra cada um dos componentes necessários para essa AD FS meta de implantação.  
  
![acesso às suas declarações](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
