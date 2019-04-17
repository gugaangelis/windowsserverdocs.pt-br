---
title: Escolhendo um adaptador de rede
description: Este tópico faz parte do guia ajuste de desempenho do subsistema de rede para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b3b9d206273dfd0e9115ebc27cf28aa960bfb0f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="choosing-a-network-adapter"></a>Escolhendo um adaptador de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para aprender alguns dos recursos dos adaptadores de rede que podem afetar suas opções de compras.

Os aplicativos com uso intenso de rede exigir adaptadores de rede de alto desempenho. Esta seção explora algumas considerações para escolher os adaptadores de rede, bem como definir as configurações do adaptador de rede diferentes para obter o melhor desempenho de rede.

> [!TIP]
>  Você pode configurar as configurações do adaptador de rede usando o Windows PowerShell. Para obter mais informações, consulte [Cmdlets de adaptador de rede no Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

##  <a name="bkmk_offload"></a>Descarregamento de recursos

Descarregamento tarefas da unidade de processamento central \(CPU\) ao adaptador de rede pode reduzir o uso da CPU no servidor, que melhora o desempenho geral do sistema.

A pilha de rede em produtos da Microsoft pode descarregar uma ou mais tarefas a um adaptador de rede se você selecionar um adaptador de rede que tem o apropriado descarregamento de recursos. A tabela a seguir fornece uma visão geral dos recursos de descarregamento diferentes que estão disponíveis no Windows Server 2016.
  
|Descarregamento de tipo|Descrição|
|------------------|-----------------|  
|Cálculo de soma de verificação para TCP|A pilha de rede pode descarregar o cálculo e validação de somas de verificação de protocolo de controle de transmissão \(TCP\) em enviar e receber os caminhos de código. Ele também pode descarregar o cálculo e a validação de IPv4 e IPv6 somas de verificação em enviar e recebem os caminhos de código.|  
|Cálculo de soma de verificação para UDP |A pilha de rede pode descarregar o cálculo e validação de somas de verificação de User Datagram Protocol \(UDP\) em enviar e receber os caminhos de código.|
|Cálculo de soma de verificação para IPv4 |A pilha de rede pode descarregar o cálculo e a validação de IPv4 somas de verificação em enviar e recebem os caminhos de código. |
|Cálculo de soma de verificação para o IPv6 |A pilha de rede pode descarregar o cálculo e a validação do IPv6 somas de verificação em enviar e recebem os caminhos de código. | 
|Segmentação de pacotes TCP grandes|A camada de transporte TCP/IP oferece suporte ao descarregamento de envio grande v2 (LSOv2). Com LSOv2, a camada de transporte TCP/IP pode descarregar a segmentação de grandes pacotes TCP para o adaptador de rede.|  
|Receber lado dimensionamento \(RSS\)|RSS é uma tecnologia de driver de rede que permite a distribuição eficiente de rede receber processamento em diversas CPUs em sistemas com vários processadores. Mais detalhes sobre RSS é fornecido neste tópico.|  
|Receber segmento unir \(RSC\)|RSC é a capacidade de pacotes do grupo juntos para minimizar o cabeçalho de processamento que é necessária para o host executar. Um máximo de 64 KB de carga recebida pode ser ocultas em um único pacote maior para processamento. Mais detalhes sobre RSC é fornecido no final deste tópico.|  
  
###  <a name="bkmk_rss"></a>Receber lado colocação em escala

Windows Server 2008, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 e Windows Server 2016 suportam \(RSS\) RSS. 

Alguns servidores são configurados com vários processadores lógicos que compartilham recursos de hardware \ (por exemplo, um core\ físico) e parceiros, que é tratado como \(SMT\) vários threads simultâneos. Intel Hyper-Threading Technology é um exemplo. RSS direciona o processamento de rede para até um processadores lógicos por núcleo. Por exemplo, em um servidor com Intel Hyper-Threading, 4 núcleos e 8 processadores lógicos, RSS usa não mais do que 4 processadores lógicos para processamento de rede.  

RSS distribui pacotes recebidos de e/s de rede entre processadores lógicos para que os pacotes que pertencem à mesma conexão TCP são processadas no mesmo lógico processador, que preserva a ordem. 

RSS também carregar saldos UDP unicast e tráfego multicast, e ele encaminha fluxos relacionados \ (que é determinado pelas hash addresses\ a origem e de destino) para o processador lógico mesmo, preservar a ordem das entradas relacionadas. Isso ajuda a melhorar o desempenho de cenários de uso intenso receber para servidores que têm menos adaptadores de rede que eles fazem qualificados processadores lógicos e escalabilidade. 

#### <a name="configuring-rss"></a>Como configurar RSS

No Windows Server 2016, você pode configurar RSS usando os cmdlets do Windows PowerShell e perfis RSS. 

Você pode definir perfis RSS, usando o **– perfil** parâmetro do **conjunto NetAdapterRss** cmdlet do Windows PowerShell.

**Comandos do Windows PowerShell para a configuração de RSS**

Cmdlets a seguir permitem que você veja e modificar os parâmetros RSS por adaptador de rede.
  
>[!NOTE]
>Para obter uma referência de comando detalhadas para cada cmdlet, incluindo a sintaxe e parâmetros, você pode clicar nos seguintes links. Além disso, você pode passar o nome do cmdlet **Get-Help** no prompt do Windows PowerShell para obter detalhes sobre cada comando.  

- [Desabilitar NetAdapterRss](https://technet.microsoft.com/library/jj130892). Esse comando desabilita RSS no adaptador de rede que você especificar.

- [Habilitar NetAdapterRss](https://technet.microsoft.com/library/jj130859). Esse comando permite RSS no adaptador de rede que você especificar.
  
- [Get-NetAdapterRss](https://technet.microsoft.com/library/jj130912). Esse comando recupera as propriedades RSS do adaptador de rede que você especificar.
  
- [Conjunto NetAdapterRss](https://technet.microsoft.com/library/jj130863). Esse comando define as propriedades RSS no adaptador de rede que você especificar.  

#### <a name="rss-profiles"></a>Perfis RSS

Você pode usar o **– perfil** parâmetro do cmdlet Set-NetAdapterRss para especificar quais processadores lógicos atribuídos para o adaptador de rede. Os valores disponíveis para este parâmetro são:

- **Mais próxima**. Números de processadores lógicos que estão próximos processador RSS base do adaptador de rede são preferenciais. Com esse perfil, o sistema operacional pode rebalancear processadores lógicos dinamicamente com base na carga.
  
- **ClosestStatic**. Números de processadores lógicos perto de processador RSS base do adaptador de rede são preferenciais. Com esse perfil, o sistema operacional não rebalancear processadores lógicos dinamicamente com base na carga.
  
- **NUMA**. Números de processadores lógicos geralmente são selecionados em nós NUMA diferentes para distribuir a carga. Com esse perfil, o sistema operacional pode rebalancear processadores lógicos dinamicamente com base na carga.
  
- **NUMAStatic**. Este é o **perfil padrão**. Números de processadores lógicos geralmente são selecionados em nós NUMA diferentes para distribuir a carga. Com esse perfil, o sistema operacional não serão rebalancear processadores lógicos dinamicamente com base na carga.

- **Conservador**. RSS usa processadores mínimo possível para manter a carga. Esta opção ajuda a reduzir o número de interrupções.

Dependendo do cenário e as características de carga de trabalho, você também pode usar outros parâmetros do **conjunto NetAdapterRss** cmdlet do Windows PowerShell para especificar o seguinte:

- Em uma base de adaptador por rede, quantos processadores lógicos podem ser usados para RSS.
- O deslocamento inicial para o intervalo de processadores lógicos.
- O nó do qual o adaptador de rede aloca memória.

A seguir estão as adicionais **conjunto NetAdapterRss** parâmetros que você pode usar para configurar RSS:

>[!NOTE]
>Na sintaxe de exemplo para cada parâmetro abaixo, o nome do adaptador de rede **Ethernet** é usado como um valor de exemplo para o **– nome** parâmetro do **conjunto NetAdapterRss** comando. Quando você executa o cmdlet, certifique-se de que o nome do adaptador de rede que você usa é apropriado para seu ambiente.

- **\ * MaxProcessors**: define o número máximo de processadores RSS a ser usado. Isso garante que o tráfego do aplicativo está vinculado a um número máximo de processadores em uma determinada interface. Sintaxe de exemplo:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\ * BaseProcessorGroup**: define o grupo de processador base de um nó NUMA. Isso afeta a matriz de processador que é usada pelo RSS. Sintaxe de exemplo:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\ * MaxProcessorGroup**: define o grupo de processador Max de um nó NUMA. Isso afeta a matriz de processador que é usada pelo RSS. Essa configuração restringir um grupo máxima do processador para que o balanceamento de carga está alinhado dentro de um grupo de k. Sintaxe de exemplo:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\ * BaseProcessorNumber**: define o número de um nó NUMA base de processador. Isso afeta a matriz de processador que é usada pelo RSS. Isso permite que processadores de particionamento em todos os adaptadores de rede. Isso é o primeiro processador lógico em processadores o intervalo de RSS que é atribuído a cada adaptador. Sintaxe de exemplo:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\ * NumaNode**: nó no que cada adaptador de rede pode alocar memória de. Isso pode ser dentro de um grupo de mil ou de k-grupos diferentes. Sintaxe de exemplo:

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\ * NumberofReceiveQueues**: se seu processadores lógicos parecem estar mal alocada para receber tráfego \ (por exemplo, conforme exibido no Gerenciador de tarefas \), você pode tentar aumentar o número de filas RSS do padrão de 2 para o máximo que é compatível com o adaptador de rede. O adaptador de rede pode ter opções para alterar o número de filas RSS como parte do driver. Sintaxe de exemplo:

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

Para obter mais informações, clique no seguinte link para baixar [Scalable Networking: eliminando o afunilamento de processamento de receber — apresentando RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) no formato do Word.
  
#### <a name="understanding-rss-performance"></a>Noções básicas sobre o desempenho de RSS

Ajuste RSS requer Noções básicas sobre a configuração e a lógica de balanceamento de carga. Para verificar se as configurações de RSS entraram em vigor, você pode examinar a saída quando você executa o **Get-NetAdapterRss** cmdlet do Windows PowerShell. A seguir está um exemplo de saída desse cmdlet.
  
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

Além de eco parâmetros que foram definidos, o aspecto importante da saída é a saída da tabela de inversão. A tabela de indireção exibe a tabela em hash que é usadas para distribuir o tráfego de entrada. Neste exemplo, a notação n:c designa Numa K-par de índice de grupo: CPU que é usado para direcionar o tráfego de entrada. Vemos exatamente 2 entradas exclusivas (0:0 e 0:4), que representam k-grupo 0/cpu0 e grupo k 0/cpu 4, respectivamente.

Há apenas um grupo de mil para esse sistema (k-grupo 0) e um n (onde n < = 128) entrada da tabela de inversão. Porque o número de filas de recebimento é definido como 2, somente 2 processadores (0:0, 0:4) são escolhidos - mesmo que processadores máximos é definido como 8. Na verdade, a tabela de indireção é hash tráfego de entrada para usar somente 2 CPUs fora do 8 que estão disponíveis.

Para utilizar totalmente as CPUs, o número de filas de receber RSS deve ser igual ou maior do que os processadores Max. No exemplo anterior, a fila de receber deve ser definida como 8 ou mais.

#### <a name="nic-teaming-and-rss"></a>Agrupamento de NIC e RSS

RSS pode ser habilitado em um adaptador de rede está agrupado com outra placa de interface de rede usando o agrupamento de placa de rede. Nesse cenário, apenas o adaptador de rede física subjacente pode ser configurado para usar RSS. Um usuário não pode definir cmdlets RSS no adaptador de rede emparelhados.
  
###  <a name="bkmk_rsc"></a>Receber segmento unir (RSC)

Receba \(RSC\) segmento unir ajuda desempenho reduzindo o número de cabeçalhos IP que são processadas para uma determinada quantidade de dados recebidos. Ele deve ser usado para ajudar a medir o desempenho de dados recebidos pelo agrupamento \(or coalescing\) os pacotes menores em unidades maiores.

Essa abordagem pode afetar a latência vantagens principalmente visto em ganhos de taxa de transferência. RSC é recomendado para aumentar a taxa de transferência para cargas de trabalho pesadas recebidas. Pense em implantar adaptadores de rede que dão suporte a RSC. 

Nestes adaptadores de rede, certifique-se de que RSC está em \ (Esta é a definindo \ padrão), a menos que tenha as cargas de trabalho específicas \ (por exemplo, baixa latência, baixa taxa de transferência networking\) esse benefício Mostrar RSC desligados.

#### <a name="understanding-rsc-diagnostics"></a>Noções básicas sobre o diagnóstico RSC

É possível diagnosticar RSC usando os cmdlets do Windows PowerShell **Get-NetAdapterRsc** e **Get-NetAdapterStatistics**.

A seguir é o exemplo de saída quando você executa o cmdlet Get-NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

O **obter** cmdlet mostra se RSC está habilitada na interface e se o TCP permite que RSC estejam em um estado operacional. O motivo da falha fornece detalhes sobre a falha para habilitar RSC na interface.

No cenário anterior, IPv4 RSC é compatível e operacionais na interface. Para entender as falhas de diagnósticos, uma possível ver os bytes agrupadas ou exceções causadas. Isso fornece uma indicação dos problemas unindo.

A seguir é o exemplo de saída quando você executa o cmdlet Get-NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC e a virtualização

RSC é compatível apenas com o host físico quando o adaptador de rede de host não está vinculado ao Switch Virtual Hyper-V. RSC é desabilitada pelo sistema operacional quando o host está vinculado ao Switch Virtual Hyper-V. Além disso, máquinas virtuais não se beneficiarão do RSC porque os adaptadores de rede virtual não dão suporte a RSC.

RSC pode ser habilitado para uma máquina virtual quando o único virtualização de entrada/saída de raiz \(SR-IOV\) está habilitado. Nesse caso, funções virtuais dão suporte a funcionalidade RSC; Portanto, máquinas virtuais também recebem a vantagem de RSC.

##  <a name="bkmk_resources"></a>Recursos do adaptador de rede

Alguns adaptadores de rede ativamente gerenciar seus recursos para obter o desempenho ideal. Vários adaptadores de rede permitem que você configurar recursos manualmente usando o **avançada de rede** guia para o adaptador. Para esses adaptadores, você pode definir os valores de um número de parâmetros, incluindo o número de buffers de receber e enviar buffers.

Configurar recursos de adaptador de rede é simplificado pelo uso de cmdlets do Windows PowerShell a seguir.

- [Get-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130901.aspx)

- [Set-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130894.aspx)

- [Enable-NetAdapter](https://technet.microsoft.com/library/jj130876.aspx)

- [Enable-NetAdapterBinding](https://technet.microsoft.com/library/jj130913.aspx)

- [Enable-NetAdapterChecksumOffload](https://technet.microsoft.com/library/jj130918.aspx)

- [Enable-NetAdapterIPSecOffload](https://technet.microsoft.com/library/jj130890.aspx)

- [Enable-NetAdapterLso](https://technet.microsoft.com/library/jj130922.aspx)

- [Enable-NetAdapterPowerManagement](https://technet.microsoft.com/library/jj130907.aspx)

- [Enable-NetAdapterQos](https://technet.microsoft.com/library/jj130866.aspx)

- [Enable-NetAdapterRDMA](https://technet.microsoft.com/library/jj130909.aspx)

- [Enable-NetAdapterSriov](https://technet.microsoft.com/library/jj130899.aspx)

Para obter mais informações, consulte [Cmdlets de adaptador de rede no Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

Para obter links para todos os tópicos neste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).