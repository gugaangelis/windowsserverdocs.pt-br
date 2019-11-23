---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guia de Design do AD FS no Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d9d7ec6f4ff575d3aac30b7127e591b78f5ef49b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359211"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guia de design de AD FS no Windows Server 


  
> [!NOTE]  
> Para obter informações sobre como implantar AD FS no Windows Server 2012 R2, consulte o [Guia de implantação do Windows server 2012 r2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
Você pode usar Active Directory® serviços de Federação \(AD FS\) com o sistema operacional Windows Server® 2012 em uma função de provedor de serviços de Federação para autenticar seus usuários diretamente em qualquer serviço Web\-ou aplicativos que residem em uma organização de parceiro de recurso, sem a necessidade de os administradores criarem ou manterem relações de confiança externas ou de floresta entre as redes de ambas as organizações e sem a necessidade dos usuários de fazer logon uma segunda vez. O processo de autenticação em uma rede, enquanto acessa recursos em outra rede, sem a carga de ações de logon repetidas pelos usuários, é conhecido como\-de logon único em \(SSO\).  
  
## <a name="about-this-guide"></a>Sobre este guia  
Este guia fornece recomendações para ajudá-lo a planejar uma nova implantação do AD FS, com base nos requisitos de sua organização \(também mencionados neste guia como metas de implantação\) e o design específico que você deseja criar. Este guia destina-se ao uso por um especialista em infraestrutura ou arquiteto de sistema. Ele realça os principais pontos de decisão conforme você planeja sua implantação de AD FS. Antes de ler este guia, você deve ter uma boa compreensão de como AD FS funciona em um nível funcional. Você também deve ter uma boa compreensão dos requisitos organizacionais que serão refletidos em seu design de AD FS.  
  
Este guia descreve um conjunto de metas de implantação baseadas em três designs de AD FS principais e ajuda você a decidir o design mais apropriado para seu ambiente. Você pode usar essas metas de implantação para formar um dos seguintes designs de AD FS abrangentes ou um design personalizado que atenda às necessidades do seu ambiente:  
  
-   SSO da Web federado para dar suporte\-a\-de negócios a cenários de\) Business \(B2B e para dar suporte à colaboração entre unidades de negócios com florestas independentes  
  
-   SSO da Web para dar suporte ao acesso do cliente a aplicativos em Business\-para\-cenários do consumidor \(B2C\)  
  
Para cada design, você encontrará diretrizes para coletar os dados requeridos sobre seu ambiente. Você pode usar essas diretrizes para planejar e projetar sua implantação de AD FS. Depois de ler este guia e concluir a coleta, documentação e mapeamento dos requisitos da sua organização, você terá as informações necessárias para iniciar a implantação de AD FS usando as diretrizes no [Guia de implantação do Windows Server 2012 AD FS](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Como identificar as metas de implantação do AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Como mapear suas metas de implantação para um design do AD FS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Determinar a topologia de implantação do AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Como planejar sua implantação](Planning-Your-Deployment.md)  
  
-   [Como planejar o posicionamento do servidor de federação](Planning-Federation-Server-Placement.md)  
  
-   [Como planejar o posicionamento do proxy do servidor de federação](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Como planejar a capacidade do servidor do AD FS](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Apêndice A: revisando requisitos de AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

