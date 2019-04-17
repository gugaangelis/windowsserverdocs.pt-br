---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: "Fornecer seu do Active Directory aos usuários acesso aos seus aplicativos com reconhecimento de declarações e serviços"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f6fb37c16c20915c0051e3a24cdb0c147ae92d9c
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Fornecer seu do Active Directory aos usuários acesso aos seus aplicativos com reconhecimento de declarações e serviços

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando você for um administrador na organização do parceiro de conta em uma implantação de \(AD FS\) de serviços de Federação do Active Directory e ter uma meta de implantação para fornecer acesso single\-sign\-on \(SSO\) para funcionários na rede corporativa aos seus recursos hospedados:  
  
-   Os funcionários estejam conectados a uma floresta do Active Directory na rede corporativa podem usar o SSO para acessar vários aplicativos ou serviços na rede de perímetro em sua própria organização. Esses aplicativos e serviços são protegidos pelo AD FS.  
  
    Por exemplo, Fabrikam pode querer funcionários tenham acesso a aplicativos baseados em Web\ que são hospedados na rede de perímetro da Fabrikam federado da rede corporativa.  
  
-   Funcionários remotos que estejam conectados a um domínio do Active Directory podem obter tokens do AD FS do servidor de Federação em sua organização para obter acesso federado protegidas AD FS\ Web\ aplicativos ou serviços que também residem em sua organização.  
  
-   Informações no repositório de atributo do Active Directory podem ser preenchidas tokens do AD FS dos funcionários.  
  
Os seguintes componentes são necessários para essa meta de implantação:  
  
-   **Active Directory serviços de domínio \(AD DS\):** AD DS contém as contas de usuário de funcionários que são usadas para gerar tokens do AD FS. Informações, como associações de grupo e atributos, é preenchidas tokens do AD FS como declarações de grupo e personalizado.  
  
    > [!NOTE]  
    > Você também pode usar Lightweight Directory Access Protocol \(LDAP\) ou a linguagem de consulta estruturada \(SQL\) para conter as identidades para a geração de token do AD FS.  
  
-   **DNS corporativa:** esta implementação de Domain Name System \(DNS\) contém um registro de recurso \(A\) host simples para que os clientes de intranet possam localizar o servidor de federação de conta. Esta implementação de DNS também pode hospedar outros registros DNS que são necessários na rede corporativa. Para obter mais informações, consulte [requisitos de resolução de nome para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de Federação do parceiro de conta:** este servidor de Federação está associado a um domínio na floresta do parceiro de conta. Ele autentica contas de usuário do funcionário e gera tokens do AD FS. O computador cliente para o funcionário realiza autenticação integrada do Windows em relação este servidor de federação para gerar um token do AD FS. Para obter mais informações, consulte [revisar a função de servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    O servidor de Federação do parceiro de conta pode autenticar os usuários a seguir:  
  
    -   Funcionários com contas de usuário neste domínio  
  
    -   Funcionários com contas de usuário em qualquer lugar na floresta  
  
    -   Funcionários com contas de usuário em qualquer lugar em florestas que sejam confiáveis por essa floresta \ (por meio de um trust\ do Windows de forma two\)  
  
-   **Funcionário:** um funcionário acessa um serviço baseado em Web\ \(through an application\) ou um aplicativo baseado em Web\ \ (por meio de um browser\ da Web com suporte) enquanto ele estiver conectado à rede corporativa. O computador do funcionário cliente na rede corporativa se comunica diretamente com o servidor de federação para autenticação.  
  
Após analisar as informações nos tópicos vinculados, você pode começar a implantar essa meta seguindo as etapas em [lista de verificação: Implementando um Design de SSO da Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
A ilustração a seguir mostra cada um dos componentes necessários para essa meta de implantação do AD FS.  
  
![Acesso a suas declarações](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
