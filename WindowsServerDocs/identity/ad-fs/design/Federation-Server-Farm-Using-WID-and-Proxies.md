---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: Farm de servidores de federação usando WID e proxies
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e372f066fc82b9857d438234b491732a177e24fa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860387"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Farm de servidores de federação usando WID e proxies

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Essa topologia de implantação para os serviços de Federação do Active Directory \(do AD FS\) é idêntico ao farm de servidores de federação com o banco de dados interno do Windows \(WID\) topologia, mas ele adiciona os computadores de proxy para o rede de perímetro para oferecer suporte a usuários externos. Esses proxies redirecionar as solicitações de autenticação de cliente que vêm de fora da rede corporativa para o farm de servidores de Federação. Nas versões anteriores do AD FS, esses proxies foram chamados proxies de servidor de Federação.  
  
> [!IMPORTANT]  
> Nos serviços de Federação do Active Directory \(do AD FS\) no Windows Server 2012 R2, a função de um proxy do servidor de Federação é tratada por um novo serviço de função acesso remoto chamado Proxy de aplicativo Web. Para habilitar o AD FS para acessibilidade de fora da rede corporativa, o que era o propósito de implantar um proxy do servidor de Federação em versões herdadas do AD FS, como o AD FS 2.0 e AD FS no Windows Server 2012, você pode implantar um ou mais proxies de aplicativo web para um D FS no Windows Server 2012 R2.  
>   
> No contexto do AD FS, Proxy de aplicativo Web funciona como um proxy do servidor de Federação do AD FS. Além disso, o Proxy de Aplicativo Web fornece funcionalidade de proxy reverso para aplicativos Web dentro da rede corporativa. Isso permite aos usuários acessá-los de qualquer dispositivo fora da rede corporativa. Para obter mais informações sobre o serviço da função Proxy de Aplicativo Web, consulte Visão geral de proxy de aplicativo Web.  
>   
> Para planejar a implantação do proxy de Aplicativo Web, é possível examinar as informações nos tópicos a seguir:  
>   
> -   [Planejar a infraestrutura de Proxy de aplicativo Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Planejar o servidor de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve várias considerações sobre o público-alvo, benefícios e limitações que estão associadas essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   As organizações com 100 ou menos relações de confiança configuradas que precisa fornecer a seus usuários internos e usuários externos \(que estão conectados a computadores que estão fisicamente localizados fora da rede corporativa\) com único sinal\-na \(SSO\) acesso a aplicativos federados ou serviços  
  
-   Organizações que precisam para fornecer a seus usuários internos e usuários externos acesso SSO para o Microsoft Office 365  
  
-   Organizações menores que os usuários externos e exigem serviços redundantes, escalonáveis  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Os mesmos beneficiam conforme listado para o [Federation Server Farm usando o WID](Federation-Server-Farm-Using-WID.md) topologia, além do benefício de fornecer acesso adicional para usuários externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   As mesmas limitações que listada para o [Federation Server Farm usando o WID](Federation-Server-Farm-Using-WID.md) topologia  

||1 \- 100 relações de confiança RP|Relações de confiança RP mais de 100 
| ----- |-----| ------ |
|1 \- 30 AD FS nós|Suportada de WID|Não há suportada usando o WID \- SQL é necessário 
|Mais de 30 AD FS nós|Não há suportada usando o WID \- SQL é necessário|Não há suportada usando o WID \- SQL é necessário  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de posicionamento e a rede de servidor  
Para implantar essa topologia, além de adicionar dois web proxies de aplicativo, certifique-se de que sua rede de perímetro também pode fornecer acesso a um sistema de nome de domínio \(DNS\) servidor e para uma segunda balanceamento de carga de rede \( O NLB\) host. O segundo host NLB deve ser configurado com um cluster NLB que usa um Internet\-endereço IP de cluster acessível e ele devem usar a mesma configuração de nome DNS de cluster do cluster NLB anterior configurado na rede corporativa \( FS.Fabrikam.com\). Os proxies de aplicativo web também devem ser configurados com a Internet\-endereços IP acessíveis.  
  
A ilustração a seguir mostra o farm de servidores de Federação existente com a topologia WID que foi descrita anteriormente e como a empresa fictícia Fabrikam, Inc., fornece acesso a um servidor DNS de perímetro, adiciona um segundo host NLB com o mesmo nome DNS de cluster \(fs.fabrikam.com\)e adiciona dois proxies do aplicativo web \(wap1 e wap2\) à rede de perímetro.  
  
![Farm de WID e proxies](media/WIDFarmADFSBlue.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies do aplicativo web, consulte "Requisitos de resolução de nome" seção [requisitos do AD FS](AD-FS-Requirements.md) e [plano da Web Infraestrutura do Proxy de aplicativo (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Consulte também  
[Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guia de Design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

