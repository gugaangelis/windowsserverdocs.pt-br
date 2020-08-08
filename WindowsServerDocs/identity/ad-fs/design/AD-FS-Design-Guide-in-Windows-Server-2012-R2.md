---
ms.assetid: a8558c9d-0606-4881-93b2-f2d2716b18e7
title: Guia de Design do AD FS no Windows Server 2012 R2
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 6ec9826ce2015197d96a182864807646a6b8115d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940402"
---
# <a name="ad-fs-design-guide-in-windows-server"></a>Guia de design de AD FS no Windows Server

\(O Serviços de Federação do Active Directory (AD FS) AD FS \) fornece recursos de SSO de logon único e de Federação de identidades simplificados e seguros \- \( \) para usuários finais que desejam acessar aplicativos em uma \- empresa AD FS protegida, em organizações parceiras de Federação ou na nuvem.

No Windows Server &reg; 2012 R2, o AD FS inclui um serviço de função de serviço de Federação que age como um provedor de identidade \( autentica os usuários para fornecer tokens de segurança a aplicativos que confiam AD FS \) ou como um provedor de Federação \( consome tokens de outros provedores de identidade e, em seguida, fornece tokens de segurança para aplicativos que confiam AD FS \) .

A função de fornecer acesso à extranet para aplicativos e serviços que são protegidos pelo AD FS no Windows Server 2012 R2 agora é executada por um novo serviço de função de Acesso Remoto chamado Proxy de Aplicativo Web. Essa é uma diferenciação das versões anteriores do Windows Server, em que essa função era manipulada por um proxy do servidor de federação do AD FS. O proxy de aplicativo Web é uma função de servidor criada para fornecer acesso para o AD FS \- cenário de extranet relacionado e outros cenários de extranet. Para obter mais informações sobre o proxy de aplicativo Web, consulte [guia passo a passos do proxy de aplicativo Web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn280944(v=ws.11)).

## <a name="about-this-guide"></a>Sobre este guia
Este guia fornece recomendações para ajudá-lo a planejar uma nova implantação do AD FS, com base nos requisitos da sua organização. Este guia destina-se ao uso por um especialista em infraestrutura ou arquiteto de sistema. Ele realça os principais pontos de decisão conforme você planeja sua implantação de AD FS. Antes de ler este guia, você deve ter uma boa compreensão de como AD FS funciona em um nível funcional. Para obter mais informações, consulte [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).

## <a name="in-this-guide"></a>Neste guia

-   [Identificar as metas de implantação do AD FS](Identify-Your-AD-FS-Deployment-Goals.md)

-   [Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)

-   [Requisitos do AD FS](AD-FS-Requirements.md)


## <a name="see-also"></a>Consulte Também
[Design do AD FS](../../ad-fs/AD-FS-Design.md)

