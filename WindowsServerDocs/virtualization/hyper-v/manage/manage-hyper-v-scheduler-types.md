---
title: Compreendendo e usando tipos de Agendador de hipervisor Hyper-V
description: Fornece informações para administradores de host do Hyper-V sobre o uso do Agendador do Hyper-V modos
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: c0c2f85fbbeca9e8ac5d40bbcb71f286fabfb65c
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501667"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>Gerenciar tipos de Agendador de hipervisor Hyper-V

>Aplica-se a: Windows 10, Windows Server 2016, Windows Server, versão 1709, o Windows Server, versão 1803, Windows Server 2019

Este artigo descreve os novos modos de lógica introduzido pela primeira vez no Windows Server 2016 de agendamento do processador virtual. Esses modos ou tipos de Agendador, determinam como o hipervisor Hyper-V aloca e gerencia o trabalho entre processadores virtuais de convidado. Um administrador de host do Hyper-V pode selecionar tipos de Agendador do hipervisor que são mais adequados para as máquinas virtuais de convidados (VMs) e configurar as máquinas virtuais para tirar proveito da lógica de agendamento.

>[!NOTE]
>Atualizações são necessárias para usar os recursos do Agendador de hipervisor descritos neste documento. Para obter detalhes, consulte [as atualizações necessárias](#required-updates).

## <a name="background"></a>Histórico

Antes de discutir a lógica e os controles por trás de agendamento do processador virtual do Hyper-V, é útil para examinar os conceitos básicos abordados neste artigo.

### <a name="understanding-smt"></a>Noções básicas sobre SMT

Simultâneo de multithreading ou SMT, é uma técnica utilizada em designs de processador moderno, o que permite que os recursos do processador para serem compartilhados por threads de execução separado e independente. SMT geralmente oferece um aumento de desempenho modestos para a maioria das cargas de trabalho ao paralelizar computações quando possível, aumentando a taxa de transferência de instrução, embora nenhum desempenho obter ou até mesmo uma pequena perda de desempenho podem ocorrer quando a contenção entre threads para recursos do processador compartilhado ocorre.
Processadores que suportam SMT são disponibilizados pela Intel e AMD. Intel refere-se a suas ofertas SMT como a tecnologia Intel Hyper Threading ou HT Intel.

Para os fins deste artigo, as descrições de SMT e como ele é utilizado pelo Hyper-V se aplicam igualmente a sistemas Intel e AMD.

* Para obter mais informações sobre a tecnologia Intel HT, consulte [tecnologia Intel Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* Para obter mais informações sobre o SMT AMD, consulte [a arquitetura principal de "Zen"](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>Noções básicas sobre como o Hyper-V virtualiza os processadores

Antes de considerar os tipos de Agendador de hipervisor, também é útil para entender a arquitetura do Hyper-V. Você pode encontrar um resumo geral na [visão geral da tecnologia do Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Estes são os conceitos importantes para este artigo:

* Hyper-V cria e gerencia as partições de máquina virtual, em qual computação recursos são alocados e compartilhados, sob o controle do hipervisor. Partições fornecem limites de isolamento forte entre todas as máquinas virtuais convidadas e entre VMs convidadas e na partição raiz.

* A partição de raiz em si é uma partição de máquina virtual, embora ele tenha privilégios muito maiores do que as máquinas virtuais de convidado e propriedades exclusivas. A partição raiz fornece os serviços de gerenciamento que controlam todas as máquinas virtuais de convidado, fornece suporte a dispositivos virtuais para convidados e gerencia todos os dispositivos e/s para máquinas virtuais convidadas. Microsoft recomenda não executando qualquer carga de trabalho do aplicativo na partição raiz.

* Cada processador virtual (VP) da partição raiz é mapeada 1:1 para um processador lógico subjacente (LP). Um host VP sempre será executado sobre o LP subjacente mesmo – não há nenhuma migração dos vice-presidentes da partição raiz.

* Por padrão, os LPs no qual executar vice-presidentes host também podem executar vice-presidentes de convidado.

* Uma VICE-PRESIDENTE de convidado pode ser agendado pelo hipervisor para ser executado em qualquer processador lógico disponível. Enquanto o Agendador de hipervisor se encarrega de considerar a localidade do cache temporal, topologia NUMA e muitos outros fatores ao agendar uma VP de convidado, por fim, o vice-Presidente pudessem ser agendada em qualquer host LP.

## <a name="hypervisor-scheduler-types"></a>Tipos de Agendador do hipervisor

Começando com o Windows Server 2016, o hipervisor Hyper-V dá suporte a vários modos de lógica de Agendador, que determinam como o hipervisor agenda processadores virtuais nos processadores lógicos subjacentes. Esses tipos do Agendador são:

- [O Agendador clássico, compartilhamento de fair](#the-classic-scheduler)
- [O Agendador de núcleo](#the-core-scheduler)
- [O Agendador de raiz](#the-root-scheduler)

### <a name="the-classic-scheduler"></a>O Agendador clássico

O Agendador clássico tem sido o padrão para todas as versões do hipervisor Hyper-V do Windows desde sua criação, incluindo o Windows Server 2016 Hyper-V. O Agendador clássico fornece uma parte justa, modelo de agendamento de round-robin preemptive para processadores virtuais de convidado.

O tipo de Agendador clássico é o mais apropriado para a maioria dos usos do Hyper-V tradicionais – para nuvens privadas, provedores de hospedagem e assim por diante. As características de desempenho são bem compreendidas e melhor são otimizadas para dar suporte a uma ampla variedade de cenários de virtualização, como excesso de assinatura de vice-presidentes para LPs, muitas VMs heterogêneos e cargas de trabalho executados simultaneamente, executando escala maior alta conjunto de VMs, com suporte completo de recursos de desempenho do Hyper-V sem restrições e muito mais.

### <a name="the-core-scheduler"></a>O Agendador de núcleo

O Agendador de núcleo do hipervisor é uma nova alternativa para a lógica de Agendador clássico, introduzida no Windows Server 2016 e Windows 10 versão 1607. O Agendador core oferece um limite de segurança forte para o isolamento de carga de trabalho de convidado e variabilidade de desempenho reduzido para cargas de trabalho dentro de VMs em execução em um host de virtualização habilitados SMT. O Agendador core permite que a SMT e não-SMT máquinas virtuais em execução simultaneamente no mesmo host de virtualização habilitados SMT.

O Agendador core utiliza a topologia SMT do host de virtualização e, opcionalmente, expõe pares SMT para máquinas virtuais convidadas e grupos de agendas de processadores virtuais de convidado da máquina virtual em grupos de processadores lógicos de SMT. Isso é feito simetricamente para que se LPs estão em grupos de dois, vice-presidentes estão agendados em grupos de dois, e um núcleo nunca é compartilhado entre as VMs.
Quando o vice-Presidente está agendada para uma máquina virtual sem SMT habilitado, vice-Presidente consumirá todos os principais quando ele é executado.

O resultado geral do Agendador core é que:

* Convidado vice-presidentes são restritos a executar em subjacente pares de núcleos físicos, isolar uma VM para os limites de núcleo de processador, reduzindo assim a vulnerabilidade a ataques de espionagem de canal lateral de máquinas virtuais mal-intencionadas.

* A variabilidade na taxa de transferência é significativamente reduzida.

* Desempenho potencialmente é reduzido, pois se apenas de um grupo de vice-presidentes pode ser executada, apenas um dos fluxos de instrução no núcleo do executa enquanto o outro é deixado inativo.

* O sistema operacional e aplicativos em execução na máquina virtual convidada podem utilizar o comportamento SMT e programando APIs (interfaces) para controlar e distribuir o trabalho entre threads SMT, assim como quando eles seriam executado não-virtualizados.

* Um limite de segurança forte para o isolamento de carga de trabalho de convidado - vice-presidentes convidado são restritos a executar em pares de núcleo físico subjacente, reduzindo a vulnerabilidade a ataques de espionagem de canal lateral.

O Agendador de núcleo será usado por padrão, a partir do Windows Server 2019. No Windows Server 2016, o Agendador de núcleo é opcional e deve ser habilitado explicitamente pelo administrador do host do Hyper-V e o Agendador clássico é o padrão.

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>Comportamento de Agendador de núcleo com o host SMT desabilitado

Se o hipervisor é configurado para usar o tipo de Agendador core, mas a capacidade SMT está desabilitado ou não está presente no host de virtualização, o hipervisor usará o comportamento de Agendador clássico, independentemente da configuração do tipo de Agendador do hipervisor.

### <a name="the-root-scheduler"></a>O Agendador de raiz

O Agendador de raiz foi introduzido com o Windows 10 versão 1803. Quando o tipo de Agendador raiz está habilitado, o hipervisor cede o controle de agendamento de trabalho para a partição raiz. O Agendador de NT na instância do sistema operacional da partição raiz gerencia todos os aspectos de agendamento de trabalho ao sistema LPs.

O Agendador de raiz aborda os requisitos exclusivos fazia com que dão suporte a uma partição de utilitário para fornecer isolamento de carga de trabalho de alta segurança, conforme usado com o Windows Defender Application Guard (WDAG). Nesse cenário, deixar o agendamento de responsabilidades para a raiz do sistema operacional oferece diversas vantagens. Por exemplo, controles de recursos de CPU aplicável para cenários de contêiner podem ser usados com a partição do utilitário, simplificando o gerenciamento e implantação. Além disso, o Agendador de sistema operacional raiz prontamente pode coletar métricas sobre a utilização da CPU dentro do contêiner de carga de trabalho e usar esses dados como entrada para a mesma política de agendamento aplicável a todas as outras cargas de trabalho no sistema. Essas mesmas métricas também ajudam claramente atributo o trabalho feito em um contêiner de aplicativo para o sistema host. Essas métricas de controle é mais difícil com cargas de trabalho de máquinas virtuais tradicionais, onde algum trabalho em nome de todas as VM em execução ocorre na partição raiz.

#### <a name="root-scheduler-use-on-client-systems"></a>Uso do Agendador de raiz em sistemas de cliente

Começando com o Windows 10 versão 1803, o Agendador de raiz é usado por padrão nos sistemas do cliente, em que o hipervisor pode ser habilitado para dar suporte ao isolamento de carga de trabalho WDAG e segurança baseada em virtualização e para a operação correta de futuros sistemas com arquiteturas de núcleo heterogêneos. Essa é a configuração de Agendador de hipervisor com suporte apenas para sistemas cliente. Os administradores não devem tentar substituir o tipo de Agendador do hipervisor padrão em sistemas de cliente do Windows 10.

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>Controles de recursos de CPU da máquina virtual e o Agendador de raiz

Os controles de recursos do processador de máquina virtual fornecidos pelo Hyper-V não são suportados quando o Agendador de raiz de hipervisor está habilitado como lógica de Agendador do sistema operacional raiz está gerenciando recursos de host de forma global e não tem conhecimento de uma VM definições de configuração específicas. Os controles de recursos do Hyper-V por VM processador, como caps, pesos e reservas, só são aplicáveis em que o hipervisor controla diretamente o vice-Presidente, agendamento, assim como acontece com os tipos de Agendador clássico e do núcleo.

#### <a name="root-scheduler-use-on-server-systems"></a>Uso do Agendador de raiz em sistemas de servidor

O Agendador de raiz não é recomendado para uso com o Hyper-V em servidores no momento, conforme suas características de desempenho ainda não foram caracterizadas totalmente e ajustadas para acomodar a grande variedade de cargas de trabalho típicas de muitas implantações de virtualização de servidor.

## <a name="enabling-smt-in-guest-virtual-machines"></a>Habilitando o SMT em máquinas virtuais convidadas

Depois que o hipervisor do host de virtualização está configurado para usar o tipo de Agendador core, as máquinas virtuais convidadas podem ser configuradas para utilizar SMT, se desejado. Expondo o fato de que os vice-presidentes são Hyper-threaded para uma máquina virtual convidada, permite que o Agendador no sistema operacional convidado e cargas de trabalho em execução na VM para detectar e utilizar a topologia SMT em seu próprio agendamento de trabalho. No Windows Server 2016, o convidado SMT não está configurado por padrão e deve ser habilitado explicitamente pelo administrador do host do Hyper-V. Começando com o Windows Server 2019, novas VMs criadas no host herdará da topologia do host SMT por padrão.  Ou seja, uma versão que 9.0 VM criada em um host com 2 threads SMT por núcleo também veria 2 threads SMT por núcleo.

PowerShell deve ser usado para habilitar o SMT em uma máquina virtual de convidado. Não há nenhuma interface do usuário fornecida no Gerenciador do Hyper-V.
Para habilitar o SMT em uma máquina virtual convidada, abra uma janela do PowerShell com permissões suficientes e o tipo:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Onde <n> é o número de threads SMT por núcleo de convidado VM será exibida.  
Observe que <n> = 0 definirá o valor de HwThreadCountPerCore para corresponder à contagem de threads do host SMT por valor principal.

>[!NOTE] 
>Definindo HwThreadCountPerCore = 0 tem suporte a partir do Windows Server 2019.

Abaixo está um exemplo de informações de sistema extraídas do sistema operacional convidado em execução em uma máquina virtual com 2 processadores virtuais e SMT habilitado. O sistema operacional convidado está detectando 2 processadores lógicos que pertencem ao mesmo núcleo.

![Captura de tela que mostra msinfo32 em uma VM com SMT habilitado convidada](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>Configurando o tipo de Agendador do hipervisor no Windows Server 2016 Hyper-V

Windows Server 2016 Hyper-V usa o modelo de Agendador do hipervisor clássico por padrão. O hipervisor pode ser configurado opcionalmente para usar o Agendador de core, para aumentar a segurança, restringindo vice-presidentes de convidado para executar nos pares SMT físicos correspondentes e suporte ao uso de máquinas virtuais com o agendamento de SMT para seus vice-presidentes de convidado.

>[!NOTE]
>A Microsoft recomenda que todos os clientes executando o Windows Server 2016 Hyper-V selecionarem o Agendador de núcleo para garantir que seus hosts de virtualização ideal estão protegidos contra máquinas virtuais convidadas potencialmente mal-intencionados.

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>Padrões de servidor 2019 Hyper-V do Windows para usar o Agendador de núcleo

Para ajudar a garantir que os hosts do Hyper-V são implantados na configuração de segurança ideal, Windows Server 2019 Hyper-V agora usarão o modelo de Agendador do hipervisor core por padrão. O administrador do host, opcionalmente, pode configurar o host para usar o Agendador clássico herdado. Os administradores devem cuidadosamente ler, compreender e considerar o impacto de que cada tipo de Agendador tem sobre a segurança e o desempenho dos hosts de virtualização antes da substituição das configurações de padrão de tipo do Agendador.  Ver [seleção do tipo de Agendador Noções básicas sobre Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) para obter mais informações.

### <a name="required-updates"></a>Atualizações necessárias

>[!NOTE]
>As seguintes atualizações são necessárias para usar os recursos do Agendador de hipervisor descritos neste documento. Essas atualizações incluem alterações para dar suporte a nova opção de BCD 'hypervisorschedulertype', que é necessária para configuração do host.

| Version | Versão  | Atualização necessária | Artigo da KB |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Nenhuma | Nenhuma |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>Selecionar o tipo de Agendador do hipervisor no Windows Server

A configuração do Agendador de hipervisor é controlada por meio de entrada de BCD hypervisorschedulertype.

Para selecionar um tipo de Agendador, abra um prompt de comando com privilégios de administrador:

``` command
     bcdedit /set hypervisorschedulertype type
```

Onde `type` é um dos:

* Clássico
* Core
* Raiz

O sistema deve ser reinicializado para todas as alterações para o tipo de Agendador do hipervisor entrem em vigor.

>[!NOTE]
>O Agendador de raiz do hipervisor não há suporte no Windows Server Hyper-V neste momento. Administradores do Hyper-V não devem tentar configurar o Agendador de raiz para uso com cenários de virtualização de servidor.

## <a name="determining-the-current-scheduler-type"></a>Determinando o tipo atual do Agendador

Você pode determinar o tipo de Agendador do hipervisor atual em uso, examinando o log do sistema no Visualizador de eventos para o evento de lançamento mais recente do hipervisor ID 2, que informa o tipo de Agendador do hipervisor configurado na inicialização do hipervisor. Eventos de lançamento do hipervisor podem ser obtidos do Visualizador de eventos do Windows, ou por meio do PowerShell.

Evento de lançamento do hipervisor ID 2 indica o tipo de Agendador do hipervisor, onde:

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Captura de tela mostrando detalhes de 2 de ID de evento de lançamento de hipervisor](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Captura de tela mostrando a exibição de evento de lançamento do hipervisor ID 2 do Visualizador de eventos](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>Consultando o Hyper-V hipervisor Agendador tipo evento de lançamento usando o PowerShell

A consulta para o evento de hipervisor ID 2 usando o PowerShell, digite os seguintes comandos em um prompt do PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Captura de tela mostrando a consulta do PowerShell e os resultados para o evento de lançamento do hipervisor ID 2](media/Hyper-V-CoreScheduler-PowerShell.png)
