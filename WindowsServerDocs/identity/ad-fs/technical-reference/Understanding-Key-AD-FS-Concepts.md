---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: Noções básicas sobre a federação do Active Directory principais conceitos de serviços
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 27282c6b88b0457af3b4cf031fdadced7b40268c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878127"
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="understanding-key-ad-fs-concepts"></a>Entendendo os conceitos do Key AD FS
É recomendável que você saiba mais sobre os conceitos importantes para os serviços de Federação do Active Directory e se familiarizar com o conjunto de recursos.  
  
> [!TIP]  
> Você pode encontrar links de recursos do AD FS adicionais na página [Mapa de Conteúdo do AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) , no TechNet Wiki da Microsoft. Essa página é gerenciada por membros da comunidade do AD FS e é monitorada regularmente pela equipe de produto do AD FS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminologia do AD FS usada neste guia  
  
|Termo do AD FS|Definição|  
|--------------|--------------|  
|Organização do parceiro de conta|Uma organização do parceiro de federação representada por uma relação de confiança do provedor de declarações no Serviço de Federação. A organização do parceiro de conta contém os usuários que acessarão Web\-com base em aplicativos no parceiro de recurso.|  
|Servidor de federação de conta|O servidor de federação da organização do parceiro de conta. O servidor de federação de conta emite tokens de segurança aos usuários com base na autenticação do usuário. O servidor autentica o usuário, extrai as informações de associação de grupo fora do repositório de atributos e atributos relevantes, agrupa essas informações em declarações e gera e assina um token de segurança \(que contém as declarações\)para retornar para o usuário — a ser usado em sua própria organização ou a serem enviados para uma organização parceira.|  
|Banco de dados de configuração do AD FS|Um banco de dados usado para armazenar todos os dados de configuração que representam uma instância ou Serviço de Federação individual do AD FS. Esses dados de configuração podem ser armazenados em um banco de dados do SQL Server ou usando o recurso de banco de dados interno do Windows incluído com o Windows Server 2016, Windows Server 2012 e 2012 R2 e Windows Server 2008 e 2008 R2. </br></br>Você pode criar o banco de dados de configuração do AD FS para o SQL Server usando o comando Fsconfig.exe\-ferramenta de linha e para o Windows Internal Database usando o Assistente de configuração do servidor de Federação do AD FS.|  
|Provedor de declarações|A organização que fornece as declarações a seus usuários. Vide “organização do parceiro de conta”.|  
|Relação de confiança do provedor de declarações|No snap do gerenciamento do AD FS\-, declarações de confianças do provedor são objetos de confiança tipicamente criados em organizações parceiras para representar a organização na relação de confiança cujas contas acessarão recursos no recurso organização do parceiro. Um objeto de confiança de provedor de declarações consiste em vários identificadores, nomes e regras para identificar esse parceiro ao Serviço de Federação local.|  
|Relação de confiança do provedor de declarações local|Um objeto de confiança que representa o AD LDS ou terceiro\-LDAP de terceiros\-com base em diretórios em um farm do AD FS. Objeto consiste em uma variedade de identificadores, nomes e regras que identificam esse LDAP de confiança do provedor de declarações de um local\-com base em diretório para o serviço de federação local.|  
|Metadados de federação|O formato de dados para comunicar as informações de configuração entre um provedor de declarações e uma terceira parte confiável a fim de facilitar a configuração apropriada das relações de confiança do provedor de declarações e dos objetos de confiança de terceira parte confiável. O formato de dados é definido no Security Assertion Markup Language \(SAML\) 2.0 e ele é estendido na WS\-federação.|  
|Servidor de federação|Um servidor Windows que tenha sido configurado usando o Assistente de configuração do servidor de Federação do AD FS para atuar na função de servidor de Federação. O servidor de federação emite tokens e faz parte de um Serviço de Federação.|  
|Proxy do servidor de federação|Um servidor Windows que tenha sido configurado usando o Assistente de configuração do AD FS Federation Server Proxy para atuar como um proxy intermediário de serviço entre um cliente da Internet e um serviço de federação localizado atrás de um firewall em uma rede corporativa.|  
|Servidor de federação primário|Um servidor Windows que tenha sido configurado na função de servidor de Federação usando o Assistente de configuração do servidor de Federação do AD FS e tem uma leitura\/gravar cópia de banco de dados de configuração do AD FS. </br></br> O servidor de Federação primário é criado quando você usa o Assistente de configuração do servidor de Federação do AD FS e selecione a opção para criar um novo serviço de Federação e transformar o computador o primeiro servidor de federação no farm. Todos os outros servidores de Federação desse farm devem replicar as alterações feitas no servidor de Federação primário para uma leitura\-apenas uma cópia do banco de dados de configuração do AD FS armazenado localmente. O termo “servidor de federação primário” não se aplicará quando o banco de dados de configuração do AD FS for armazenado em um banco de dados SQL, uma vez que todos os servidores de federação podem ler e gravar igualmente em um banco de dados de configuração armazenado em SQL Server.|  
|Terceira parte confiável|A organização que recebe e processa as declarações. Vide “organização do parceiro de recurso”.|  
|Objeto de confiança de terceira parte confiável|No snap do gerenciamento do AD FS\-no, os objetos de confiança da terceira parte confiável são objetos de confiança tipicamente criados em:<br /><br />-Organizações parceiras para representar a organização na relação de confiança cujas contas acessarão recursos na organização do parceiro de recurso da conta.<br />-Organizações parceiras para representar a relação de confiança entre o serviço de Federação e uma única web\-com base em aplicativo.<br /><br />Um objeto de confiança de terceira parte confiável consiste em uma variedade de identificadores, nomes e regras para identificar esse parceiro ou web\-aplicativo para o serviço de federação local.|  
|Servidor de federação de recurso|O servidor de federação da organização do parceiro de recurso. O servidor de federação de recurso tipicamente emite tokens de segurança aos usuários com base em um token de segurança emitido por um servidor de federação de conta. O servidor recebe o token de segurança, verifica a assinatura, aplica-se a lógica da regra de declaração às declarações desagrupadas para produzir as declarações de saída desejadas, gera um novo token de segurança \(com as declarações de saída\) com base nas informações no token de segurança de entrada e assina o novo token para retornar para o usuário e, finalmente, para o aplicativo Web.|  
|Organização do parceiro de recurso|Um parceiro de federação que é representado por um objeto de confiança de terceira parte confiável no Serviço de Federação. O parceiro de recurso emite declarações\-com base em tokens de segurança que contém a Web publicado\-com base em aplicativos que os usuários no parceiro de conta podem acessar.|  
  
## <a name="overview-of-ad-fs"></a>Visão geral do AD FS  
O AD FS é uma solução de acesso de identidade que oferece aos computadores cliente \(internos ou externos à sua rede\) com acesso de SSO contínuo para protegida Internet\-voltado para a aplicativos ou serviços, mesmo quando as contas de usuário e aplicativos estão localizados em redes ou organizações completamente diferentes.  
  
Quando um aplicativo ou serviço estiver em uma rede e uma conta de usuário estiver em outra rede, normalmente o usuário deve apresentar credenciais secundárias ao tentar acessar o aplicativo ou serviço. Essas credenciais secundárias representam a identidade do usuário no realm em que reside o aplicativo ou serviço. Elas são normalmente necessárias ao servidor Web que hospeda o aplicativo ou serviço para que ele possa tomar a decisão mais apropriada sobre a autorização.  
  
Com o AD FS, as organizações podem ignorar as solicitações de credenciais secundárias fornecendo relações de confiança \(relações de confiança de Federação\) que essas organizações podem usar para projetar de identidade e acesso direitos digitais um usuário confiáveis parceiros. Nesse ambiente federado, cada organização continua a gerenciar suas próprias identidades, podendo também projetar e aceitar com segurança identidades de outras organizações.  
  
-   [A função dos repositórios de atributos](The-Role-of-Attribute-Stores.md)  
  
-   [A função do banco de dados de configuração do AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [A função das declarações](The-Role-of-Claims.md)  
  
-   [A função de regras de declaração](The-Role-of-Claim-Rules.md)  
  
-   [A função do mecanismo de declarações](The-Role-of-the-Claims-Engine.md)  
  
-   [A função do Pipeline de declarações](The-Role-of-the-Claims-Pipeline.md)  
  
-   [A função da linguagem de regra de declaração](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Determinar o tipo de modelo de regra de declaração para usar](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Como URIs são usados no AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

