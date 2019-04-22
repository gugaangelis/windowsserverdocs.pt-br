---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: Determinar sua estratégia de aplicativo federado no parceiro de recurso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aca47658cc5a20f63dbd59a26ebe135dd04def92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811927"
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Determinar sua estratégia de aplicativo federado no parceiro de recurso

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma parte importante da criação de um novo serviços de Federação do Active Directory \(do AD FS\) infra-estrutura na organização do parceiro de recurso é determinar o conjunto completo de aplicativos e serviços que serão usados para participar de Federação e quais parceiros de conta serão os destinatários desses recursos. Antes de criar uma estratégia de serviços e aplicativo federado, considere as seguintes perguntas:  
  
-   Será habilitará e implantará um aplicativo ASP.NET ou um Windows Communication Foundation \(WCF\) serviço de Federação?  
  
-   Os usuários em sua rede corporativa precisarão de acesso ao serviço ou aplicativo federado por meio da Autenticação integrada do Windows?  
  
-   O serviço ou aplicativo federado será usado pelos usuários na sua rede de perímetro? Em caso afirmativo, a Autenticação Integrada do Windows será necessária?  
  
-   São todos os servidores Web que hospedam aplicativos federados estão executando um sistema operacional Windows Server e serviços de informações da Internet \(IIS\)?  
  
-   Para quem o serviço ou aplicativo federado fornecerá recursos?  
  
Respondendo a essas perguntas ajudará você a planejar um design sólido do AD FS. Também ajudará você na criação de uma estratégia de serviços e aplicativo federado que seja econômica e eficiente em termos de recursos. Para obter mais informações sobre como criar a estratégia de serviços e aplicativo federado apropriada para sua organização, consulte os tópicos a seguir neste guia:  
  
-   [Forneça o acesso de usuários do Active Directory para seus serviços e aplicativos com reconhecimento de declarações](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Forneça o acesso de usuários do Active Directory para os aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [Fornecer aos usuários de outra organização acesso a seus aplicativos com reconhecimento de declarações e serviços](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obter mais informações sobre como criar um declarações\-aplicativo com reconhecimento de ASP.NET ou serviço do WCF, consulte [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

