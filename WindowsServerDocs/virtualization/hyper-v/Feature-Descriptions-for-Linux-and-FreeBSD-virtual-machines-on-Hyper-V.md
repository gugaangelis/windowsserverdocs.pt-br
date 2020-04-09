---
title: Descrições de recursos para máquinas virtuais Linux e FreeBSD no Hyper-V
description: Descreve os recursos que afetam os componentes principais, como rede, armazenamento, memória ao usar o Linux e o FreeBSD em uma máquina virtual
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: d3dfcd3bdd4c30e5bd1a51d8e842aeca8831babf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853269"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>Descrições de recursos para máquinas virtuais Linux e FreeBSD no Hyper-V

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7,1, Windows 7

Este artigo descreve os recursos disponíveis em componentes como núcleo, rede, armazenamento e memória ao usar o Linux e o FreeBSD em uma máquina virtual.

## <a name="core"></a>Core

|**Recurso**|**Descrição**|
|-|-|
|Desligamento integrado|Com esse recurso, um administrador pode desligar as máquinas virtuais do Gerenciador do Hyper-V. Para obter mais informações, consulte [desligar sistema operacional](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_Shutdown).|
|Sincronização de horário|Esse recurso garante que o tempo mantido dentro de uma máquina virtual seja mantido sincronizado com o tempo mantido no host. Para obter mais informações, consulte [sincronização de horário](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_time).|
|Tempo preciso do Windows Server 2016|Esse recurso permite que o convidado use o recurso de tempo preciso do Windows Server 2016, o que melhora a sincronização de horário com o host para 1 MS precisão. Para obter mais informações, consulte [tempo preciso do Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-2016-accurate-time)|
|Suporte a multiprocessamento|Com esse recurso, uma máquina virtual pode usar vários processadores no host Configurando várias CPUs virtuais.|
|Pulsação|Com esse recurso, o host pode acompanhar o estado da máquina virtual. Para obter mais informações, consulte [pulsação](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_heartbeat).|
|Suporte integrado ao mouse|Com esse recurso, você pode usar um mouse na área de trabalho de uma máquina virtual e também usar o mouse diretamente entre a área de trabalho do Windows Server e o console do Hyper-V para a máquina virtual.|
|Dispositivo de armazenamento específico do Hyper-V|Esse recurso concede acesso de alto desempenho a dispositivos de armazenamento que estão conectados a uma máquina virtual.|
|Dispositivo de rede específico do Hyper-V|Esse recurso concede acesso de alto desempenho aos adaptadores de rede que estão anexados a uma máquina virtual.|

## <a name="networking"></a>Rede

|**Recurso**|**Descrição**|
|-|-|
|Quadros jumbo|Com esse recurso, um administrador pode aumentar o tamanho de quadros de rede além de 1500 bytes, o que leva a um aumento significativo no desempenho da rede.|
|Marcação e entroncamento de VLAN|Esse recurso permite que você configure o tráfego de rede virtual (VLAN) para máquinas virtuais.|
|Migração ao vivo|Com esse recurso, você pode migrar uma máquina virtual de um host para outro host. Para obter mais informações, consulte [visão geral da máquina Virtual migração ao vivo](https://technet.microsoft.com/library/hh831435.aspx).|
|Injeção de IP estático|Com esse recurso, você pode replicar o endereço IP estático de uma máquina virtual após o failover para sua réplica em um host diferente. Essa replicação de IP garante que as cargas de trabalho de rede continuem a funcionar sem problemas após um evento de failover.|
|vRSS (virtual Receive no lado do recebimento)|Espalha a carga de um adaptador de rede virtual entre vários processadores virtuais em uma máquina virtual. Para obter mais informações, consulte [dimensionamento virtual no lado do recebimento no Windows Server 2012 R2](https://technet.microsoft.com/library/dn383582.aspx).|
|Segmentação de TCP e descarregamentos de soma de verificação|Transfere o trabalho de segmentação e soma de verificação da CPU convidada para o comutador virtual do host ou adaptador de rede durante transferências de dados de rede.|
|Descarregamento de recebimento grande (LRO)|Aumenta a taxa de transferência de entrada de conexões de largura de banda alta agregando vários pacotes em um buffer maior, diminuindo a sobrecarga da CPU.|
|SR-IOV|Dispositivos de e/s de raiz única usam DDA para permitir que convidados acessem partes de placas NIC específicas, permitindo latência reduzida e maior taxa de transferência. O SR-IOV requer drivers de PF (função física atualizada) nos drivers de host e de função virtual (FV) no convidado.|

## <a name="storage"></a>Armazenamento

|**Recurso**|**Descrição**|
|-|-|
|Redimensionamento de VHDX|Com esse recurso, um administrador pode redimensionar um arquivo. vhdx de tamanho fixo que está anexado a uma máquina virtual. Para obter mais informações, consulte [visão geral do redimensionamento de disco rígido virtual online](https://technet.microsoft.com/library/dn282286.aspx).|
|Fibre Channel Virtual|Com esse recurso, as máquinas virtuais podem reconhecer um dispositivo de Fiber Channel e montá-lo nativamente. Para obter mais informações, consulte [visão geral do Fibre Channel virtual do Hyper-V](https://technet.microsoft.com/library/hh831413.aspx).|
|Backup de máquina virtual ao vivo|Esse recurso facilita o backup sem tempo de inatividade de máquinas virtuais em tempo real.<p>Observe que a operação de backup não terá sucesso se a máquina virtual tiver VHDs (discos rígidos virtuais) hospedados no armazenamento remoto, como um compartilhamento de protocolo SMB ou um volume iSCSI. Além disso, certifique-se de que o destino de backup não resida no mesmo volume que o volume de backup.|
|Suporte a corte|As dicas de corte notificam a unidade de que determinados setores que foram alocados anteriormente não são mais exigidos pelo aplicativo e podem ser limpos. Esse processo é normalmente usado quando um aplicativo faz alocações de espaço grandes por meio de um arquivo e, em seguida, gerencia automaticamente as alocações para o arquivo, por exemplo, para arquivos de disco rígido virtual.|
|WWN DO SCSI|O driver storvsc extrai informações de WWN (World Wide Name) da porta e do nó de dispositivos conectados à máquina virtual e cria os arquivos sysfs apropriados. |

## <a name="memory"></a>Memória

|**Recurso**|**Descrição**|
|-|-|
|Suporte ao kernel de PAE|A tecnologia de extensão de endereço físico (PAE) permite que um kernel de 32 bits acesse um espaço de endereço físico maior do que 4GB. Distribuições mais antigas do Linux, como RHEL 5. x, usadas para enviar um kernel separado que foi habilitado para PAE. Distribuições mais recentes, como RHEL 6. x, têm suporte de PAE pré-compilado.|
|Configuração da lacuna de MMIO|Com esse recurso, os fabricantes de dispositivos podem configurar o local da lacuna de e/s mapeada da memória (MMIO). A lacuna MMIO normalmente é usada para dividir a memória física disponível entre os sistemas operacionais (JeOS) de um dispositivo e a infraestrutura de software real que alimenta o dispositivo.|
|Memória Dinâmica-adição a quente|O host pode aumentar ou diminuir dinamicamente a quantidade de memória disponível para uma máquina virtual enquanto ela está em operação. Antes do provisionamento, o administrador habilita Memória Dinâmica no painel de configurações da máquina virtual e especifica a memória de inicialização, memória mínima e memória máxima para a máquina virtual. Quando a máquina virtual está na operação Memória Dinâmica não pode ser desabilitada e apenas as configurações mínima e máxima podem ser alteradas. (É uma prática recomendada especificar esses tamanhos de memória como múltiplos de 128 MB.)<p>Quando a máquina virtual é iniciada pela primeira vez, a memória disponível é igual à **memória de inicialização**. À medida que a demanda de memória aumenta devido a cargas de trabalho de aplicativo, o Hyper-V pode alocar dinamicamente mais memória para a máquina virtual por meio do mecanismo de adição automática, se houver suporte para essa versão do kernel. A quantidade máxima de memória alocada é limitada pelo valor do parâmetro **Maximum Memory** .<p>A guia memória do Gerenciador do Hyper-V exibirá a quantidade de memória atribuída à máquina virtual, mas as estatísticas de memória na máquina virtual mostrarão a maior quantidade de memória alocada.<p>Para obter mais informações, consulte [visão geral do memória dinâmica do Hyper-V](https://technet.microsoft.com/library/hh831766.aspx).<p>|
|Memória Dinâmica-balões|O host pode aumentar ou diminuir dinamicamente a quantidade de memória disponível para uma máquina virtual enquanto ela está em operação. Antes do provisionamento, o administrador habilita Memória Dinâmica no painel de configurações da máquina virtual e especifica a memória de inicialização, memória mínima e memória máxima para a máquina virtual. Quando a máquina virtual está na operação Memória Dinâmica não pode ser desabilitada e apenas as configurações mínima e máxima podem ser alteradas. (É uma prática recomendada especificar esses tamanhos de memória como múltiplos de 128 MB.)<p>Quando a máquina virtual é iniciada pela primeira vez, a memória disponível é igual à **memória de inicialização**. À medida que a demanda de memória aumenta devido a cargas de trabalho de aplicativo, o Hyper-V pode alocar dinamicamente mais memória para a máquina virtual por meio do mecanismo de adição automática (acima). Como a demanda de memória diminui o Hyper-V pode desprovisionar automaticamente a memória da máquina virtual por meio do mecanismo de balão. O Hyper-V não desprovisionará memória abaixo do parâmetro de **memória mínima** .<p>A guia memória do Gerenciador do Hyper-V exibirá a quantidade de memória atribuída à máquina virtual, mas as estatísticas de memória na máquina virtual mostrarão a maior quantidade de memória alocada.<p>Para obter mais informações, consulte [visão geral do memória dinâmica do Hyper-V](https://technet.microsoft.com/library/hh831766.aspx).<p>|
|Redimensionamento de memória de Runtime|Um administrador pode definir a quantidade de memória disponível para uma máquina virtual enquanto ela estiver em operação, aumentando a memória ("adição automática") ou reduzindo-a ("remoção automática"). A memória é retornada ao Hyper-V por meio do driver de balão (consulte "Memória Dinâmica-Balloon"). O driver de balão mantém uma quantidade mínima de memória livre após o balão, chamado de "andar", portanto, a memória atribuída não pode ser reduzida abaixo da demanda atual mais essa quantidade de piso. A guia memória do Gerenciador do Hyper-V exibirá a quantidade de memória atribuída à máquina virtual, mas as estatísticas de memória na máquina virtual mostrarão a maior quantidade de memória alocada. (É uma prática recomendada especificar valores de memória como múltiplos de 128 MB.)|

## <a name="video"></a>Vídeo

|**Recurso**|**Descrição**|
|-|-|
|Dispositivo de vídeo específico do Hyper-V|Esse recurso fornece gráficos de alto desempenho e resolução superior para máquinas virtuais. Este dispositivo não fornece o modo de sessão avançado ou recursos do RemoteFX.|

## <a name="miscellaneous"></a>Diversos

|**Recurso**|**Descrição**|
|-|-|
|Troca de KVP (par chave-valor)|Esse recurso fornece um serviço do Exchange KVP (par chave/valor) para máquinas virtuais. Normalmente, os administradores usam o mecanismo KVP para executar operações de dados personalizados de leitura e gravação em uma máquina virtual. Para obter mais informações, consulte [troca de dados: usando pares chave-valor para compartilhar informações entre o host e o convidado no Hyper-V](https://technet.microsoft.com/library/dn798287.aspx).|
|Interrupção não mascarável|Com esse recurso, um administrador pode emitir NMI (interrupções não mascaráveis) para uma máquina virtual. NMIs são úteis para obter despejos de memória de sistemas operacionais que se tornaram sem resposta devido a bugs de aplicativos. Esses despejos de memória podem ser diagnosticados após a reinicialização.|
|Cópia de arquivo do host para o convidado|Esse recurso permite que os arquivos sejam copiados do computador físico do host para as máquinas virtuais convidadas sem usar o adaptador de rede. Para obter mais informações, consulte [serviços de convidado](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_guest).|
|comando lsvmbus|Este comando obtém informações sobre dispositivos no barramento de máquina virtual (VMBus) do Hyper-V semelhante a comandos de informações como lspci.|
|Soquetes do Hyper-V|Esse é um canal de comunicação adicional entre o host e o sistema operacional convidado. Para carregar e usar o módulo de kernel de soquetes do Hyper-V, consulte [criar seus próprios serviços de integração](https://msdn.microsoft.com/virtualization/hyperv_on_windows/develop/make_mgmt_service).|
|Passagem de PCI/DDA|Com os administradores do Windows Server 2016, é possível passar por dispositivos PCI Express por meio do mecanismo de atribuição de dispositivo discreto. Dispositivos comuns são placas de rede, placas gráficas e dispositivos de armazenamento especiais. A máquina virtual exigirá o driver apropriado para usar o hardware exposto. O hardware deve ser atribuído à máquina virtual para ser usado.<p>Para obter mais informações, consulte [atribuição de dispositivo discreto-descrição e plano de fundo](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/).<p>DDA é um pré-requisito para a rede SR-IOV. As portas virtuais precisarão ser atribuídas à máquina virtual e a máquina virtual deverá usar os drivers de FV (função virtual) corretos para a multiplexação de dispositivo.|

## <a name="generation-2-virtual-machines"></a>Máquinas virtuais de 2ª geração

|**Recurso**|**Descrição**|
|-|-|
|Inicializar usando UEFI|Esse recurso permite que as máquinas virtuais inicializem usando o Unified Extensible Firmware Interface (UEFI).<p>Para obter mais informações, consulte [Visão geral da máquina virtual de geração 2 ](https://technet.microsoft.com/library/dn282285.aspx).|
|Inicialização segura|Esse recurso permite que as máquinas virtuais usem o modo de inicialização segura baseado em UEFI. Quando uma máquina virtual é inicializada no modo seguro, vários componentes do sistema operacional são verificados usando as assinaturas presentes no armazenamento de dados UEFI.<p>Para saber mais, confira [Inicialização Segura](https://technet.microsoft.com/library/dn486875.aspx).|

## <a name="see-also"></a>Consulte também

* [Máquinas virtuais CentOS e Red Hat Enterprise Linux com suporte no Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Debian com suporte no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Práticas recomendadas para executar o FreeBSD no Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)