---
ms.assetid: 4ddb927d-d65e-491d-840a-16049c083d13
title: A função dos repositórios de atributos
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 595f0b3b11172df9bf95d3bb8aab90368bb0e77f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860189"
---
# <a name="the-role-of-attribute-stores"></a>A função dos repositórios de atributos
Serviços de Federação do Active Directory (AD FS) usa o termo "repositórios de atributo" para se referir a diretórios ou bancos de dados que uma organização usa para armazenar suas contas de usuário e seus valores de atributo associados. Depois de configurado em uma organização de provedor de identidade, AD FS recupera esses valores de atributo da loja e cria declarações com base nessas informações para que um aplicativo Web ou serviço hospedado em uma organização de terceira parte confiável possa tomar as decisões de autorização apropriadas sempre que um usuário federado \(um usuário cuja conta esteja armazenada na organização do provedor de identidade\) tentar acessar o aplicativo ou serviço.  
  
Para obter mais informações sobre como as declarações são geradas, consulte [The Role of Claims](The-Role-of-Claims.md).  
  
## <a name="how-attribute-stores-fit-in-with-your-ad-fs-deployment-goals"></a>Como os repositórios de atributos se ajustam às suas metas de implantação do AD FS  
O local do repositório de atributos de usuário e o local de onde os usuários se autenticam determinam como você cria AD FS para dar suporte às identidades de usuário. Dependendo de onde o repositório de atributos está localizado e de onde os usuários acessarão o aplicativo \(em uma intranet ou na Internet\), você pode usar uma das seguintes metas de implantação:  
  
-   [Forneça aos seus Active Directory usuários acesso aos seus aplicativos e serviços com reconhecimento de declaração](https://technet.microsoft.com/library/dd807071.aspx)– nessa meta, os usuários em sua organização acessam um aplicativo ou serviço protegido por AD FS \(seu próprio aplicativo ou serviço ou o aplicativo ou serviço de um parceiro\) quando os usuários estão conectados ao Active Directory na intranet corporativa.  
  
-   [Forneça aos seus Active Directory usuários acesso aos aplicativos e serviços de outras organizações](https://technet.microsoft.com/library/dd807123.aspx)— nessa meta, os usuários em sua organização acessam um aplicativo ou serviço protegido por AD FS \(seu próprio aplicativo ou serviço ou o serviço ou aplicativo de um parceiro\) quando os usuários estão conectados a um repositório de atributos na intranet corporativa e quando fazem logon remotamente pela Internet.  
  
-   [Forneça aos usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declaração](https://technet.microsoft.com/library/dd807099.aspx)– nessa meta, as contas de usuário em outra organização localizadas em um repositório de atributos na intranet corporativa da organização devem acessar um aplicativo protegido por AD FS em sua organização. Essa meta também funciona quando as contas de usuário baseadas no consumidor\-que estão localizadas em um repositório de atributos na rede de perímetro da sua organização devem ser fornecidas com acesso a um aplicativo protegido por AD FS em sua organização.  
  
Dependendo do posicionamento do repositório de atributos e de outros requisitos da sua organização, você pode combinar várias dessas metas de implantação para concluir o design de sua implantação de AD FS.  
  
## <a name="attribute-stores-that-are-supported-by-ad-fs"></a>Repositórios de atributos que têm suporte pelo AD FS  
O AD FS dá suporte a uma ampla variedade de repositórios de diretórios e bancos de dados que você pode usar para extrair os valores de atributo definidos pelo administrador\-e preencher as declarações com esses valores. O AD FS dá suporte a qualquer um dos diretórios ou bancos de dados a seguir como repositórios de atributos:  
  
-   Active Directory no Windows Server 2003, Active Directory Domain Services \(AD DS\) no Windows Server 2008, AD DS no Windows Server 2012 e 2012 R2 e no Windows Server 2016. 
  
-   Todas as edições do Microsoft SQL Server 2005, SQL Server 2008, SQL Server 2012, SQL Server 2014 e SQL Server 2016  
  
-   Repositórios de atributos personalizados  
  

