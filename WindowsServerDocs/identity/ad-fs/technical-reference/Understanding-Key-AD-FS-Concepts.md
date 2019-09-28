---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: Entendendo os principais conceitos de Serviços de Federação do Active Directory (AD FS)
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d212c2f820204bae8e1e55dc1ebc44200e266a18
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407312"
---
# <a name="understanding-key-ad-fs-concepts"></a>Entendendo os conceitos do Key AD FS
É recomendável que você saiba mais sobre os conceitos importantes para Serviços de Federação do Active Directory (AD FS) e familiarize-se com seu conjunto de recursos.  
  
> [!TIP]  
> Você pode encontrar links de recursos do AD FS adicionais na página [Mapa de Conteúdo do AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) , no TechNet Wiki da Microsoft. Essa página é gerenciada por membros da comunidade do AD FS e é monitorada regularmente pela equipe de produto do AD FS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminologia do AD FS usada neste guia  
  
|Termo do AD FS|Definição|  
|--------------|--------------|  
|Organização do parceiro de conta|Uma organização do parceiro de federação representada por uma relação de confiança do provedor de declarações no Serviço de Federação. A organização do parceiro de conta contém os usuários que acessarão aplicativos Web @ no__t-0based no parceiro de recurso.|  
|Servidor de federação de conta|O servidor de federação da organização do parceiro de conta. O servidor de federação de conta emite tokens de segurança aos usuários com base na autenticação do usuário. O servidor autentica o usuário, extrai os atributos relevantes e as informações de associação de grupo do repositório de atributos, empacota essas informações em declarações e gera e assina um token de segurança \(which contém as declarações @ no__t-1 para retornar para o usuário, seja para ser usado em sua própria organização ou para ser enviado para uma organização parceira.|  
|Banco de dados de configuração do AD FS|Um banco de dados usado para armazenar todos os dados de configuração que representam uma instância ou Serviço de Federação individual do AD FS. Esses dados de configuração podem ser armazenados em um banco de dado SQL Server ou usando o recurso de banco de dados interno do Windows incluído no Windows Server 2016, Windows Server 2012 e 2012 R2 e Windows Server 2008 e 2008 R2. </br></br>Você pode criar o banco de dados de configuração do AD FS para SQL Server usando o comando fsconfig. exe, a ferramenta no__t-0line e o banco de dados interno do Windows usando o assistente de configuração do servidor de Federação AD FS.|  
|Provedor de declarações|A organização que fornece as declarações a seus usuários. Vide “organização do parceiro de conta”.|  
|Relação de confiança do provedor de declarações|No snap-no__t de gerenciamento de AD FS-0in, as relações de confiança do provedor de declarações são objetos de confiança normalmente criados em organizações de parceiros de recursos para representar a organização na relação de confiança cujas contas acessarão recursos no parceiro de recurso organizações. Um objeto de confiança de provedor de declarações consiste em vários identificadores, nomes e regras para identificar esse parceiro ao Serviço de Federação local.|  
|Relação de confiança do provedor de declarações local|Um objeto de confiança que representa AD LDS ou três diretórios 0party LDAP @ no__t-1based em um farm AD FS. Um objeto de confiança do provedor de declarações local consiste em uma variedade de identificadores, nomes e regras que identificam esse diretório LDAP @ no__t-0based para o Serviço de Federação local.|  
|Metadados de federação|O formato de dados para comunicar as informações de configuração entre um provedor de declarações e uma terceira parte confiável a fim de facilitar a configuração apropriada das relações de confiança do provedor de declarações e dos objetos de confiança de terceira parte confiável. O formato de dados é definido em Security Assertion Markup Language \(SAML @ no__t-1 2,0 e é estendido em WS @ no__t-2Federation.|  
|Servidor de federação|Um Windows Server que foi configurado usando o assistente de configuração do servidor de Federação AD FS para atuar na função de servidor de Federação. O servidor de federação emite tokens e faz parte de um Serviço de Federação.|  
|Proxy do servidor de federação|Um Windows Server que foi configurado usando o assistente de configuração de proxy de servidor de Federação AD FS para atuar como um serviço de proxy intermediário entre um cliente de Internet e um Serviço de Federação que está localizado atrás de um firewall em uma rede corporativa.|  
|Servidor de federação primário|Um Windows Server que foi configurado na função de servidor de Federação usando o AD FS assistente de configuração do servidor de Federação e tem uma cópia de leitura @ no__t-0write do banco de dados de configuração do AD FS. </br></br> O servidor de Federação primário é criado quando você usa o assistente de configuração do servidor de Federação AD FS e seleciona a opção para criar um novo Serviço de Federação e tornar esse computador o primeiro servidor de Federação no farm. Todos os outros servidores de Federação neste farm devem replicar as alterações feitas no servidor de Federação primário para uma cópia @ no__t-0only de leitura do banco de dados de configuração do AD FS que é armazenado localmente. O termo “servidor de federação primário” não se aplicará quando o banco de dados de configuração do AD FS for armazenado em um banco de dados SQL, uma vez que todos os servidores de federação podem ler e gravar igualmente em um banco de dados de configuração armazenado em SQL Server.|  
|Terceira parte confiável|A organização que recebe e processa as declarações. Vide “organização do parceiro de recurso”.|  
|Objeto de confiança de terceira parte confiável|No snap do AD FS Management @ no__t-0in, as relações de confiança de terceira parte confiável são objetos de confiança normalmente criados em:<br /><br />-Organizações parceiras de contas para representar a organização na relação de confiança cujas contas acessarão recursos na organização do parceiro de recursos.<br />-Organizações parceiras de recursos para representar a confiança entre o Serviço de Federação e um único aplicativo Web @ no__t-0based.<br /><br />Um objeto de confiança de terceira parte confiável consiste em uma variedade de identificadores, nomes e regras que identificam esse parceiro ou Web @ no__t-0application para o Serviço de Federação local.|  
|Servidor de federação de recurso|O servidor de federação da organização do parceiro de recurso. O servidor de federação de recurso tipicamente emite tokens de segurança aos usuários com base em um token de segurança emitido por um servidor de federação de conta. O servidor recebe o token de segurança, verifica a assinatura, aplica a lógica de regra de declaração às declarações não empacotadas para produzir as declarações de saída desejadas, gera um novo token de segurança \(with as declarações de saída @ no__t-1 com base nas informações de entrada token de segurança e assina o novo token para retornar ao usuário e, por fim, para o aplicativo Web.|  
|Organização do parceiro de recurso|Um parceiro de federação que é representado por um objeto de confiança de terceira parte confiável no Serviço de Federação. O parceiro de recurso emite tokens de segurança @ no__t-0based que contém aplicativos Web @ no__t-1based publicados que os usuários do parceiro de conta podem acessar.|  
  
## <a name="overview-of-ad-fs"></a>Visão geral do AD FS  
AD FS é uma solução de acesso de identidade que fornece aos computadores cliente \(internal ou externa à sua rede @ no__t-1 com acesso de SSO contínuo a aplicativos ou serviços protegidos da Internet @ no__t-2facing, mesmo quando as contas de usuário e os aplicativos são localizado em redes ou organizações completamente diferentes.  
  
Quando um aplicativo ou serviço estiver em uma rede e uma conta de usuário estiver em outra rede, normalmente o usuário deve apresentar credenciais secundárias ao tentar acessar o aplicativo ou serviço. Essas credenciais secundárias representam a identidade do usuário no realm em que reside o aplicativo ou serviço. Elas são normalmente necessárias ao servidor Web que hospeda o aplicativo ou serviço para que ele possa tomar a decisão mais apropriada sobre a autorização.  
  
Com o AD FS, as organizações podem ignorar solicitações de credenciais secundárias fornecendo relações de confiança \(federation trusts @ no__t-1 que essas organizações podem usar para projetar a identidade digital e os direitos de acesso de um usuário para parceiros confiáveis. Nesse ambiente federado, cada organização continua a gerenciar suas próprias identidades, podendo também projetar e aceitar com segurança identidades de outras organizações.  
  
-   [A função de repositórios de atributos](The-Role-of-Attribute-Stores.md)  
  
-   [A função do banco de dados de configuração do AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [A função das declarações](The-Role-of-Claims.md)  
  
-   [A função das regras de declaração](The-Role-of-Claim-Rules.md)  
  
-   [A função do mecanismo de declarações](The-Role-of-the-Claims-Engine.md)  
  
-   [A função do pipeline de declarações](The-Role-of-the-Claims-Pipeline.md)  
  
-   [A função da linguagem da regra de declaração](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Determinar o tipo de modelo de regra de declaração a ser usado](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Como URIs são usados no AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

