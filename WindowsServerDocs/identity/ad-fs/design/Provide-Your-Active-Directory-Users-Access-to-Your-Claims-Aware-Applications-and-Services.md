---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços com reconhecimento de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8c3db2873e1c7a0fa217ba37b9439cc38dfafc36
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191004"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços com reconhecimento de declarações

Quando você for um administrador na organização do parceiro de conta em um Active Directory Federation Services \(do AD FS\) implantação e você tem uma meta de implantação para fornecer um único\-sinal\-em \( SSO\) acesso para os funcionários na rede corporativa aos recursos hospedados:  
  
-   Funcionários que estiverem conectados a uma floresta do Active Directory na rede corporativa podem usar o SSO para acessar vários aplicativos ou serviços na rede de perímetro da sua organização. Esses aplicativos e serviços são protegidos pelo AD FS.  
  
    Por exemplo, Fabrikam pode querer que os funcionários da rede corporativa tenham acesso federado a Web\-com base em aplicativos que são hospedados na rede de perímetro da Fabrikam.  
  
-   Funcionários remotos que efetuaram logon em um domínio do Active Directory podem obter tokens do AD FS do servidor de federação na sua organização para ganhar acesso federado ao AD FS\-protegidos Web\-com base em aplicativos ou serviços que também residem em sua organização.  
  
-   Informações do armazenamento de atributo do Active Directory podem ser preenchidas em tokens do AD FS dos funcionários.  
  
Os seguintes componentes são necessários para essa meta de implantação:  
  
-   **Serviços de domínio do Active Directory \(AD DS\):** O AD DS contém contas de usuário dos funcionários que são usadas para gerar tokens do AD FS. Informações, como associações de grupo e atributos, são preenchidas em tokens do AD FS como declarações de grupo e declarações personalizadas.  
  
    > [!NOTE]  
    > Você também pode usar o Lightweight Directory Access Protocol \(LDAP\) ou Structured Query Language \(SQL\) para conter as identidades do AD FS geração de token.  
  
-   **DNS corporativo:** Essa implementação do sistema de nomes de domínio \(DNS\) contém um host simple \(um\) de registro de recurso para que os clientes da intranet possam localizar o servidor de federação de conta. Essa implementação do DNS também pode hospedar outros registros DNS necessários na rede corporativa. Para obter mais informações, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de Federação do parceiro de conta:** Esse servidor de Federação está ingressado em um domínio na floresta do parceiro de conta. Ele autentica as contas de usuário do funcionário e gera tokens do AD FS. O computador cliente para o funcionário executa autenticação integrada do Windows em relação a esse servidor de federação para gerar um token do AD FS. Para obter mais informações, consulte [Review the Role of the Federation Server in the Account Partner](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    O servidor de Federação do parceiro de conta pode autenticar os usuários a seguir:  
  
    -   Funcionários com contas de usuário neste domínio  
  
    -   Funcionários com contas de usuário em qualquer lugar deste domínio  
  
    -   Os funcionários com contas de usuário em qualquer lugar em florestas são confiáveis por essa floresta \(por meio de um dois\-maneira de relação de confiança do Windows\)  
  
-   **Funcionário:** Um funcionário acessa um Web\-serviço baseado em \(por meio de um aplicativo\) ou uma Web\-aplicativo baseado no \(por meio de um navegador da Web com suporte\) enquanto ele estiver conectado à rede corporativa. Computador de cliente do funcionário na rede corporativa se comunica diretamente com o servidor de federação para autenticação.  
  
Depois de revisar as informações nos tópicos vinculados, você pode começar a implantar essa meta, seguindo as etapas em [lista de verificação: Implementando um Design SSO da Web federado](../../ad-fs/deployment/Checklist--Implementing-a-Federated-Web-SSO-Design.md).  
  
A ilustração a seguir mostra cada um dos componentes necessários para essa meta de implantação do AD FS.  
  
![acesso a suas declarações](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
