---
title: Compreendendo e usando os tipos de Agendador de hipervisor do Hyper-V
description: Fornece informações para os administradores de host do Hyper-V no uso dos modos de Agendador do Hyper-V
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.localizationpriority: low
ms.assetid: 6cb13f84-cb50-4e60-a685-54f67c9146be
ms.openlocfilehash: 954efafe3185cadb347384c3c93a2eb8ef895143
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963552"
---
# <a name="managing-hyper-v-hypervisor-scheduler-types"></a>Gerenciando tipos de Agendador de hipervisor do Hyper-V

>Aplica-se a: Windows 10, Windows Server 2016, Windows Server, versão 1709, Windows Server, versão 1803, Windows Server 2019

Este artigo descreve os novos modos de lógica de agendamento do processador virtual introduzidos pela primeira vez no Windows Server 2016. Esses modos, ou tipos de Agendador, determinam como o hipervisor do Hyper-V aloca e gerencia o trabalho entre processadores virtuais convidados. Um administrador de host do Hyper-V pode selecionar os tipos de Agendador de hipervisor mais adequados para as VMs (máquinas virtuais) convidadas e configurar as VMs para aproveitar a lógica de agendamento.

> [!NOTE]
> São necessárias atualizações para usar os recursos do Agendador de hipervisor descritos neste documento. Para obter detalhes, consulte [atualizações necessárias](#required-updates).

## <a name="background"></a>Segundo plano

Antes de discutir a lógica e os controles por trás do agendamento do processador virtual do Hyper-V, é útil examinar os conceitos básicos abordados neste artigo.

### <a name="understanding-smt"></a>Entendendo o SMT

O multithreading simultâneo, ou SMT, é uma técnica utilizada em designs de processador modernos que permite que os recursos do processador sejam compartilhados por threads de execução separados e independentes. O SMT geralmente oferece um aumento de desempenho modesto para a maioria das cargas de trabalho ao paralelizar cálculos quando possível, aumentando a taxa de transferência de instrução, embora nenhum lucro de desempenho ou até mesmo uma pequena perda de desempenho possa ocorrer quando a contenção entre threads de recursos de processador compartilhados ocorre.
Os processadores com suporte a SMT estão disponíveis na Intel e na AMD. A Intel se refere a suas ofertas de SMT como tecnologia Intel Hyper Threading ou Intel HT.

Para os fins deste artigo, as descrições de SMT e como ela é utilizada pelo Hyper-V aplicam-se igualmente aos sistemas Intel e AMD.

* Para obter mais informações sobre a tecnologia Intel HT, consulte [tecnologia Hyper-Threading da Intel](https://www.intel.com/content/www/us/en/architecture-and-technology/hyper-threading/hyper-threading-technology.html)

* Para obter mais informações sobre o AMD SMT, consulte [a arquitetura principal do "Zen"](https://www.amd.com/en/technologies/zen-core)

## <a name="understanding-how-hyper-v-virtualizes-processors"></a>Entendendo como o Hyper-V virtualiza processadores

Antes de considerar os tipos de Agendador de hipervisor, também é útil entender a arquitetura do Hyper-V. Você pode encontrar um resumo geral em [visão geral da tecnologia Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-technology-overview). Estes são conceitos importantes para este artigo:

* O Hyper-V cria e gerencia partições de máquina virtual, entre as quais os recursos de computação são alocados e compartilhados, sob o controle do hipervisor. As partições fornecem limites de isolamento fortes entre todas as máquinas virtuais convidadas e entre as VMs convidadas e a partição raiz.

* A partição raiz é, em si, uma partição de máquina virtual, embora tenha propriedades exclusivas e privilégios muito maiores do que as máquinas virtuais convidadas. A partição raiz fornece os serviços de gerenciamento que controlam todas as máquinas virtuais convidadas, fornece suporte a dispositivos virtuais para convidados e gerencia todas as e/s de dispositivo para máquinas virtuais convidadas. A Microsoft recomenda enfaticamente não executar nenhuma carga de trabalho de aplicativo na partição raiz.

* Cada processador virtual (VP) da partição raiz é mapeado 1:1 para um processador lógico subjacente (LP). Um VP de host sempre é executado no mesmo LP subjacente – não há nenhuma migração do VPSs da partição raiz.

* Por padrão, o LPs em que o host VPSs executado também pode executar o convidado VPSs.

* Um vice-presidente convidado pode ser agendado pelo hipervisor para ser executado em qualquer processador lógico disponível. Enquanto o Agendador de hipervisor se preocupa em considerar a localidade do cache temporal, a topologia NUMA e muitos outros fatores ao programar um vice-presidente convidado, por fim, o VP pode ser agendado em qualquer host LP.

## <a name="hypervisor-scheduler-types"></a>Tipos de Agendador de hipervisor

A partir do Windows Server 2016, o hipervisor do Hyper-V dá suporte a vários modos de lógica do Agendador, que determinam como o hipervisor agenda processadores virtuais nos processadores lógicos subjacentes. Esses tipos de Agendador são:

### <a name="the-classic-scheduler"></a>O Agendador clássico

O Agendador clássico tem sido o padrão para todas as versões do hipervisor do Windows Hyper-V desde seu início, incluindo o Windows Server 2016 Hyper-V. O Agendador clássico fornece um modelo de agendamento Fair share, Preemptive Round Robin para processadores virtuais convidados.

O tipo de Agendador clássico é o mais apropriado para a grande maioria dos usos do Hyper-V tradicionais – para nuvens privadas, provedores de hospedagem e assim por diante. As características de desempenho são bem compreendidas e são melhor otimizadas para dar suporte a uma ampla gama de cenários de virtualização, como excesso de assinaturas de VPSs para LPs, execução simultânea de várias VMs e cargas de trabalho heterogêneas, execução de VMs de alto desempenho de escala maiores, suporte ao conjunto completo de recursos do Hyper-V sem restrições e muito mais.

### <a name="the-core-scheduler"></a>O Agendador principal

O Agendador principal do hipervisor é uma nova alternativa à lógica clássica do Agendador, introduzida no Windows Server 2016 e no Windows 10 versão 1607. O Agendador principal oferece um forte limite de segurança para isolamento de carga de trabalho de convidado e variabilidade de desempenho reduzida para cargas de trabalho dentro de VMs em execução em um host de virtualização habilitado para SMT. O Agendador principal permite executar máquinas virtuais SMT e não SMT simultaneamente no mesmo host de virtualização habilitado para SMT.

O Agendador principal utiliza a topologia SMT do host de virtualização e, opcionalmente, expõe pares de SMT para máquinas virtuais convidadas e agenda grupos de processadores virtuais convidados da mesma máquina virtual em grupos de processadores lógicos SMT. Isso é feito de forma simétrica para que, se LPs estiverem em grupos de dois, VPSs sejam agendados em grupos de dois, e um núcleo nunca seja compartilhado entre VMs.
Quando o VP está agendado para uma máquina virtual sem SMT habilitado, esse VP consome todo o núcleo quando ele é executado.

O resultado geral do Agendador principal é que:

* Os VPSs convidados são restritos a serem executados em pares de núcleos físicos subjacentes, isolando uma VM para limites de núcleo do processador, reduzindo assim a vulnerabilidade a ataques de espionagem de canal lateral de VMs mal-intencionadas.

* A variabilidade na taxa de transferência é significativamente reduzida.

* O desempenho é potencialmente reduzido, pois se apenas um de um grupo de VPSs puder ser executado, apenas um dos fluxos de instrução no núcleo será executado enquanto o outro permanecerá ocioso.

* O sistema operacional e os aplicativos em execução na máquina virtual convidada podem utilizar o comportamento SMT e as interfaces de programação (APIs) para controlar e distribuir o trabalho entre threads SMT, assim como fariam quando são executados não virtualizados.

* Um limite de segurança forte para isolamento de carga de trabalho de convidado – os VPSs convidados são restritos a serem executados em pares de núcleos físicos subjacentes, reduzindo a vulnerabilidade a ataques de espionagem de canal lateral.

O Agendador principal é usado por padrão a partir do Windows Server 2019. No Windows Server 2016, o Agendador principal é opcional e deve ser explicitamente habilitado pelo administrador do host Hyper-V e o Agendador clássico é o padrão.

#### <a name="core-scheduler-behavior-with-host-smt-disabled"></a>Comportamento do Agendador principal com host SMT desabilitado

Se o hipervisor estiver configurado para usar o tipo de Agendador principal, mas o recurso SMT estiver desabilitado ou não estiver presente no host de virtualização, o hipervisor usará o comportamento do Agendador clássico, independentemente da configuração do tipo de Agendador do hipervisor.

### <a name="the-root-scheduler"></a>O Agendador raiz

O Agendador raiz foi introduzido com o Windows 10 versão 1803. Quando o tipo de Agendador raiz é habilitado, o hipervisor fica com o controle do agendamento de trabalho para a partição raiz. O Agendador do NT na instância do sistema operacional da partição raiz gerencia todos os aspectos do trabalho de agendamento para o System LPs.

O Agendador raiz aborda os requisitos exclusivos inerentes ao suporte a uma partição de utilitário para fornecer isolamento de carga de trabalho forte, conforme usado com o WDAG (Windows Defender Application Guard). Nesse cenário, deixar responsabilidades de agendamento para o sistema operacional raiz oferece várias vantagens. Por exemplo, os controles de recursos de CPU aplicáveis a cenários de contêiner podem ser usados com a partição do utilitário, simplificando o gerenciamento e a implantação. Além disso, o Agendador do sistema operacional raiz pode rapidamente coletar métricas sobre a utilização da CPU da carga de trabalho dentro do contêiner e usar esses dados como entrada para a mesma política de agendamento aplicável a todas as outras cargas de trabalho no sistema. Essas mesmas métricas também ajudam claramente o trabalho de atributo feito em um contêiner de aplicativo para o sistema host. Controlar essas métricas é mais difícil com as cargas de trabalho de máquinas virtuais tradicionais, em que alguns trabalhos em todas as VMs em execução ocorrem na partição raiz.

#### <a name="root-scheduler-use-on-client-systems"></a>Uso do Agendador raiz em sistemas cliente

A partir do Windows 10 versão 1803, o Agendador raiz é usado por padrão somente em sistemas cliente, em que o hipervisor pode ser habilitado para dar suporte a segurança baseada em virtualização e isolamento de carga de trabalho WDAG e para uma operação adequada de sistemas futuros com arquiteturas de núcleos heterogêneos. Essa é a única configuração do Agendador de hipervisor com suporte para sistemas cliente. Os administradores não devem tentar substituir o tipo de Agendador de hipervisor padrão em sistemas de cliente do Windows 10.

#### <a name="virtual-machine-cpu-resource-controls-and-the-root-scheduler"></a>Controles de recurso de CPU da máquina virtual e o Agendador raiz

Os controles de recurso do processador de máquina virtual fornecidos pelo Hyper-V não têm suporte quando o Agendador raiz do hipervisor está habilitado, pois a lógica do Agendador do sistema operacional raiz está Gerenciando recursos de host em uma base global e não tem conhecimento das definições de configuração específicas de uma VM. Os controles de recurso do processador do Hyper-V por VM, como limites, pesos e reservas, são aplicáveis somente quando o hipervisor controla diretamente o agendamento de VP, como com os tipos de Agendador clássico e principal.

#### <a name="root-scheduler-use-on-server-systems"></a>Uso do Agendador raiz em sistemas de servidor

O Agendador raiz não é recomendado para uso com o Hyper-V nos servidores no momento, pois suas características de desempenho ainda não foram totalmente caracterizadas e ajustadas para acomodar a ampla gama de cargas de trabalho típicas de muitas implantações de virtualização de servidor.

## <a name="enabling-smt-in-guest-virtual-machines"></a>Habilitando SMT em máquinas virtuais convidadas

Depois que o hipervisor do host de virtualização estiver configurado para usar o tipo de Agendador principal, as máquinas virtuais convidadas poderão ser configuradas para utilizar SMT, se desejado. Expor o fato de que os VPSs são hiperthreads para uma máquina virtual convidada permite que o Agendador no sistema operacional convidado e as cargas de trabalho em execução na VM detectem e utilizem a topologia de SMT em seu próprio agendamento. No Windows Server 2016, o convidado SMT não é configurado por padrão e deve ser habilitado explicitamente pelo administrador do host Hyper-V. A partir do Windows Server 2019, novas VMs criadas no host herdam a topologia SMT do host por padrão.  Ou seja, uma VM da versão 9,0 criada em um host com 2 threads SMT por núcleo também veria dois threads SMT por núcleo.

O PowerShell deve ser usado para habilitar o SMT em uma máquina virtual convidada; Não há nenhuma interface do usuário fornecida no Gerenciador do Hyper-V.
Para habilitar o SMT em uma máquina virtual convidada, abra uma janela do PowerShell com permissões suficientes e digite:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <n>
```

Em que <n> é o número de threads SMT por núcleo que a VM convidada vê.
Observe que <n> = 0 define o valor de HwThreadCountPerCore para corresponder à contagem de threads de SMT do host por valor principal.

> [!NOTE]
> A configuração de HwThreadCountPerCore = 0 tem suporte a partir do Windows Server 2019.

Veja abaixo um exemplo de informações do sistema que são obtidas do sistema operacional convidado em execução em uma máquina virtual com 2 processadores virtuais e SMT habilitados. O sistema operacional convidado está detectando 2 processadores lógicos que pertencem ao mesmo núcleo.

![Captura de tela que mostra o MSInfo32 em uma VM convidada com SMT habilitado](media/Hyper-V-CoreScheduler-VM-Msinfo32.png)

## <a name="configuring-the-hypervisor-scheduler-type-on-windows-server-2016-hyper-v"></a>Configurando o tipo de Agendador de hipervisor no Windows Server 2016 Hyper-V

O Windows Server 2016 Hyper-V usa o modelo de Agendador de hipervisor clássico por padrão. O hipervisor pode ser configurado opcionalmente para usar o Agendador principal, para aumentar a segurança, restringindo o VPSs de convidados a ser executado em pares de SMT físicos correspondentes e para dar suporte ao uso de máquinas virtuais com agendamento SMT para seus VPSs convidados.

> [!NOTE]
> A Microsoft recomenda que todos os clientes que executam o Windows Server 2016 Hyper-V selecionem o Agendador principal para garantir que seus hosts de virtualização sejam protegidos de forma ideal contra VMs convidadas potencialmente mal-intencionadas.

## <a name="windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler"></a>O Windows Server 2019 Hyper-V usa como padrão o Agendador principal

Para ajudar a garantir que os hosts do Hyper-V sejam implantados na configuração de segurança ideal, o Hyper-V do Windows Server 2019 agora usa o modelo do Agendador do hipervisor principal por padrão. O administrador de host pode, opcionalmente, configurar o host para usar o Agendador clássico herdado. Os administradores devem ler atentamente, entender e considerar os impactos que cada tipo de Agendador tem sobre a segurança e o desempenho dos hosts de virtualização antes de substituir as configurações padrão do tipo de Agendador.  Consulte [noções básicas sobre a seleção do tipo de Agendador do Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/understanding-hyper-v-scheduler-type-selection) para obter mais informações.

### <a name="required-updates"></a>Atualizações necessárias

> [!NOTE]
> As atualizações a seguir são necessárias para usar os recursos do Agendador de hipervisor descritos neste documento. Essas atualizações incluem alterações para dar suporte à nova `hypervisorschedulertype` opção BCD, que é necessária para a configuração do host.

| Versão | Versão  | Atualização necessária | Artigo da base de dados |
|--------------------|------|---------|-------------:|
|Windows Server 2016 | 1607 | 2018, 7 C | [KB4338822](https://support.microsoft.com/help/4338822/windows-10-update-kb4338822) |
|Windows Server 2016 | 1703 | 2018, 7 C | [KB4338827](https://support.microsoft.com/help/4338827/windows-10-update-kb4338827) |
|Windows Server 2016 | 1.709 | 2018, 7 C | [KB4338817](https://support.microsoft.com/help/4338817/windows-10-update-kb4338817) |
|Windows Server 2019 | 1804 | Nenhum | Nenhum |

## <a name="selecting-the-hypervisor-scheduler-type-on-windows-server"></a>Selecionando o tipo de Agendador de hipervisor no Windows Server

A configuração do Agendador de hipervisor é controlada por meio da entrada BCD hypervisorschedulertype.

Para selecionar um tipo de Agendador, abra um prompt de comando com privilégios de administrador:

```
bcdedit /set hypervisorschedulertype type
```

Onde `type` é um de:

* Clássico
* Núcleo
* Root

O sistema deve ser reinicializado para que as alterações no tipo de Agendador de hipervisor entrem em vigor.

> [!NOTE]
> O Agendador raiz do hipervisor não tem suporte no Windows Server Hyper-V no momento. Os administradores do Hyper-V não devem tentar configurar o Agendador raiz para uso com cenários de virtualização de servidor.

## <a name="determining-the-current-scheduler-type"></a>Determinando o tipo de Agendador atual

Você pode determinar o tipo de Agendador de hipervisor atual em uso examinando o log do sistema em Visualizador de Eventos para obter a ID de evento 2 de inicialização do hipervisor mais recente, que relata o tipo de Agendador de hipervisor configurado no lançamento do hipervisor. Os eventos de inicialização do hipervisor podem ser obtidos no Windows Visualizador de Eventos ou por meio do PowerShell.

A ID do evento 2 de inicialização do hipervisor denota o tipo de Agendador do hipervisor, em que:

- 1 = Agendador clássico, SMT desabilitado

- 2 = Agendador clássico

- 3 = Agendador principal

- 4 = Agendador raiz

![Captura de tela mostrando detalhes da ID do evento 2 de inicialização do hipervisor](media/Hyper-V-CoreScheduler-EventID2-Details.png)

![Captura de tela mostrando Visualizador de Eventos exibição da ID 2 do evento de inicialização do hipervisor](media/Hyper-V-CoreScheduler-EventViewer.png)

### <a name="querying-the-hyper-v-hypervisor-scheduler-type-launch-event-using-powershell"></a>Consultando o evento de inicialização do tipo de Agendador do hipervisor Hyper-V usando o PowerShell

Para consultar a ID de evento 2 do hipervisor usando o PowerShell, insira os comandos a seguir em um prompt do PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Hypervisor"; ID=2} -MaxEvents 1
```

![Captura de tela mostrando a consulta do PowerShell e os resultados da ID do evento 2 de inicialização do hipervisor](media/Hyper-V-CoreScheduler-PowerShell.png)
