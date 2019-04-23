---
title: Gateway de RAS para SDN
description: Você pode usar este tópico para saber mais sobre o Gateway de RAS, que é uma baseada em software, multilocatário, o roteador capaz do Border Gateway Protocol (BGP) no Windows Server 2016.
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
ms.openlocfilehash: 4f1ad0b3f0b5921a53faa8a45baae9f0b8711873
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856267"
---
# <a name="ras-gateway-for-sdn"></a>Gateway de RAS para SDN

>Aplica-se a: Windows Server (canal semestral), Gateway RAS para SDN do Windows Server 2016. # #  


Gateway de RAS é um multilocatário baseado em software, o roteador habilitado do Border Gateway Protocol (BGP) criado para provedores de serviço de nuvem (CSPs) e empresas que hospedam várias redes virtuais de locatário usando a virtualização de rede do Hyper-V. Gateways de RAS roteia o tráfego de rede entre a rede física e os recursos de rede VM, independentemente do local. Você pode rotear o tráfego de rede no mesmo local físico ou em vários locais diferentes.   

A multilocação é a capacidade de uma infraestrutura de nuvem para oferecer suporte as cargas de trabalho de máquina virtual de vários locatários, ainda isolá-las umas das outras, embora todas as cargas de trabalho executados na mesma infraestrutura. As várias cargas de trabalho de um locatário individual podem se interconectar e serem gerenciadas remotamente, mas esses sistemas não interconectam com as cargas de trabalho de outros locatários, nem outros locatários podem gerenciá-las remotamente.

  
> [!NOTE]  
> Além deste tópico, estão disponíveis os seguintes tópicos de Gateway de RAS.  
>   
> -   [O que há de novo no Gateway de RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Arquitetura de implantação do Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [Alta disponibilidade do Gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Border Gateway Protocol &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Referência de comando do PowerShell do Windows via protocolo BGP](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Pré-requisitos para instalar o Gateway de RAS para SDN  
Você não pode usar a interface do Windows para instalar o acesso remoto quando você deseja implantar o Gateway de RAS no modo multilocatário para uso com SDN. Em vez disso, você deve usar o Windows PowerShell.  
  
Mas antes de instalar o Gateway de RAS por meio do Windows PowerShell, você deve usar o Windows PowerShell para adicionar o **RemoteAccess** recurso do Windows. Para fazer isso, execute o seguinte comando no prompt do Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Este comando adiciona o **RemoteAccess** recurso e os comandos do Windows PowerShell para o recurso.  
  
Depois que você adicionou **RemoteAccess** ao seu servidor, você pode instalar o acesso remoto como um Gateway de RAS com o modo de multilocatário e Border Gateway Protocol (BGP).  
  
Para obter mais informações, consulte o tópico de referência do Windows PowerShell [Install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Recursos de Gateway RAS  
Estes são os recursos de Gateway de RAS no Windows Server 2016. Você pode implantar o Gateway de RAS em pools de alta disponibilidade que usam todos esses recursos ao mesmo tempo.  
  
-   **VPN site a site**. Esse recurso de Gateway de RAS permite que você conecte duas redes em locais físicos diferentes da Internet por meio de uma conexão de VPN site a site. Para os CSPs que hospedam muitos locatários em seus datacenters, o Gateway de RAS fornece uma solução de gateway multilocatário que permite que seus locatários acessar e gerenciar seus recursos em conexões de VPN site a site de sites remotos, e que permite que o fluxo do tráfego de rede entre recursos virtuais no seu datacenter e sua rede física.  
  
-   **VPN ponto a site**. Esse recurso de Gateway de RAS permite que os funcionários da organização ou os administradores para se conectar à rede da sua organização de locais remotos.  Para implantações de multilocatários, os administradores de rede do locatário podem usar conexões de VPN ponto a site para acessar recursos de rede virtual no datacenter do CSP.  
  
-   **Túnel GRE**. Encapsulamento de roteamento genérico (GRE) com base em túneis permitem a conectividade entre redes virtuais de locatário e redes externas. Uma vez que o protocolo GRE é leve e o suporte para GRE está disponível na maioria dos dispositivos de rede, ele se torna uma opção ideal para túnel quando a criptografia de dados não é necessária. Suporte a GRE em túneis do Site a Site (S2S) resolve o problema de encaminhamento entre redes virtuais de locatário e redes externas de locatário usando um gateway de multilocatário, conforme descrito mais adiante neste tópico.  
  
-   **Roteamento dinâmico com o Border Gateway Protocol (BGP)**. O BGP reduz a necessidade de configuração de roteamento manual em roteadores, porque ele é um protocolo de roteamento dinâmico e aprende rotas entre sites conectados usando conexões VPN site a site automaticamente. Se sua organização tiver vários sites que estão conectados por meio de roteadores BGP habilitado, como o Gateway de RAS, o BGP permite que os roteadores calcular automaticamente e usar as rotas válidas entre si em caso de interrupção de rede ou a falha. Para obter mais informações, consulte [4271 RFC](https://tools.ietf.org/html/rfc4271).  
  

  


