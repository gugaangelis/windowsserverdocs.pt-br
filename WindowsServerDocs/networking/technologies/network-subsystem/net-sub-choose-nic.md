---
title: Escolhendo um adaptador de rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2b50f4b286e90a450278243c0294ea0aa7f221bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875847"
---
# <a name="choosing-a-network-adapter"></a>Escolhendo um adaptador de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para aprender alguns dos recursos dos adaptadores de rede que podem afetar suas opções de compras.

Aplicativos com uso intenso de rede requerem adaptadores de rede de alto desempenho. Essa seção explora algumas considerações para escolher os adaptadores de rede, bem como definir as configurações de adaptador de rede diferente para obter o melhor desempenho de rede.

> [!TIP]
>  Você pode configurar as configurações do adaptador de rede usando o Windows PowerShell. Para obter mais informações, consulte [Cmdlets do adaptador de rede no Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

##  <a name="bkmk_offload"></a> Recursos de descarregamento

Descarregar tarefas da unidade de processamento central \(CPU\) à rede, adaptador pode reduzir o uso da CPU no servidor, o que melhora o desempenho geral do sistema.

A pilha de rede nos produtos da Microsoft pode descarregar uma ou mais tarefas a um adaptador de rede, se você selecionar um adaptador de rede que tenha apropriado de recursos de descarregamento. A tabela a seguir fornece uma visão geral dos recursos de descarregamento diferentes que estão disponíveis no Windows Server 2016.
  
|Tipo de descarregamento|Descrição|
|------------------|-----------------|  
|Cálculo de soma de verificação para TCP|A pilha de rede pode descarregar o cálculo e a validação de protocolo TCP \(TCP\) somas de verificação em enviar e receber caminhos de código. Ele também pode descarregar o cálculo e a validação do IPv4 e IPv6 as somas de verificação em enviar e recebem caminhos de código.|  
|Cálculo de soma de verificação para UDP |A pilha de rede pode descarregar o cálculo e a validação do protocolo de datagrama de usuário \(UDP\) somas de verificação em enviar e receber caminhos de código.|
|Cálculo de soma de verificação para IPv4 |A pilha de rede pode descarregar o cálculo e a validação do IPv4, as somas de verificação em enviar e recebem caminhos de código. |
|Cálculo de soma de verificação para IPv6 |A pilha de rede pode descarregar o cálculo e a validação do IPv6 as somas de verificação em enviar e recebem caminhos de código. | 
|Segmentação de grandes pacotes TCP|A camada de transporte TCP/IP oferece suporte ao descarregamento de envio grande v2 (LSOv2). Com LSOv2, a camada de transporte TCP/IP pode descarregar a segmentação de grandes pacotes TCP para o adaptador de rede.|  
|O Receive Side Scaling \(RSS\)|O RSS é uma tecnologia de driver de rede que permite a distribuição eficiente de rede de processamento de recebimento entre várias CPUs em sistemas multiprocessadores. Mais detalhes sobre o RSS é fornecido neste tópico.|  
|Receber união de segmentos \(RSC\)|A RSC é a capacidade de pacotes de grupo em conjunto para minimizar o cabeçalho de processamento que é necessária para o host executar. Um máximo de 64 KB de conteúdo recebido pode ser Unido em um único pacote maior para processamento. Mais detalhes sobre a RSC é fornecido neste tópico.|  
  
###  <a name="bkmk_rss"></a> O Receive Side Scaling

Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 e Windows Server 2008, suporte para RSS \(RSS\). 

Alguns servidores são configurados com vários processadores lógicos que compartilham recursos de hardware \(como um núcleo físico\) e que é tratado como simultâneo de multithreading \(SMT\) pares. A tecnologia hyperthreading Intel é um exemplo. RSS direciona o processamento de rede para até um processador lógico por núcleo. Por exemplo, em um servidor com Intel Hyper-Threading, 4 núcleos e 8 processadores lógicos, o RSS usa não mais do que 4 processadores lógicos para processamento de rede.  

RSS distribui os pacotes recebidos de e/s de rede entre os processadores lógicos para que os pacotes que pertencem à mesma conexão TCP são processados sob o mesmo processador lógico, que preserva a ordenação. 

RSS também carregar o tráfego multicast e saldos UDP unicast, e ele encaminha os fluxos relacionados \(que é determinado pelo hash os endereços de origem e destino\) para o mesmo processador lógico, preservando a ordem das entradas relacionadas. Isso ajuda a melhorar a escalabilidade e desempenho em cenários com recepção intensa para servidores que têm adaptadores de rede menos do que processadores lógicos qualificados. 

#### <a name="configuring-rss"></a>Configurar o RSS

No Windows Server 2016, você pode configurar o RSS usando cmdlets do Windows PowerShell e perfis RSS. 

Você pode definir perfis RSS usando o **– perfil** parâmetro do **Set-NetAdapterRss** cmdlet do Windows PowerShell.

**Comandos do Windows PowerShell para configuração do RSS**

Os cmdlets a seguir permitem que você consulte e modificar parâmetros do RSS por adaptador de rede.
  
>[!NOTE]
>Para obter uma referência de comando detalhadas de cada cmdlet, incluindo sintaxe e parâmetros, você pode clicar em links a seguir. Além disso, você pode passar o nome do cmdlet **Get-Help** prompt do Windows PowerShell para obter detalhes sobre cada comando.  

- [Disable-NetAdapterRss](https://technet.microsoft.com/library/jj130892). Este comando desabilita o RSS no adaptador de rede que você especificar.

- [Enable-NetAdapterRss](https://technet.microsoft.com/library/jj130859). Este comando habilita o RSS no adaptador de rede que você especificar.
  
- [Get-NetAdapterRss](https://technet.microsoft.com/library/jj130912). Esse comando recupera as propriedades RSS do adaptador de rede que você especificar.
  
- [Set-NetAdapterRss](https://technet.microsoft.com/library/jj130863). Este comando define as propriedades RSS no adaptador de rede que você especificar.  

#### <a name="rss-profiles"></a>Perfis RSS

Você pode usar o **– perfil** parâmetro do cmdlet Set-NetAdapterRss para especificar quais processadores lógicos são atribuídos a qual adaptador de rede. Os valores disponíveis para esse parâmetro são:

- **Mais próximo**. Números de processador lógico que estão próximo processador RSS da base do adaptador de rede são preferenciais. Com esse perfil, o sistema operacional pode reequilibrar dinamicamente com base na carga de processadores lógicos.
  
- **ClosestStatic**. Números de processador lógico próximo processador RSS da base do adaptador de rede são preferenciais. Com esse perfil, o sistema operacional não reequilibrar dinamicamente com base na carga de processadores lógicos.
  
- **NUMA**. Números de processadores lógicos em geral são selecionados em diferentes nós para distribuir a carga. Com esse perfil, o sistema operacional pode reequilibrar dinamicamente com base na carga de processadores lógicos.
  
- **NUMAStatic**. Esse é o **perfil padrão**. Números de processadores lógicos em geral são selecionados em diferentes nós para distribuir a carga. Com esse perfil, o sistema operacional não irá rebalancear dinamicamente com base na carga de processadores lógicos.

- **Conservadora**. RSS usa o mínimo de processadores suficientes para sustentar a carga. Essa opção ajuda a reduzir o número de interrupções.

Dependendo do cenário e as características de carga de trabalho, você também pode usar outros parâmetros do **Set-NetAdapterRss** cmdlet do Windows PowerShell para especificar o seguinte:

- Em uma base de adaptador por rede, como número de processadores lógicos pode ser usado para RSS.
- O deslocamento inicial para o intervalo de processadores lógicos.
- O nó do qual o adaptador de rede aloca memória.

A seguir é adicionais **Set-NetAdapterRss** parâmetros que você pode usar para configurar o RSS:

>[!NOTE]
>Na sintaxe de exemplo para cada parâmetro abaixo, o nome do adaptador de rede **Ethernet** é usado como um valor de exemplo para o **– nome** parâmetro do **Set-NetAdapterRss** comando. Quando você executa o cmdlet, certifique-se de que o nome do adaptador de rede que você usa é apropriado para seu ambiente.

- **\* MaxProcessors**: Define o número máximo de processadores RSS a ser usado. Isso garante que o tráfego de aplicativo está associado a um número máximo de processadores em uma determinada interface. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\* BaseProcessorGroup**: Define o grupo de processador de base de um nó NUMA. Isso afeta a matriz de processador que é usada pelo RSS. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\* MaxProcessorGroup**: Define o grupo de processador de máximo de um nó NUMA. Isso afeta a matriz de processador que é usada pelo RSS. Essa configuração restringe um grupo de processador máximo para que o balanceamento de carga é alinhado dentro de um grupo de k. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\* BaseProcessorNumber**: Define o número de processador de base de um nó NUMA. Isso afeta a matriz de processador que é usada pelo RSS. Isso permite que o particionamento de processadores em adaptadores de rede. Isso é o primeiro processador lógico nos processadores de intervalo de RSS que é atribuído a cada adaptador. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\* NumaNode**: O nó NUMA que cada adaptador de rede pode alocar memória do. Isso pode ser dentro de um grupo de k ou de grupos diferentes de k. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\* NumberofReceiveQueues**: Se os processadores lógicos parecerem subutilizados para receber tráfego \(por exemplo, como exibido no Gerenciador de tarefas\), você pode tentar aumentar o número de filas RSS do padrão de 2 para o máximo que é compatível com o adaptador de rede . O adaptador de rede pode ter opções para alterar o número de filas RSS como parte do driver. Exemplo de sintaxe:

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

Para obter mais informações, clique no seguinte link para baixar [Scalable Networking: Eliminando o gargalo de processamento de recebimento — Introdução ao RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) no formato Word.
  
#### <a name="understanding-rss-performance"></a>Noções básicas sobre o desempenho de RSS

O ajuste de RSS exige noções básicas sobre a configuração e a lógica de balanceamento de carga. Para verificar se as configurações de RSS entraram em vigor, você pode examinar a saída quando você executa o **Get-NetAdapterRss** cmdlet do Windows PowerShell. A seguir está um exemplo de saída deste cmdlet.
  
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

Além de ecoar os parâmetros que foram definidos, o principal aspecto da saída é a saída de tabela de indireção. A tabela de indireção exibe os buckets de tabela de hash são usados para distribuir o tráfego de entrada. Neste exemplo, a notação n:c designa Numa K-par de índice do grupo: CPU que é usado para direcionar o tráfego de entrada. Podemos ver exatamente 2 entradas exclusivas (0:0 e 0:4), que representam 0/cpu0 grupo k e grupo k 0/cpu 4, respectivamente.

Há apenas um grupo de k para esse sistema (k-grupo 0) e um n (em que n < = 128) entrada da tabela de indireção. Como o número de filas de recebimento é definido como 2, somente 2 processadores (0:0, 0:4) são escolhidos - mesmo que processadores máximas é definida como 8. Na verdade, a tabela de indireção é hash de tráfego de entrada para usar somente 2 CPUs fora a 8, que estão disponíveis.

Para utilizar totalmente as CPUs, o número de filas de recebimento de RSS deve ser igual ou maior que o máximo de processadores. No exemplo anterior, a fila de recebimento deve ser definida como 8 ou maior.

#### <a name="nic-teaming-and-rss"></a>Agrupamento NIC e RSS

RSS pode ser habilitada em um adaptador de rede é agrupado com outra placa de interface de rede usando o agrupamento NIC. Nesse cenário, somente o adaptador de rede física subjacente pode ser configurado para usar o RSS. Um usuário não é possível definir os cmdlets RSS no adaptador de rede agrupados.
  
###  <a name="bkmk_rsc"></a> Receber união de segmentos (RSC)

Receber união de segmentos \(RSC\) ajuda no desempenho, reduzindo o número de cabeçalhos de IP que serão processados para uma determinada quantidade de dados recebidos. Ele deve ser usado para ajudar a dimensionar o desempenho de dados recebidos pelo agrupamento \(ou de união\) os pacotes menores em unidades maiores.

Essa abordagem pode afetar a latência com benefícios principalmente vistos em ganhos de taxa de transferência. A RSC é recomendável aumentar a taxa de transferência para cargas de trabalho pesadas recebidas. Considere a implantação de adaptadores de rede que dão suporte a RSC. 

Esses adaptadores de rede, verifique se a RSC na \(essa é a configuração padrão\), a menos que você tiver cargas de trabalho específicas \(por exemplo, baixa latência, a rede de taxa de transferência de baixa\) que se beneficiam de apresentação de RSC que está sendo desativado .

#### <a name="understanding-rsc-diagnostics"></a>Noções básicas de diagnóstico RSC

Você pode diagnosticar RSC usando os cmdlets do Windows PowerShell **Get-NetAdapterRsc** e **Get-NetAdapterStatistics**.

A seguir está o exemplo de saída quando você executar o cmdlet Get-NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

O **obter** cmdlet mostra se a RSC é habilitada na interface e se o TCP permite que a RSC estar em um estado operacional. O motivo da falha fornece detalhes sobre a falha ao habilitar a RSC nessa interface.

No cenário anterior, a RSC IPv4 é suportados e operacionais na interface. Para entender as falhas de diagnóstico, é possível ver os bytes conciliados ou exceções causadas. Isso fornece uma indicação dos problemas de união.

A seguir está o exemplo de saída quando você executar o cmdlet Get-NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>A RSC e virtualização

A RSC é suportada apenas no host físico quando o adaptador de rede do host não está associado ao comutador Virtual Hyper-V. A RSC é desabilitada pelo sistema operacional quando o host está associado ao comutador Virtual Hyper-V. Além disso, as máquinas virtuais não se beneficiarão da RSC porque os adaptadores de rede virtual não dão suporte a RSC.

A RSC pode ser habilitada para uma máquina virtual quando único Root Input/Output Virtualization \(SR-IOV\) está habilitado. Nesse caso, a funções virtuais dão suporte a funcionalidade da RSC; Portanto, as máquinas virtuais também recebem o benefício da RSC.

##  <a name="bkmk_resources"></a> Recursos do adaptador de rede

Alguns adaptadores de rede gerenciar ativamente os seus recursos para obter o melhor desempenho. Vários adaptadores de rede permitem que você configurar manualmente os recursos usando o **Advanced Networking** guia para o adaptador. Para esses adaptadores, você pode definir os valores de um número de parâmetros, incluindo o número de buffers de recepção e buffers de envio.

Configurando recursos de adaptador de rede é simplificada pelo uso dos cmdlets do Windows PowerShell a seguir.

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

Para obter mais informações, consulte [Cmdlets do adaptador de rede no Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

Para obter links para todos os tópicos neste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).