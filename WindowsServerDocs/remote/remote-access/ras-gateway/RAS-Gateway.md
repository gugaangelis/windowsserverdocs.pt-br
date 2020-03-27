---
title: Gateway de RAS
description: Este tópico, destinado a profissionais de ti (tecnologia da informação), fornece informações gerais sobre o gateway de RAS, incluindo modos de implantação de gateway RAS e recursos no Windows Server 2016.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: acaa46b7-09b1-4707-9562-116df8db17eb
ms.author: lizross
author: eross-msft
ms.date: 05/23/2018
ms.openlocfilehash: 762ba98a57db1411098c6ae6a8394e9a9b063181
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308533"
---
# <a name="ras-gateway"></a>Gateway de RAS

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

O gateway de RAS é um roteador de software e gateway que você pode usar no modo de locatário único ou em modo multilocatário.  
  
- O modo de **locatário único** permite que organizações de qualquer tamanho implantem o gateway como um exterior ou uma rede virtual privada (VPN) de borda para a Internet e um servidor DirectAccess. No modo de locatário único, você pode implantar o gateway RAS em um servidor físico ou VM (máquina virtual) executando o Windows Server 2016.  
  
- O **modo multilocatário** permite que os CSPs (provedores de serviços de nuvem) e as empresas usem o gateway RAS para habilitar o roteamento de tráfego de rede de nuvem e de datacenter entre redes físicas e virtuais, incluindo a Internet. Para o modo multilocatário, é recomendável implantar o gateway RAS em VMs que executam o Windows Server 2016.  
  
> [!NOTE]  
> O gateway RAS dá suporte a IPv4 e IPv6, incluindo o encaminhamento IPv4 e IPv6. Quando você configura o gateway RAS com NAT (conversão de endereços de rede), somente NAT44 tem suporte.  
  
## <a name="who-will-be-interested-in-ras-gateway"></a>Quem estará interessado no gateway de RAS?
  
Se você for um administrador de sistema, arquiteto de rede ou outro profissional de ti, o gateway de RAS poderá lhe interessar em uma ou mais das seguintes circunstâncias:  
  
-   Você projeta ou dá suporte à infraestrutura de TI para uma organização que está usando ou planejando usar o Hyper-V para implantar VMs (máquinas virtuais) em redes virtuais.  
  
-   Você projeta ou dá suporte à infraestrutura de TI de uma organização que implantou ou está planejando implantar tecnologias de nuvem.  
  
-   Você deseja fornecer conectividade total de rede entre redes físicas e redes virtuais.  
  
-   Você deseja fornecer aos clientes de sua organização acesso a suas redes virtuais pela Internet.  
  
-   Você deseja fornecer aos funcionários da sua organização acesso remoto à rede da sua organização.  
  
-   Você deseja conectar escritórios em locais físicos diferentes pela Internet.  
 
Este tópico, destinado a profissionais de ti (tecnologia da informação), fornece informações gerais sobre o gateway de RAS, incluindo modos de implantação de gateway RAS e recursos. 
  
Este tópico contém as seguintes seções.  
  
  
-   [Modos de implantação de gateway de RAS](#bkmk_modes)  
  
-   [Clustering de gateway RAS para alta disponibilidade](#bkmk_clustering)  
  
-   [Recursos de gateway de RAS](#bkmk_features)  
  
-   [Cenários de implantação de gateway de RAS](#bkmk_deploy)  
  
-   [Ferramentas de gerenciamento de gateway RAS](#bkmk_manage)  
  


  
## <a name="ras-gateway-deployment-modes"></a><a name="bkmk_modes"></a>Modos de implantação de gateway de RAS  
O gateway de RAS inclui os seguintes modos de implantação.  
  
### <a name="single-tenant-mode"></a>Modo de locatário único  
Para a maioria das organizações, o uso do gateway RAS no modo de locatário único é a configuração típica. No modo de locatário único, você pode implantar o gateway RAS como um servidor VPN de borda, um servidor DirectAccess do Edge ou ambos simultaneamente. Nessa configuração, o gateway de RAS fornece aos funcionários remotos conectividade à sua rede usando conexões VPN ou DirectAccess. Além disso, o modo de locatário único permite que você conecte escritórios em locais físicos diferentes pela Internet.  
  
### <a name="multitenant-mode"></a>Modo multilocatário  
Se sua organização for um CSP ou uma empresa com vários locatários, você poderá implantar o gateway RAS no modo multilocatário para fornecer roteamento de tráfego de rede de e para redes físicas e virtuais.  
  
A multilocação é a capacidade de uma infraestrutura de nuvem dar suporte às cargas de trabalho de máquina virtual de vários locatários, mas ainda isolá-las umas das outras, enquanto todas as cargas de trabalho são executadas na mesma infraestrutura. As várias cargas de trabalho de um locatário individual podem se interconectar e serem gerenciadas remotamente, mas esses sistemas não interconectam com as cargas de trabalho de outros locatários, nem outros locatários podem gerenciá-las remotamente.  
  
Por exemplo, uma Empresa pode ter várias sub-redes virtuais diferentes, cada uma dedicada a servir um departamento específico, como Pesquisa e Desenvolvimento ou Contabilidade. Em outro exemplo, um CSP tem muitos locatários com sub-redes virtuais isoladas existentes no mesmo datacenter físico. Em ambos os casos, o gateway RAS pode rotear o tráfego para e de cada locatário, mantendo o isolamento projetado de cada locatário. Esse recurso torna o gateway de RAS com reconhecimento de multilocatário.  
  
As redes virtuais são criadas usando a virtualização de rede Hyper-V, que é uma tecnologia que foi introduzida no Windows Server 2012 e aprimorada no Windows Server 2016. O gateway de RAS é integrado à virtualização de rede Hyper-V e é capaz de rotear o tráfego de rede efetivamente em circunstâncias em que há muitos clientes ou locatários diferentes – que têm redes virtuais isoladas no mesmo datacenter.  
  
A virtualização de rede do Hyper-V fornece a capacidade de implantar uma rede de máquina virtual (VM) que é independente da rede física subjacente. Com redes VM, que são compostas por uma ou mais sub-redes virtuais, o local físico exato de uma sub-rede IP é dissociado da topologia de rede virtual. Como resultado, você pode mover facilmente suas sub-redes locais para a nuvem, preservando, ao mesmo tempo, os endereços IP e a topologia existentes na nuvem. Essa capacidade de preservar a infraestrutura permite que os serviços existentes continuem funcionando, sem reconhecimento do local físico das sub-redes. Ou seja, a Virtualização de Rede Hyper-V permite uma nuvem híbrida integrada.  
  
> [!NOTE]  
> A virtualização de rede do Hyper-V é uma tecnologia de sobreposição de rede que usa o[NVGRE](https://tools.ietf.org/html/draft-sridharan-virtualization-nvgre-00)(encapsulamento de roteamento genérico de virtualização de rede), que permite aos locatários trazer seu próprio espaço de endereço e permite uma escalabilidade melhor do que é possível usando VLANs para isolamento de locatário.  
  
No Windows Server 2016, o gateway RAS roteia o tráfego de rede entre a rede física e os recursos da rede VM, independentemente de onde os recursos estão localizados. Você pode usar o gateway de RAS para rotear o tráfego de rede entre redes físicas e virtuais no mesmo local físico ou em vários locais físicos diferentes.  
  
Por exemplo, se você tiver uma rede física e uma rede virtual no mesmo local físico, poderá implantar um computador que executa o Hyper-V configurado com uma VM de gateway de RAS para atuar como um gateway de encaminhamento e rotear o tráfego entre o virtual e o físico às.  
  
Em outro exemplo, se suas redes virtuais existirem na nuvem, o CSP poderá implantar um gateway de RAS para que você possa criar uma conexão site a site de VPN (rede virtual privada) entre o servidor VPN e o gateway de RAS do CSP; Quando esse link for estabelecido, você poderá se conectar aos recursos virtuais na nuvem pela conexão VPN.  
  
Para obter mais informações, consulte [alta disponibilidade do gateway de Ras](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md).  
  
## <a name="clustering-ras-gateway-for-high-availability"></a><a name="bkmk_clustering"></a>Clustering de gateway RAS para alta disponibilidade  
O gateway de RAS é implantado em um computador dedicado que executa o Hyper-V e que está configurado com uma VM. A VM é então configurada como um gateway de RAS.  
  
Para obter alta disponibilidade de recursos de rede, você pode implantar o gateway RAS com failover usando dois servidores host físicos executando o Hyper-V que estão cada um também executando uma VM (máquina virtual) configurada como um gateway. Em seguida, as VMs do gateway são configuradas como um cluster para fornecer proteção de failover contra interrupções da rede e falha de hardware.  
  
Por exemplo, se sua organização for uma empresa com uma implantação de nuvem privada, talvez você precise apenas de duas VMs de gateway RAS, cada uma instalada em um computador diferente executando o Hyper-V. Nesse cenário, as VMs de gateway de RAS são adicionadas a um cluster para fornecer alta disponibilidade.  
  
Em outro exemplo, se sua organização for um provedor de serviços de nuvem (CSP) com locatários 200 em seu datacenter, você poderá usar oito VMs de gateway de RAS, com cada par de VMs de gateway de RAS em cluster fornecendo serviços de roteamento para locatários de 50. Nesse cenário, dois computadores que executam o Hyper-V têm quatro VMs configuradas como gateways de RAS. Em seguida, configure quatro clusters de VM de gateway RAS, cada cluster contendo uma VM de cada computador que executa o Hyper-V.  
  
Quando você implanta o gateway RAS, os servidores host que executam o Hyper-V e as VMs que você configura como gateways devem estar executando o Windows Server 2012 R2 ou o Windows Server 2016.  
  
## <a name="ras-gateway-features"></a><a name="bkmk_features"></a>Recursos de gateway de RAS  
O gateway de RAS inclui os seguintes recursos.  
  
-   **VPN site a site**. Esse recurso de gateway RAS permite que você conecte duas redes em locais físicos diferentes pela Internet usando uma conexão VPN site a site. Se você tiver um escritório principal e várias filiais, poderá implantar um gateway de RAS de borda em cada local e criar conexões site a site para fornecer o fluxo de tráfego de rede entre os locais. Para CSPs que hospedam muitos locatários em seu datacenter, o gateway de RAS fornece uma solução de gateway multilocatário que permite que seus locatários acessem e gerenciem seus recursos em conexões VPN site a site de sites remotos e que permite o fluxo de tráfego de rede entre recursos virtuais em seu datacenter e sua rede física.  
  
-   **VPN ponto a site**. Esse recurso de gateway RAS permite que os funcionários ou administradores da organização se conectem à rede da sua organização a partir de locais remotos. Para implantações de locatário único do gateway de RAS, os funcionários remotos podem se conectar à rede da sua organização usando uma conexão VPN. Essa conexão permite que eles usem recursos de rede internos, como sites da intranet e servidores de arquivos. Para implantações multilocatários, os administradores de rede de locatários podem usar conexões VPN ponto a site para acessar recursos de rede virtual no datacenter do CSP.  
  
-   **Roteamento dinâmico com Border Gateway Protocol (BGP)** . O BGP reduz a necessidade de configuração de roteamento manual em roteadores, porque ele é um protocolo de roteamento dinâmico e aprende rotas entre sites conectados usando conexões VPN site a site automaticamente. Se sua organização tiver vários sites que estão conectados usando roteadores habilitados para BGP, como o gateway RAS, o BGP permitirá que os roteadores calculem e usem automaticamente rotas válidas entre si no caso de interrupção ou falha da rede. Para obter mais informações, consulte [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  
-   **NAT (conversão de endereços de rede)** . A NAT (conversão de endereços de rede) permite que você compartilhe uma conexão com a Internet pública por meio de uma única interface com um único endereço IP público. Os computadores na rede privada usam endereços particulares e não roteáveis. O NAT mapeia os endereços privados para o endereço público. Esse recurso de gateway RAS permite que os funcionários da organização com implantações de locatário único acessem recursos da Internet por trás do gateway. Para CSPs, esse recurso permite que aplicativos que estão sendo executados em VMs de locatário acessem a Internet. Por exemplo, uma VM de locatário configurada como um servidor Web pode contatar recursos financeiros externos para processar transações de cartão de crédito.  

  
## <a name="ras-gateway-deployment-scenarios"></a><a name="bkmk_deploy"></a>Cenários de implantação de gateway de RAS  
A seguir estão os cenários de implantação recomendados para o gateway de RAS.  
  
-   **Enterprise Edge – implantação de locatário único**. Com a implantação de um único locatário empresarial, você pode conectar um físico a vários outros locais físicos pela Internet usando o recurso de VPN site a site-e Border Gateway Protocol (BGP) permite usar o roteamento dinâmico. Você também pode fornecer aos funcionários remotos acesso à rede da sua organização com conexões VPN ponto a site e conexões do DirectAccess. (As conexões do DirectAccess estão sempre ativas e também fornecem a vantagem de que você pode gerenciar facilmente os computadores conectados usando o DirectAccess, pois eles são conectados sempre que estão conectados à Internet.) Você também pode configurar gateways de RAS corporativos de locatário único com NAT, para que os computadores em sua intranet possam se comunicar facilmente com a Internet.  
  
-   **Borda do provedor de serviços de nuvem-implantação multilocatário**. A implantação multilocatário do gateway de RAS para CSPs permite que você ofereça aos seus locatários todos os recursos disponíveis com a implantação de locatário único do Enterprise Edge. As conexões VPN site a site entre as redes virtuais de locatário em seu datacenter e os locais de rede de locatário na Internet significam que os locatários têm acesso contínuo aos seus recursos de nuvem o tempo todo. O acesso VPN ponto a site para locatários significa que os administradores de locatários sempre podem se conectar às suas redes virtuais em seu datacenter para gerenciar seus recursos. O BGP fornece roteamento dinâmico e mantém os locatários conectados a seus ativos mesmo quando ocorrem problemas de rede na Internet ou em outro lugar. E o NAT permite que as VMs de locatário se conectem aos recursos na Internet, como recursos de processamento de cartão de crédito.  
  
## <a name="ras-gateway-management-tools"></a><a name="bkmk_manage"></a>Ferramentas de gerenciamento de gateway RAS  
A seguir estão as ferramentas de gerenciamento para o gateway de RAS.  
  
-   No Windows Server 2016, para implantar um roteador de gateway de RAS, você deve usar comandos do Windows PowerShell. Para obter mais informações, consulte [cmdlets de acesso remoto](https://docs.microsoft.com/powershell/module/remoteaccess) para windows Server 2016 e Windows 10.  
  
-   No System Center 2012 R2 Virtual Machine Manager (VMM), o gateway RAS é chamado de gateway do Windows Server. Um conjunto limitado de opções de configuração de Border Gateway Protocol (BGP) está disponível na interface de software do VMM, incluindo o **endereço IP de BGP local** e os **números de sistema autônomo (ASN)** , **lista de endereços IP de pares de BGP**e **valores de ASN**. No entanto, você pode usar os comandos BGP do Windows PowerShell de Acesso Remoto para configurar todos os outros recursos do Gateway do Windows Server. Para obter mais informações, consulte [Virtual Machine Manager (VMM)](https://technet.microsoft.com/system-center-docs/vmm/vmm) e [cmdlets de acesso remoto](https://technet.microsoft.com/library/hh918399.aspx) para Windows Server 2016 e Windows 10.  
  
## <a name="related-topics"></a>Tópicos relacionados
- [Alta disponibilidade do gateway de RAS](../../../networking/sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
- [Túnel de GRE no Windows Server](gre-tunneling-windows-server.md)
- [Desempenho e taxa de transferência do túnel de GRE do Gateway de RAS](RAS-Gateway-GRE-Perf.md)
