---
title: DCB (Data Center Bridging)
description: Você pode usar este tópico para obter informações introdutórias sobre o Data Center Bridging no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: da58f312-bd3b-4bb6-98ca-6177869dd6ad
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7cb488192a873743db27d9c1d09c5912bc810bb8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834177"
---
# <a name="data-center-bridging-dcb"></a>Ponte de Data Center \(DCB\)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter informações introdutórias sobre o Data Center Bridging \(DCB\).

O DCB é um pacote do Institute of Electrical and Electronics Engineers \(IEEE\) padrões que permitem estruturas convergidas no data center, em que o armazenamento de dados de rede, cluster Inter\-comunicação de processo \(IPC\), e todo o tráfego de gerenciamento compartilham a mesma infraestrutura de rede Ethernet.

>[!NOTE]
>Além deste tópico, a seguinte documentação de DCB está disponível
>
>- [Instalar o DCB no Windows Server 2016 ou Windows 10](dcb-install.md)
>- [Gerenciar o Data Center Bridging (DCB)](dcb-manage.md)

O DCB fornece hardware\-com base em alocação de largura de banda para um tipo específico de tráfego de rede e melhora a confiabilidade do transporte Ethernet com o uso de prioridade\-com base em controle de fluxo.

Hardware\-alocação de largura de banda com base é essencial se tráfego desviado do sistema operacional e é descarregado para um adaptador de rede convergida, que pode dar suporte a Internet Small Computer System Interface \(iSCSI\), Acesso remoto direto à memória \(RDMA\) por Ethernet ou Fibre Channel por Ethernet \(FCoE\).

Prioridade\-controle de fluxo com base é essencial se o protocolo de camada superior, como o Fibre Channel, presume um transporte subjacente sem perdas.

## <a name="dcb-protocols-and-management-options"></a>O DCB protocolos e opções de gerenciamento

O DCB consiste o seguinte conjunto de protocolos. 

- Aprimorado o serviço de transmissão \(ETS\) – IEEE 802.1Qaz, que tem como base a 802.1p e 802.1Q padrões
- Controle de fluxo de prioridade \(PFS\), IEEE 802.1Qbb 
- Protocolo de troca de DCB \(DCBX\), IEEE 802.1AB, estendido no 802.1Qaz padrão.

O protocolo DCBX permite que você configure o DCB em um comutador, que, em seguida, pode configurar automaticamente um dispositivo final, como um computador executando o Windows Server 2016.

Se você preferir gerenciar DCB de um comutador, você não precisa instalar o DCB como um recurso no Windows Server 2016, no entanto, essa abordagem inclui algumas limitações.

Como DCBX somente pode informar o adaptador de rede do host de tamanhos de classe ETS e habilitação de PFC, no entanto, hosts do Windows Server geralmente exigem que o DCB é instalado para que o tráfego é mapeado para classes ETS.

Aplicativos do Windows normalmente não são criados para participar DCBX trocas. Por isso, o host deve ser configurado separadamente dos comutadores de rede, mas com configurações idênticas.

Se você optar por gerenciar DCB de um comutador, você ainda pode exibir as configurações no Windows Server 2016 usando comandos do Windows PowerShell.

##  <a name="important-dcb-functionality"></a>Funcionalidades importantes da DCB

A lista a seguir resume a funcionalidade fornecida pelo DCB.

1. Fornece interoperabilidade entre o DCB\-adaptadores de rede habilitados e o DCB\-comutadores com capacidade.

2. Fornece um transporte Ethernet sem perda entre um computador que executa o Windows Server 2016 e seu comutador vizinho através da ativação em prioridade\-com base em controle de fluxo no adaptador de rede.

3. Fornece a capacidade de alocar largura de banda para um controle de tráfego \(TC\) em termos percentuais, onde o TC pode consistir em uma ou mais classes de tráfego que são diferenciadas por classe de tráfego 802.1p \(prioridade\) indicadores.

4. Permite que os administradores do servidor ou administradores de rede atribuam um aplicativo a uma determinada classe de tráfego ou prioridade com base em protocolos bem conhecidos, porta TCP/UDP bem conhecida ou porta NetworkDirect usada por esse aplicativo.

5. Fornece gerenciamento de DCB através da instrumentação de gerenciamento do Windows do Windows Server 2016 \(WMI\) e o Windows PowerShell. Para obter mais informações, consulte a seção [comandos do Windows PowerShell para DCB](#bkmk_wps) mais adiante neste tópico, além dos tópicos a seguir.
    - [Componentes DCB fornecido pelo sistema](https://msdn.microsoft.com/windows/hardware/drivers/network/system-provided-dcb-components)
    - [Requisitos de QoS do NDIS para ponte de Data Center](https://msdn.microsoft.com/windows/hardware/drivers/network/ndis-qos-requirements-for-data-center-bridging)

6. Fornece gerenciamento de DCB através da diretiva de grupo do Windows Server 2016.

7. Oferece suporte a coexistência de qualidade de serviço do Windows Server 2016 \(QoS\) soluções.

>[!NOTE]
>Antes de usar qualquer RDMA over Converged Ethernet \(RoCE\) versão de RDMA, você deve habilitar o DCB. Enquanto não é necessário para o protocolo do IP ampla área RDMA \(iWARP\) redes, teste determinou que todos Ethernet\-com base em RDMA tecnologias a trabalharem melhor com DCB. Por isso, você deve considerar usar o DCB para implantações de RDMA iWARP. Para obter mais informações, consulte [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

##  <a name="practical-applications-of-dcb"></a>Aplicativos práticos de DCB

Muitas organizações têm grande Fiber Channel \(FC\) SAN \(SAN\) instalações para serviço de armazenamento. A SAN em FC exige adaptadores de rede especiais em servidores e comutadores FC na rede. Essas organizações normalmente também usam switches e adaptadores de rede Ethernet.

Na maioria dos casos, o hardware FC é significativamente mais caro de implantar do que o hardware Ethernet, o que resulta em grandes investimentos de capital. Além disso, o requisito para separado Ethernet SAN FC adaptador e comutador hardware de rede e requer espaço adicional, energia e resfriamento capacidade em um datacenter, o que resulta em despesas operacionais adicionais e contínuas.

De uma perspectiva de custo, é vantajoso para muitas organizações mesclar sua tecnologia de FC com sua Ethernet\-com base em solução de hardware para fornecer dados de serviços de rede e armazenamento.

### <a name="using-dcb-for-an-ethernet-based-converged-fabric-for-storage-and-data-networking"></a>Usando o DCB para Ethernet\-com base em uma malha convergida para o armazenamento e rede de dados

Para organizações que já tem uma grande SAN em FC mas querem migrar evitando investimentos adicionais em tecnologia de FC, o DCB permite que você crie com base em Ethernet convergida malha de armazenamento e rede de dados. Convergida do DCB malha pode reduzir o custo total futuras de propriedade \(TCO\) e simplificar o gerenciamento.

Para hosters que já adotaram ou que planejam adotar, o iSCSI como sua solução de armazenamento, o DCB pode fornecer hardware\-assistido por reserva de largura de banda para o tráfego de iSCSI para garantir o isolamento de desempenho. E, ao contrário de outras soluções proprietárias, o DCB é padrões\-com base - e, portanto, relativamente fácil de implantar e gerenciar em um ambiente de rede heterogênea.

Um Windows Server 2016\-implementação de DCB baseada no atenua muitos dos problemas que podem ocorrer quando convergido soluções de malha são fornecidas por vários fabricantes originais do equipamento \(OEMs\).

Implementações de soluções proprietárias fornecidas por vários OEMs podem não interagir umas com as outras, podem ser difíceis de gerenciar e normalmente têm agendas de manutenção de software diferente. 

Por outro lado, o Windows Server 2016 DCB é padrões\-com base, e você pode implantar e gerenciar o DCB em uma rede heterogênea.

## <a name="bkmk_wps"></a>Comandos do Windows PowerShell para DCB

Há comandos DCB Windows PowerShell para o Windows Server 2016 e Windows Server 2012 R2. Você pode usar todos os comandos do Windows Server 2012 R2 no Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandos do Windows Server 2016 Windows PowerShell do DCB

O tópico a seguir para o Windows Server 2016 fornece descrições de cmdlet do Windows PowerShell e a sintaxe para todos os Data Center Bridging \(DCB\) qualidade de serviço \(QoS\)\-cmdlets específicos. Ela lista os cmdlets em ordem alfabética com base no verbo no início do cmdlet.

- [Módulo DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Comandos do Windows Server 2012 R2 Windows PowerShell do DCB

O tópico a seguir para o Windows Server 2012 R2 fornece descrições de cmdlet do Windows PowerShell e a sintaxe para todos os Data Center Bridging \(DCB\) qualidade de serviço \(QoS\)\-cmdlets específicos. Ela lista os cmdlets em ordem alfabética com base no verbo no início do cmdlet.

- [Data Center Bridging (DCB) Cmdlets de qualidade de serviço (QoS) no Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
