---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guia de Design do AD FS no Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: e8675c255032bca4623a9649bfc0bcca478008e3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854889"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guia de design de AD FS no Windows Server 

Serviços de Federação do Active Directory (AD FS) \(AD FS\) fornece Federação de identidades simplificada e segura e\-de logon único da Web em \(recursos de\) de SSO para usuários finais que desejam acessar aplicativos em uma AD FS\-segura, em organizações parceiras de Federação ou na nuvem.  
  
No Windows Server&reg; 2012 R2, o AD FS inclui um serviço de função de serviço de Federação que atua como um provedor de identidade \(autentica os usuários para fornecer tokens de segurança aos aplicativos que confiam AD FS\) ou como um provedor de Federação \(consome tokens de outros provedores de identidade e, em seguida, fornece tokens de segurança para aplicativos que confiam AD FS\).  
  
A função de fornecer acesso à extranet para aplicativos e serviços que são protegidos pelo AD FS no Windows Server 2012 R2 agora é executada por um novo serviço de função de Acesso Remoto chamado Proxy de Aplicativo Web. Essa é uma diferenciação das versões anteriores do Windows Server, em que essa função era manipulada por um proxy do servidor de federação do AD FS. O proxy de aplicativo Web é uma função de servidor criada para fornecer acesso para o AD FS\-cenário de extranet relacionado e outros cenários de extranet. Para obter mais informações sobre o proxy de aplicativo Web, consulte [guia passo a passos do proxy de aplicativo Web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>Sobre este guia  
Este guia fornece recomendações para ajudá-lo a planejar uma nova implantação do AD FS, com base nos requisitos da sua organização. Este guia destina-se ao uso por um especialista em infraestrutura ou arquiteto de sistema. Ele realça os principais pontos de decisão conforme você planeja sua implantação de AD FS. Antes de ler este guia, você deve ter uma boa compreensão de como AD FS funciona em um nível funcional. Para obter mais informações, consulte [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Identificar as metas de implantação do AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisitos do AD FS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Consulte também  
[Design do AD FS](../../ad-fs/AD-FS-Design.md)  
  

