---
ms.assetid: 9eab8c43-a0f2-4d19-a5a4-e1399f0d5f25
title: "Determinar sua estratégia de aplicativo federado no parceiro de recurso"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: aca47658cc5a20f63dbd59a26ebe135dd04def92
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="determine-your-federated-application-strategy-in-the-resource-partner"></a>Determinar sua estratégia de aplicativo federado no parceiro de recurso

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Uma parte importante da projetar uma nova infraestrutura de \(AD FS\) de serviços de Federação do Active Directory na organização do parceiro de recurso é determinar o conjunto completo de aplicativos e serviços que será usado para participar na federação e parceiros qual conta será os destinatários desses recursos. Antes de criar um aplicativo federado e estratégia de serviços, considere as seguintes perguntas:  
  
-   Você habilitando e implantando um aplicativo ASP.NET ou um serviço Windows Communication Foundation \(WCF\) para federação?  
  
-   Os usuários em sua rede corporativa exigirá o acesso ao serviço por meio de autenticação integrada do Windows ou aplicativo federado?  
  
-   O aplicativo federado ou serviço ser usado pelos usuários em sua rede perímetro? Em caso afirmativo, autenticação integrada do Windows será necessária?  
  
-   São todos os servidores Web que executam um sistema operacional Windows Server e o Internet Information Services \(IIS\) os aplicativos host federado?  
  
-   Quem o aplicativo federado ou o serviço fornece recursos para?  
  
Responder a essas perguntas ajudarão a planejar um design AD FS sólido. Ela também ajudará você na criação de um aplicativo federado e estratégia de serviços que é bom custo-benefício e recursos eficientes. Para obter mais informações sobre o design do aplicativo federado mais apropriado e estratégia de serviços para sua organização, consulte os tópicos a seguir neste guia:  
  
-   [Fornecer seu do Active Directory aos usuários acesso aos seus aplicativos com reconhecimento de declarações e serviços](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
-   [Fornecer o acesso de usuários do Active Directory para os aplicativos e serviços de outras organizações](Provide-Your-Active-Directory-Users-Access-to-the-Applications-and-Services-of-Other-Organizations.md)  
  
-   [Fornecer aos usuários em outra organização acesso aos seus aplicativos com reconhecimento de declarações e serviços](Provide-Users-in-Another-Organization-Access-to-Your-Claims-Aware-Applications-and-Services.md)  
  
Para obter mais informações sobre como criar um aplicativo de reconhecimento de claims\ ASP.NET ou serviço WCF, consulte [Windows Identity Foundation SDK](https://go.microsoft.com/fwlink/?LinkId=122266).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)

