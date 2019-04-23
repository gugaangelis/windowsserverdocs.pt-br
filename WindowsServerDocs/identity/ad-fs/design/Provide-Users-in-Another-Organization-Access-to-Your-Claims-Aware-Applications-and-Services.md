---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4ec08182e2523b0fcb16088ec9c1d094a5923fe
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836527"
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando você for um administrador na organização do parceiro de recurso nos serviços de Federação do Active Directory \(do AD FS\) e você tem uma meta de implantação para fornecer acesso federado para usuários em outra organização \(o organização do parceiro de conta\) para uma de declarações\-uma Web ou aplicativo com reconhecimento\-com base em serviço que está localizado em sua organização \(a organização do parceiro de recurso\):  
  
-   Usuários em sua organização e em organizações que tenham configurado uma federação de relação de confiança para sua organização federados \(as organizações do parceiro de conta\) pode acessar o AD FS aplicativo protegido ou serviço que é hospedado pela sua organização. Para obter mais informações, consulte [Federated Web SSO Design](Federated-Web-SSO-Design.md).  
  
    Por exemplo, Fabrikam pode querer que os funcionários da rede corporativa tenham acesso federado a serviços Web hospedados na Contoso.  
  
-   Usuários federados que não tenham associação direta com uma organização confiável \(como clientes individuais\), que são conectados a um repositório de atributos hospedado em sua rede de perímetro, podem acessar vários AD FS\- aplicativos protegidos, que também estão hospedados na rede de perímetro, efetuando logon uma vez a partir de computadores cliente que estão localizados na Internet. Em outras palavras, quando você hospeda contas de clientes para permitir o acesso a aplicativos ou serviços na rede de perímetro, os clientes que você hospeda em um repositório de atributos podem acessar um ou mais aplicativos ou serviços na rede de perímetro simplesmente efetuando logon uma vez. Para obter mais informações, consulte [Web SSO Design](Web-SSO-Design.md).  
  
    Por exemplo, Fabrikam pode querer que seus clientes tenham única\-sinal\-nos \(SSO\) acesso a vários aplicativos ou serviços que são hospedados em sua rede de perímetro.  
  
Os seguintes componentes são necessários para essa meta de implantação:  
  
-   **Serviços de domínio do Active Directory \(AD DS\):** O servidor de Federação do parceiro de recurso deve ser associado a um domínio do Active Directory.  
  
-   **DNS de perímetro:** Sistema de nomes de domínio \(DNS\) deve conter um host simple \(um\) para que computadores cliente possam localizar o servidor de Federação do parceiro de recurso e o servidor Web de registro de recurso. O servidor DNS pode hospedar outros registros DNS que também são necessários na rede de perímetro. Para obter mais informações, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de Federação do parceiro de recurso:** O servidor de Federação do parceiro de recurso valida os tokens do AD FS que os parceiros de conta enviam. Descoberta de parceiro de conta é executada através desse servidor de Federação. Para obter mais informações, consulte [Review the Role of the Federation Server in the Resource Partner](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Servidor Web:** O servidor Web pode hospedar um aplicativo Web ou um serviço Web. O servidor Web confirma que recebe tokens do AD FS válidos de usuários federados antes de permitir acesso ao aplicativo Web protegido ou serviço Web.  
  
    Usando o Windows Identity Foundation \(WIF\), você pode desenvolver seu aplicativo da Web ou serviço de forma que ele aceite federada a solicitações de logon do usuário que são feitas com qualquer método de logon padrão, como nome de usuário e senha.  
  
Depois de revisar as informações nos tópicos vinculados, você pode começar a implantar essa meta, seguindo as etapas em [lista de verificação: Implementando um Design SSO da Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) e [lista de verificação: Implementando um Design SSO da Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
A ilustração a seguir mostra cada um dos componentes necessários para essa meta de implantação do AD FS.  
  
![acesso a suas declarações](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
