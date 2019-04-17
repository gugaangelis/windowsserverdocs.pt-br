---
title: Noções básicas e uso de tipos de Agendador de hipervisor do Hyper-V
description: Fornece informações para administradores de host do Hyper-V no uso de Agendador do Hyper-V modos
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: 7af6d68b02367d349580eacb27405c6f37e97ff8
ms.sourcegitcommit: 3883eebbba70bfea0221e510863ee1a724a5f926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2018
ms.locfileid: "5783688"
---
# Gerenciamento de tipos de Agendador de hipervisor do Hyper-V

>Aplicável a: Windows 10, Windows Server 2016, Windows Server, versão 1709, Windows Server, versão 1803, Windows Server 2019

Este artigo descreve novos modos de processador virtual agendamento lógica introduzida no Windows Server 2016. Esses modos, ou tipos de Agendador, determinam como o hipervisor Hyper-V aloca e gerencia o trabalho em processadores virtuais convidadas. Um administrador do host Hyper-V pode selecionar tipos de Agendador de hipervisor que são mais adequado para as convidado VMs (máquinas virtuais) e configurar as VMs para tirar proveito da lógica do agendamento.

>[!NOTE]
>Atualizações são necessárias para usar os recursos do Agendador de hipervisor descritos neste documento. Para obter detalhes, consulte [as atualizações necessárias](#required-updates).

## Tela de fundo

Antes de abordar a lógica e os controles por trás de agendamento do processador virtual do Hyper-V, é útil examinar os conceitos básicos abordados neste artigo.

### Noções básicas sobre SMT

Multithreading simultâneo ou SMT, é uma técnica utilizada em designs de processador moderno que permite que os recursos do processador ser compartilhada pelos segmentos de execução separados, independentes. SMT geralmente oferece um aumento de desempenho moderado a maioria das cargas de trabalho por paralelizar cálculos quando possível, aumentando a taxa de transferência de instrução, embora nenhum desempenho obter ou até mesmo uma pequena perda no desempenho pode ocorrer quando contenção entre threads para recursos do processador compartilhado ocorre.
Processadores que suportam SMT estão disponíveis Intel e AMD. Intel refere-se a suas ofertas SMT como a tecnologia Intel Hyper Threading ou HT Intel.

Para os fins deste artigo, as descrições de SMT e como ele é utilizado pelo Hyper-V se aplicam igualmente para sistemas Intel e AMD.

* Para obter mais informações sobre a tecnologia Intel HT, consulte a [Tecnologia Intel Hyper-Threading](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* Para obter mais informações sobre AMD SMT, consulte [A arquitetura de Core "Zen"](https://www.amd.com/en/technologies/zen-core)

## Noções básicas sobre como o Hyper-V virtualiza processadores

Antes de considerar hipervisor tipos Agendador, também é útil entender a arquitetura do Hyper-V. Você pode encontrar um resumo geral na [Visão geral da tecnologia Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Estes são conceitos importantes para este artigo:

* Hyper-V cria e gerencia as partições de máquina virtual, em quais computação os recursos são alocados e compartilhados, sob o controle do hipervisor. Partições fornecem limites de isolamento forte entre todas as máquinas virtuais de convidado e entre VMs convidadas e a partição raiz.

* A partição raiz em si for uma partição de máquina virtual, embora ele tenha propriedades exclusivas e muito maiores privilégios de máquinas virtuais convidadas. A partição raiz fornece os serviços de gerenciamento que controlam todas as máquinas virtuais de convidado, oferece suporte a dispositivos virtuais para convidados e gerencia todos os dispositivos e/s para máquinas virtuais convidadas. A Microsoft recomenda não executando qualquer cargas de trabalho do aplicativo na partição raiz.

* Cada processador virtual (vice-Presidente) da partição raiz é 1:1 mapeada para um processador lógico subjacente (LP). Um host vice-Presidente sempre será executado no mesmo LP subjacente – não há nenhuma migração de VPs da partição raiz.

* Por padrão, os LPs em que executar o host VPs também podem executar VPs convidado.

* Um VICE-PRESIDENTE convidado pode ser agendado pelo hipervisor para ser executado em qualquer processador lógico disponível. Enquanto o Agendador de hipervisor cuida considerar localidade temporal cache, NUMA topologia e muitos outros fatores ao agendar um VICE-PRESIDENTE convidado, por fim, o vice-Presidente poderia ser agendado em qualquer host LP.

## Tipos de Agendador do hipervisor

Começando com o Windows Server 2016, o hipervisor Hyper-V oferece suporte a vários modos de lógica Agendador, que determinam como o hipervisor agenda processadores virtuais nos processadores lógicos subjacentes. Esses tipos de Agendador são:

- [O Agendador clássico, fração justa](#the-classic-scheduler)
- [O Agendador core](#the-core-scheduler)
- [O Agendador de raiz](#the-root-scheduler)

### O Agendador clássico

O Agendador clássico tem sido o padrão para todas as versões do hipervisor Hyper-V do Windows desde sua criação, incluindo o Windows Server 2016 Hyper-V. O Agendador clássico fornece uma parte justa, modelo de agendamento de rodízio preventivo para processadores virtuais convidadas.

O tipo de Agendador clássico é o mais apropriado para a maioria dos usos de Hyper-V tradicionais – para nuvens privadas, os provedores de hospedagem e assim por diante. As características de desempenho sejam bem entendidas e melhor são otimizadas para dar suporte a uma ampla variedade de cenários de virtualização, como a sobreposição de VPs a LPs, executando muitos heterogêneas VMs e cargas de trabalho simultaneamente, executando escala maior alta conjunto de desempenho VMs, dando suporte completo de recursos do Hyper-V sem restrições e muito mais.

### O Agendador core

O Agendador do hipervisor core é uma nova alternativa à lógica Agendador clássico, introduzida no Windows Server 2016 e Windows 10 versão 1607. O Agendador core oferece um limite de segurança forte para o isolamento da carga de trabalho de convidado e variação de redução de desempenho para cargas de trabalho dentro de VMs em execução em um host de virtualização SMT habilitado. O Agendador core permite executar máquinas virtuais SMT e não-SMT simultaneamente no mesmo host de virtualização habilitada SMT.

O Agendador core utiliza a topologia SMT do host de virtualização e, opcionalmente, expõe pares SMT máquinas virtuais convidadas e agendas grupos de processadores virtuais convidado a mesma máquina virtual em grupos de processadores lógicos SMT. Isso é feito simétrica para que se LPs estiverem em grupos de dois, VPs sejam agendados em grupos de dois, e um núcleo nunca é compartilhado entre as VMs.
Quando o vice-Presidente está agendada para uma máquina virtual sem SMT habilitado, vice-Presidente consumirá todos os principais quando ele é executado.

O resultado geral do Agendador core é que:

* Convidado VPs são restringidos seja executado em subjacente pares de núcleo físico, isolando uma VM aos limites de núcleo do processador, reduzindo assim a vulnerabilidade a ataques de espionagem do canal de VMs mal-intencionados.

* Variação na taxa de transferência é reduzida significativamente.

* Desempenho potencialmente é reduzido, pois se apenas um de um grupo de VPs puder ser executado, apenas um dos fluxos instrução no principal é executado enquanto o outro é deixado ocioso.

* O sistema operacional e aplicativos em execução na máquina virtual convidada podem utilizar comportamento SMT e (APIs) para controlar e distribuir o trabalho em threads SMT, assim como eles seriam executado quando não virtualizados interfaces de programação.

* Um limite de segurança forte para o isolamento de carga de trabalho de convidado - convidado VPs são restringidos seja executado em pares de núcleo físico subjacente, reduzindo a vulnerabilidade a ataques de espionagem de canal.

O Agendador de núcleo será usado por padrão a partir do Windows Server 2019. No Windows Server 2016, o Agendador core é opcional e deve ser habilitado explicitamente pelo administrador do host Hyper-V e o Agendador clássico é o padrão.

#### Comportamento de Agendador de núcleo com host SMT desabilitado

Se o hipervisor está configurado para usar o tipo de Agendador core, mas a funcionalidade SMT é desabilitado ou não está presente no host de virtualização, o hipervisor usará o comportamento de Agendador clássico, independentemente do tipo de Agendador de hipervisor configuração.

### O Agendador de raiz

O Agendador de raiz foi introduzido com o Windows 10 versão 1803. Quando o tipo de Agendador raiz está habilitado, o hipervisor cedes controle de agendamento de trabalho para a partição raiz. O Agendador NT na instância do sistema operacional da partição raiz gerencia todos os aspectos do agendamento de trabalho ao sistema LPs.

O Agendador de raiz aborda os requisitos exclusivos inerentes com uma partição de utilitário de suporte para fornecer isolamento de carga de trabalho forte, como usado com o Windows Defender Application Guard (WDAG). Nesse cenário, deixar o agendamento responsabilidades para a raiz do sistema operacional oferece várias vantagens. Por exemplo, controles de recursos de CPU aplicável a cenários de contêiner podem ser usados com a partição do utilitário, simplificando o gerenciamento e implantação. Além disso, o Agendador do sistema operacional raiz prontamente pode coletar métricas sobre utilização da CPU dentro do contêiner de carga de trabalho e usar esses dados como entrada para a mesma política agendamento aplicável para todas as outras cargas de trabalho no sistema. Essas mesmas métricas também ajudam a claramente atributo trabalhar concluído em um contêiner de aplicativo para o sistema host. Essas métricas de rastreamento é mais difícil com cargas de trabalho tradicional máquina virtual, onde algum trabalho em nome da VM em execução todos os ocorre na partição raiz.

#### Uso do Agendador de raiz nos sistemas do cliente

Começando com Windows 10 versão 1803, o Agendador de raiz é usado por padrão nos sistemas do cliente só, onde o hipervisor pode ser habilitado para dar suporte a segurança baseada em virtualização e o isolamento de carga de trabalho WDAG e para a operação correta dos sistemas futuros com arquiteturas de núcleo heterogêneos. Essa é a configuração de Agendador do hipervisor com suporte somente para sistemas do cliente. Os administradores não devem tentar substituir o tipo de Agendador de hipervisor padrão em sistemas de cliente do Windows 10.

#### Controles de recursos de CPU de máquina virtual e o Agendador de raiz

Os controles de recursos de processador de máquina virtual fornecidos pelo Hyper-V não são suportados quando o Agendador de raiz do hipervisor está habilitado como a lógica de Agendador do sistema operacional raiz é gerenciamento de recursos de host em uma base global e não tem conhecimento de uma VM definições de configuração específicas. Os controles de recursos do Hyper-V-VM processador, como caps, espessuras e reservas, são aplicáveis somente onde o hipervisor controla diretamente vice-Presidente agendamento, assim como acontece com os tipos de Agendador clássico e core.

#### Uso do Agendador de raiz nos sistemas de servidor

O Agendador de raiz não é recomendado para uso com o Hyper-V em servidores neste momento, como suas características de desempenho ainda não foram caracterizadas totalmente e ajustadas para acomodar a ampla variedade de cargas de trabalho típicas de muitas implantações de virtualização do servidor.

## Habilitar SMT em máquinas virtuais convidadas

Depois que o hipervisor do host de virtualização estiver configurado para usar o tipo de Agendador core, máquinas virtuais convidadas podem ser configuradas para utilizar SMT se desejado. Expor o fato de que VPs são hiperprocessamento para uma máquina virtual convidada permite que o Agendador no sistema operacional convidado e cargas de trabalho em execução na VM para detectar e utilizar a topologia de SMT em seu próprio agendamento de trabalho. No Windows Server 2016, convidado SMT não é configurado por padrão e deve ser habilitado explicitamente pelo administrador do host Hyper-V. Começando com o Windows Server 2019, novas VMs criadas no host herdarão topologia SMT do host por padrão.  Ou seja, uma versão que VM 9.0 criada em um host com 2 threads SMT por núcleo também veria 2 threads SMT por núcleo.

PowerShell deve ser usado para habilitar SMT em uma máquina virtual de convidado; Não há nenhuma interface do usuário fornecida no Gerenciador do Hyper-V.
Para habilitar SMT em uma máquina virtual convidada, abra uma janela do PowerShell com permissões suficientes e digite:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Onde <n> é o número de threads SMT por núcleo convidado VM verá.  
Observe que <n> = 0 definirá o valor de HwThreadCountPerCore para corresponder a contagem de threads SMT do host por valor de núcleo.

>[!NOTE] 
>Definindo HwThreadCountPerCore = 0 é compatível a partir do Windows Server 2019.

Abaixo está um exemplo de informações do sistema obtido a partir do sistema operacional convidado em execução em uma máquina virtual com 2 processadores virtuais e SMT habilitada. O sistema operacional convidado está detectando 2 processadores lógicos pertencentes ao mesmo núcleo.

![Captura de tela que mostra msinfo32 em um convidado VM com SMT habilitado](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## Configurando o tipo de Agendador do hipervisor no Windows Server 2016 Hyper-V

Windows Server 2016 Hyper-V usa o modelo de Agendador do hipervisor clássico por padrão. O hipervisor pode ser configurado opcionalmente para usar o Agendador de núcleo, para aumentar a segurança restringindo VPs convidado seja executado em pares SMT físicos correspondentes e suporte ao uso de máquinas virtuais com SMT agendamento para seus VPs convidado.

>[!NOTE]
>A Microsoft recomenda que todos os clientes que executam o Windows Server 2016 Hyper-V selecionarem o Agendador de core para garantir que seus hosts de virtualização ideal estão protegidos contra VMs convidadas potencialmente mal-intencionados.

## Padrões do Windows Server 2019 Hyper-V usando o Agendador core

Para ajudar a garantir a hosts Hyper-V são implantados em configuaration a segurança ideais, Windows Server 2019 Hyper-V agora usa o modelo de Agendador de hipervisor core por padrão. O administrador do host, opcionalmente, pode configurar o host para usar o Agendador clássico herdado. Os administradores cuidadosamente devem ler, entender e considere os impactos de que cada tipo de Agendador tem sobre a segurança e o desempenho de hosts de virtualização antes de substituir as configurações do Agendador tipo padrão.  Consulte [seleção de tipo de Agendador de Noções básicas de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) para obter mais informações.

### Atualizações necessárias

>[!NOTE]
>As atualizações a seguir são necessários para usar os recursos do Agendador de hipervisor descritos neste documento. Essas atualizações incluem alterações para oferecer suporte a nova opção de BCD 'hypervisorschedulertype', que é necessária para a configuração de host.

| Versão | Versão  | Atualização necessária | Artigo KB |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018.07 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018.07 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1709 | 2018.07 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Nenhum(a) | Nenhum(a) |

## Selecionando o tipo de Agendador de hipervisor no Windows Server

A configuração do hipervisor Agendador é controlada por meio da entrada de BCD hypervisorschedulertype.

Para selecionar um tipo de Agendador, abra um prompt de comando com privilégios de administrador:

``` command
     bcdedit /set hypervisorschedulertype type
```

Onde `type` é um dos:

* Clássico
* Core

O sistema deve ser reinicializado para as alterações para o tipo de Agendador de hipervisor entrem em vigor.

>[!NOTE]
>O Agendador de raiz do hipervisor não é suportado no Windows Server Hyper-V neste momento. Os administradores do Hyper-V não devem tentar configurar o Agendador de raiz para uso com cenários de virtualização do servidor.

## Determinar o tipo de Agendador atual

Você pode determinar o tipo de Agendador de hipervisor atual em uso examinando o log de Sysem no Visualizador de eventos para o evento de lançamento mais recente do hipervisor ID 2, que relata o tipo de Agendador de hipervisor configurado na inicialização do hipervisor. Eventos de inicialização do hipervisor podem ser obtidos do Visualizador de eventos do Windows, ou por meio do PowerShell.

Evento de lançamento do hipervisor 2 ID indica o tipo de Agendador do hipervisor, onde:

    1 = Classic scheduler, SMT disabled

    2 = Classic scheduler

    3 = Core scheduler

    4 = Root scheduler

![Captura de tela mostrando detalhes de evento 2 de ID de inicialização de hipervisor](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Captura de tela mostrando o Visualizador de eventos exibindo o evento de lançamento do hipervisor ID 2](media/Hyper-V-CoreScheduler-EventViewer.png)

### Consultando o Hyper-V hipervisor Agendador tipo evento de lançamento usando o PowerShell

A consulta para o evento de hipervisor ID 2 usando o PowerShell, digite os seguintes comandos em um prompt do PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Captura de tela mostrando a consulta do PowerShell e resultados de hipervisor iniciar evento ID 2](media/Hyper-V-CoreScheduler-PowerShell.png)
