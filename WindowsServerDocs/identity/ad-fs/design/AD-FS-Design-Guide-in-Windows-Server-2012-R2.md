---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guia de Design do AD FS no Windows Server 2012 R2
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 498b399818fb8c9e463f9990fa13c87648c0a33d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="ad-fs-design-guide-in-windows-server-2012-r2"></a>Guia de Design do AD FS no Windows Server 2012 R2

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Active Directory serviços de Federação \(AD FS\) fornece federação de identidade simplificada, seguros e Web única sign\ \(SSO\) recursos para os usuários finais que quiserem acessar aplicativos dentro de uma empresa de AD FS\ protegido, em organizações de parceiros de Federação ou na nuvem.  
  
No Windows Server® 2012 R2, o AD FS inclui um serviço de função de serviço de federação que atua como um provedor de identidade \ (autentica usuários para fornecer os tokens de segurança para aplicativos que confia FS\ AD) ou como um provedor de Federação \ (consome tokens de outros provedores de identidade e, em seguida, fornece tokens de segurança para aplicativos que confia FS\ AD).  
  
A função de fornecer extranet acesso aos aplicativos e serviços que são protegidos pelo AD FS no Windows Server 2012 R2 agora é realizada por um novo serviço de função de acesso remoto chamado Proxy de aplicativo Web. Essa é uma divergência das versões anteriores do Windows Server em que essa função foi manipulada por um proxy de servidor de Federação do AD FS. Proxy de aplicativo Web é uma função de servidor projetada para fornecer acesso para o cenário de extranet relacionados à publicidade FS\ e outros cenários extranet. Para obter mais informações sobre o Proxy de aplicativo Web, consulte [guia passo a passo do Web Application Proxy](https://technet.microsoft.com/library/dn280944.aspx).  
  
## <a name="about-this-guide"></a>Sobre este guia  
Este guia fornece recomendações para ajudá-lo a planejar uma nova implantação do AD FS, com base nos requisitos da sua organização. Este guia destina-se ao uso por um engenheiro de especialista ou sistema de infraestrutura. Ele destaca os pontos de decisão principal ao planejar sua implantação do AD FS. Antes de ler este guia, você deve ter uma boa compreensão de como o AD FS funciona em um nível funcional. Para obter mais informações, consulte [conceitos-chave de Noções básicas sobre AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
## <a name="in-this-guide"></a>Neste guia  
  
-   [Identificar as metas de implantação do AD FS](Identify-Your-AD-FS-Deployment-Goals.md)  
  
-   [Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
  
-   [Requisitos do AD FS](AD-FS-Requirements.md)  
  
  
## <a name="see-also"></a>Consulte também  
[AD FS Design](../../ad-fs/AD-FS-Design.md)  
  

