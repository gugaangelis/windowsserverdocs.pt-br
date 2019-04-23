---
ms.assetid: bb16e39d-566d-436c-b957-394c06d556db
title: Guia de Design do AD FS no Windows Server 2012
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e660c1dabcc5a683fa74068ea148fd4efbeee569
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890207"
---
# <a name="ad-fs-design-guide-in-windows-server-2012"></a>Guia de Design do AD FS no Windows Server 2012

>Aplica-se a: Windows Server 2012
  
> [!NOTE]  
> Para obter informações sobre como implantar o AD FS no Windows Server 2012 R2, consulte [guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md).  
  
Você pode usar os serviços de Federação do Active Directory® \(do AD FS\) com o Windows Server® 2012 o sistema operacional em uma federação dos serviços de função de provedor para sem interrupções, autenticar usuários para qualquer Web\-com base em serviços ou aplicativos que residem em uma organização de parceiro de recurso, sem a necessidade dos administradores criar ou manter relações de confiança externas ou relações de confiança de floresta entre as redes de ambas as organizações e sem a necessidade dos usuários façam logon uma segunda vez. O processo de autenticação a uma rede e acessar recursos em outra rede — sem a sobrecarga de ações de logon repetidas por usuários — é conhecido como logon único\-na \(SSO\).  
  
## <a name="about-this-guide"></a>Sobre este guia  
Este guia fornece recomendações para ajudá-lo a planejar uma nova implantação do AD FS, com base nos requisitos da sua organização \(também referido neste guia como metas de implantação\) e no design específico que você deseja criar. Este guia destina-se ao uso por um especialista em infraestrutura ou arquiteto de sistema. Ele destaca seus principais pontos de decisão ao planejar sua implantação do AD FS. Antes de ler este guia, você deve ter uma boa compreensão do funcionamento do AD FS em um nível funcional. Você também deve ter uma boa compreensão dos requisitos organizacionais que serão refletidos no seu design do AD FS.  
  
Este guia descreve um conjunto de metas de implantação que se baseiam em três designs do AD FS primários e ajudá-lo a decidir os designs mais adequados para seu ambiente. Você pode usar essas metas de implantação para um dos seguintes designs do AD FS abrangentes ou um design personalizado que atenda às necessidades do seu ambiente:  
  
-   SSO da Web para oferecer suporte aos negócios federado\-para\-negócios \(B2B\) cenários e dá suporte à colaboração entre unidades empresariais com florestas independentes  
  
-   Web SSO para dar suporte ao acesso de cliente para aplicativos nos negócios\-à\-consumidor \(B2C\) cenários  
  
Para cada design, você encontrará diretrizes para coletar os dados requeridos sobre seu ambiente. Em seguida, você pode usar essas diretrizes para planejar e projetar sua implantação do AD FS. Depois de ler este guia e terminar de coletar, documentar e mapear as necessidades da sua organização, você terá as informações necessárias para começar a implantar o AD FS usando as diretrizes a [Windows Server 2012 do AD FS Deployment Guide](../../ad-fs/deployment/Windows-Server-2012-AD-FS-Deployment-Guide.md).  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Identificando as metas de implantação do AD FS](Identifying-Your-AD-FS-Deployment-Goals.md)  
  
-   [Mapeando suas metas de implantação para um Design do AD FS](Mapping-Your-Deployment-Goals-to-an-AD-FS-Design.md)  
  
-   [Determinar a topologia de implantação do AD FS](Determine-Your-AD-FS-Deployment-Topology.md)  
  
-   [Planejando a implantação](Planning-Your-Deployment.md)  
  
-   [Planejando o posicionamento do servidor de Federação](Planning-Federation-Server-Placement.md)  
  
-   [Planejando o posicionamento do Proxy de servidor de Federação](Planning-Federation-Server-Proxy-Placement.md)  
  
-   [Planejamento de capacidade do AD FS Server](Planning-for-AD-FS-Server-Capacity.md)  
  
-   [Apêndice a: Revisar os requisitos do AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)  
  

