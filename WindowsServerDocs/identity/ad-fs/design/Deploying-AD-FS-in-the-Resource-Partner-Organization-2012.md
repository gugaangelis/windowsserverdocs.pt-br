---
ms.assetid: 39acccd9-0402-49ca-8ce1-b239e1e7e455
title: Implantando o AD FS na organização do parceiro de recurso
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: ae43da6a8aa2968cc5123c9bc707c44c77fa6ea2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942911"
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Implantando o AD FS na organização do parceiro de recurso

A organização do parceiro de recurso no Serviços de Federação do Active Directory (AD FS) \( AD FS \) representa a organização cujos servidores Web podem ser protegidos por um \- servidor de Federação do lado do recurso. O servidor de Federação no parceiro de recurso usa os tokens de segurança que são produzidos pelo parceiro de conta para fornecer declarações aos servidores Web que estão localizados no parceiro de recurso.

Em cenários em que você precisa fornecer acesso a serviços federados ou aplicativos a muitos usuários diferentes — quando alguns usuários residem em diferentes organizações — você pode configurar o servidor de Federação de recursos para que possa implantar vários parceiros de conta.

Para obter mais informações sobre como instalar e configurar a organização do parceiro de recurso, consulte [Checklist: Configuring the Resource Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).

## <a name="in-this-section"></a>Nesta seção

-   [Analisar a função do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)

-   [Analisar a função do proxy do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)

-   [Determinar sua estratégia de aplicativo federado no parceiro de recurso](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)


## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
