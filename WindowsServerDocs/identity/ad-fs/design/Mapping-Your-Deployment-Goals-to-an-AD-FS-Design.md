---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: Mapeando suas metas de implantação para um design do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 048bce75c52895b2d9e215bdccef9cb13dc23533
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866817"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mapeando suas metas de implantação para um design do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Depois de terminar de revisar os serviços de Federação do Active Directory existente \(do AD FS\) metas de implantação e determinar quais metas estão relacionadas à sua implantação, você pode mapear essas metas para um design específico do AD FS. Para obter mais informações sobre o AD FS predefinidas metas de implantação, consulte [identificando as metas de implantação do AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).  
  
Use a tabela a seguir para determinar qual design do AD FS é mapeado para a combinação apropriada de AD FS, as metas de implantação para sua organização. Esta tabela refere-se somente para os dois designs do AD FS primários, conforme descrito neste guia. No entanto, você pode criar um híbrido ou design personalizado do AD FS, usando qualquer combinação das metas de implantação do AD FS para atender às necessidades da sua organização.  
  
|Meta de implantação do AD FS|[Design SSO da Web](Web-SSO-Design.md)|[Design de SSO da Web federado](Federated-Web-SSO-Design.md)|  
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|  
|[Forneça o acesso de usuários do Active Directory para seus serviços e aplicativos com reconhecimento de declarações](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Não|Sim, no parceiro de conta|  
|[Forneça o acesso de usuários do Active Directory para os aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|Não|Sim, opcional no parceiro de conta|  
|[Fornecer aos usuários de outra organização acesso a seus aplicativos com reconhecimento de declarações e serviços](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Sim|Sim|  

## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
  

