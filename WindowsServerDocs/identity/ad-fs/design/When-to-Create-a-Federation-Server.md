---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: "Quando criar um servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8013764b88a1061cfcaa3a507466c111bfd59aad
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="when-to-create-a-federation-server"></a>Quando criar um servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando você cria um federação serverin serviços de Federação do Active Directory \(AD FS\), você fornece um meio pelo qual sua organização pode:  
  
-   Envolver na Web sign\ single\ em \ (SSO\) – com base em comunicação com outra organização \ (que também tem pelo menos um servidor de Federação \) e, quando necessário, com os funcionários em sua própria organização \ (que precisam de acesso ao longo do Internet\).  
  
-   Habilite serviços front-end representar os usuários a serviços de infraestrutura usando a delegação de identidade. Para obter mais informações, consulte [quando delegação de identidade de uso](When-to-Use-Identity-Delegation.md).  
  
As seções a seguir descrevem algumas das decisões importantes para determinar quando e onde deseja criar um ou mais servidores de Federação.  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>Determinar a função organizacional para o servidor de Federação  
Para tomar uma decisão consciente sobre quando criar um novo servidor de federação, primeiro você deve determinar em qual organização o servidor for residir. A função que reproduz um servidor de Federação em uma organização depende se você colocar o servidor de federação na organização do parceiro de conta ou na organização de parceiros do recurso.  
  
Quando um servidor de Federação é colocado na rede da empresa do parceiro de conta, sua função é autenticar as credenciais do usuário do navegador, serviço Web ou clientes de seletor de identidade e enviar tokens de segurança para os clientes. Para obter mais informações, consulte [revisar a função de servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
Quando um servidor de Federação é colocado na rede da empresa do parceiro de recurso, sua função é autenticar usuários, com base em um token de segurança emitido por um servidor de federação na organização do parceiro de recurso, ou sua função é redirecionar solicitações de tokens de aplicativos Web configurados ou serviços da Web para a organização de parceiro de conta que o cliente pertence. Para obter mais informações, consulte [revisar a função de servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>Determinar qual AD FS de design para implantar  
Você pode criar servidores de Federação em sua organização sempre que você deseja implantar qualquer um dos seguintes designs do AD FS:  
  
-   [Design SSO da Web](Web-SSO-Design.md)  
  
-   [Design SSO da Web federado](Federated-Web-SSO-Design.md)  
  
Se necessário, uma organização que implanta um design de SSO da Web federado pode configurar um servidor de Federação único para que ele atua na função de parceiro de conta e na função de parceiro de recurso. Nesse caso, o servidor de Federação pode produzir tokens de linguagem de marcação de asserção de segurança \(SAML\), com base em contas de usuário em sua própria organização, ou token redirecionar solicitações para a organização, com base em onde residem contas dos usuários.  
  
> [!NOTE]  
> Para o design de SSO da Web federado, deve haver pelo menos um servidor de federação no parceiro de conta e pelo menos um servidor de federação no parceiro de recurso.  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>Diferenças entre um servidor de Federação e um proxy de servidor de Federação  
Um servidor de Federação poderá servir páginas da Web para sign\-in política de autenticação e descoberta da mesma forma que faz um proxy de servidor de Federação. As principais diferenças entre um servidor de Federação e um proxy de servidor de federação tem que fazer com que operações de uma reunião servidor pode executar que não é possível executar um proxy de servidor de Federação.  
  
A seguir estão as operações que somente um servidor de Federação pode executar:  
  
-   O servidor de Federação executa as operações de criptografia que produzem o token. Embora proxies de servidor de Federação não consegue produzir tokens, eles podem ser usados para rotear ou redirecionar os tokens para clientes e, quando necessário, para o servidor de Federação. Para obter mais informações sobre como usar servidores de federação, consulte [quando criar um Proxy de servidor de Federação](When-to-Create-a-Federation-Server-Proxy.md).  
  
-   Servidores de Federação suportam ao uso da autenticação integrada do Windows para clientes na rede corporativa; proxies de servidor de Federação não. Para obter mais informações sobre como usar a autenticação integrada do Windows com o servidor de federação, consulte [quando criar um Farm de servidores de Federação](When-to-Create-a-Federation-Server-Farm.md).  
  
> [!CAUTION]  
> A comunicação entre instâncias do AD LDS e bancos de dados do SQL Server configuration, repositórios de atributo do SQL Server, controladores de domínio e servidores de Federação não é integridade ou confidencialidade protegido por padrão. Para atenuar isso, considere a possibilidade de proteger o canal de comunicação entre esses servidores usando IPSEC ou usando uma conexão segura fisicamente entre todos esses servidores. Para comunicação entre servidores de Federação e servidores SQL, considere usar proteção SSL na cadeia de caracteres de conexão. Para as conexões entre controladores de domínio e servidores de federação, considere a possibilidade de ativar Kerberos assinatura e criptografia. Para LDAP, LDAP\/S não tem suporte para anúncios LDS\/AD DS.  
  
## <a name="how-to-create-a-federation-server"></a>Como criar um servidor de Federação  
Você pode criar um servidor de Federação usando o Assistente para configuração do servidor de Federação do AD FS ou a ferramenta de linha de command\ Fsconfig.exe. Quando você usa qualquer uma dessas ferramentas, você pode selecionar qualquer uma das opções a seguir para criar um servidor de Federação.  
  
-   Criar um servidor de Federação stand\ sozinho  
  
    Para obter mais informações sobre como configurar um servidor de Federação stand\ sozinho, consulte [criar um servidor de Federação autônomo](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md).  
  
-   Criar o primeiro servidor de Federação em um farm de servidores de Federação  
  
    Para obter mais informações sobre como configurar o primeiro servidor de Federação ou adicionar um servidor de federação para um farm, consulte [criar o primeiro servidor de Federação em um Farm de servidores de Federação](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
-   Adicionar um servidor de federação para um farm de servidores de Federação  
  
    Para obter mais informações sobre como adicionar um servidor de federação para um farm, consulte [adicionar um servidor de federação para um Farm de servidores de Federação](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md).  
  
Para obter mais informações sobre como cada uma dessas opções funcionar, consulte [a função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
Para obter mais informações sobre como configurar todos os pré-requisitos necessários para implantar um servidor de federação, consulte [lista de verificação: configuração de um servidor federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

