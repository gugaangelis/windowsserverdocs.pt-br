---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: Planejando sua implantação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5c7cec9ad92605f3dc98f8ce8fb7853a7ae61299
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863687"
---
# <a name="planning-your-deployment"></a>Planejando sua implantação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ao planejar cross\-organizacionais \(federação\-com base\) colaboração usando serviços de Federação do Active Directory \(AD FS\), primeiro determine se sua organização hospedará um recurso da Web para ser acessado por outras organizações na Internet ou se você for fornecer acesso ao recurso da Web para os funcionários em sua organização. Essa determinação afeta como implantar o AD FS, e é fundamental no planejamento de infraestrutura do AD FS.  
  
> [!NOTE]  
> Certifique-se de que a função que a organização desempenha no contrato de federação é claramente compreendida por todas as partes.  
  
Para o [Federated Web SSO Design](Federated-Web-SSO-Design.md), o AD FS usa termos como *parceiro de conta* \(também conhecido como *provedor de identidade* no snap do gerenciamento do AD FS\-na\) e *parceiro de recurso* \(também conhecido como *terceira* no snap do gerenciamento do AD FS\-em\) para ajudar a diferenciar a organização que hospeda as contas \(o parceiro de conta\) da organização que hospeda a Web\-com base em recursos \(o parceiro de recurso\).  
  
Em [Web SSO Design](Web-SSO-Design.md), a organização atua em ambas as funções de parceiro de conta e parceiro de recurso porque ela fornece aos usuários acesso aos seus aplicativos.  
  
Os tópicos a seguir explicam que alguns dos AD FS conceitos da organização do parceiro. Elas também contêm links para tópicos que contêm informações sobre como instalar e configurar organizações de parceiros de conta e organizações do parceiro de recurso com base em suas metas de implantação do AD FS no guia de implantação do AD FS.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Práticas recomendadas para o seguro de planejamento e implantação do AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Planejamento para interoperabilidade com o AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Quando usar a delegação de identidade](When-to-Use-Identity-Delegation.md)  
  
-   [Implantar o AD FS na organização do parceiro de conta](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Implantar o AD FS na organização do parceiro de recurso](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


