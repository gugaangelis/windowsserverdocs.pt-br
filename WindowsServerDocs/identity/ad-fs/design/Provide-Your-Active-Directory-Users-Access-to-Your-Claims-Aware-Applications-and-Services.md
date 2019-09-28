---
ms.assetid: d254fca3-85a1-424d-ac22-d6687ec3798e
title: Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços com reconhecimento de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 48436f8e98af965f2bc2b38d296c4a15924e4db1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407957"
---
# <a name="provide-your-active-directory-users-access-to-your-claims-aware-applications-and-services"></a>Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços com reconhecimento de declarações

Quando você é um administrador na organização do parceiro de conta em uma implantação Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 e tem uma meta de implantação para fornecer um único @ no__t-2sign @ no__t-3on \(SSO @ no__t-5 Access para os funcionários em a rede corporativa para seus recursos hospedados:  
  
-   Funcionários que estiverem conectados a uma floresta do Active Directory na rede corporativa podem usar o SSO para acessar vários aplicativos ou serviços na rede de perímetro da sua organização. Esses aplicativos e serviços são protegidos por AD FS.  
  
    Por exemplo, a Fabrikam pode querer que os funcionários da rede corporativa tenham acesso federado a aplicativos Web @ no__t-0based hospedados na rede de perímetro para a Fabrikam.  
  
-   Os funcionários remotos que fizeram logon em um domínio de Active Directory podem obter AD FS tokens do servidor de Federação em sua organização para obter acesso federado AD FS a aplicativos Web @ no__t-0secured, ou serviços do no__t, que também residem em seu organizações.  
  
-   Informações do armazenamento de atributo do Active Directory podem ser preenchidas em tokens do AD FS dos funcionários.  
  
Os seguintes componentes são necessários para essa meta de implantação:  
  
-   **Active Directory Domain Services \(AD DS @ no__t-2:** O AD DS contém contas de usuário dos funcionários que são usadas para gerar tokens do AD FS. Informações, como associações de grupo e atributos, são preenchidas em tokens do AD FS como declarações de grupo e declarações personalizadas.  
  
    > [!NOTE]  
    > Você também pode usar o protocolo de acesso de diretório leve \(LDAP @ no__t-1 ou linguagem SQL \(SQL @ no__t-3 para conter as identidades para AD FS geração de token.  
  
-   **DNS corporativo:** Essa implementação do sistema de nomes de domínio \(DNS @ no__t-1 contém um registro de recurso de host simples \(A @ no__t-3 para que os clientes de intranet possam localizar o servidor de Federação de conta. Essa implementação do DNS também pode hospedar outros registros DNS necessários na rede corporativa. Para obter mais informações, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   **Servidor de Federação do parceiro de conta:** Esse servidor de Federação é associado a um domínio na floresta do parceiro de conta. Ele autentica as contas de usuário do funcionário e gera tokens do AD FS. O computador cliente do funcionário executa a autenticação integrada do Windows nesse servidor de Federação para gerar um token de AD FS. Para obter mais informações, consulte [analisar a função do servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
    O servidor de Federação do parceiro de conta pode autenticar os seguintes usuários:  
  
    -   Funcionários com contas de usuário neste domínio  
  
    -   Funcionários com contas de usuário em qualquer lugar deste domínio  
  
    -   Funcionários com contas de usuário em qualquer lugar em florestas que são confiáveis para essa floresta \(through a dois @ no__t-1way confiança do Windows @ no__t-2  
  
-   **Funcionário:** Um funcionário acessa um serviço Web @ no__t-0based \(through um aplicativo @ no__t-2 ou um aplicativo Web @ no__t-3based \(through um navegador da Web com suporte @ no__t-5 enquanto ele está conectado à rede corporativa. O computador cliente do funcionário na rede corporativa se comunica diretamente com o servidor de Federação para autenticação.  
  
Depois de examinar as informações nos tópicos vinculados, você pode começar a implantar essa meta seguindo as etapas em [Checklist: Implementando um design de SSO da Web federado @ no__t-0.  
  
A ilustração a seguir mostra cada um dos componentes necessários para essa AD FS meta de implantação.  
  
![acesso às suas declarações](media/31394ea8-fecb-4372-ac3f-cc3cf566ffc9.gif)  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
