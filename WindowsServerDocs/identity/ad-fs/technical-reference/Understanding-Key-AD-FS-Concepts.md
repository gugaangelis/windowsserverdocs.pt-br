---
ms.assetid: 204f5fe9-3611-4da0-b057-a386004b4598
title: "Conceitos dos serviços de Federação do Active Directory principais de Noções básicas sobre"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 27282c6b88b0457af3b4cf031fdadced7b40268c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="understanding-key-ad-fs-concepts"></a>Noções básicas sobre chaves do AD FS conceitos
É recomendável que você saiba mais sobre os conceitos importantes para os serviços de Federação do Active Directory e se familiarizar com o conjunto de recursos.  
  
> [!TIP]  
> Você pode encontrar links adicionais de recurso do AD FS no [mapa de conteúdo do AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) página no Wiki do Microsoft TechNet. Esta página é gerenciada por membros da comunidade AD FS e monitorada regularmente pela equipe de produto do AD FS.  
  
## <a name="ad-fs-terminology-used-in-this-guide"></a>Terminologia do AD FS usada neste guia  
  
|Termo do AD FS|Definição|  
|--------------|--------------|  
|Organização de parceiro de conta|Uma organização de parceiro de federação que é representada por uma relação de confiança do provedor de declarações no serviço de Federação. A organização de parceiro de conta contém os usuários que terá acesso a aplicativos baseados em Web\ no parceiro de recurso.|  
|Servidor de federação de conta|O servidor de federação na organização do parceiro de conta. O servidor de Federação conta emite tokens de segurança para os usuários com base na autenticação do usuário. O servidor autentica o usuário, extrai os atributos relevantes e informações de associação de grupo para fora da loja de atributo, empacota essas informações em declarações e gera e assina um token de segurança \ (que contém o claims\) para retornar para o usuário — seja usado em sua própria organização ou a ser enviada para uma organização de parceiro.|  
|Banco de dados de configuração do AD FS|Um banco de dados usado para armazenar todos os dados de configuração que representa uma única instância do AD FS ou serviço de Federação. Esses dados de configuração podem ser armazenados em um banco de dados do SQL Server ou usando o recurso de banco de dados interno do Windows incluídos no Windows Server 2016, Windows Server 2012 e 2012 R2 e Windows Server 2008 e 2008 R2. </br></br>Você pode criar o banco de dados de configuração do AD FS para SQL Server usando a ferramenta de linha de command\ Fsconfig.exe e para o banco de dados interno Windows usando o Assistente para configuração do servidor de Federação do AD FS.|  
|Provedor de declarações|A organização que fornece declarações para seus usuários. Veja a organização de parceiro de conta.|  
|Requerimentos judiciais ou Extrajudiciais provedor confiança|No AD FS snap\-in Gerenciamento, relações de confiança de provedor de declarações são geralmente criados em organizações de parceiros de recurso para representar a organização da relação de confiança cujas contas estarão acessando recursos na organização de parceiros do recurso de objetos de confiança. Um objeto de confiança do provedor de declarações consiste em uma variedade de identificadores, nomes e as regras que identificam esse parceiro para o serviço de federação local.|  
|Confiança de provedor de declarações de local|Um objeto de confiança que representa o AD LDS ou third\ terceiros com base em LDAP\ diretórios em uma farm AD FS. Um local declarações de confiança do provedor objeto consiste em uma variedade de identificadores, nomes e as regras que identifica esse diretório baseado em LDAP\ para o serviço de federação local.|  
|Metadados de Federação|O formato de dados para comunicar informações de configuração entre um provedor de declarações e um terceiro para facilitar a configuração adequada de requerimentos judiciais ou Extrajudiciais provedor relações de confiança e terceira parte. O formato de dados é definido em linguagem de marcação de asserção de segurança \(SAML\) 2.0, e ele é estendido no WS\ federação.|  
|Servidor de Federação|Um servidor Windows que tenha sido configurado usando o Assistente para configuração do servidor de Federação do AD FS para agir na função de servidor de Federação. Um servidor de Federação emite tokens e serve como parte de um serviço de Federação.|  
|Proxy do servidor de Federação|Um servidor Windows que tenha sido configurado usando o Assistente de configuração do AD FS federação servidor Proxy para atuar como um proxy intermediário serviço entre um cliente de Internet e um serviço de Federação está localizado atrás de um firewall em uma rede corporativa.|  
|Servidor de Federação principal|Um servidor Windows que tiver sido configurado na função de servidor de Federação usando o Assistente para configuração do servidor de Federação do AD FS e tem uma cópia read\/gravação do banco de dados de configuração do AD FS. </br></br> O servidor de Federação principal é criado quando você usar o Assistente para configuração do servidor de Federação do AD FS e selecione a opção de criar um novo serviço de Federação e tornar o computador primeiro servidor de federação no farm. Todos os outros servidores de Federação neste farm devem replicar as alterações feitas no servidor de Federação principal em uma cópia somente read\ do banco de dados de configuração do AD FS que é armazenado localmente. O termo "servidor de Federação principal" não se aplica quando o banco de dados de configuração do AD FS é armazenado em um banco de dados SQL como todos os servidores de Federação igualmente podem ler e gravar em um banco de dados de configuração armazenado em um servidor SQL.|  
|Terceira parte confiável|A organização que recebe e processa requerimentos judiciais ou Extrajudiciais. Consulte organização parceiro de recurso.|  
|Dependência de confiança de terceiros|No AD FS gerenciamento snap\-in, contar relações de confiança de terceiros objetos de confiança normalmente são criados em:<br /><br />-Organizações parceiras para representar a organização da relação de confiança cujas contas estarão acessando recursos na organização de parceiros do recurso da conta.<br />-As organizações parceiras de resource para representar a relação de confiança entre o serviço de Federação e um único aplicativo com base em web\.<br /><br />Um objeto de confiança de terceiros terceira consiste em uma variedade de identificadores, nomes e regras que identifiquem este parceiro ou web\-aplicativo para o serviço de federação local.|  
|Servidor de Federação do recurso|O servidor de federação na organização do parceiro de recurso. O servidor de Federação do recurso geralmente emite tokens de segurança para usuários com base em um token de segurança emitido por um servidor de federação de conta. O servidor recebe o token de segurança, verifica a assinatura, se aplica a lógica de regra de declaração para as declarações descompactadas para produzir as declarações de saída desejadas, gera um novo token de segurança \ (com a saída claims\) com base nas informações no token de segurança de entrada e assina o novo token para retornar ao usuário e, finalmente, para o aplicativo Web.|  
|Organização de parceiro de recurso|Um parceiro de federação que é representado por uma terceira parte de confiança no serviço de Federação. O parceiro de recurso emite tokens de segurança baseada em claims\ que contém os aplicativos publicados com base em Web\ que os usuários no parceiro de conta podem acessar.|  
  
## <a name="overview-of-ad-fs"></a>Visão geral do AD FS  
AD FS é uma solução de acesso de identidade que fornece os computadores cliente \ (interno ou externo para sua rede \) com acesso perfeito de SSO para aplicativos voltados para o Internet\ ou serviços, mesmo quando as contas de usuário e os aplicativos estão localizados em redes completamente diferentes ou organizações protegido.  
  
Quando um aplicativo ou serviço está em uma rede e uma conta de usuário estiver em outra rede, normalmente o usuário é solicitado credenciais secundário quando tentar acessar o aplicativo ou serviço. Essas credenciais secundárias representam a identidade do usuário no território em que o aplicativo ou serviço reside. Eles geralmente são necessárias pelo servidor Web que hospeda o aplicativo ou serviço para que ele possa fazer a decisão de autorização mais apropriada.  
  
Com o AD FS, as organizações podem ignorar solicitações de credenciais secundárias, fornecendo confiança relações \(federation trusts\) que essas empresas podem usar para projetar acesso e identidade direitos digitais um usuário para parceiros confiáveis. Nesse ambiente federado, cada organização continua a gerenciar suas próprias identidades, mas cada organização também com segurança pode projetar e aceitar identidades de outras organizações.  
  
-   [A função dos repositórios de atributo](The-Role-of-Attribute-Stores.md)  
  
-   [A função do banco de dados de configuração do AD FS](The-Role-of-the-AD-FS-Configuration-Database.md)  
  
-   [A função de reivindicações](The-Role-of-Claims.md)  
  
-   [A função de regras de declaração](The-Role-of-Claim-Rules.md)  
  
-   [A função do mecanismo requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Engine.md)  
  
-   [A função do Pipeline requerimentos judiciais ou Extrajudiciais](The-Role-of-the-Claims-Pipeline.md)  
  
-   [A função da linguagem de regra de declaração](The-Role-of-the-Claim-Rule-Language.md)  
  
-   [Determinar o tipo de modelo de regra de declaração para usar](Determine-the-Type-of-Claim-Rule-Template-to-Use.md)  
  
-   [Como os URIs são usados no AD FS](How-URIs-Are-Used-in-AD-FS.md)  
  

