---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: Determinar sua estratégia de aplicativo federado no parceiro de recurso
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: f3600b51dc78a7b11c9531daad4ad4b7c7e5f2a2
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942770"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Determinar sua estratégia de aplicativo federado no parceiro de recurso

Uma parte importante da criação de uma nova Serviços de Federação do Active Directory (AD FS) \( AD FS \) infraestrutura na organização do parceiro de recursos é determinar o conjunto completo de aplicativos e serviços que serão usados para participar da Federação e quais parceiros de conta serão os destinatários desses recursos. Antes de criar uma estratégia de serviços e aplicativo federado, considere as seguintes perguntas:

-   Você estará Habilitando e implantando um aplicativo ASP.NET ou um \( serviço WCF Windows Communication Foundation \) para Federação?

-   Os usuários em sua rede corporativa precisarão de acesso ao serviço ou aplicativo federado por meio da Autenticação integrada do Windows?

-   O serviço ou aplicativo federado será usado pelos usuários na sua rede de perímetro? Em caso afirmativo, a Autenticação Integrada do Windows será necessária?

-   Todos os servidores Web que hospedam aplicativos federados que executam um sistema operacional Windows Server e Serviços de Informações da Internet o \( IIS \) ?

-   Para quem o serviço ou aplicativo federado fornecerá recursos?

Responder a essas perguntas ajudará você a planejar um design de AD FS sólido. Também ajudará você na criação de uma estratégia de serviços e aplicativo federado que seja econômica e eficiente em termos de recursos. Para obter mais informações sobre como criar a estratégia de serviços e aplicativo federado apropriada para sua organização, consulte os tópicos a seguir neste guia:

-   [Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços com reconhecimento de declarações](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)

-   [Fornecer a seus usuários do Active Directory acesso a aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)

-   [Fornecer a usuários de outra organização acesso a seus aplicativos e serviços com reconhecimento de declarações](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)

Para obter mais informações sobre como criar um \- aplicativo ASP.NET com reconhecimento de declarações ou um serviço WCF, consulte [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

