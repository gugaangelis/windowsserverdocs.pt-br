---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: Planejando sua implantação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 607dc34c8f44d8d96a8dc0c9d1ed004edc799167
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407995"
---
# <a name="planning-your-deployment"></a>Planejando sua implantação

Ao planejar a colaboração entre @ no__t-0organizational \(federation @ no__t-2based @ no__t-3 usando Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-5, primeiro determine se a sua organização hospedará um recurso da Web a ser acessado por outros organizações na Internet ou se você fornecer acesso ao recurso da Web para funcionários em sua organização. Essa determinação afeta a maneira como você implanta AD FS e é fundamental no planejamento de sua infraestrutura de AD FS.  
  
> [!NOTE]  
> Certifique-se de que a função que a organização desempenha no contrato de federação é claramente compreendida por todas as partes.  
  
Para o [design de SSO da Web federado](Federated-Web-SSO-Design.md), o AD FS usa termos como *parceiro de conta* \(also referido como *provedor de identidade* no snap-in de AD FS Management @ no__t-4in @ no__t-5 e parceiro de *recurso* \(also referido como  *terceira parte confiável* no snap do AD FS Management @ no__t-9in @ no__t-10 para ajudar a diferenciar a organização que hospeda as contas 1o parceiro de conta @ no__t-12 da organização que hospeda os recursos Web @ no__t-13based 4 recursos parceiro @ no__t-15.  
  
Em [Web SSO Design](Web-SSO-Design.md), a organização atua em ambas as funções de parceiro de conta e parceiro de recurso porque ela fornece aos usuários acesso aos seus aplicativos.  
  
Os tópicos a seguir explicam alguns dos conceitos da organização de parceiros AD FS. Eles também contêm links para tópicos no guia de implantação de AD FS que contêm informações sobre como configurar e configurar organizações de parceiros de conta e organizações de parceiros de recursos com base em suas metas de implantação de AD FS.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Melhores práticas para o planejamento e a implantação seguros do AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Planejamento para interoperabilidade com o AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Quando usar a delegação de identidade](When-to-Use-Identity-Delegation.md)  
  
-   [Como implantar o AD FS na organização do parceiro de conta](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Como implantar o AD FS na organização do parceiro de recurso](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


