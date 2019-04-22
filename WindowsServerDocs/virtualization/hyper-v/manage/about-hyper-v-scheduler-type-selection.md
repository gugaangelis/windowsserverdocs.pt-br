---
title: Sobre a seleção de tipo de Agendador de hipervisor Hyper-V
description: Fornece informações para administradores de host do Hyper-V sobre o uso do Agendador do Hyper-V modos
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823877"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>Sobre a seleção de tipo de Agendador de hipervisor Hyper-V

Aplica-se a:

* Windows Server 2016
* Windows Server, versão 1709
* Windows Server, versão 1803
* Windows Server 2019

Este documento descreve alterações importantes para o padrão do Hyper-V e recomendado o uso de hipervisor de tipos do Agendador. Essas alterações afetar as duas desempenho de segurança e a virtualização do sistema. Os administradores de host de virtualização devem examinar e compreender as alterações e as implicações descritas neste documento e avaliar cuidadosamente os impactos, orientações de implantação sugerida e fatores de risco envolvido para melhor entender como implantar e gerenciar Hosts do Hyper-V em caso do panorama da segurança rápida mudança.

>[!IMPORTANT]
>Atualmente conhecido cego em várias arquiteturas de processador de vulnerabilidades podem ser exploradas por uma VM convidada mal-intencionado por meio do comportamento de agendamento do tipo clássico Agendador hipervisor herdado quando executado em hosts com acesso simultâneo de segurança de canal lateral Multithreading (SMT) habilitado.  Se explorasse com êxito, uma carga de trabalho mal-intencionado poderia observar dados fora do seu limite de partição. Essa classe de ataques pode ser reduzida, configurando o hipervisor Hyper-V para utilizar o tipo de Agendador de núcleo do hipervisor e VMs convidadas de reconfiguração. Com o Agendador de núcleo, o hipervisor restringe vice-presidentes de convidado da VM para ser executado no mesmo núcleo de processador físico, portanto isolando fortemente a capacidade da VM para acessar os dados para os limites do núcleo físico no qual ele é executado.  Isso é uma mitigação altamente eficaz contra esses ataques de canal lateral, o que impede que a VM de observar quaisquer artefatos de outras partições, se a raiz ou outra partição de convidado.  Portanto, a Microsoft está alterando o padrão e recomendado definições de configuração para hosts de virtualização e VMs convidadas.

## <a name="background"></a>Histórico

Começando com o Windows Server 2016, o Hyper-V dá suporte a vários métodos de agendamento e gerenciamento de processadores virtuais, chamados de tipos de Agendador do hipervisor.  Uma descrição detalhada de todos os tipos de Agendador de hipervisor pode ser encontrada no [Entendendo e usando tipos de Agendador de hipervisor Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).

>[!NOTE]
>Novos tipos de Agendador do hipervisor introduzido pela primeira vez com o Windows Server 2016 e não estão disponíveis em versões anteriores. Todas as versões do Hyper-V antes do Windows Server 2016 suportam apenas o Agendador clássico. Suporte para o Agendador core apenas recentemente foi publicado.

## <a name="about-hypervisor-scheduler-types"></a>Sobre tipos de Agendador do hipervisor

Este artigo aborda especificamente o uso do novo tipo de Agendador do hipervisor core versus o Agendador "clássico" herdado e como esses tipos do Agendador se interseccionam com o uso de Symmetric multithreading ou SMT.  É importante entender as diferenças entre os agendadores do core e o clássico e como cada coloca o trabalho de VMs convidadas nos processadores do sistema subjacente.

### <a name="the-classic-scheduler"></a>O Agendador clássico

O Agendador clássico refere-se para o método de round robin fair-compartilhamento de agendamento de trabalho em processadores virtuais (VPs) em todo o sistema - incluindo vice-presidentes raiz, bem como pelo VP que pertencem a VMs convidadas. O Agendador clássico foi o tipo de agendador padrão usado em todas as versões do Hyper-V (até 2019 Windows Server, conforme descrito neste documento).  As características de desempenho do Agendador clássico são bem compreendidas e o Agendador clássico é demonstrado para dar suporte a superassinatura de cargas de trabalho - ou seja, superassinatura taxa de VP:LP do host por uma margem razoável faz (dependendo do tipos de cargas de trabalho que está sendo virtualizadas, a utilização geral do recurso, etc.).

Quando executado em um host de virtualização com SMT habilitado, o Agendador clássico agendará vice-presidentes convidado de qualquer VM em cada thread SMT que pertencem a um núcleo de forma independente. Portanto, as VMs diferentes podem ser executados no mesmo núcleo ao mesmo tempo (em execução em um thread de um núcleo, enquanto outra VM estiver em execução no thread de uma VM).

### <a name="the-core-scheduler"></a>O Agendador de núcleo

O Agendador core aproveita as propriedades do SMT para fornecer isolamento de cargas de trabalho de convidado, o que afeta o desempenho do sistema e segurança. O Agendador core garante que os vice-presidentes de uma VM são agendadas em threads SMT irmão. Isso é feito simetricamente para que se LPs estão em grupos de dois, vice-presidentes estão agendados em grupos de dois, e um núcleo de CPU do sistema nunca é compartilhado entre as VMs.

Agendando convidado vice-presidentes em pares SMT subjacentes, o Agendador core oferece um limite de segurança forte para o isolamento de carga de trabalho e também pode ser usado para reduzir a variabilidade de desempenho para cargas de trabalho confidenciais de latência.

Observe que, quando o vice-Presidente está agendada para uma máquina virtual sem SMT habilitado, vice-Presidente consumirá todos os principais quando ele é executado e irmão do core thread SMT precisará ficar ocioso.  Isso é necessário fornecer o isolamento de carga de trabalho correto, mas afeta o desempenho geral do sistema, especialmente à medida que o sistema LPs tornam-se assinado excesso - ou seja, quando taxa VP:LP total excede 1:1. Portanto, executar as VMs convidadas configuradas sem vários threads por núcleo é uma configuração ideal.

### <a name="benefits-of-the-using-the-core-scheduler"></a>Benefícios de usar o Agendador de núcleo

O Agendador core oferece os seguintes benefícios:

* Um limite de segurança forte para o isolamento de carga de trabalho de convidado - vice-presidentes convidado são restritos a executar em pares de núcleo físico subjacente, reduzindo a vulnerabilidade a ataques de espionagem de canal lateral.

* Variabilidade de redução de carga de trabalho - variabilidade de taxa de transferência de carga de trabalho de convidado é reduzida significativamente, oferecendo maior consistência de carga de trabalho.

* Uso do SMT em convidado VMs – o sistema operacional e aplicativos em execução na máquina virtual convidada pode utilizar o comportamento SMT e programar APIs (interfaces) para controlar e distribuir o trabalho entre threads SMT, assim como quando eles seriam executado não-virtualizados.

O Agendador de núcleo é usado atualmente em hosts de virtualização do Azure, especificamente para aproveitar o limite de segurança forte e variabilty de baixa carga de trabalho. A Microsoft acredita que o tipo de Agendador core deve ser e continuará a ser o hipervisor padrão do tipo para a maioria dos cenários de virtualização de agendamento.  Portanto, para garantir que nossos clientes estão seguros por padrão, Microsoft está fazendo essa alteração para o Windows Server 2019 agora.

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>Core Agendador impactos no desempenho em cargas de trabalho de convidado

Embora seja necessário reduzir efetivamente determinadas classes de vulnerabilidades, o Agendador de núcleo também potencialmente pode reduzir o desempenho. Os clientes podem ver a diferença entre as características de desempenho com suas VMs e impactos para a capacidade de carga de trabalho geral de seus hosts de virtualização. Em casos em que o Agendador do núcleo deve executar uma não - SMT vice-Presidente, apenas um dos fluxos de instrução no núcleo lógico subjacente é executado enquanto o outro deve ficar ocioso. Isso limitará a capacidade total do host para cargas de trabalho de convidado.

Esses impactos no desempenho podem ser minimizados, seguindo as diretrizes de implantação neste documento. Administradores de hosts cuidadosamente devem considerar específicas de virtualização em cenários de implantação e balancear sua tolerância a riscos de segurança contra a necessidade de densidade de carga de trabalho máxima, o excesso de consolidação de hosts de virtualização, etc.

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Alterações no padrão e configurações recomendadas para o Windows Server 2016 e Windows Server 2019

Implantando hosts Hyper-V com a postura de segurança máxima requer o uso do tipo de Agendador de núcleo do hipervisor. Para garantir que nossos clientes estão seguros por padrão, a Microsoft está mudando o seguinte padrão e as configurações recomendadas.

>[!NOTE]
>Embora o suporte interno para os tipos de Agendador do hipervisor foi incluído na versão inicial do Windows Server 2016, Windows Server 1709 e Windows Server 1803, atualizações são necessárias para acessar o controle de configuração que permite selecionar o tipo de Agendador do hipervisor.  Consulte a [Entendendo e usando tipos de Agendador de hipervisor Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) para obter detalhes sobre essas atualizações.

### <a name="virtualization-host-changes"></a>Alterações de host de virtualização

* O hipervisor usará o Agendador core por padrão desde o com o Windows Server 2019.

* Microsoft reccommends de configurar o Agendador core no Windows Server 2016. O tipo de Agendador de núcleo de hipervisor é suportado no Windows Server 2016, no entanto, o padrão é o Agendador clássico. O Agendador de núcleo é opcional e deve ser habilitado explicitamente pelo administrador do host do Hyper-V.

### <a name="virtual-machine-configuration-changes"></a>Alterações de configuração de máquina virtual

* No Windows Server de 2019, o novas máquinas virtuais criadas usando a versão da VM padrão 9.0 herdará automaticamente as propriedades SMT (habilitado ou desabilitado) do host de virtualização. Ou seja, se SMT estiver habilitado no host físico, VMs criadas recentemente também terá SMT habilitado e herdará a topologia SMT do host por padrão, com a VM ter o mesmo número de threads de hardware por núcleo, como o sistema subjacente. Isso será refletido na configuração da VM com HwThreadCountPerCore = 0, onde 0 indica que a VM deve herdar configurações de SMT do host.

* Máquinas virtuais existentes com uma versão VM do 8.2 ou anterior será reter sua configuração original de processador VM para HwThreadCountPerCore, e o padrão para convidados de versão 8.2 VM é HwThreadCountPerCore = 1. Quando esses convidados executados em um host do Windows Server 2019, eles serão tratados da seguinte maneira:

    1. Se a VM tem uma contagem de vice-Presidente é menor que ou igual à contagem de núcleos de LP, a VM será tratada o como um não - SMT VM pelo Agendador de núcleo. Quando o vice-Presidente de convidado é executado em um único thread SMT, irmão do core thread SMT será ociosos. Isso é não ideais e resultará em perda geral do desempenho.

    2. Se a VM tiver mais vice-presidentes de núcleos de LP, a VM será tratada como uma VM SMT pelo Agendador de núcleo. No entanto, a VM não obedecerão outras indicações de que se trata de uma VM SMT. Por exemplo, o uso da instrução CPUID ou as APIs do Windows para consultar a topologia da CPU pelo sistema operacional ou aplicativos não indicará que o SMT está habilitado.

* Quando uma VM existente é explicitamente atualizada de versões VM mais recente para a versão 9.0 por meio da operação de atualização-VM, VM reterá seu valor atual para HwThreadCountPerCore.  A VM não terá SMT force habilitado.

* No Windows Server 2016, a Microsoft recomenda a habilitação SMT para VMs convidadas.  Por padrão, as VMs criadas no Windows Server 2016 seria ter SMT desabilitado, que é HwThreadCountPerCore é definido como 1, a menos que explicitamente alterado.

>[!NOTE]
>Windows Server 2016 não oferece suporte à configuração HwThreadCountPerCore como 0.

#### <a name="managing-virtual-machine-smt-configuration"></a>Gerenciamento de configuração da máquina virtual SMT

Configuração de SMT da máquina virtual convidada é definida em uma base por VM. O administrador do host pode inspecionar e definir a configuração da VM SMT para selecionar entre as seguintes opções:

    1. Configurar VMs para executar como SMT habilitado para, opcionalmente, herdando a topologia SMT host automaticamente

    2. Configurar VMs para executar como não-SMT

O SMT lt;{0}&gt para uma VM é exibido nos painéis de resumo no console do Gerenciador do Hyper-V.  Definindo configurações de SMT da VM pode ser feito usando as configurações de VM ou o PowerShell.

#### <a name="configuring-vm-smt-settings-using-powershell"></a>Definindo configurações de VM SMT usando o PowerShell

Para configurar as configurações de SMT para uma máquina virtual convidada, abra uma janela do PowerShell com permissões suficientes e o tipo:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Onde:

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Para ler as configurações de SMT para uma máquina virtual convidada, abra uma janela do PowerShell com permissões suficientes e o tipo:

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Observe que máquinas virtuais configuradas com HwThreadCountPerCore convidado = 0 indica que a SMT será habilitado para o convidado e irá expor o mesmo número de threads SMT para o convidado enquanto eles estiverem em subjacente host de virtualização, normalmente 2.

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>VMs convidadas pode observar as alterações a topologia de CPU em cenários de mobilidade VM

O sistema operacional e aplicativos em uma VM podem ver as alterações para o host e as configurações de VM antes e depois que os eventos de ciclo de vida da VM, como migração ao vivo ou salvar e restaurar operações. Durante uma operação na máquina virtual, o estado é salvo e restaurado, a configuração de HwThreadCountPerCore da VM e o valor realizado (ou seja, a combinação computada de configuração da VM e configuração do host de origem) são migradas. A máquina virtual continuará em execução com essas configurações no host de destino. No momento, a VM for desligada e iniciado novamente, é possível que o valor realizado observado pela VM será alterado. Isso deve ser benigno, como sistema operacional e aplicativo de software de camada deve procurar informações sobre a topologia da CPU como parte de seus fluxos de código de inicialização e a inicialização normais. No entanto, porque esses inicialização do tempo de inicialização sequências são ignoradas durante as operações em tempo real de migração ou salvar/restaurar, as VMs que passam por essas transições de estado poderia observar originalmente computada valor obtido até que eles são desligados e iniciado novamente.  

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>Alertas sobre configurações de VM não ideais

Máquinas de virtuais configuradas com vice-presidentes mais que o número de núcleos físicos no resultado de host em uma configuração não ideal. O Agendador de hipervisor tratará essas VMs como se eles têm reconhecimento de SMT. No entanto, sistema operacional e software de aplicativo na VM será apresentada uma topologia de CPU mostrando o SMT está desabilitado. Quando essa condição for detectada, o processo de trabalho do Hyper-V registrará um evento no host de virtualização, o administrador do host de aviso de que a configuração da VM é não ideais e recomendar SMT ser habilitada para a VM.

#### <a name="how-to-identify-non-optimally-configured-vms"></a>Como identificar de forma não otimizada configurou as VMs

Você pode identificar - SMT VMs não examinando o Log do sistema no Visualizador de eventos para o evento de processo de trabalho do Hyper-V 3498 ID, que será disparado para uma máquina virtual sempre que o número de vice-presidentes na VM é maior que a contagem de núcleos físicos. Eventos de processo de trabalho podem ser obtidos no Visualizador de eventos, ou por meio do PowerShell.

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>Consultando o evento de VM do Hyper-V trabalho processo usando o PowerShell

A consulta para o evento 3498 ID do processo de trabalho do Hyper-V usando o PowerShell, digite os seguintes comandos em um prompt do PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>Impactos do convidado SMT lt;{0}&gt sobre o uso de hipervisor esclarecimentos para sistemas operacionais convidados

O hipervisor da Microsoft oferece vários esclarecimentos ou dicas, que o sistema operacional em execução em uma VM convidada podem ser consultadas e usar para disparar as otimizações, como aqueles que podem se beneficiar o desempenho ou caso contrário, melhorar a manipulação de várias condições durante a execução virtualizados. Iluminismo introduzido recentemente um está relacionado a manipulação de agendamento do processador virtual e o uso de atenuações de sistema operacional para ataques de canal lateral que exploram SMT.

>[!NOTE]
>A Microsoft recomenda que os administradores de host habilite SMT para VMs convidadas otimizar o desempenho da carga de trabalho.

Os detalhes desse iluminismo convidado são fornecidos abaixo, no entanto a consideração importante para os administradores de host de virtualização é que as máquinas virtuais devem ter HwThreadCountPerCore configurado de acordo com a configuração do host física SMT. Isso permite que o hipervisor relatar que não há nenhum núcleo não-arquitetura de compartilhamento. Portanto, qualquer otimizações de suporte do sistema operacional convidado que exigem o iluminismo podem ser ativadas. No Windows Server de 2019, criar novas VMs e deixe o valor padrão de HwThreadCountPerCore (0). Migração de VMs mais antigas do Windows Server 2016 hosts podem ser atualizados para a versão de configuração do Windows Server 2019. Depois de fazer isso, a Microsoft recomenda definir HwThreadCountPerCore = 0.  No Windows Server 2016, a Microsoft recomenda definir HwThreadCountPerCore para coincidir com a configuração do host (normalmente 2).

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>Detalhes do iluminismo NoNonArchitecturalCoreSharing

A partir do Windows Server 2016, o hipervisor define um novo iluminismo para descrever o manuseio de vice-Presidente de agendamento e o posicionamento para o sistema operacional convidado. Esse iluminismo é definido na [v5.0c de especificação funcional de nível de parte superior do hipervisor](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

Folha CPUID sintética hipervisor CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] indica que um processador virtual nunca compartilham um núcleo físico com outro processador virtual, com exceção de processadores virtuais que são relatados como irmão SMT threads. Por exemplo, uma convidado VP nunca será executada em um thread SMT junto com uma VP de raiz em execução simultaneamente em um irmão de thread SMT no mesmo núcleo de processador. Esta condição só é possível quando em execução virtualizada e, portanto, representa um comportamento SMT não arquitetônicas que também tem graves implicações de segurança. O sistema operacional convidado pode usar NoNonArchitecturalCoreSharing = 1, como uma indicação de que é seguro habilitar otimizações, o que podem ajudá-lo a evitar a sobrecarga de desempenho de configuração STIBP.

Em determinadas configurações, o hipervisor não indicará que NoNonArchitecturalCoreSharing = 1. Por exemplo, se um host tem SMT habilitado e configurado para usar o Agendador clássico de hipervisor, NoNonArchitecturalCoreSharing será 0. Isso pode impedir que os convidados habilitado para habilitar determinadas otimizações. Portanto, a Microsoft recomenda que os administradores de host usando o SMT contam com o Agendador de núcleo do hipervisor e certifique-se de que as máquinas virtuais forem configuradas para herdar a configuração SMT do host para garantir um desempenho ideal de carga de trabalho.

## <a name="summary"></a>Resumo

O panorama de ameaças de segurança continua a evoluir. Para garantir que nossos clientes estão seguros por padrão, a Microsoft está mudando a configuração padrão para o hipervisor e máquinas virtuais começando no Windows Server 2019 Hyper-V e fornecendo atualizado diretrizes e recomendações para clientes que executam o Windows Server 2016 Hyper-V. Os administradores de host de virtualização devem:

* Ler e entender as diretrizes fornecidas neste documento

* Avalie cuidadosamente e ajustar suas implantações de virtualização para garantir que ele cumpra a segurança, desempenho, densidade de virtualização e metas de capacidade de resposta da carga de trabalho para seus requisitos exclusivos

* Considere novamente Configurando hosts Windows Server 2016 Hyper-V existentes para aproveitar os benefícios de alta segurança oferecidos pelo Agendador de núcleo do hipervisor

* Não - SMT VMs existentes para reduzir os impactos de desempenho de restrições impostas pelo isolamento VP que trata de vulnerabilidades de segurança de hardware de agendamento de atualização
