---
title: Escolhendo um adaptador de rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: dcscontentpm
ms.author: v-tea
author: Teresa-Motiv
ms.openlocfilehash: c1095f3f5ea44b22c4cec4a871f6fc6210e92ab1
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87991639"
---
# <a name="choosing-a-network-adapter"></a>Escolhendo um adaptador de rede

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para aprender alguns dos recursos dos adaptadores de rede que podem afetar suas opções de compra.

Aplicativos com uso intensivo de rede exigem Adaptadores de rede de alto desempenho. Esta seção explora algumas considerações sobre como escolher adaptadores de rede, bem como definir configurações de adaptador de rede diferentes para obter o melhor desempenho de rede.

> [!TIP]
>  Você pode definir as configurações do adaptador de rede usando o Windows PowerShell. Para obter mais informações, consulte [cmdlets do adaptador de rede no Windows PowerShell](/powershell/module/netadapter).

##  <a name="offload-capabilities"></a><a name="bkmk_offload"></a>Recursos de descarga

O descarregamento de tarefas da CPU da unidade de processamento central \( \) para o adaptador de rede pode reduzir o uso da CPU no servidor, o que melhora o desempenho geral do sistema.

A pilha de rede nos produtos da Microsoft pode descarregar uma ou mais tarefas em um adaptador de rede se você selecionar um adaptador de rede que tenha os recursos de descarregamento apropriados. A tabela a seguir fornece uma breve visão geral de diferentes recursos de descarregamento que estão disponíveis no Windows Server 2016.

|Tipo de descarregamento|Descrição|
|------------------|-----------------|
|Cálculo da soma de verificação para TCP|A pilha de rede pode descarregar o cálculo e a validação das somas de verificação TCP do protocolo de controle de transmissão \( \) nos caminhos de código de envio e recebimento. Ele também pode descarregar o cálculo e a validação de somas de verificação de IPv4 e IPv6 em caminhos de código de envio e recebimento.|
|Cálculo da soma de verificação para UDP |A pilha de rede pode descarregar o cálculo e a validação de somas de verificação UDP do protocolo de datagrama de usuário \( \) em caminhos de código de envio e recebimento.|
|Cálculo da soma de verificação para IPv4 |A pilha de rede pode descarregar o cálculo e a validação de somas de verificação de IPv4 em caminhos de código de envio e recebimento. |
|Cálculo da soma de verificação para IPv6 |A pilha de rede pode descarregar o cálculo e a validação de somas de verificação do IPv6 em caminhos de código de envio e recebimento. |
|Segmentação de pacotes TCP grandes|A camada de transporte TCP/IP dá suporte ao LSOv2 (carregamento de envio grande) v2. Com o LSOv2, a camada de transporte TCP/IP pode descarregar a segmentação de pacotes TCP grandes no adaptador de rede.|
|RSS de escala lateral de recebimento \(\)|O RSS é uma tecnologia de driver de rede que permite a distribuição eficiente de processamento de recebimento de rede em várias CPUs em sistemas multiprocessadores. Mais detalhes sobre o RSS são fornecidos posteriormente neste tópico.|
|União de segmento de recebimento \( RSC\)|O RSC é a capacidade de agrupar pacotes para minimizar o processamento de cabeçalho necessário para que o host seja executado. Um máximo de 64 KB de carga recebida pode ser agrupado em um único pacote maior para processamento. Mais detalhes sobre o RSC são fornecidos posteriormente neste tópico.|

###  <a name="receive-side-scaling"></a><a name="bkmk_rss"></a>Receber dimensionamento lateral

O Windows Server 2016, o Windows Server 2012, o Windows Server 2012 R2, o Windows Server 2008 R2 e o Windows Server 2008 dão suporte ao RSS de dimensionamento lateral de recebimento \( \) .

Alguns servidores são configurados com vários processadores lógicos que compartilham recursos de hardware \( como um núcleo físico \) e que são tratados como \( peers de SMT de vários threads simultâneos \) . A tecnologia Intel Hyper-Threading é um exemplo. O RSS direciona o processamento de rede para até um processador lógico por núcleo. Por exemplo, em um servidor com Intel Hyper-Threading, 4 núcleos e 8 processadores lógicos, o RSS usa no máximo 4 processadores lógicos para processamento de rede.

O RSS distribui pacotes de e/s de rede de entrada entre processadores lógicos para que os pacotes que pertencem à mesma conexão TCP sejam processados no mesmo processador lógico, que preserva a ordenação.

O RSS também equilibra o tráfego de difusão e unicast UDP e roteia os fluxos relacionados \( que são determinados pelo hash dos endereços de origem e de destino \) para o mesmo processador lógico, preservando a ordem das entradas relacionadas. Isso ajuda a melhorar a escalabilidade e o desempenho para cenários de uso intensivo de recebimento para servidores que têm menos adaptadores de rede do que os processadores lógicos qualificados.

#### <a name="configuring-rss"></a>Configurando RSS

No Windows Server 2016, você pode configurar o RSS usando os cmdlets do Windows PowerShell e os perfis RSS.

Você pode definir perfis RSS usando o parâmetro **– Profile** do cmdlet **set-NetAdapterRss** do Windows PowerShell.

**Comandos do Windows PowerShell para configuração de RSS**

Os cmdlets a seguir permitem ver e modificar os parâmetros RSS por adaptador de rede.

>[!NOTE]
>Para obter uma referência de comando detalhada para cada cmdlet, incluindo sintaxe e parâmetros, você pode clicar nos links a seguir. Além disso, você pode passar o nome do cmdlet para **Get-Help** no prompt do Windows PowerShell para obter detalhes sobre cada comando.

- [Disable-NetAdapterRss](/powershell/module/netadapter/Disable-NetAdapterRss). Esse comando desabilita o RSS no adaptador de rede que você especificar.

- [Enable-NetAdapterRss](/powershell/module/netadapter/Enable-NetAdapterRss). Esse comando habilita o RSS no adaptador de rede que você especificar.

- [Get-NetAdapterRss](/powershell/module/netadapter/Get-NetAdapterRss). Esse comando recupera as propriedades de RSS do adaptador de rede que você especificar.

- [Set-NetAdapterRss](/powershell/module/netadapter/Set-NetAdapterRss). Esse comando define as propriedades de RSS no adaptador de rede que você especificar.

#### <a name="rss-profiles"></a>Perfis de RSS

Você pode usar o parâmetro **– Profile** do cmdlet Set-NetAdapterRss para especificar quais processadores lógicos são atribuídos a qual adaptador de rede. Os valores disponíveis para esse parâmetro são:

- **Mais próximo**. Os números de processador lógicos próximos ao processador RSS base do adaptador de rede são preferenciais. Com esse perfil, o sistema operacional pode reequilibrar os processadores lógicos dinamicamente com base na carga.

- **ClosestStatic**. Os números de processador lógico próximo ao processador RSS de base do adaptador de rede são preferenciais. Com esse perfil, o sistema operacional não reequilibra dinamicamente os processadores lógicos com base na carga.

- **Numa**. Os números de processador lógicos geralmente são selecionados em diferentes nós NUMA para distribuir a carga. Com esse perfil, o sistema operacional pode reequilibrar os processadores lógicos dinamicamente com base na carga.

- **NUMAStatic**. Esse é o **perfil padrão**. Os números de processador lógicos geralmente são selecionados em diferentes nós NUMA para distribuir a carga. Com esse perfil, o sistema operacional não reequilibrará os processadores lógicos dinamicamente com base na carga.

- **Conservador**. O RSS usa o menor número possível de processadores para sustentar a carga. Essa opção ajuda a reduzir o número de interrupções.

Dependendo do cenário e das características de carga de trabalho, você também pode usar outros parâmetros do cmdlet **set-NetAdapterRss** do Windows PowerShell para especificar o seguinte:

- Em uma base de adaptador por rede, quantos processadores lógicos podem ser usados para RSS.
- O deslocamento inicial para o intervalo de processadores lógicos.
- O nó do qual o adaptador de rede aloca memória.

A seguir estão os parâmetros **set-NetAdapterRss** adicionais que você pode usar para configurar o RSS:

>[!NOTE]
>Na sintaxe de exemplo para cada parâmetro abaixo, o nome do adaptador de rede **Ethernet** é usado como um valor de exemplo para o parâmetro **– Name** do comando **set-NetAdapterRss** . Ao executar o cmdlet, verifique se o nome do adaptador de rede que você usa é apropriado para seu ambiente.

- ** \* MaxProcessors**: define o número máximo de processadores RSS a serem usados. Isso garante que o tráfego do aplicativo esteja associado a um número máximo de processadores em uma determinada interface. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name "Ethernet" –MaxProcessors <value>`

- ** \* BaseProcessorGroup**: define o grupo de processadores base de um nó numa. Isso afeta a matriz do processador usada pelo RSS. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name "Ethernet" –BaseProcessorGroup <value>`

- ** \* MaxProcessorGroup**: define o grupo de processador máximo de um nó numa. Isso afeta a matriz do processador usada pelo RSS. Definir isso restringiria um grupo de processadores máximo para que o balanceamento de carga seja alinhado em um grupo k. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name "Ethernet" –MaxProcessorGroup <value>`

- ** \* BaseProcessorNumber**: define o número de processador base de um nó numa. Isso afeta a matriz do processador usada pelo RSS. Isso permite o particionamento de processadores entre adaptadores de rede. Esse é o primeiro processador lógico no intervalo de processadores RSS que é atribuído a cada adaptador. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name "Ethernet" –BaseProcessorNumber <Byte Value>`

- ** \* NumaNode**: o nó numa para o qual cada adaptador de rede pode alocar memória. Isso pode estar dentro de um grupo k ou de diferentes grupos k. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name "Ethernet" –NumaNodeID <value>`

- ** \* NumberofReceiveQueues**: se os processadores lógicos parecem estar subutilizados para receber tráfego \( , por exemplo, como exibido no Gerenciador de tarefas \) , você pode tentar aumentar o número de filas RSS do padrão de 2 para o máximo com suporte no seu adaptador de rede. O adaptador de rede pode ter opções para alterar o número de filas RSS como parte do driver. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name "Ethernet" –NumberOfReceiveQueues <value>`

Para obter mais informações, clique no link a seguir para baixar a [rede escalonável: eliminando o afunilamento de processamento de recebimento, apresentando o RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) no formato Word.

#### <a name="understanding-rss-performance"></a>Noções básicas sobre o desempenho do RSS

O ajuste do RSS requer a compreensão da configuração e da lógica de balanceamento de carga. Para verificar se as configurações de RSS entraram em vigor, você pode examinar a saída ao executar o cmdlet **Get-NetAdapterRss** do Windows PowerShell. Veja a seguir um exemplo de saída desse cmdlet.

```

PS C:\Users\Administrator> get-netadapterrss
Name                           : testnic 2
InterfaceDescription           : Broadcom BCM5708C NetXtreme II GigE (NDIS VBD Client) #66
Enabled                        : True
NumberOfReceiveQueues          : 2
Profile                        : NUMAStatic
BaseProcessor: [Group:Number]  : 0:0
MaxProcessor: [Group:Number]   : 0:15
MaxProcessors                  : 8

IndirectionTable: [Group:Number]:
     0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4
…
(# indirection table entries are a power of 2 and based on # of processors)
…
                          0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4
```

Além dos parâmetros de eco que foram definidos, o aspecto principal da saída é a saída da tabela de indireção. A tabela de indireção exibe os buckets de tabela de hash que são usados para distribuir o tráfego de entrada. Neste exemplo, a notação n:c designa o par de índice K-Group: CPU de numa que é usado para direcionar o tráfego de entrada. Vemos exatamente duas entradas exclusivas (0:0 e 0:4), que representam k-Group 0/CPU0 e k-Group 0/CPU 4, respectivamente.

Há apenas um grupo k para este sistema (k-Group 0) e uma entrada de tabela de indireção n (em que n <= 128). Como o número de filas de recebimento está definido como 2, somente 2 processadores (0:0, 0:4) são escolhidos, embora os processadores máximos estejam definidos como 8. Na verdade, a tabela de indireção está aplicando hash ao tráfego de entrada para usar apenas 2 CPUs dos 8 que estão disponíveis.

Para utilizar totalmente as CPUs, o número de filas de recebimento RSS deve ser igual ou maior que os processadores máximos. No exemplo anterior, a fila de recebimento deve ser definida como 8 ou superior.

#### <a name="nic-teaming-and-rss"></a>Agrupamento NIC e RSS

O RSS pode ser habilitado em um adaptador de rede que está agrupado com outra placa de interface de rede usando o agrupamento NIC. Nesse cenário, somente o adaptador de rede física subjacente pode ser configurado para usar o RSS. Um usuário não pode definir os cmdlets RSS no adaptador de rede agrupado.

###  <a name="receive-segment-coalescing-rsc"></a><a name="bkmk_rsc"></a>União de segmento de recebimento (RSC)

A União de segmento \( de recebimento RSC \) ajuda o desempenho, reduzindo o número de cabeçalhos IP que são processados para uma determinada quantidade de dados recebidos. Ele deve ser usado para ajudar a dimensionar o desempenho de dados recebidos agrupando \( ou unindo \) os pacotes menores em unidades maiores.

Essa abordagem pode afetar a latência com os benefícios vistos principalmente nos ganhos da produtividade. O RSC é recomendado para aumentar a taxa de transferência para cargas de trabalho pesadas recebidas. Considere a implantação de adaptadores de rede que dão suporte a RSC.

Nesses adaptadores de rede, verifique se o RSC está nessa \( configuração padrão \) , a menos que você tenha cargas de trabalho específicas, \( por exemplo, baixa latência, rede de baixa taxa de transferência \) que mostre os benefícios do RSC sendo desativado.

#### <a name="understanding-rsc-diagnostics"></a>Compreendendo o diagnóstico de RSC

Você pode diagnosticar o RSC usando os cmdlets do Windows PowerShell **Get-NetAdapterRsc** e **Get-NetAdapterStatistics**.

A seguir, há um exemplo de saída quando você executa o cmdlet Get-NetAdapterRsc.

```

PS C:\Users\Administrator> Get-NetAdapterRsc

Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure
                                            Reason
----                           -----------  -----------  --------------- --------------- ----------------- ------------
Ethernet                       True         False        True            False                  NoFailure       NicProperties

```

O cmdlet **Get** mostra se o RSC está habilitado na interface e se o TCP permite que o RSC esteja em um estado operacional. O motivo da falha fornece detalhes sobre a falha ao habilitar o RSC nessa interface.

No cenário anterior, o IPv4 RSC tem suporte e está operacional na interface. Para entender as falhas de diagnóstico, é possível ver os bytes ou as exceções Unidas causadas. Isso fornece uma indicação dos problemas de União.

A seguir, há um exemplo de saída quando você executa o cmdlet Get-NetAdapterStatistics.

```
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics "myAdapter"
PS C:\Users\Administrator> $x.rscstatistics

CoalescedBytes       : 0
CoalescedPackets     : 0
CoalescingEvents     : 0
CoalescingExceptions : 0

```

#### <a name="rsc-and-virtualization"></a>RSC e virtualização

Somente há suporte para RSC no host físico quando o adaptador de rede do host não está associado ao comutador virtual do Hyper-V. O RSC é desabilitado pelo sistema operacional quando o host está associado ao comutador virtual do Hyper-V. Além disso, as máquinas virtuais não têm o benefício de RSC porque os adaptadores de rede virtual não dão suporte a RSC.

O RSC pode ser habilitado para uma máquina virtual quando o Sr-IOV de virtualização de entrada/saída de raiz única \( \) estiver habilitado. Nesse caso, as funções virtuais dão suporte ao recurso RSC; Portanto, as máquinas virtuais também recebem o benefício do RSC.

##  <a name="network-adapter-resources"></a><a name="bkmk_resources"></a>Recursos do adaptador de rede

Alguns adaptadores de rede gerenciam ativamente seus recursos para obter um desempenho ideal. Vários adaptadores de rede permitem configurar manualmente os recursos usando a guia **rede avançada** para o adaptador. Para esses adaptadores, você pode definir os valores de um número de parâmetros, incluindo o número de buffers de recebimento e buffers de envio.

A configuração de recursos do adaptador de rede é simplificada pelo uso dos seguintes cmdlets do Windows PowerShell.

- [Get-NetAdapterAdvancedProperty](/powershell/module/netadapter/Get-NetAdapterAdvancedProperty)

- [Set-NetAdapterAdvancedProperty](/powershell/module/netadapter/Set-NetAdapterAdvancedProperty)

- [Habilitar-netadapter](/powershell/module/netadapter/Enable-NetAdapte)

- [Habilitar-netadaptadorbinding](/powershell/module/netadapter/Enable-NetAdapterBinding)

- [Habilitar-NetAdapterChecksumOffload](/powershell/module/netadapter/Enable-NetAdapterChecksumOffload)

- [Habilitar-NetAdapterIPSecOffload](/powershell/module/netadapter/Enable-NetAdapterChecksumOffload)

- [Habilitar-NetAdapterLso](/powershell/module/netadapter/Enable-NetAdapterLso)

- [Habilitar-NetAdapterPowerManagement](/powershell/module/netadapter/Enable-NetAdapterPowerManagement)

- [Habilitar-NetAdapterQos](/powershell/module/netadapter/Enable-NetAdapterQos)

- [Habilitar-NetAdapterRDMA](/powershell/module/netadapter/Enable-NetAdapterRDMA)

- [Habilitar-NetAdapterSriov](/powershell/module/netadapter/Enable-NetAdapterSriov)

Para obter mais informações, consulte [cmdlets do adaptador de rede no Windows PowerShell](/powershell/module/netadapter).

Para obter links para todos os tópicos deste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).