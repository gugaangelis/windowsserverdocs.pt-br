---
ms.assetid: f0464182-56a2-4bfa-a8c8-7e39c1bd62d3
title: Farm de servidores de federação usando WID e proxies
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6a123afaebba002b8ee4fb98d5cee5aded286a96
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359126"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Farm de servidores de federação usando WID e proxies

Essa topologia de implantação para Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 é idêntica ao farm de servidores de Federação com a topologia do banco de dados interno do Windows \(WID @ no__t-3, mas adiciona computadores proxy à rede de perímetro para dar suporte usuários externos. Esses proxies redirecionam as solicitações de autenticação de cliente que vêm de fora de sua rede corporativa para o farm de servidores de Federação. Nas versões anteriores do AD FS, esses proxies eram chamados de proxies de servidor de Federação.  
  
> [!IMPORTANT]  
> No Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 no Windows Server 2012 R2, a função de um proxy de servidor de Federação é tratada por um novo serviço de função de acesso remoto chamado proxy de aplicativo Web. Para habilitar sua AD FS para acessibilidade de fora da rede corporativa, que foi a finalidade de implantar um proxy de servidor de Federação em versões herdadas do AD FS, como AD FS 2,0 e AD FS no Windows Server 2012, você pode implantar um ou mais proxies de aplicativo Web para um D FS no Windows Server 2012 R2.  
>   
> No contexto de AD FS, o proxy de aplicativo Web funciona como um proxy de servidor de Federação AD FS. Além disso, o Proxy de Aplicativo Web fornece funcionalidade de proxy reverso para aplicativos Web dentro da rede corporativa. Isso permite aos usuários acessá-los de qualquer dispositivo fora da rede corporativa. Para obter mais informações sobre o serviço da função Proxy de Aplicativo Web, consulte Visão geral de proxy de aplicativo Web.  
>   
> Para planejar a implantação do proxy de Aplicativo Web, é possível examinar as informações nos tópicos a seguir:  
>   
> -   [Planejar a WAP (infraestrutura de proxy de aplicativo Web)](https://technet.microsoft.com/library/dn383648.aspx)  
> -   [Planejar o servidor proxy de aplicativo Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve as várias considerações sobre o público-alvo, os benefícios e as limitações associados a essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   Organizações com 100 ou menos relações de confiança configuradas que precisam fornecer os usuários internos e externos \(who estão conectados a computadores que estão fisicamente localizados fora da rede corporativa @ no__t-1 com Sign-2on único. \(SSO @ no__t-4 acesso a aplicativos ou serviços federados  
  
-   Organizações que precisam fornecer os usuários internos e usuários externos com acesso de SSO a Microsoft Office 365  
  
-   Organizações menores que têm usuários externos e exigem serviços redundantes e escalonáveis  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Os mesmos benefícios listados para o [farm de servidores de Federação usando](Federation-Server-Farm-Using-WID.md) a topologia wid, além do benefício de fornecer acesso adicional para usuários externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   As mesmas limitações listadas para o [farm de servidores de Federação usando](Federation-Server-Farm-Using-WID.md) a topologia wid  

||1 \- a 100 de confianças RP|Mais de 100 RP relações de confiança 
| ----- |-----| ------ |
|1 \- 30 nós AD FS|WID com suporte|Sem suporte usando o \- SQL wid necessário 
|Mais de 30 nós AD FS|Sem suporte usando o \- SQL wid necessário|Sem suporte usando o \- SQL wid necessário  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento do servidor  
Para implantar essa topologia, além de adicionar dois proxies de aplicativo Web, você deve verificar se a rede de perímetro também pode fornecer acesso a um sistema de nome de domínio \(DNS @ no__t-1 e a um segundo balanceamento de carga de rede \(NLB @ no__t-3. O segundo host NLB deve ser configurado com um cluster NLB que usa um endereço IP de cluster do Internet @ no__t-0accessible e deve usar a mesma configuração de nome DNS do cluster que o cluster NLB anterior que você configurou na rede corporativa @no__t -1fs. fabrikam. com @ no__t-2. Os proxies de aplicativo Web também devem ser configurados com endereços IP de Internet @ no__t-0accessible.  
  
A ilustração a seguir mostra o farm de servidores de Federação existente com a topologia WID que foi descrita anteriormente e como a empresa fictícia Fabrikam, Inc., fornece acesso a um servidor DNS de perímetro, adiciona um segundo host NLB com o mesmo nome DNS de cluster @no__t -0fs. fabrikam. com @ no__t-1 e adiciona dois proxies de aplicativo Web \(wap1 e WAP2 @ no__t-3 à rede  
  
![Farm de WID e proxies](media/WIDFarmADFSBlue.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies de aplicativo Web, consulte a seção "requisitos de resolução de nomes" em [AD FS requisitos](AD-FS-Requirements.md) e [planejar a infraestrutura de proxy de aplicativo Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx).  
  
## <a name="see-also"></a>Consulte também  
[Planejar a topologia de implantação do AD FS](Plan-Your-AD-FS-Deployment-Topology.md)  
[Guia de design do AD FS no Windows Server 2012 R2](AD-FS-Design-Guide-in-Windows-Server-2012-R2.md)  
  

