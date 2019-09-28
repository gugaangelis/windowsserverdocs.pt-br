---
ms.assetid: 8890ccc9-068d-4da2-bd51-8a2964173ff1
title: Farm de servidores de federação usando WID e proxies
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 60072037aea4ecd81376e1334f3a89b7bb2ff851
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408082"
---
# <a name="federation-server-farm-using-wid-and-proxies"></a>Farm de servidores de federação usando WID e proxies

Essa topologia de implantação para Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 é idêntica ao farm de servidores de Federação com a topologia do banco de dados interno do Windows \(WID @ no__t-3, mas adiciona proxies de servidor de Federação à rede de perímetro para suporte a usuários externos. Os proxies de servidor de Federação redirecionam as solicitações de autenticação de cliente que vêm de fora de sua rede corporativa para o farm de servidores de Federação.  
  
## <a name="deployment-considerations"></a>Considerações sobre a implantação  
Esta seção descreve as várias considerações sobre o público-alvo, os benefícios e as limitações associados a essa topologia de implantação.  
  
### <a name="who-should-use-this-topology"></a>Quem deve usar essa topologia?  
  
-   Organizações com 100 ou menos relações de confiança configuradas que precisam fornecer os usuários internos e externos \(who estão conectados a computadores que estão fisicamente localizados fora da rede corporativa @ no__t-1 com Sign-2on único. \(SSO @ no__t-4 acesso a aplicativos ou serviços federados  
  
-   Organizações que precisam fornecer os usuários internos e usuários externos com acesso de SSO a Microsoft Office 365  
  
-   Organizações menores que têm usuários externos e exigem serviços redundantes e escalonáveis  
  
### <a name="what-are-the-benefits-of-using-this-topology"></a>Quais são os benefícios de usar essa topologia?  
  
-   Os mesmos benefícios listados para o [farm de servidores de Federação usando](Federation-Server-Farm-Using-WID-2012.md) a topologia wid, além do benefício de fornecer acesso adicional para usuários externos  
  
### <a name="what-are-the-limitations-of-using-this-topology"></a>Quais são as limitações do uso dessa topologia?  
  
-   As mesmas limitações listadas para o [farm de servidores de Federação usando](Federation-Server-Farm-Using-WID-2012.md) a topologia wid  
  
## <a name="server-placement-and-network-layout-recommendations"></a>Recomendações de layout de rede e posicionamento do servidor  
Para implantar essa topologia, além de adicionar dois proxies de servidor de Federação, você deve verificar se a rede de perímetro também pode fornecer acesso a um sistema de nome de domínio \(DNS @ no__t-1 Server e a um segundo balanceamento de carga de rede \(NLB @ no__t-3. O segundo host NLB deve ser configurado com um cluster NLB que usa um endereço IP de cluster do Internet @ no__t-0accessible e deve usar a mesma configuração de nome DNS do cluster que o cluster NLB anterior que você configurou na rede corporativa @no__t -1fs. fabrikam. com @ no__t-2. Os proxies de servidor de Federação também devem ser configurados com endereços IP de Internet @ no__t-0accessible.  
  
A ilustração a seguir mostra o farm de servidores de Federação existente com a topologia WID que foi descrita anteriormente e como a empresa fictícia Fabrikam, Inc., fornece acesso a um servidor DNS de perímetro, adiciona um segundo host NLB com o mesmo nome DNS de cluster @no__t -0fs. fabrikam. com @ no__t-1 e adiciona dois proxies de servidor de Federação \(fsp1 e fsp2 @ no__t-3 à  
  
![farm de servidores usando WID](media/FarmWIDProxies.gif)  
  
Para obter mais informações sobre como configurar seu ambiente de rede para uso com servidores de Federação ou proxies de servidor de Federação, consulte [requisitos de resolução de nomes para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md) ou [requisitos de resolução de nomes para Federação Proxies do servidor](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
