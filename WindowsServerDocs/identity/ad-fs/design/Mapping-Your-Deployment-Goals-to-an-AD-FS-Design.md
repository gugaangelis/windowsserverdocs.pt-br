---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: "Mapeando as metas de implantação para um Design do AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mapeando as metas de implantação para um Design do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Após revisar as metas de implantação de \(AD FS\) serviços de Federação do Active Directory existentes e determinar quais metas estão relacionadas à sua implantação, você pode mapear essas metas para um design AD FS específico. Para obter mais informações sobre o AD FS predefinidas metas de implantação, consulte [identificando as metas de implantação do AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Use a tabela a seguir para determinar quais design AD FS é mapeado para a combinação adequada do AD FS metas de implantação para sua organização. Esta tabela refere-se apenas aos dois projetos de AD FS principais, conforme descrito neste guia. No entanto, você pode criar um design personalizado do AD FS ou híbridos usando qualquer combinação das metas de implantação do AD FS para atender às necessidades da sua organização.  
  
|Meta de implantação do AD FS|[Design SSO da Web](Web-SSO-Design.md)|[Design SSO da Web federado](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Fornecer seu do Active Directory aos usuários acesso aos seus aplicativos com reconhecimento de declarações e serviços](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Não|Sim, o parceiro de conta|  
|[Fornecer o acesso de usuários do Active Directory para os aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|Não|Sim, opcional no parceiro de conta|  
|[Fornecer aos usuários em outra organização acesso aos seus aplicativos com reconhecimento de declarações e serviços](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Sim|Sim|  

## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

