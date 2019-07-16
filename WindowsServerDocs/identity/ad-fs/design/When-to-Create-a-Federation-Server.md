---
ms.assetid: 824005ae-c3c1-459b-9baa-1660158918ab
title: Quando criar um Servidor de Federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e61c734780baa1482670af3f24697c10345b292
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190592"
---
# <a name="when-to-create-a-federation-server"></a>Quando criar um Servidor de Federação

Quando você cria uma federação serverin serviços de Federação do Active Directory \(do AD FS\), fornece um meio pelo qual sua organização pode:  
  
-   Envolver-se em único da Web\-sinal\-nos \(SSO\)– com base em comunicação com outra organização \(que também tem pelo menos um servidor de Federação\) e, quando necessário, com o os funcionários em sua organização \(que precisam de acesso pela Internet\).  
  
-   Habilitar serviços de front-end para representar usuários em serviços de infraestrutura usando a delegação de identidade. Para obter mais informações, consulte [When to Use Identity Delegation](When-to-Use-Identity-Delegation.md).  
  
As seções a seguir descrevem algumas das principais decisões para determinar quando e onde criar um ou mais servidores de Federação.  
  
## <a name="determine-the-organizational-role-for-the-federation-server"></a>Determinar a função organizacional do servidor de federação  
Para tomar uma decisão informada sobre quando criar um novo servidor de federação, primeiro você deve determinar em qual organização o servidor residirá. A função desempenhada por um servidor de Federação em uma organização depende se você colocar o servidor de federação na organização do parceiro de conta ou na organização do parceiro de recurso.  
  
Quando um servidor de Federação é colocado na rede corporativa do parceiro de conta, sua função é autenticar as credenciais do usuário do navegador, o serviço Web ou clientes de seletor de identidade e enviar tokens de segurança para os clientes. Para obter mais informações, consulte [analisar a função do servidor de federação no parceiro de conta](Review-the-Role-of-the-Federation-Server-in-the-Account-Partner.md).  
  
Quando um servidor de Federação é colocado na rede corporativa do parceiro de recurso, sua função é autenticar usuários, com base em um token de segurança emitido por um servidor de federação na organização do parceiro de recurso, ou sua função é redirecionar as solicitações de tokens aplicativos Web configurados ou serviços da Web para a organização do parceiro de conta que o cliente pertence. Para obter mais informações, consulte [analisar a função do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md).  
  
## <a name="determine-which-ad-fs-design-to-deploy"></a>Determinar qual design do AD FS será implantado  
Você pode criar servidores de federação na sua organização sempre que você deseja implantar qualquer um dos seguintes designs do AD FS:  
  
-   [Design SSO da Web](Web-SSO-Design.md)  
  
-   [Design SSO da Web federado](Federated-Web-SSO-Design.md)  
  
Se necessário, uma organização que implanta um design SSO da Web federado pode configurar um servidor de Federação único para que ele atue na função de parceiro de conta e na função de parceiro de recurso. Nesse caso, o servidor de Federação pode produzir Security Assertion Markup Language \(SAML\) tokens, com base em contas de usuário em sua própria organização ou redirecionar solicitações de token para a organização, com base em onde residem as contas de usuários .  
  
> [!NOTE]  
> Para o design de SSO da Web federado, deve haver pelo menos um servidor de federação no parceiro de conta e pelo menos um servidor de federação no parceiro de recurso.  
  
## <a name="differences-between-a-federation-server-and-a-federation-server-proxy"></a>Diferenças entre um servidor de federação e um proxy do servidor de federação  
Um servidor de Federação pode servir páginas da Web para uma entrada\-na diretiva, autenticação e descoberta da mesma maneira que faz um proxy do servidor de Federação. As principais diferenças entre um servidor de Federação e um proxy do servidor de federação tem a ver com quais operações uma federação de servidor pode executar para que um proxy do servidor de Federação não pode executar.  
  
A seguir está as operações que pode ser executadas apenas um servidor de Federação:  
  
-   O servidor de federação realiza as operações criptográficas que produzem o token. Embora os proxies de servidor de Federação não possam produzir tokens, eles podem ser usados para encaminhar ou redirecionar os tokens aos clientes e, quando necessário, para o servidor de Federação. Para obter mais informações sobre como usar os servidores de federação, consulte [When to Create a Federation Server Proxy](When-to-Create-a-Federation-Server-Proxy.md).  
  
-   Servidores de Federação suportam o uso de autenticação integrada do Windows para clientes na rede corporativa; Não use proxies de servidor de Federação. Para obter mais informações sobre como usar a autenticação integrada do Windows com o servidor de federação, consulte [quando criar um Farm de servidores de Federação](When-to-Create-a-Federation-Server-Farm.md).  
  
> [!CAUTION]  
> A comunicação entre os servidores de federação e os bancos de dados de configuração do SQL Server, os repositórios de atributos do SQL Server, os controladores de domínio e as instâncias de AD LDS não tem sua integridade ou confidencialidade protegida de modo padrão. Para atenuar isso, considere a possibilidade de proteger o canal de comunicação entre esses servidores usando IPSEC ou uma conexão segura fisicamente entre todos esses servidores. Para a comunicação entre os servidores de federação e os SQL Servers, considere usar a proteção SSL na cadeia de conexão. Para conexões entre servidores de federação e controladores de domínio, considere habilitar a criptografia e autenticação Kerberos. Para LDAP, LDAP\/S não é compatível com o AD LDS\/AD DS.  
  
## <a name="how-to-create-a-federation-server"></a>Como criar um servidor de federação  
Você pode criar um servidor de Federação usando o Assistente de configuração do servidor de Federação do AD FS ou o comando Fsconfig.exe\-ferramenta de linha. Ao usar uma dessas ferramentas, escolha uma das opções a seguir para criar um servidor de federação.  
  
-   Criar um suporte\-servidor de Federação autônomo  
  
    Para obter mais informações sobre como configurar um modo de espera\-servidor de Federação autônomo, consulte [Create a stand-alone Federation Server](../../ad-fs/deployment/Create-a-Stand-Alone-Federation-Server.md).  
  
-   Criar o primeiro servidor de federação em um farm de servidores de federação  
  
    Para obter mais informações sobre como configurar o primeiro servidor de Federação ou adicionar um servidor de Federação a um farm, consulte [criar o primeiro servidor de Federação em um Farm de servidores de Federação](../../ad-fs/deployment/Create-the-First-Federation-Server-in-a-Federation-Server-Farm.md).  
  
-   Adicionar um servidor de federação a um farm de servidores de federação  
  
    Para obter mais informações sobre como adicionar um servidor de Federação a um farm, consulte [adicionar um servidor de Federação a um Farm de servidores de Federação](../../ad-fs/deployment/Add-a-Federation-Server-to-a-Federation-Server-Farm.md).  
  
Para obter mais informações sobre como cada uma dessas opções funcionam, consulte [The Role of the AD FS Configuration Database](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md).  
  
Para obter mais informações sobre como configurar todos os pré-requisitos necessários para implantar um servidor de federação, consulte [lista de verificação: Configurando um servidor de Federação](../../ad-fs/deployment/Checklist--Setting-Up-a-Federation-Server.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

