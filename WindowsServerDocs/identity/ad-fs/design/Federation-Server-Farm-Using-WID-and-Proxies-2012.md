---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: "Usando o trabalho e Proxies Farm do servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4bd815daccdd72a8c612b9b728ce12378c1926e7
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Usando o trabalho e Proxies Farm do servidor de Federação

>Aplica-se a: Windows Server 2012

Essa topologia de implantação para serviços de Federação do Active Directory \(AD FS\) é idêntica ao farm do servidor de federação com topologia \(WID\) banco de dados interno do Windows, mas adiciona proxies de servidor de Federação à rede perímetro para dar suporte a usuários externos. Os proxies de servidor de Federação redirecionar solicitações de autenticação de cliente que vêm de fora da rede corporativa para farm do servidor de Federação.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que são associadas essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   As organizações com 100 ou menos relações de confiança configurado que precisam fornecer a seus usuários internos e usuários externos \ (que estão conectadas a computadores que estão localizados fisicamente fora a rede \ corporativa) único sign\ em \(SSO\) acesso a serviços ou aplicativos federados  
  
-   Organizações que precisam fornecer seus usuários internos e usuários externos com acesso de logon único para o Microsoft Office 365  
  
-   Organizações menores que têm usuários externos e exigem serviços redundantes, escalonáveis  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são as vantagens de usar essa topologia?  
  
-   Os mesmos benefícios conforme listado para o [federação Server Farm usando trabalho](Federation-Server-Farm-Using-WID-2012.md) topologia, além do benefício de fornecer acesso adicional para usuários externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações de usar essa topologia?  
  
-   As mesmas limitações listados para o [federação Server Farm usando trabalho](Federation-Server-Farm-Using-WID-2012.md) topologia  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento de servidor  
Para implantar essa topologia, além de adicionar dois proxies de servidor de federação, você deve garantir que sua rede perímetro também pode fornecer acesso a um servidor Domain Name System \(DNS\) e um segundo host \(NLB\) balanceamento de carga de rede. O segundo host NLB deve ser configurado com um cluster NLB que usa um endereço IP de cluster Internet\ acessíveis, e ele deve usar a mesma configuração de nome do cluster DNS do cluster NLB anterior que você configurou no \(fs.fabrikam.com\) a rede corporativa. Os proxies de servidor de Federação também devem ser configurados com endereços IP Internet\ acessível.  
  
A ilustração a seguir mostra o farm de servidores de Federação existentes com topologia de trabalho que foi descrita anteriormente e como empresa fictícia Fabrikam, Inc., fornece acesso a um servidor DNS perímetro, adiciona um segundo host NLB com o mesmo cluster DNS nome \(fs.fabrikam.com\) e adiciona dois proxies de servidor de Federação \(fsp1 and fsp2\) à rede perímetro.  
  
![Farm de servidores usando o trabalho](media/FarmWIDProxies.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies de servidor de federação, consulte [requisitos de resolução de nome para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md) ou [requisitos de resolução de nome de Proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
