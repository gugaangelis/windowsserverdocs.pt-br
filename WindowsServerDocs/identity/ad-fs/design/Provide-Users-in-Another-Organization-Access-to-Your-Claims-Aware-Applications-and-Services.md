---
ms.assetid: de7e1e4a-f96d-4b59-ac9b-f65f5d37a96f
title: "Fornecer aos usuários em outra organização acesso aos seus aplicativos com reconhecimento de declarações e serviços"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b4ec08182e2523b0fcb16088ec9c1d094a5923fe
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="provide-users-in-another-organization-access-to-your-claims-aware-applications-and-services"></a>Fornecer aos usuários em outra organização acesso aos seus aplicativos com reconhecimento de declarações e serviços

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando você for um administrador na organização do parceiro de recurso de serviços de Federação do Active Directory \(AD FS\) e ter uma meta de implantação para fornecer acesso federado para os usuários em outra organização \ (o organization\ conta de parceiro) para um aplicativo com reconhecimento de claims\ ou um serviço baseado em Web\ que está localizado em sua organização \ (o organization\ do parceiro de recursos):  
  
-   Os usuários em sua organização e em organizações que configurou uma federação confiam em sua organização federados \ (conta parceiro organizations\) pode acessar o AD FS protegidas aplicativo ou serviço que está hospedado por sua organização. Para obter mais informações, consulte [Design de SSO da Web federado](Federated-Web-SSO-Design.md).  
  
    Por exemplo, Fabrikam pode querer seus funcionários tenham acesso a serviços Web que são hospedados na Contoso federado da rede corporativa.  
  
-   Os usuários que não têm nenhuma associação direta com uma organização confiável federados \ (como customers\ individual), que estão conectadas a um repositório de atributo que está hospedado em sua rede perímetro, poderá acessar protegidas AD FS\ vários aplicativos, que também estão hospedados em sua rede de perímetro, fazendo logon uma vez de computadores cliente que estão localizados na Internet. Em outras palavras, quando você hospeda contas de cliente para habilitar o acesso a aplicativos ou serviços em sua rede perímetro, os clientes que você hospede em um repositório de atributo podem acessar um ou mais aplicativos ou serviços na rede de perímetro simplesmente fazendo logon uma vez. Para obter mais informações, consulte [Web SSO Design](Web-SSO-Design.md).  
  
    Por exemplo, Fabrikam pode querer seus clientes sign\ single\ em \(SSO\) acessem vários aplicativos ou serviços que são hospedados na sua rede do perímetro.  
  
Os seguintes componentes são necessários para essa meta de implantação:  
  
-   **Active Directory serviços de domínio \(AD DS\):** o servidor de Federação do parceiro de recurso deve ser adicionado a um domínio do Active Directory.  
  
-   **Perímetro DNS:** domínio Name System \(DNS\) deve conter um registro de recurso \(A\) host simples para que os computadores cliente podem localizar o servidor de Federação do parceiro de recurso e o servidor Web. O servidor DNS pode hospedar outros registros DNS que também são necessários na rede de perímetro. Para obter mais informações, consulte [requisitos de resolução de nome para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de Federação do parceiro de recurso:** o servidor de Federação do parceiro de recurso valida os tokens do AD FS que enviar os parceiros de conta. Descoberta de parceiros de conta é realizada por meio esse servidor de Federação. Para obter mais informações, consulte [revisar a função de servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
-   **Servidor Web:** o servidor Web pode hospedar um aplicativo da Web ou um serviço Web. O servidor Web confirma que ele recebe válidos tokens do AD FS do usuários federados antes de permitir acesso ao serviço Web ou aplicativo Web protegido.  
  
    Usando o Windows Identity Foundation \(WIF\), você pode desenvolver seu aplicativo da Web ou serviço para que ele aceite solicitações de logon de usuário federado são feitas com qualquer método de logon padrão, como nome de usuário e senha.  
  
Após analisar as informações nos tópicos vinculados, você pode começar a implantar essa meta seguindo as etapas em [lista de verificação: Implementando um Design de SSO da Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md) e [lista de verificação: Implementando um Design de SSO Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
A ilustração a seguir mostra cada um dos componentes necessários para essa meta de implantação do AD FS.  
  
![Acesso a suas declarações](media/75358b16-2a6f-4e16-9cc4-b0e614480305.gif)  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
