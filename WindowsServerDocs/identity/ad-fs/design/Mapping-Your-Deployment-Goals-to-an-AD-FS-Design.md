---
ms.assetid: 68979914-8a1c-465a-bd37-08df30722d69
title: Mapeando suas metas de implantação para um design do AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 730ece1cfc345334e39a018cda3b49b92247f413
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945264"
---
# <a name="mapping-your-deployment-goals-to-an-ad-fs-design"></a>Mapeando suas metas de implantação para um design do AD FS


Depois de concluir a revisão do Serviços de Federação do Active Directory (AD FS) existente \( AD FS \) metas de implantação e determinar quais metas estão relacionadas à sua implantação, você pode mapear essas metas para um design de AD FS específico. Para obter mais informações sobre AD FS metas de implantação predefinidas, consulte [identificando suas metas de implantação de AD FS](Identifying-Your-AD-FS-Deployment-Goals.md).

Use a tabela a seguir para determinar qual AD FS design mapeia para a combinação apropriada de AD FS objetivos de implantação para sua organização. Esta tabela refere-se apenas aos dois designs de AD FS primários, conforme descrito neste guia. No entanto, você pode criar um design de AD FS híbrido ou personalizado usando qualquer combinação das metas de implantação AD FS para atender às necessidades da sua organização.

|Meta de implantação AD FS|[Design SSO da Web](Web-SSO-Design.md)|[Design SSO da Web federado](Federated-Web-SSO-Design.md)|
|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
|[Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços com reconhecimento de declarações](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Não|Sim, no parceiro de conta|
|[Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)|Não|Sim, opcional no parceiro de conta|
|[Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)|Sim|Sim|

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


