---
title: Ponte (DCB) do Data Center
description: Você pode usar este tópico para obter informações básicas sobre dados central ponte do Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d465e855adc387d7136919ac11fbab56c792c34
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="data-center-bridging-dcb"></a>Ponte \(DCB\) do Data Center

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para obter informações básicas sobre \(DCB\) ponte do Centro de dados.

DCB é um conjunto de padrões \(IEEE\) Institute of Electrical and eletrônicos engenheiros que permitem convergidos malhas no data center, onde o armazenamento, rede de dados, cluster \(IPC\) comunicação Inter\ processos e tráfego de gerenciamento compartilham a mesma infraestrutura de rede Ethernet.

>[!NOTE]
>Além de neste tópico, a seguinte documentação DCB está disponível
>
>- [Instalar DCB no Windows Server 2016 ou Windows 10](dcb-install.md)
>- [Gerenciar Data Center ponte (DCB)](dcb-manage.md)

DCB fornece alocação de largura de banda baseado em hardware\ para um tipo específico de tráfego de rede e aumenta a confiabilidade de transporte de Ethernet com o uso do controle de fluxo com base em priority\.

Alocação de largura de banda baseado em Hardware\ é essencial se tráfego ignora o sistema operacional e é transferido para um adaptador de rede convergida, que pode ser compatíveis com Internet Small Computer System Interface \(iSCSI\), remoto acesso direto à memória \(RDMA\) via Ethernet ou canal de fibra sobre \(FCoE\) Ethernet.

Controle de fluxo com base em Priority\ é essencial se o protocolo de camada superior, como o canal de fibra, presume um transporte subjacente sem perdas.

## <a name="dcb-protocols-and-management-options"></a>DCB protocolos e as opções de gerenciamento

DCB consiste no conjunto seguinte de protocolos. 

- Avançado serviço de transmissão \(ETS\) – IEEE 802.1Qaz, que se baseia no 802.1 P e 802.1 q padrões
- Controle de fluxo de prioridade \(PFS\), IEEE 802.1Qbb 
- Protocolo DCB \(DCBX\), IEEE 802.1AB, como estendida no 802.1Qaz padrão.

O protocolo DCBX permite que você configure DCB em um switch, que automaticamente, em seguida, pode configurar um dispositivo final, como um computador que executa o Windows Server 2016.

Se você preferir gerenciem DCB de um botão, você não precisa instalar DCB como um recurso no Windows Server 2016, no entanto, essa abordagem inclui algumas limitações.

Porque DCBX só pode informar o adaptador de rede de host de tamanhos de classe ETS e habilitação PFC, no entanto, hosts do Windows Server geralmente exigem que DCB esteja instalado para que o tráfego é mapeado para classes ETS.

Geralmente, aplicativos do Windows não são projetados para participar DCBX bolsas de valores. Por isso, o host deve ser configurado separadamente dos comutadores de rede, mas com configurações idênticas.

Se você optar por gerenciar DCB de uma opção, você ainda pode exibir as configurações no Windows Server 2016 usando comandos do Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Funcionalidade DCB importante

Esta é uma lista que resume a funcionalidade fornecida pelo DCB.

1. Fornece a interoperabilidade entre os adaptadores de rede compatíveis com DCB\ e comutadores compatíveis com DCB\.

2. Fornece um transporte Ethernet sem perdas entre um computador executando o Windows Server 2016 e sua opção de vizinho ao ativar o controle de fluxo com base em priority\ no adaptador de rede.

3. Fornece a capacidade para alocar largura de banda para um controle de tráfego \(TC\) por percentual, onde o TC pode consistir em uma ou mais classes de tráfego são diferenciados por indicadores de \(priority\) de classe de tráfego 802.1p.

4. Permite que os administradores de servidor ou administradores de rede atribuir um aplicativo para uma classe de tráfego específico ou a prioridade com base em protocolos conhecidos, porta TCP/UDP conhecida ou porta NetworkDirect usada pelo aplicativo.

5. Fornece gerenciamento DCB por meio do Windows Server 2016 Windows Management Instrumentation \(WMI\) e do Windows PowerShell. Para obter mais informações, consulte a seção [comandos do Windows PowerShell para DCB](#bkmk_wps) posteriormente neste tópico, além dos tópicos a seguir.
    - [Componentes DCB fornecido pelo sistema](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [Requisitos de QoS NDIS para ponte de dados central](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Fornece gerenciamento DCB pela política de grupo do Windows Server 2016.

7. Dá suporte a coexistência de soluções de \(QoS\) de qualidade do serviço do Windows Server 2016.

>[!NOTE]
>Antes de usar qualquer RDMA sobre em versão de RDMA \(RoCE\) convergidos Gigabit Ethernet, você deve habilitar DCB. Embora não seja necessário para redes de \(iWARP\) largo área RDMA protocolo, teste tiver determinado que todas as tecnologias com base em Ethernet\ RDMA funcionem melhor com DCB. Por isso, você deve considerar o uso DCB para implantações de RDMA iWARP. Para obter mais informações, consulte [acesso direto de memória remoto (RDMA) e agrupamento Embedded Switch (conjunto)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Aplicativos práticos de DCB

Muitas organizações têm grande canal de fibra \(FC\) armazenamento área \(SAN\) instalações de rede para o serviço de armazenamento. SAN FC exige adaptadores de rede especiais em servidores e switches FC na rede. Essas organizações geralmente também usam switches e adaptadores de rede Ethernet.

Na maioria dos casos, é significativamente mais caro implantar o que o hardware de Ethernet, o que resulta em grandes despesas FC hardware. Além disso, o requisito para separado Ethernet e SAN FC adaptador e alternar hardware de rede requer espaço adicional, energia e resfriamento capacidade em um datacenter, o que resulta em despesas operacionais adicionais e contínuas.

De uma perspectiva de custo, é vantajoso para muitas organizações mesclar sua tecnologia FC com a solução de hardware baseadas em Ethernet\ para fornecer serviços de rede de dados e de armazenamento.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>Usando DCB para uma malha convergida com base em Ethernet\ para dados de rede e armazenamento

Para organizações que já tem uma grande SAN FC, mas desejam migrar para longe de investimento adicional na tecnologia FC, DCB permite que você construa uma Ethernet com base em convergidos malha para armazenamento e rede de dados. Um DCB convergidos malha pode reduzir o custo total futuro de propriedade \(TCO\) e simplificar o gerenciamento.

Para hosters quem adotaram ou que pretende adotar, iSCSI como sua solução de armazenamento, DCB pode fornecer reserva de largura de banda assistido hardware\ para tráfego iSCSI para garantir o isolamento de desempenho. E, ao contrário de outras soluções de proprietárias, DCB é baseada em standards\ - e, portanto, relativamente fácil de implantar e gerenciar em um ambiente de rede heterogêneas.

Uma implementação do Windows Server baseada em 2016\ de DCB alivia muitos dos problemas que podem ocorrer quando convergidos soluções de malha são fornecidas por vários fabricantes originais do equipamento \(OEMs\).

Implementações de soluções proprietárias fornecidas pelos OEMs vários não pode interoperar com um ao outro, pode ser difícil de gerenciar e normalmente terão cronogramas de manutenção de software diferentes. 

Por outro lado, Windows Server 2016 DCB é baseado em standards\, e você pode implantar e gerenciar DCB em uma rede heterogênea.

## <a name="bkmk_wps"></a>Comandos do Windows PowerShell para DCB

Existem comandos DCB Windows PowerShell para Windows Server 2016 e Windows Server 2012 R2. Você pode usar todos os comandos do Windows Server 2012 R2 no Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandos do Windows Server 2016 Windows PowerShell para DCB

O seguinte tópico para o Windows Server 2016 fornece sintaxe e as descrições do cmdlet do Windows PowerShell para todos os dados central ponte \(DCB\) qualidade de serviço \ cmdlets de \-specific (QoS\). Ela lista os cmdlets em ordem alfabética, com base no verbo no início do cmdlet.

- [Módulo DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Comandos do Windows Server 2012 R2 Windows PowerShell para DCB

O seguinte tópico para o Windows Server 2012 R2 fornece sintaxe e as descrições do cmdlet do Windows PowerShell para todos os dados central ponte \(DCB\) qualidade de serviço \ cmdlets de \-specific (QoS\). Ela lista os cmdlets em ordem alfabética, com base no verbo no início do cmdlet.

- [Centro de dados ponte (DCB) qualidade de serviço (QoS) Cmdlets do Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
