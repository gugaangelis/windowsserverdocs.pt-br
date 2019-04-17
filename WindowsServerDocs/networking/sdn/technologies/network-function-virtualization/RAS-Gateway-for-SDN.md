---
title: Gateway RAS para SDN
description: Você pode usar este tópico para saber mais sobre RAS Gateway, que é um baseada em software, multitenant, roteador capaz de Border Gateway Protocol (BGP) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 052911dcd52df82ef4e259de0c64078c54f00195
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="ras-gateway-for-sdn"></a>Gateway RAS para SDN

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre o Gateway RAS, que é um baseada em software, vários locatários, Border Gateway Protocol (BGP) compatíveis com roteador no Windows Server 2016 é projetado para provedores de serviço de nuvem (CSPs) e as empresas que hospedam várias redes virtuais locatário usando a virtualização de rede do Hyper-V.  
  
> [!NOTE]  
> Além neste tópico, os seguintes tópicos de Gateway RAS estão disponíveis.  
>   
> -   [O que há de novo no RAS Gateway](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Arquitetura de implantação do RAS Gateway](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [Gateway RAS alta disponibilidade](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Protocolo de Gateway de borda & #40; BGP & #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Referência de comando do PowerShell BGP Windows](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
No Windows Server 2016, Gateway RAS encaminha o tráfego de rede entre a rede física e recursos de rede VM, independentemente de onde os recursos estão localizados. Você pode usar o Gateway RAS para rotear o tráfego de rede entre redes físicos e virtuais no mesmo local físico ou em vários locais físicos diferentes pela Internet.  
  
Multilocação é a capacidade de uma infraestrutura de nuvem suporte as cargas de trabalho de máquina virtual de vários locatários, ainda isole-as entre si, enquanto todos as cargas de trabalho são executados na mesma infraestrutura. Várias cargas de trabalho de um locatário individual podem interconexão e ser gerenciadas remotamente, mas esses sistemas não interconexão com as cargas de trabalho de outros locatários nem outros locatários remotamente gerenciá-los.  
  
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Pré-requisitos para instalar o Gateway RAS em SDN  
Você não pode usar a interface do Windows para instalar o acesso remoto quando você deseja implantar RAS Gateway no modo de vários locatários para uso com SDN. Em vez disso, você deve usar o Windows PowerShell.  
  
Mas antes de instalar o Gateway RAS usando o Windows PowerShell, você deve usar o Windows PowerShell para adicionar o **acesso remoto** recurso do Windows. Para fazer isso, execute o seguinte comando no prompt do Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Este comando adiciona o **acesso remoto** recurso e os comandos do Windows PowerShell para o recurso.  
  
Depois que você adicionou **acesso remoto** ao servidor, você pode instalar o acesso remoto como um Gateway RAS com o modo de vários locatários e Border Gateway Protocol (BGP).  
  
Para obter mais informações, consulte o tópico de referência do Windows PowerShell [acesso remoto instalar](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Recursos de Gateway RAS  
A seguir é recursos de Gateway RAS no Windows Server 2016. Você pode implantar Gateway RAS em pools de alta disponibilidade que usam todos esses recursos ao mesmo tempo.  
  
-   **VPN to-site**. Esse recurso RAS Gateway permite que você conecte duas redes em locais físicos diferentes da Internet usando uma conexão de VPN-to-site. Para os CSPs que hospedam vários locatários em datacenters, RAS Gateway oferece uma solução de vários locatários gateway que permite que seu locatários acessar e gerenciar seus recursos em conexões de VPN-to-site de locais remotos, e que permite o fluxo de tráfego de rede entre recursos virtuais no data center e sua rede física.  
  
-   **Site de ponto de VPN**. Esse recurso RAS Gateway permite que os funcionários da organização ou administradores para se conectar à rede da sua organização de locais remotos.  Para implantações de vários locatários, os administradores de rede de locatário podem usar conexões VPN ponto ao site para acessar recursos de rede virtual no datacenter CSP.  
  
-   **Encapsulamento de GRE**. Encapsulamento de roteamento genérico (GRE) com base em túneis habilitar conectividade entre redes virtuais locatário e redes externas. Como o protocolo GRE é leve e suporte para GRE está disponível na maioria dos dispositivos de rede se torna uma opção ideal para encapsulamento onde a criptografia de dados não é necessária. Suporte GRE em túneis do Site para outro (S2S) resolve o problema de encaminhamento entre redes virtuais locatário e locatário redes externo usando um gateway de vários locatário, conforme descrito mais adiante neste tópico.  
  
-   **Roteamento dinâmico com Border Gateway Protocol (BGP)**. BGP reduz a necessidade de configuração manual rota nos roteadores porque ele é um protocolo de roteamento dinâmico e aprende automaticamente rotas entre sites que estão conectados por meio de conexões VPN ao site. Se sua organização tiver vários sites que estão conectados usando habilitado BGP roteadores como Gateway RAS, BGP permite que os roteadores automaticamente calcular e usar rotas válidas uns aos outros, no caso de interrupção de rede ou a falha. Para obter mais informações, consulte [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  

  


