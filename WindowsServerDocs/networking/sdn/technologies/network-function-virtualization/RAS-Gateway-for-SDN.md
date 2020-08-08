---
title: Gateway de RAS para SDN
description: Você pode usar este tópico para saber mais sobre o gateway de RAS, que é um roteador com capacidade de Border Gateway Protocol de vários locatários, baseado em software (BGP) no Windows Server 2016.
manager: grcusanz
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: anpaul
author: AnirbanPaul
ms.openlocfilehash: 52f0118f6e9636329fe75e9e8ac008d22dcd1cda
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952309"
---
# <a name="ras-gateway-for-sdn"></a>Gateway de RAS para SDN

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016 # # gateway de RAS para SDN


O gateway de RAS é um roteador compatível com BGP (baseado em software, multilocatário Border Gateway Protocol) projetado para CSPs (provedores de serviços de nuvem) e empresas que hospedam várias redes virtuais de locatário usando a virtualização de rede Hyper-V. Os gateways RAS roteiam o tráfego de rede entre a rede física e os recursos da rede VM, independentemente do local. Você pode rotear o tráfego de rede no mesmo local físico ou em vários locais diferentes.

A multilocação é a capacidade de uma infraestrutura de nuvem dar suporte às cargas de trabalho de máquina virtual de vários locatários, mas ainda isolá-las umas das outras, enquanto todas as cargas de trabalho são executadas na mesma infraestrutura. As várias cargas de trabalho de um locatário individual podem se interconectar e serem gerenciadas remotamente, mas esses sistemas não interconectam com as cargas de trabalho de outros locatários, nem outros locatários podem gerenciá-las remotamente.


> [!NOTE]
> Além deste tópico, os tópicos de gateway RAS a seguir estão disponíveis.
>
> -   [O que há de novo no gateway de RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)
> -   [Arquitetura de implantação do gateway de RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)
> -   [Alta disponibilidade do gateway de RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)
> -   [Border Gateway Protocol &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)
> -   [Referência de comando do Windows PowerShell BGP](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)


## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Pré-requisitos para instalar o gateway de RAS para SDN
Não é possível usar a interface do Windows para instalar o acesso remoto quando você deseja implantar o gateway RAS no modo multilocatário para uso com SDN. Em vez disso, você deve usar o Windows PowerShell.

Mas antes de instalar o gateway RAS usando o Windows PowerShell, você deve usar o Windows PowerShell para adicionar o recurso de **RemoteAccess** do Windows. Para fazer isso, execute o comando a seguir no prompt do Windows PowerShell.

`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`

Esse comando adiciona o recurso **RemoteAccess** e os comandos do Windows PowerShell para o recurso.

Depois de adicionar o **RemoteAccess** ao servidor, você pode instalar o acesso remoto como um gateway RAS com o modo multilocatário e o Border Gateway Protocol (BGP).

Para obter mais informações, consulte o tópico de referência do Windows PowerShell [install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx).

## <a name="ras-gateway-features"></a>Recursos de gateway de RAS
A seguir estão os recursos de gateway de RAS no Windows Server 2016. Você pode implantar o gateway RAS em pools de alta disponibilidade que usam todos esses recursos de uma só vez.

-   **VPN site a site**. Esse recurso de gateway RAS permite que você conecte duas redes em locais físicos diferentes pela Internet usando uma conexão VPN site a site. Para CSPs que hospedam muitos locatários em seu datacenter, o gateway de RAS fornece uma solução de gateway multilocatário que permite que seus locatários acessem e gerenciem seus recursos em conexões VPN site a site de sites remotos e que permite o fluxo de tráfego de rede entre recursos virtuais em seu datacenter e sua rede física.

-   **VPN ponto a site**. Esse recurso de gateway RAS permite que os funcionários ou administradores da organização se conectem à rede da sua organização a partir de locais remotos.  Para implantações multilocatários, os administradores de rede de locatários podem usar conexões VPN ponto a site para acessar recursos de rede virtual no datacenter do CSP.

-   **Túnel GRE**. Túneis baseados em GRE (encapsulamento de roteamento genérico) permitem a conectividade entre redes virtuais de locatário e redes externas. Uma vez que o protocolo GRE é leve e o suporte para GRE está disponível na maioria dos dispositivos de rede, ele se torna uma opção ideal para túnel quando a criptografia de dados não é necessária. O suporte a GRE em túneis de S2S (site a site) resolve o problema de encaminhamento entre redes virtuais de locatário e redes externas de locatário usando um gateway de multilocatário, conforme descrito posteriormente neste tópico.

-   **Roteamento dinâmico com Border Gateway Protocol (BGP)**. O BGP reduz a necessidade de configuração de roteamento manual em roteadores, porque ele é um protocolo de roteamento dinâmico e aprende rotas entre sites conectados usando conexões VPN site a site automaticamente. Se sua organização tiver vários sites que estão conectados usando roteadores habilitados para BGP, como o gateway RAS, o BGP permitirá que os roteadores calculem e usem automaticamente rotas válidas entre si no caso de interrupção ou falha da rede. Para obter mais informações, consulte [RFC 4271](https://tools.ietf.org/html/rfc4271).





