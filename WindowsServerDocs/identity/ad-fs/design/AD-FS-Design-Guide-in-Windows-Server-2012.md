---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guia de Design do AD FS no Windows Server 2012
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e660c1dabcc5a683fa74068ea148fd4efbeee569
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012"></a>Guia de Design do AD FS no Windows Server 2012

>Aplica-se a: Windows Server 2012
  
> [!NOTE]  
> Para obter informações sobre como implantar o AD FS no Windows Server 2012 R2, consulte [guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
Você pode usar serviços de Federação do Active Directory® \(AD FS\) ao sistema operacional Windows Server® 2012 em uma função de provedor de serviços de Federação perfeitamente autenticar os usuários para serviços baseados em Web\ ou aplicativos que residem em uma organização de parceiro de recurso, sem a necessidade de administradores criar ou manter relações de confiança externas ou relações de confiança de floresta entre as redes de ambas as organizações e sem a necessidade dos usuários façam logon em uma segunda vez. O processo de autenticar em uma rede e acessar recursos em outra rede — sem a sobrecarga de ações de logon repetidas por usuários — é conhecido como único sign\ em \(SSO\).  
  
## <a name="about-this-guide"></a>Sobre este guia  
Este guia fornece recomendações para ajudá-lo a planejar uma nova implantação do AD FS, com base nos requisitos da sua organização \ (também chamado neste guia de implantação goals\) e o design específico que você deseja criar. Este guia destina-se ao uso por um engenheiro de especialista ou sistema de infraestrutura. Ele destaca os pontos de decisão principal ao planejar sua implantação do AD FS. Antes de ler este guia, você deve ter uma boa compreensão de como o AD FS funciona em um nível funcional. Você também deve ter uma boa compreensão dos requisitos organizacionais que serão refletidas no design do AD FS.  
  
Este guia descreve um conjunto de metas de implantação que se baseiam em três designs do AD FS principais, e ele ajuda você a decidir o design mais apropriado para seu ambiente. Você pode usar essas metas de implantação para formar um os seguintes designs do AD FS abrangentes ou um design personalizado que atenda às necessidades do seu ambiente:  
  
-   SSO da Web federado para dar suporte a cenários \(B2B\) business\-to\-business e dar suporte a colaboração entre as unidades de negócios com florestas independentes  
  
-   SSO dar suporte ao acesso para aplicativos cliente em cenários \(B2C\) business\-to\-consumidor da Web  
  
Para cada projeto, você encontrará diretrizes para coletar os dados necessários sobre seu ambiente. Em seguida, você pode usar estas diretrizes para planejar e projetar sua implantação do AD FS. Depois de ler este guia e concluir a coleta, documentar e mapeamento de requisitos da sua organização, você terá as informações necessárias para iniciar a implantação do AD FS usando as orientações a [guia de implantação do Windows Server 2012 AD FS](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Identificando as metas de implantação do AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Mapeando as metas de implantação para um Design do AD FS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Determinar a topologia de implantação do AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Planejar sua implantação](Planning-Your-Deployment.md)  
  
-   [Planejar o posicionamento do servidor de Federação](Planning-Federation-Server-Placement.md)  
  
-   [Planejar o posicionamento de Proxy do servidor de Federação](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Planejando para o AD FS servidor capacidade](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Apêndice a: examinando os requisitos do AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

