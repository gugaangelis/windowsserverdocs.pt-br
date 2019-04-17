---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: "A função dos repositórios de atributo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 730411ed7efbb9cf0db3d7e94a486cec4c363849
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
 >Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-attribute-stores"></a>A função dos repositórios de atributo
Serviços de Federação do Active Directory usa o termo "lojas de atributo" para consultar bancos de dados que uma organização usa para armazenar suas contas de usuário e seus valores de atributo associado ou diretórios. Depois que ele é configurado em uma organização de provedor de identidade, o AD FS recupera esses valores de atributo da loja e cria declarações com base nessas informações para que um aplicativo da Web ou serviço que está hospedado em uma terceira organização pode tomar as decisões de autorização de apropriado sempre que um usuário federado \ (um usuário cuja conta é armazenada na organization\ de provedor de identidade) tentativas de acessar o aplicativo ou serviço.  
  
Para obter mais informações sobre como as declarações são geradas, consulte [a função de declarações](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>Como o atributo armazena estar de acordo com suas metas de implantação do AD FS  
O local do repositório de atributo do usuário e o local do qual os usuários são autenticados determinar o design do AD FS para dar suporte às identidades do usuário. Dependendo de onde se encontra o repositório de atributo e onde os usuários acessarão o aplicativo \ (em uma intranet ou na Internet\), você pode usar um dos seguintes metas de implantação:  
  
-   [Fornecer o Active Directory aos usuários acesso aos seus aplicativos com reconhecimento de declarações e serviços](https://technet.microsoft.com/library/dd807071.aspx)— esse objetivo, os usuários em sua organização acessam um AD FS – protegidas aplicativo ou serviço \ (seu próprio aplicativo ou serviço ou um parceiro aplicativo ou intelegente) quando os usuários estão conectados ao Active Directory na intranet corporativa.  
  
-   [Forneça o Active Directory aos usuários acesso aos aplicativos e serviços de outras organizações](https://technet.microsoft.com/library/dd807123.aspx)— esse objetivo, os usuários em sua organização acessam um AD FS – protegidas aplicativo ou serviço \ (seu próprio aplicativo ou serviço ou um parceiro aplicativo ou intelegente) quando os usuários estão conectados a um repositório de atributo na intranet corporativa e quando eles fizerem logon remotamente da Internet.  
  
-   [Fornecer aos usuários em outra organização acesso a seus aplicativos com reconhecimento de declarações e serviços](https://technet.microsoft.com/library/dd807099.aspx)— esse objetivo, contas de usuário em outra organização que estão localizadas em um repositório de atributo na intranet corporativa da organização devem acessar um anúncio aplicativo FS – protegidas em sua organização. Essa meta também funciona quando contas de usuário baseada em consumer\ que estão localizadas em um repositório de atributo na rede do perímetro da sua organização devem ser fornecidas com acesso a um anúncio aplicativo FS – protegidas em sua organização.  
  
Dependendo do posicionamento do atributo da store e outros requisitos da sua organização, você pode combinar vários essas metas de implantação para concluir o design da implantação do AD FS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Atributo repositórios que são suportados pelo AD FS  
AD FS dá suporte a uma ampla variedade de diretório e armazena de banco de dados que você pode usar para extração de valores de atributos definidos pelo administrator\ e preenchendo requerimentos judiciais ou Extrajudiciais com esses valores. AD FS dá suporte a qualquer um dos seguintes diretórios ou bancos de dados como repositórios de atributo:  
  
-   Active Directory no Windows Server 2003, serviços de domínio do Active Directory \(AD DS\) no Windows Server 2008, o AD DS no Windows Server 2012 e 2012 R2 e Windows Server 2016. 
  
-   Todas as edições do Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 e SQL Server 2016  
  
-   Repositórios de atributo personalizado  
  

