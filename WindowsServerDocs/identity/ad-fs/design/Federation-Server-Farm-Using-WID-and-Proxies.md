---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: "Usando o trabalho e Proxies Farm do servidor de Federação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e372f066fc82b9857d438234b491732a177e24fa
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Usando o trabalho e Proxies Farm do servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Essa topologia de implantação para serviços de Federação do Active Directory \(AD FS\) é idêntica ao farm do servidor de federação com topologia \(WID\) banco de dados interno do Windows, mas ele adiciona proxy computadores à rede perímetro para dar suporte a usuários externos. Estes proxies redirecionar solicitações de autenticação de cliente que vêm de fora da rede corporativa para farm do servidor de Federação. Em versões anteriores do AD FS, estes proxies foram chamadas proxies de servidor de Federação.  
  
> [!IMPORTANT]  
> Em serviços de Federação do Active Directory \(AD FS\) no Windows Server 2012 R2, a função de um proxy de servidor de Federação é manipulada por um novo serviço de função de acesso remoto chamado Proxy de aplicativo Web. Para habilitar o AD FS para acessibilidade de fora da rede corporativa, foi a finalidade de implantação de um proxy de servidor de Federação em versões herdadas do AD FS, como o AD FS 2.0 e do AD FS no Windows Server 2012, você pode implantar um ou mais proxies de aplicativo web do AD FS no Windows Server 2012 R2.  
>   
> No contexto do AD FS, Proxy de aplicativo Web funciona como um proxy de servidor de Federação do AD FS. Além disso, o Proxy de aplicativo Web fornece funcionalidade de proxy inverso para aplicativos web dentro de sua rede corporativa permitir que os usuários em qualquer dispositivo para acessá-los de fora da rede corporativa. Para obter mais informações sobre o serviço de função de Proxy de aplicativo Web, consulte Visão geral de Proxy de aplicativo Web.  
>   
> Para planejar a implantação do proxy de aplicativo Web, você pode examinar as informações nos tópicos a seguir:  
>   
> -   [Planejar a infraestrutura de Proxy de aplicativo Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Planeje o servidor de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que são associadas essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   As organizações com 100 ou menos relações de confiança configurado que precisam fornecer a seus usuários internos e usuários externos \ (que estão conectadas a computadores que estão localizados fisicamente fora a rede \ corporativa) único sign\ em \(SSO\) acesso a serviços ou aplicativos federados  
  
-   Organizações que precisam fornecer seus usuários internos e usuários externos com acesso de logon único para o Microsoft Office 365  
  
-   Organizações menores que têm usuários externos e exigem serviços redundantes, escalonáveis  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são as vantagens de usar essa topologia?  
  
-   Os mesmos benefícios conforme listado para o [federação Server Farm usando trabalho](Federation-Server-Farm-Using-WID.md) topologia, além do benefício de fornecer acesso adicional para usuários externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações de usar essa topologia?  
  
-   As mesmas limitações listados para o [federação Server Farm usando trabalho](Federation-Server-Farm-Using-WID.md) topologia  

||1 \-100 relações de confiança RP|Mais de 100 relações de confiança RP 
| ----- |-----| ------ |
|1 \-30 AD FS nós|Trabalho com suporte|Não permitido usando trabalho \-SQL necessária 
|Mais de 30 AD FS nós|Não permitido usando trabalho \-SQL necessária|Não permitido usando trabalho \-SQL necessária  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento de servidor  
Para implantar essa topologia, além de adicionar dois proxies de aplicativo web, você deve garantir que sua rede perímetro também pode fornecer acesso a um servidor Domain Name System \(DNS\) e um segundo host \(NLB\) balanceamento de carga de rede. O segundo host NLB deve ser configurado com um cluster NLB que usa um endereço IP de cluster Internet\ acessíveis, e ele deve usar a mesma configuração de nome do cluster DNS do cluster NLB anterior que você configurou no \(fs.fabrikam.com\) a rede corporativa. Os proxies de aplicativo web também devem ser configurados com endereços IP Internet\ acessível.  
  
A ilustração a seguir mostra o farm de servidores de Federação existentes com topologia de trabalho que foi descrita anteriormente e como empresa fictícia Fabrikam, Inc., fornece acesso a um servidor DNS perímetro, adiciona um segundo host NLB com o mesmo cluster DNS nome \(fs.fabrikam.com\) e adiciona dois proxies de aplicativo web \(wap1 and wap2\) à rede perímetro.  
  
![Proxies e trabalho Farm](media/WIDFarmADFSBlue.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies de aplicativo da web, consulte "Requisitos de resolução de nome" seção [AD FS requisitos](AD-FS-Requirements.md) e [planejar a infraestrutura de Proxy de aplicativos Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Consulte também  
[Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guia de Design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

