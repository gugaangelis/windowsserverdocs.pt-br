---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guia de Design do AD FS no Windows Server 2012 R2
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2d15f680f28c54da75100a03f7b85e880442d9be
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191743"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guia de Design do AD FS no Windows Server 

Serviços de Federação do Active Directory \(do AD FS\) fornece logon único da Web e federação de identidade simplificada e segura\-na \(SSO\) recursos para os usuários finais que desejam acessar aplicativos dentro de um AD FS\-protegidos da empresa, em organizações de parceiros de federação, ou na nuvem.  
  
No Windows Server® 2012 R2, o AD FS inclui um serviço de função serviço de federação que atua como um provedor de identidade \(autentica usuários para fornecer tokens de segurança para aplicativos que confiam no AD FS\) ou como um provedor de Federação \( consome tokens de outros provedores de identidade e, em seguida, fornece tokens de segurança para aplicativos que confiam no AD FS\).  
  
A função de fornecer acesso à extranet para aplicativos e serviços que são protegidos pelo AD FS no Windows Server 2012 R2 agora é executada por um novo serviço de função de Acesso Remoto chamado Proxy de Aplicativo Web. Essa é uma diferenciação das versões anteriores do Windows Server, em que essa função era manipulada por um proxy do servidor de federação do AD FS. Proxy de aplicativo Web é uma função de servidor projetada para fornecer acesso para o AD FS\-relacionados ao cenário de extranet e outros cenários de extranet. Para obter mais informações sobre o Proxy de aplicativo Web, consulte [guia de instruções passo a passo de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>Sobre este guia  
Este guia fornece recomendações para ajudá-lo a planejar uma nova implantação do AD FS, com base nos requisitos da sua organização. Este guia destina-se ao uso por um especialista em infraestrutura ou arquiteto de sistema. Ele destaca seus principais pontos de decisão ao planejar sua implantação do AD FS. Antes de ler este guia, você deve ter uma boa compreensão do funcionamento do AD FS em um nível funcional. Para obter mais informações, consulte [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Identificar as metas de implantação do AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisitos do AD FS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Consulte também  
[Design do AD FS](../../ad-fs/AD-FS-Design.md)  
  

