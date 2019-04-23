---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: A função dos repositórios de atributos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 730411ed7efbb9cf0db3d7e94a486cec4c363849
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860407"
---
 >Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="the-role-of-attribute-stores"></a>A função dos repositórios de atributos
Serviços de Federação do Active Directory usa o termo "repositórios de atributos" para fazer consultar diretórios ou bancos de dados que uma organização usa para armazenar suas contas de usuário e seus valores de atributo associados. Depois que ele é configurado em uma organização do provedor de identidade, o AD FS recupera esses valores de atributo do repositório e cria declarações com base nessas informações, para que um aplicativo Web ou serviço que é hospedado em uma organização de terceira parte confiável possa fazer apropriado decisões de autorização, sempre que um usuário federado \(um usuário cuja conta é armazenada na organização do provedor de identidade\) tenta acessar o aplicativo ou serviço.  
  
Para obter mais informações sobre como as declarações são geradas, consulte [The Role of Claims](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>Como os repositórios de atributos se ajustam às suas metas de implantação do AD FS  
O local do repositório de atributos de usuário e o local do qual os usuários se autenticam determinam como você projeta o AD FS para dar suporte as identidades de usuário. Dependendo de onde se encontra o repositório de atributos e onde os usuários acessarão o aplicativo \(em uma intranet ou na Internet\), você pode usar uma das seguintes metas de implantação:  
  
-   [Fornecer seu Active Directory Users Access a seus aplicativos com reconhecimento de declarações e serviços](https://technet.microsoft.com/library/dd807071.aspx)— nesse objetivo, os usuários em sua organização acessam um aplicativo do AD FS – protegido ou serviço \(seu próprio aplicativo ou serviço ou um aplicativo ou serviço do parceiro\) quando os usuários estão conectados ao Active Directory na intranet corporativa.  
  
-   [Fornecer o acesso de usuários do Active Directory para os aplicativos e serviços de outras organizações](https://technet.microsoft.com/library/dd807123.aspx)— nesse objetivo, os usuários em sua organização acessam um aplicativo do AD FS – protegido ou serviço \(seu próprio aplicativo ou serviço ou um aplicativo ou serviço do parceiro\) quando os usuários estiverem conectados a um repositório de atributos na intranet corporativa e quando eles fizerem logon remotamente pela Internet.  
  
-   [Fornecer aos usuários de outra organização acesso a seus aplicativos com reconhecimento de declarações e serviços](https://technet.microsoft.com/library/dd807099.aspx)— nesse objetivo, contas de usuário em outra organização que estão localizadas em um repositório de atributos na intranet corporativa da organização que devem acessar um AD FS – aplicativo protegido em sua organização. Essa meta também funciona ao consumidor\-contas de usuário baseada em que estão localizadas em um repositório de atributos na rede de perímetro da sua organização devem ser fornecidas com o acesso a um AD FS – protegido o aplicativo em sua organização.  
  
Dependendo do posicionamento do repositório de atributos e outros requisitos da sua organização, você pode combinar várias dessas metas de implantação para concluir o design da sua implantação do AD FS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Repositórios de atributos que têm suporte pelo AD FS  
O AD FS dá suporte a uma ampla gama de diretório e banco de dados armazena o que você pode usar para extrair o administrador\-definidos valores de atributo e preencher declarações com esses valores. O AD FS dá suporte a qualquer um dos seguintes diretórios ou bancos de dados como repositórios de atributos:  
  
-   Active Directory no Windows Server 2003, serviços de domínio do Active Directory \(AD DS\) no Windows Server 2008, AD DS no Windows Server 2012 e 2012 R2 e Windows Server 2016. 
  
-   Todas as edições do Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 e SQL Server 2016  
  
-   Repositórios de atributos personalizados  
  

