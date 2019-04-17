---
title: Adaptadores de rede de ajuste de desempenho
description: Este tópico faz parte do guia ajuste de desempenho do subsistema de rede para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1c6d36966ce6e2d407b6568e16946745256ee69b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="performance-tuning-network-adapters"></a>Adaptadores de rede de ajuste de desempenho

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para adaptadores de rede de ajuste de desempenho que estão instalados em computadores que executam o Windows Server 2016.

Determinando as configurações de ajuste corretas para o adaptador de rede dependem as seguintes variáveis:

- O adaptador de rede e o conjunto de recursos  

- O tipo de carga de trabalho realizado pelo servidor  

- Os recursos de hardware e software de servidor  

- Suas metas de desempenho para o servidor  

Se o adaptador de rede oferece opções de ajuste, você pode otimizar o uso de recursos e a taxa de transferência de rede para obter throughput ideal com base nos parâmetros descritos acima.  

As seções a seguir descrevem algumas das opções de ajuste de desempenho.  

##  <a name="bkmk_offload"></a>Habilitar recursos de descarregamento

Ativar recursos de descarregamento de adaptador de rede geralmente é útil. Às vezes, entretanto, o adaptador de rede não é potente o suficiente para lidar com os recursos de descarregamento com alta taxa de transferência.

>[!IMPORTANT]
>Não use os recursos de descarregamento **descarga de tarefa de IPsec** ou **Descarregue o TCP chaminés **. Essas tecnologias foram preteridas no Windows Server 2016 e podem afetar negativamente o servidor e o desempenho de rede. Além disso, essas tecnologias podem não ser suportadas pela Microsoft no futuro.

Por exemplo, permitindo descarregamento de segmentação pode reduzir a taxa de transferência sustentável máxima em alguns adaptadores de rede por causa de recursos limitados de hardware. No entanto, se a taxa de transferência reduzida não deve ser uma limitação, você deve habilitar os recursos de descarregamento, até mesmo para esse tipo de adaptador de rede.

>[!NOTE]
> Alguns adaptadores de rede exigem recursos de descarregamento ser habilitadas independentemente para enviar e receber caminhos.

##  <a name="bkmk_rss_web"></a>Habilitando receber lado dimensionamento (RSS) para servidores Web

RSS pode melhorar o desempenho e escalabilidade da web quando houver menos adaptadores de rede que processadores lógicos no servidor. Quando todo o tráfego da web está passando por adaptadores de rede compatíveis com RSS, solicitações da web de diferentes conexões podem ser processadas simultaneamente em CPUs diferentes.

É importante observar que devido a lógica no RSS e HTTPS \(HTTP\) para distribuição da carga de protocolo, o desempenho pode ser degradado gravemente se um adaptador de rede não oferece suporte a RSS aceita o tráfego da web em um servidor que tem um ou mais adaptadores de rede compatíveis com RSS. Neste caso, você deve usar os adaptadores de rede compatíveis com RSS ou desabilitar RSS nas propriedades do adaptador de rede **propriedades avançadas** guia. Para determinar se um adaptador de rede é capaz de RSS, você pode exibir as informações de RSS nas propriedades do adaptador de rede **propriedades avançadas** guia.

### <a name="rss-profiles-and-rss-queues"></a>Perfis RSS e filas RSS

O perfil RSS predefinidas padrão é NUMA estático, que altera o comportamento padrão de versões anteriores do sistema operacional. Para começar a usar perfis de RSS, você pode analisar os perfis disponíveis para entender quando eles são benéficos e como eles se aplicam ao seu ambiente de rede e o hardware.

Por exemplo, se você abrir o Gerenciador de tarefas e examinar os processadores lógicos em seu servidor, e eles parecem estar mal alocada para receber tráfego, você pode tentar aumentar o número de filas RSS do padrão de 2 para o máximo que é compatível com o adaptador de rede. O adaptador de rede pode ter opções para alterar o número de filas RSS como parte do driver.

##  <a name="bkmk_resources"></a>Aumentar os recursos de adaptador de rede

Para adaptadores de rede que permitem uma configuração manual de recursos, como receberem e enviar buffers, você deve aumentar os recursos alocados. 

Alguns adaptadores de rede baixo seus buffers de recebimento para conservar alocada memória do host. O valor baixo resulta em pacotes removidos e uma redução de desempenho. Portanto, para cenários de uso intenso recebimento, recomendamos que você aumentar o valor de buffer de recebimento para o máximo.

>[!NOTE]
>Se um adaptador de rede não exponha configuração manual de recurso, ou dinamicamente configura os recursos ou os recursos são definidos como um valor fixo que não pode ser alterado.

### <a name="enabling-interrupt-moderation"></a>Habilitando a moderação de interrupção

Para controlar a moderação de interrupção, alguns adaptadores de rede expor níveis de moderação de interrupção diferentes, unindo parâmetros de buffer (às vezes separadamente para enviar e receber buffers), ou ambos.

Você deve considerar moderação de interrupção para cargas de trabalho vinculado à CPU e leve em consideração a compensação entre a economia de CPU de host e a latência versus o host maior economia de CPU devido a interrupções de mais e menos latência. Se o adaptador de rede não executa a moderação de interrupção, mas ela expõe buffer unir, aumentando o número de buffers agrupados permite mais buffers por enviar ou recebe, que melhora o desempenho.

##  <a name="bkmk_low"></a>Ajuste de desempenho para processamento de pacotes de baixa latência

Muitos adaptadores de rede oferecem opções para otimizar a latência induzidas pelo sistema operacional. Latência é o tempo decorrido entre o driver de rede processar um pacote de entrada e o driver de rede enviar o pacote novamente. Geralmente, esse tempo é medido em microssegundos. Para comparação, o tempo de transmissão de transmissões de pacote longas distâncias geralmente é medido em milissegundos \ (uma ordem de magnitude larger\). Esse ajuste não reduz o tempo que um pacote passa em trânsito.

A seguir estão algumas sugestões para redes microssegundos confidenciais de ajuste de desempenho.

- Defina os BIOS do computador **alto desempenho**, com estados C desabilitados. No entanto, observe que se trata de sistema e BIOS dependentes, e alguns sistemas fornecerá maior desempenho se os controles de sistema operacional de gerenciamento de energia. Você pode verificar e ajustar suas configurações de gerenciamento de energia de **configurações** ou usando o **powercfg** comando. Para obter mais informações, consulte [opções de linha de comando de Powercfg](https://technet.microsoft.com/library/cc748940.aspx)

- Defina o perfil de gerenciamento de energia do sistema operacional como **alto desempenho sistema**. Observe que isso não funcionarão corretamente se o sistema BIOS tiver sido definido para desabilitar o controle do sistema operacional de gerenciamento de energia.

- Habilite descarrega estático, por exemplo, somas de verificação de UDP, TCP somas de verificação e enviar grandes descarregamento LSO ().

- Habilite RSS se o tráfego é multi-transmitidas, como receber multicast de grande volume.

-   Desabilitar o **moderação interromper** configuração para drivers de placa de rede que exigem a mais baixa latência possível. Lembre-se de que isso pode usar mais tempo de CPU e ele representa um equilíbrio.

- Manipule as interrupções de adaptador de rede e DPCs em um processador de núcleo que compartilha o cache de CPU com o núcleo do que está sendo usado pelo programa (thread de usuário) que está controlando o pacote. Ajuste de afinidade de CPU pode ser usado para direcionar um processo para determinados processadores lógicos em conjunto com a configuração de RSS para realizar essa tarefa. Usar o mesmo núcleo para o thread do modo de interrupção, DPC e usuário apresenta pior desempenho como carga aumenta, porque o thread, ISR e DPC disputem o uso do núcleo.

##  <a name="bkmk_smi"></a>Interrupções de gerenciamento do sistema

Muitos sistemas de hardware usam \(SMI\) interrompe de gerenciamento de sistema para uma variedade de funções de manutenção, inclusive relatórios de erros de memória \(ECC\) do código de correção de erro, compatibilidade, controle do ventilador e BIOS herdado USB controlados gerenciamento de energia. 

O SMI é a interrupção de prioridade mais alta do sistema e coloca a CPU em um modo de gerenciamento, que antecipa todas as outras atividades enquanto ele é executado em uma rotina do serviço de interrupção, normalmente contida no BIOS.

Infelizmente, isso pode resultar em picos de latência de 100 microssegundos ou mais. 

Se você precisar atingir a mais baixa latência, você deve solicitar uma versão do BIOS do seu provedor de hardware que reduz SMIs para o menor grau possível. Essas perguntas são chamadas de "baixa latência BIOS" ou "SMI BIOS gratuito". Em alguns casos, não é possível para uma plataforma de hardware eliminar a atividade SMI completamente porque ela é usada para controlar as funções essenciais (por exemplo, resfriamento fãs).

>[!NOTE]
>O sistema operacional não pode exercer nenhum controle sobre SMIs porque os processadores lógicos está em execução no modo de manutenção especial, o que impede a intervenção do sistema operacional.

##  <a name="bkmk_tcp"></a>TCP de ajuste de desempenho

 Você pode TCP usando os seguintes itens de ajuste de desempenho.

###  <a name="bkmk_tcp_params"></a>Ajuste automático da janela de recepção de TCP

Antes do Windows Server 2008, a pilha de rede usada uma janela de lado receber de tamanho fixo (65.535) que limitado a taxa de transferência potencial geral para conexões. Uma das formas mais significativas alterações à pilha de TCP é TCP receber o ajuste automático da janela. 

Você pode calcular a taxa de transferência total de uma única conexão quando você usa um tamanho fixo TCP receber janela como:

**A taxa de transferência realizáveis total em bytes = TCP receber o tamanho da janela em bytes \ * (1 / latência de conexão em segundos)**

Por exemplo, a taxa de transferência realizáveis total é somente 51 Mbps em uma conexão com a latência de 10 ms \ (um valor razoável para uma rede corporativa grande infrastructure\). 

Com o ajuste automático, no entanto, a janela de recebimento lado é ajustável, e pode aumentar para atender às demandas do remetente. É possível que uma conexão obter uma taxa de linha inteira de uma conexão Gbps 1. Cenários de uso de rede deve ter sido limitados no passado pela taxa de transferência realizáveis total de conexões TCP totalmente agora podem usar a rede.

#### <a name="deprecated-tcp-parameters"></a>Parâmetros TCP preteridos

As seguintes configurações do registro do Windows Server 2003 não têm suporte e são ignoradas em versões posteriores.

Todas essas configurações tinham o seguinte local do registro:

    ```  
    HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\Tcpip\Parameters  
    ```  

- TcpWindowSize

- NumTcbTablePartitions  

- MaxHashTableSize  



###  <a name="bkmk_wfp"></a>Plataforma para filtros do Windows

A WFP Windows Filtering Platform () que foi introduzida no Windows Vista e Windows Server 2008 fornece APIs para fornecedores independentes de software não são da Microsoft (ISVs) para criar pacotes do processamento de filtros. Exemplos incluem firewall e antivírus.

>[!NOTE]
>Um filtro WFP insatisfatoriamente elaborado pode diminuir significativamente o desempenho de rede do servidor. Para obter mais informações, consulte [processamento de portabilidade de pacotes de Drivers e aplicativos para WFP](https://msdn.microsoft.com/windows/hardware/gg463267.aspx) no Centro de desenvolvimento do Windows.


Para obter links para todos os tópicos neste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).
