---
title: Descrições de recursos para máquinas virtuais de Linux e FreeBSD no Hyper-V
description: Descreve os recursos que afetam os componentes principais, como rede, armazenamento, memória ao usar o Linux e FreeBSD em uma máquina virtual
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a9ee931d-91fc-40cf-9a15-ed6fa6965cb6
author: shirgall
ms.author: kathydav
ms.date: 10/03/2016
ms.openlocfilehash: a574275f6d3495a9cc9bff36fa785f28a7cd8d6f
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222880"
---
# <a name="feature-descriptions-for-linux-and-freebsd-virtual-machines-on-hyper-v"></a>Descrições de recursos para máquinas virtuais de Linux e FreeBSD no Hyper-V

>Aplica-se a: Windows Server 2016, Hyper-V Server 2016, Windows Server 2012 R2, Hyper-V Server 2012 R2, Windows Server 2012, do Hyper-V Server 2012, Windows Server 2008 R2, Windows 10, Windows 8.1, Windows 8, Windows 7.1, Windows 7

Este artigo descreve os recursos disponíveis em componentes como core, rede, armazenamento e memória ao usar o Linux e FreeBSD em uma máquina virtual.

## <a name="core"></a>Core

|**Recurso**|**Descrição**|
|-|-|
|Desligamento integrado|Com esse recurso, um administrador pode desligar máquinas virtuais do Gerenciador do Hyper-V. Para obter mais informações, consulte [desligamento do sistema operacional](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_Shutdown).|
|Sincronização de hora|Esse recurso garante que o tempo de manutenção em uma máquina virtual é mantido sincronizado com o tempo de manutenção no host. Para obter mais informações, consulte [sincronização de hora](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_time).|
|Hora precisa do Windows Server 2016|Esse recurso permite que o convidado usar o recurso de tempo preciso do Windows Server 2016, o que melhora a sincronização de horário com o host para 1ms precisão. Para obter mais informações, consulte [tempo preciso do Windows Server 2016](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-2016-accurate-time)|
|Suporte Multiprocessing|Com esse recurso, uma máquina virtual pode usar vários processadores no host por meio da configuração de várias CPUs virtuais.|
|Pulsação|Com esse recurso, o host pode acompanhar o estado da máquina virtual. Para obter mais informações, consulte [pulsação](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_heartbeat).|
|Suporte a mouse integrado|Com esse recurso, você pode usar um mouse na área de trabalho da máquina virtual e também usar o mouse perfeitamente entre a área de trabalho do Windows Server e o console do Hyper-V para a máquina virtual.|
|Dispositivo de armazenamento específico do Hyper-V|Esse recurso concede acesso de alto desempenho para os dispositivos de armazenamento que estão anexados a uma máquina virtual.|
|Dispositivo de rede específico do Hyper-V|Esse recurso concede acesso de alto desempenho para os adaptadores de rede que estão anexados a uma máquina virtual.|

## <a name="networking"></a>Rede

|**Recurso**|**Descrição**|
|-|-|
|Quadros jumbo|Com esse recurso, um administrador pode aumentar o tamanho de estruturas de rede, além de 1.500 bytes, que leva a um aumento significativo no desempenho da rede.|
|Marcação de VLAN e entroncamento|Esse recurso permite que você configure o tráfego de LAN (VLAN) virtual para máquinas virtuais.|
|Migração ao vivo|Com esse recurso, você pode migrar uma máquina virtual de um host para outro host. Para obter mais informações, consulte [visão geral de migração ao vivo Máquina Virtual](https://technet.microsoft.com/library/hh831435.aspx).|
|Injeção de endereço IP estático|Com esse recurso, você pode replicar o endereço IP estático de uma máquina virtual depois que tiver feito failover à sua réplica em um host diferente. Tal replicação IP garante que as cargas de trabalho de rede continuem a funcionar perfeitamente após um evento de failover.|
|vRSS (Virtual Receive Side Scaling)|Distribui a carga de um adaptador de rede virtual entre vários processadores virtuais em uma máquina virtual. Para obter mais informações, consulte [Virtual Receive-side Scaling in Windows Server 2012 R2](https://technet.microsoft.com/library/dn383582.aspx).|
|Descarregamento de soma de verificação e de segmentação TCP|Transferências de segmentação e soma de verificação funcionam da CPU convidado para o adaptador de rede ou switch de virtual do host durante as transferências de dados de rede.|
|Grande receber descarregamento (LRO)|Aumenta a produtividade entrada de conexões de alta largura de banda por agregar vários pacotes em um buffer maior, diminuindo a sobrecarga da CPU.|
|SR-IOV|Dispositivos de e/s de raiz únicos usam DDA para permitir o acesso de convidados para partes de placas NIC específicas, permitindo uma latência reduzida e maior taxa de transferência. SR-IOV exige drivers função atualizado físico (PF) no host e a função virtual (FV) na convidada.|

## <a name="storage"></a>Armazenamento

|**Recurso**|**Descrição**|
|-|-|
|Redimensionamento VHDX|Com esse recurso, um administrador pode redimensionar um arquivo. vhdx de tamanho fixo que é anexado a uma máquina virtual. Para obter mais informações, consulte [Online Virtual Hard Disk redimensionamento Overview](https://technet.microsoft.com/library/dn282286.aspx).|
|Fibre Channel Virtual|Com esse recurso, as máquinas virtuais pode reconhecer um dispositivo de canal de fibra e montá-lo de forma nativa. Para obter mais informações, consulte [visão geral do Hyper-V Virtual Fibre Channel](https://technet.microsoft.com/library/hh831413.aspx).|
|Backup de máquina virtual ao vivo|Esse recurso facilita o backup de máquinas virtuais do tempo de inatividade zero.<br /><br />Observe que a operação de backup não terão êxito se a máquina virtual possui discos de rígidos virtuais (VHDs) que são hospedados no armazenamento remoto como um compartilhamento de bloco de mensagens de servidor (SMB) ou um volume iSCSI. Além disso, certifique-se de que o destino de backup não reside no mesmo volume como volume de backup.|
|Suporte de CORTE|Dicas de CORTE notificam a unidade que determinados setores que foram alocados anteriormente não são mais necessários pelo aplicativo e podem ser limpos. Esse processo normalmente é usado quando um aplicativo faz com que as alocações de espaço grande por meio de um arquivo e, em seguida, gerencia automaticamente as alocações para o arquivo, por exemplo, para arquivos de disco rígido virtual.|
|SCSI WWN|O driver storvsc extrai informações de nome WWN (World Wide) de porta e do nó de dispositivos conectados à máquina virtual e cria os arquivos de sysfs apropriado. |

## <a name="memory"></a>Memória

|**Recurso**|**Descrição**|
|-|-|
|Suporte do Kernel PAE|Tecnologia de endereço PAE (extensão) físico permite que um kernel de 32 bits acessar um espaço de endereço físico que é maior que 4GB. Distribuições de Linux mais antigas, como RHEL 5.x usados para enviar um kernel separada que estava PAE habilitada. Distribuições mais recentes como o RHEL 6. x tem pré-criadas suporte PAE.|
|Configuração de lacuna MMIO|Com esse recurso, os fabricantes de dispositivo podem configurar o local do espaço de memória mapeada e/s (MMIO). A lacuna MMIO normalmente é usada para dividir a memória física disponível entre apenas suficiente sistemas operacionais de um dispositivo (JeOS) e a infraestrutura de software reais que alimenta o dispositivo.|
|Memória dinâmica - quente|O host pode aumentar ou diminuir a quantidade de memória disponível para uma máquina virtual enquanto ela estiver em operação dinamicamente. Antes de provisionar, o administrador permite memória dinâmica no painel de configurações da máquina Virtual e especifique a memória de inicialização, memória mínima e máxima de memória para a máquina virtual. Quando a máquina virtual está em operação de que memória dinâmica não podem ser desabilitada e somente as configurações de mínimo e máximo pode ser alterado. (Ele é uma prática recomendada para especificar esses tamanhos de memória como múltiplos de 128MB).<br /><br />Quando a máquina virtual é iniciada pela primeira vez disponível a memória é igual a **memória de inicialização**. Conforme aumenta a demanda de memória devido a cargas de trabalho de aplicativo o Hyper-V dinamicamente poderá alocar mais memória à máquina virtual por meio do mecanismo quente, se compatível com essa versão do kernel. A quantidade máxima de memória alocada é limitada pelo valor de **memória máxima** parâmetro.<br /><br />Na guia de memória do Hyper-V manager exibirá a quantidade de memória atribuída à máquina virtual, mas as estatísticas de memória dentro da máquina virtual mostrará a maior quantidade de memória alocada.<br /><br />Para obter mais informações, consulte [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Memória dinâmica - inflação|O host pode aumentar ou diminuir a quantidade de memória disponível para uma máquina virtual enquanto ela estiver em operação dinamicamente. Antes de provisionar, o administrador permite memória dinâmica no painel de configurações da máquina Virtual e especifique a memória de inicialização, memória mínima e máxima de memória para a máquina virtual. Quando a máquina virtual está em operação de que memória dinâmica não podem ser desabilitada e somente as configurações de mínimo e máximo pode ser alterado. (Ele é uma prática recomendada para especificar esses tamanhos de memória como múltiplos de 128MB).<br /><br />Quando a máquina virtual é iniciada pela primeira vez disponível a memória é igual a **memória de inicialização**. Conforme aumenta a demanda de memória devido a cargas de trabalho de aplicativo do Hyper-V dinamicamente poderá alocar mais memória à máquina virtual por meio do mecanismo de quente (acima). Conforme a demanda de memória diminui o Hyper-V pode automaticamente desprovisionar memória da máquina virtual por meio do mecanismo de balão. Hyper-V não cancelará memória abaixo de **memória mínima** parâmetro.<br /><br />Na guia de memória do Hyper-V manager exibirá a quantidade de memória atribuída à máquina virtual, mas as estatísticas de memória dentro da máquina virtual mostrará a maior quantidade de memória alocada.<br /><br />Para obter mais informações, consulte [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx).<br /><br />|
|Redimensionamento de memória de tempo de execução|Um administrador pode definir a quantidade de memória disponível para uma máquina virtual enquanto ela estiver em operação, aumentando a memória ("quente") ou reduzi-lo ("Hot remover"). Memória é retornada para o Hyper-V por meio do driver de balão (consulte "Inflação de – de memória dinâmica"). O driver de balão mantém uma quantidade mínima de memória livre após inchamento, chamado "base", portanto, atribuído a memória não pode ser reduzida abaixo a demanda atual mais essa quantidade de chão. Na guia de memória do Hyper-V manager exibirá a quantidade de memória atribuída à máquina virtual, mas as estatísticas de memória dentro da máquina virtual mostrará a maior quantidade de memória alocada. (Ele é uma prática recomendada para especificar valores de memória como múltiplos de 128MB).|

## <a name="video"></a>Vídeo

|**Recurso**|**Descrição**|
|-|-|
|Dispositivo de vídeo específico do Hyper-V|Esse recurso fornece gráficos de alto desempenho e a resolução superior para máquinas virtuais. Este dispositivo não fornece recursos de modo de sessão avançado ou o RemoteFX.|

## <a name="miscellaneous"></a>Diversos

|**Recurso**|**Descrição**|
|-|-|
|Exchange KVP (par chave-valor)|Esse recurso fornece serviço do exchange (KVP) para máquinas virtuais de um par chave/valor. Geral, os administradores usam o mecanismo KVP para realizar a leitura e gravação de operações de dados personalizados em uma máquina virtual. Para obter mais informações, consulte [troca de dados: Usando pares chave-valor para compartilhar informações entre o host e convidado no Hyper-V](https://technet.microsoft.com/library/dn798287.aspx).|
|Interrupção não mascarável|Com esse recurso, um administrador pode emitir não mascarável interrompe NMI () para uma máquina virtual. NMIs são úteis para obter os despejos de memória dos sistemas operacionais que se tornaram sem resposta devido a bugs de aplicativos. Esses despejos de memória podem ser diagnosticados após a reinicialização.|
|Cópia do arquivo do host para a convidada|Esse recurso permite que os arquivos a serem copiados do computador físico host para máquinas virtuais convidadas sem usar o adaptador de rede. Para obter mais informações, consulte [serviços de convidado](https://technet.microsoft.com/library/dn798297(WS.11).aspx#BKMK_guest).|
|comando lsvmbus|Esse comando obtém informações sobre os dispositivos no barramento de máquina virtual do Hyper-V (VMBus) semelhantes aos comandos de informações como lspci.|
|Soquetes do Hyper-V|Isso é um canal de comunicação adicional entre o sistema operacional host e convidado. Para carregar e usar o módulo de kernel de soquetes do Hyper-V, consulte [tornar seus próprios serviços de integração](https://msdn.microsoft.com/virtualization/hyperv_on_windows/develop/make_mgmt_service).|
|Passagem/DDA de PCI|Com o Windows Server 2016 os administradores podem passar por dispositivos PCI Express por meio do mecanismo de atribuição de dispositivo discretos. Dispositivos comuns são placas de rede, placas de vídeo e dispositivos de armazenamento especial. A máquina virtual exigirá o driver apropriado para usar o hardware exposto. O hardware deve ser atribuído à máquina virtual para que ele seja usado.<br /><br />Para obter mais informações, consulte [atribuição de dispositivo discretos – descrição e o plano de fundo](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/).<br /><br />DDA é um pré-requisito para a rede da SR-IOV. Portas virtuais precisará ser atribuído à máquina virtual e a máquina virtual deve usar os drivers corretos de função Virtual (FV) multiplexação de dispositivo.|

## <a name="generation-2-virtual-machines"></a>Máquinas virtuais de 2ª geração

|**Recurso**|**Descrição**|
|-|-|
|Inicialização usando UEFI|Esse recurso permite que as máquinas virtuais para inicialização usando Unified Extensible Firmware Interface (UEFI).<br /><br />Para obter mais informações, consulte [Visão geral da máquina virtual de geração 2](https://technet.microsoft.com/library/dn282285.aspx).|
|Inicialização segura|Esse recurso permite que máquinas virtuais para usar o modo de inicialização segura de UEFI com base. Quando uma máquina virtual é inicializada no modo seguro, vários componentes do sistema operacional são verificados usando assinaturas presentes no repositório de dados de UEFI.<br /><br />Para saber mais, confira [Inicialização Segura](https://technet.microsoft.com/library/dn486875.aspx).|

## <a name="see-also"></a>Consulte também

* [Suporte para CentOS e Red Hat Enterprise Linux máquinas virtuais do Hyper-V](Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Debian com suporte no Hyper-V](Supported-Debian-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais do Oracle Linux com suporte no Hyper-V](Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md)

* [De máquinas virtuais SUSE com suporte no Hyper-V](Supported-SUSE-virtual-machines-on-Hyper-V.md)

* [Máquinas virtuais do Ubuntu com suporte no Hyper-V](Supported-Ubuntu-virtual-machines-on-Hyper-V.md)

* [Máquinas de virtuais FreeBSD com suporte no Hyper-V](Supported-FreeBSD-virtual-machines-on-Hyper-V.md)

* [Práticas recomendadas para executar o Linux no Hyper-V](Best-Practices-for-running-Linux-on-Hyper-V.md)

* [Práticas recomendadas para executar o FreeBSD no Hyper-V](Best-practices-for-running-FreeBSD-on-Hyper-V.md)