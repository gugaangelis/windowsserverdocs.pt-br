---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Farm de servidores de federação usando WID e proxies
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 9c6dba880b80de43bb713d1b4495f0e03d56a695
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853089"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Farm de servidores de federação usando WID e proxies

Essa topologia de implantação para Serviços de Federação do Active Directory (AD FS) \(AD FS\) é idêntica ao farm de servidores de Federação com o banco de dados interno do Windows \(a topologia\) WID, mas adiciona proxies de servidor de Federação à rede de perímetro para dar suporte a usuários externos. Os proxies de servidor de Federação redirecionam as solicitações de autenticação de cliente que vêm de fora de sua rede corporativa para o farm de servidores de Federação.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve as várias considerações sobre o público-alvo, os benefícios e as limitações associados a essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   Organizações com 100 ou menos relações de confiança configuradas que precisam fornecer os usuários internos e usuários externos \(que estão conectados a computadores que estão localizados fisicamente fora da rede corporativa\) com o\-de logon único no \(SSO\) acesso a aplicativos ou serviços federados  
  
-   Organizações que precisam fornecer os usuários internos e usuários externos com acesso de SSO a Microsoft Office 365  
  
-   Organizações menores que têm usuários externos e exigem serviços redundantes e escalonáveis  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Os mesmos benefícios listados para o [farm de servidores de Federação usando](Federation-Server-Farm-Using-WID-2012.md) a topologia wid, além do benefício de fornecer acesso adicional para usuários externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   As mesmas limitações listadas para o [farm de servidores de Federação usando](Federation-Server-Farm-Using-WID-2012.md) a topologia wid  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento do servidor  
Para implantar essa topologia, além de adicionar dois proxies de servidor de Federação, você deve verificar se a rede de perímetro também pode fornecer acesso a um sistema de nomes de domínio \(servidor de\) DNS e a um segundo balanceamento de carga de rede \(host\) NLB. O segundo host NLB deve ser configurado com um cluster NLB que usa um endereço IP de cluster acessível pela Internet\-e deve usar a mesma configuração de nome DNS do cluster que o cluster NLB anterior que você configurou na rede corporativa \(fs.fabrikam.com\). Os proxies do servidor de Federação também devem ser configurados com Internet\-endereços IP acessíveis.  
  
A ilustração a seguir mostra o farm de servidores de Federação existente com a topologia WID que foi descrita anteriormente e como a empresa fictícia Fabrikam, Inc., fornece acesso a um servidor DNS de perímetro, adiciona um segundo host NLB com o mesmo nome DNS de cluster \(fs.fabrikam.com\)e adiciona dois proxies de servidor de Federação \(fsp1 e fsp2\) à rede de perímetro.  
  
![farm de servidores usando WID](media/FarmWIDProxies.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies de servidor de Federação, consulte [requisitos de resolução de nomes para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md) ou [requisitos de resolução de nomes para proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
