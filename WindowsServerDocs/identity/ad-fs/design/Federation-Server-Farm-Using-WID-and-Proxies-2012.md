---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Farm de servidores de federação usando WID e proxies
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 19e73e43a863ec60fbc9da09b24173220bb331ed
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191362"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Farm de servidores de federação usando WID e proxies

Essa topologia de implantação para os serviços de Federação do Active Directory \(do AD FS\) é idêntico ao farm de servidores de federação com o banco de dados interno do Windows \(WID\) topologia, mas ele adiciona os proxies de servidor de Federação para a rede de perímetro para oferecer suporte a usuários externos. Os proxies de servidor de Federação redirecionar as solicitações de autenticação de cliente que vêm de fora da rede corporativa para o farm de servidores de Federação.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que estão associadas essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   As organizações com 100 ou menos relações de confiança configuradas que precisa fornecer a seus usuários internos e usuários externos \(que estão conectados a computadores que estão fisicamente localizados fora da rede corporativa\) com único sinal\-na \(SSO\) acesso a aplicativos federados ou serviços  
  
-   Organizações que precisam para fornecer a seus usuários internos e usuários externos acesso SSO para o Microsoft Office 365  
  
-   Organizações menores que os usuários externos e exigem serviços redundantes, escalonáveis  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Os mesmos beneficiam conforme listado para o [Federation Server Farm usando o WID](Federation-Server-Farm-Using-WID-2012.md) topologia, além do benefício de fornecer acesso adicional para usuários externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   As mesmas limitações que listada para o [Federation Server Farm usando o WID](Federation-Server-Farm-Using-WID-2012.md) topologia  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de posicionamento e a rede de servidor  
Para implantar essa topologia, além de adicionar dois proxies de servidor de federação, certifique-se de que sua rede de perímetro também pode fornecer acesso a um sistema de nome de domínio \(DNS\) server e para uma segunda Network Load Balancing \(NLB\) host. O segundo host NLB deve ser configurado com um cluster NLB que usa um Internet\-endereço IP de cluster acessível e ele devem usar a mesma configuração de nome DNS de cluster do cluster NLB anterior configurado na rede corporativa \( FS.Fabrikam.com\). Os proxies de servidor de Federação também devem ser configurados com a Internet\-endereços IP acessíveis.  
  
A ilustração a seguir mostra o farm de servidores de Federação existente com a topologia WID que foi descrita anteriormente e como a empresa fictícia Fabrikam, Inc., fornece acesso a um servidor DNS de perímetro, adiciona um segundo host NLB com o mesmo nome DNS de cluster \(fs.fabrikam.com\), e adiciona dois proxies de servidor de Federação \(fsp1 e fsp2\) à rede de perímetro.  
  
![farm de servidores usando o WID](media/FarmWIDProxies.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies do servidor de federação, consulte [requisitos de resolução de nome para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md) ou [nome Requisitos de resolução para Proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
