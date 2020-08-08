---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: Planejar a implantação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 9ada8872c7d74e4a0a10504ffaf34a235536dcbb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954312"
---
# <a name="planning-your-deployment"></a>Planejar a implantação

Ao planejar a \- \( \- colaboração baseada em Federação entre organizações \) usando serviços de Federação do Active Directory (AD FS) \( AD FS \) , primeiro determine se a sua organização hospedará um recurso da Web para ser acessado por outras organizações pela Internet ou se você fornecerá acesso ao recurso da Web para funcionários em sua organização. Essa determinação afeta a maneira como você implanta AD FS e é fundamental no planejamento de sua infraestrutura de AD FS.

> [!NOTE]
> Certifique-se de que a função que a organização desempenha no contrato de federação é claramente compreendida por todas as partes.

Para o [design de SSO da Web federado](Federated-Web-SSO-Design.md), AD FS usa termos como *parceiro de conta* \( também conhecido como *provedor de identidade* no snap-in de gerenciamento de AD FS \- \) e parceiro de *recurso* \( também conhecido como *terceira parte confiável* no snap-in de gerenciamento de AD FS \- \) para ajudar a diferenciar a organização que hospeda as contas \( do parceiro de conta \) da organização que hospeda os \- recursos baseados na Web \( do parceiro de recurso \) .

Em [Web SSO Design](Web-SSO-Design.md), a organização atua em ambas as funções de parceiro de conta e parceiro de recurso porque ela fornece aos usuários acesso aos seus aplicativos.

Os tópicos a seguir explicam alguns dos conceitos da organização de parceiros AD FS. Eles também contêm links para tópicos no guia de implantação de AD FS que contêm informações sobre como configurar e configurar organizações de parceiros de conta e organizações de parceiros de recursos com base em suas metas de implantação de AD FS.

## <a name="in-this-section"></a>Nesta seção

-   [Melhores práticas para o planejamento e a implantação seguros do AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)

-   [Planejamento para interoperabilidade com o AD FS 1.x](Planning-for-Interoperability-with-AD-FS-1.x.md)

-   [Quando usar a delegação de identidade](When-to-Use-Identity-Delegation.md)

-   [Como implantar o AD FS na organização do parceiro de conta](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)

-   [Como implantar o AD FS na organização do parceiro de recurso](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


