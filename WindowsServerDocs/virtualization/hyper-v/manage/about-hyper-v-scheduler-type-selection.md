---
title: Sobre a seleção do tipo de Agendador de hipervisor do Hyper-V
description: Fornece informações para os administradores de host do Hyper-V no uso dos modos de Agendador do Hyper-V
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: 1e77535548cccd1c821163dabbad381f35d2948a
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70872060"
---
# <a name="about-hyper-v-hypervisor-scheduler-type-selection"></a>Sobre a seleção do tipo de Agendador de hipervisor do Hyper-V

Aplica-se a:

* Windows Server 2016
* Windows Server, versão 1709
* Windows Server, versão 1803
* Windows Server 2019

Este documento descreve alterações importantes no padrão do Hyper-V e no uso recomendado de tipos de Agendador de hipervisor. Essas alterações afetam a segurança do sistema e o desempenho da virtualização. Os administradores de host de virtualização devem revisar e entender as alterações e as implicações descritas neste documento e avaliar cuidadosamente os impactos, orientações de implantação sugerida e fatores de risco envolvidos para entender melhor como implantar e gerenciar Hosts Hyper-V diante do panorama de segurança de alteração rápida.

>[!IMPORTANT]
>As vulnerabilidades de segurança de canal lateral conhecidas no momento em várias arquiteturas de processador podem ser exploradas por uma VM convidada mal-intencionada por meio do comportamento de agendamento do tipo de Agendador clássico do hipervisor herdado quando executado em hosts com simultaneamente Multithreading (SMT) habilitado.  Se explorada com êxito, uma carga de trabalho mal-intencionada pode observar dados fora de seu limite de partição. Essa classe de ataques pode ser atenuada Configurando o hipervisor do Hyper-V para utilizar o tipo de Agendador principal do hipervisor e reconfigurar as VMs convidadas. Com o Agendador principal, o hipervisor restringe o VPSs de uma VM convidada a ser executado no mesmo núcleo de processador físico, o que isola fortemente a capacidade da VM de acessar dados para os limites do núcleo físico em que ele é executado.  Essa é uma mitigação altamente eficaz contra esses ataques de canal lateral, o que impede que a VM Observe quaisquer artefatos de outras partições, seja a raiz ou outra partição de convidado.  Portanto, a Microsoft está alterando as definições de configuração padrão e recomendadas para hosts de virtualização e VMs convidadas.

## <a name="background"></a>Informações preliminares

A partir do Windows Server 2016, o Hyper-V dá suporte a vários métodos de agendamento e gerenciamento de processadores virtuais, chamados de tipos de Agendador de hipervisor.  Uma descrição detalhada de todos os tipos de Agendador de hipervisor pode ser encontrada em [compreendendo e usando os tipos de Agendador de hipervisor do Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).

>[!NOTE]
>Novos tipos de Agendador de hipervisor foram introduzidos primeiro com o Windows Server 2016 e não estão disponíveis em versões anteriores. Todas as versões do Hyper-V anteriores ao Windows Server 2016 dão suporte apenas ao agendador clássico. O suporte para o Agendador principal foi publicado apenas recentemente.

## <a name="about-hypervisor-scheduler-types"></a>Sobre os tipos de Agendador de hipervisor

Este artigo se concentra especificamente no uso do novo tipo de Agendador de núcleo do hipervisor em comparação com o Agendador "clássico" herdado e como esses tipos de Agendador interseccionam com o uso de multithreading simétrico ou SMT.  É importante entender as diferenças entre os agendadores básico e clássico e como cada lugar funciona de VMs convidadas nos processadores de sistema subjacentes.

### <a name="the-classic-scheduler"></a>O Agendador clássico

O Agendador clássico refere-se ao fair-share, round robin método de agendamento de trabalho em processadores virtuais (VPSs) no sistema, incluindo raiz VPSs, bem como VPSs que pertencem a VMs convidadas. O Agendador clássico tem sido o tipo de agendador padrão usado em todas as versões do Hyper-V (até o Windows Server 2019, conforme descrito aqui).  As características de desempenho do Agendador clássico são bem compreendidas, e o Agendador clássico é demonstrado para dar suporte ably a excesso de cargas de trabalho, ou seja, a excesso de assinatura da proporção de VP: LP do host por uma margem razoável (dependendo do tipos de cargas de trabalho virtualizadas, utilização geral de recursos, etc.).

Quando executado em um host de virtualização com SMT habilitado, o Agendador clássico agendará o convidado VPSs de qualquer VM em cada thread do SMT que pertença a um núcleo de forma independente. Portanto, VMs diferentes podem ser executadas no mesmo núcleo ao mesmo tempo (uma VM em execução em um thread de um núcleo enquanto outra VM está em execução no outro thread).

### <a name="the-core-scheduler"></a>O Agendador principal

O Agendador principal aproveita as propriedades de SMT para fornecer isolamento de cargas de trabalho de convidado, o que afeta o desempenho da segurança e do sistema. O Agendador principal garante que o VPSs de uma VM seja agendado em threads SMT irmãos. Isso é feito de forma simétrica para que, se LPs estiverem em grupos de dois, VPSs sejam agendados em grupos de dois, e um núcleo de CPU do sistema nunca seja compartilhado entre VMs.

Ao agendar VPSs de convidado em pares SMT subjacentes, o Agendador principal oferece um forte limite de segurança para isolamento de carga de trabalho e também pode ser usado para reduzir a variabilidade de desempenho para cargas de trabalho sensíveis à latência.

Observe que, quando o VP está agendado para uma máquina virtual sem SMT habilitado, esse VP consumirá todo o núcleo quando for executado, e o thread de SMT irmão do núcleo será deixado ocioso.  Isso é necessário para fornecer o isolamento de carga de trabalho correto, mas afeta o desempenho geral do sistema, especialmente à medida que o sistema LPs se torna excessivamente assinado – ou seja, quando a taxa VP total: LP excede 1:1. Portanto, a execução de VMs convidadas configuradas sem vários threads por núcleo é uma configuração abaixo do ideal.

### <a name="benefits-of-the-using-the-core-scheduler"></a>Benefícios do uso do Agendador principal

O Agendador principal oferece os seguintes benefícios:

* Um limite de segurança forte para isolamento de carga de trabalho de convidado – os VPSs convidados são restritos a serem executados em pares de núcleos físicos subjacentes, reduzindo a vulnerabilidade a ataques de espionagem de canal lateral.

* Redução da carga de trabalho-a taxa de transferência de carga de trabalho de convidado é reduzida significativamente, oferecendo maior consistência de carga de trabalho

* Uso de SMT em VMs convidadas – o sistema operacional e os aplicativos em execução na máquina virtual convidada podem utilizar o comportamento e as APIs (interfaces de programação) do SMT para controlar e distribuir trabalhos entre threads de SMT, assim como fariam quando são executados não virtualizados.

O Agendador principal é usado atualmente em hosts de virtualização do Azure, especificamente para aproveitar o forte limite de segurança e baixa carga de trabalho variabilty. A Microsoft acredita que o tipo de Agendador principal deve ser e continuará a ser o tipo de agendamento de hipervisor padrão para a maioria dos cenários de virtualização.  Portanto, para garantir que nossos clientes sejam seguros por padrão, a Microsoft está fazendo essa alteração para o Windows Server 2019 agora.

### <a name="core-scheduler-performance-impacts-on-guest-workloads"></a>Principais impactos no desempenho do Agendador em cargas de trabalho convidadas

Embora seja necessário reduzir efetivamente determinadas classes de vulnerabilidades, o Agendador central também pode reduzir o desempenho. Os clientes podem ver uma diferença nas características de desempenho com suas VMs e impactos na capacidade geral da carga de trabalho de seus hosts de virtualização. Nos casos em que o Agendador principal deve executar um VP não SMT, apenas um dos fluxos de instrução no núcleo lógico subjacente é executado enquanto o outro deve ficar ocioso. Isso limitará a capacidade total do host para cargas de trabalho convidadas.

Esses impactos no desempenho podem ser minimizados seguindo as diretrizes de implantação deste documento. Os administradores de host devem considerar cuidadosamente seus cenários de implantação de virtualização específicos e equilibrar sua tolerância para o risco de segurança contra a necessidade de densidade máxima de carga de trabalho, consolidação excessiva de hosts de virtualização, etc.

## <a name="changes-to-the-default-and-recommended-configurations-for-windows-server-2016-and-windows-server-2019"></a>Alterações nas configurações padrão e recomendadas para o Windows Server 2016 e o Windows Server 2019

A implantação de hosts Hyper-V com a postura de segurança máxima requer o uso do tipo de Agendador principal do hipervisor. Para garantir que nossos clientes sejam protegidos por padrão, a Microsoft está alterando as configurações padrão e recomendadas a seguir.

>[!NOTE]
>Embora o suporte interno do hipervisor para os tipos de Agendador tenha sido incluído na versão inicial do Windows Server 2016, do Windows Server 1709 e do Windows Server 1803, as atualizações são necessárias para acessar o controle de configuração que permite selecionar o tipo de Agendador de hipervisor.  Consulte [compreendendo e usando os tipos de Agendador de hipervisor do Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) para obter detalhes sobre essas atualizações.

### <a name="virtualization-host-changes"></a>Alterações do host de virtualização

* O hipervisor usará o Agendador principal por padrão, começando com o Windows Server 2019.

* Microsoft reccommends Configurando o Agendador principal no Windows Server 2016. O tipo de Agendador principal do hipervisor tem suporte no Windows Server 2016, mas o padrão é o Agendador clássico. O Agendador principal é opcional e deve ser explicitamente habilitado pelo administrador do host Hyper-V.

### <a name="virtual-machine-configuration-changes"></a>Alterações de configuração de máquina virtual

* No Windows Server 2019, novas máquinas virtuais criadas usando a versão de VM padrão 9,0 herdarão automaticamente as propriedades SMT (habilitadas ou desabilitadas) do host de virtualização. Ou seja, se SMT estiver habilitado no host físico, as VMs recém-criadas também terão SMT habilitado e herdarão a topologia SMT do host por padrão, com a VM com o mesmo número de threads de hardware por núcleo do sistema subjacente. Isso será refletido na configuração da VM com HwThreadCountPerCore = 0, em que 0 indica que a VM deve herdar as configurações de SMT do host.

* As máquinas virtuais existentes com uma versão de VM de 8,2 ou anterior manterão sua configuração de processador de VM original para HwThreadCountPerCore, e o padrão para 8,2 VM da versão convidada será HwThreadCountPerCore = 1. Quando esses convidados são executados em um host do Windows Server 2019, eles serão tratados da seguinte maneira:

    1. Se a VM tiver uma contagem de VP menor ou igual à contagem de núcleos de LP, a VM será tratada como uma VM não SMT pelo Agendador principal. Quando o VP convidado é executado em um único thread SMT, o thread SMT irmão do núcleo estará ocioso. Isso não é o ideal e resultará em perda geral de desempenho.

    2. Se a VM tiver mais VPSs do que os núcleos de LP, a VM será tratada como uma VM SMT pelo Agendador principal. No entanto, a VM não observará outras indicações de que ela é uma VM SMT. Por exemplo, o uso da instrução CPUID ou das APIs do Windows para consultar topologia de CPU pelo sistema operacional ou aplicativos não indicará que SMT está habilitado.

* Quando uma VM existente é explicitamente atualizada de versões de VM preenchiam para a versão 9,0 por meio da operação UPDATE-VM, a VM reterá seu valor atual para HwThreadCountPerCore.  A VM não terá o SMT forçado habilitado.

* No Windows Server 2016, a Microsoft recomenda habilitar o SMT para VMs convidadas.  Por padrão, as VMs criadas no Windows Server 2016 teriam SMT desabilitado, ou seja, HwThreadCountPerCore é definido como 1, a menos que seja explicitamente alterado.

>[!NOTE]
>O Windows Server 2016 não oferece suporte à definição de HwThreadCountPerCore como 0.

#### <a name="managing-virtual-machine-smt-configuration"></a>Gerenciando a configuração de SMT da máquina virtual

A configuração de SMT da máquina virtual convidada é definida por VM. O administrador de host pode inspecionar e configurar a configuração de SMT de uma VM para selecionar uma das seguintes opções:

    1. Configurar VMs para execução como SMT, opcionalmente herdando a topologia SMT do host automaticamente

    2. Configurar VMs para execução como não SMT

O SMT configuração para uma VM é exibido nos painéis de resumo no console do Gerenciador do Hyper-V.  Definir as configurações de SMT de uma VM pode ser feito usando as configurações de VM ou o PowerShell.

#### <a name="configuring-vm-smt-settings-using-powershell"></a>Definindo configurações de SMT de VM usando o PowerShell

Para definir as configurações de SMT para uma máquina virtual convidada, abra uma janela do PowerShell com permissões suficientes e digite:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Sendo que:

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Para ler as configurações de SMT para uma máquina virtual convidada, abra uma janela do PowerShell com permissões suficientes e digite:

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Observe que as VMs convidadas configuradas com HwThreadCountPerCore = 0 indicam que o SMT será habilitado para o convidado e exporá o mesmo número de threads de SMT para o convidado como estão no host de virtualização subjacente, normalmente 2.

### <a name="guest-vms-may-observe-changes-to-cpu-topology-across-vm-mobility-scenarios"></a>As VMs convidadas podem observar alterações na topologia de CPU entre cenários de mobilidade de VM

O sistema operacional e os aplicativos em uma VM podem ver alterações nas configurações de host e VM antes e depois dos eventos de ciclo de vida da VM, como migração dinâmica ou operações de salvar e restaurar. Durante uma operação na qual o estado da VM é salvo e restaurado, a configuração de HwThreadCountPerCore da VM e o valor realizado (ou seja, a combinação computada da configuração da VM e do host de origem) são migradas. A VM continuará em execução com essas configurações no host de destino. No ponto em que a VM é desligada e reiniciada, é possível que o valor realizado observado pela VM seja alterado. Isso deve ser benigno, pois o software da camada do sistema operacional e do aplicativo deve procurar informações de topologia de CPU como parte dos fluxos normais de inicialização e de código de inicialização. No entanto, como essas sequências de inicialização de tempo de inicialização são ignoradas durante a migração ao vivo ou operações de salvar/restaurar, as VMs que passam por essas transições de estado podem observar o valor realizado originalmente computado até que eles sejam desligados e reiniciados.  

### <a name="alerts-regarding-non-optimal-vm-configurations"></a>Alertas sobre configurações de VM não ideais

Máquinas virtuais configuradas com mais VPSs do que os núcleos físicos no host resultam em uma configuração não ideal. O Agendador de hipervisor tratará essas VMs como se elas reconhecem SMT. No entanto, o sistema operacional e o software de aplicativo na VM serão apresentados uma topologia de CPU mostrando que SMT está desabilitada. Quando essa condição for detectada, o processo de trabalho do Hyper-V registrará um evento no host de virtualização aviso o administrador do host de que a configuração da VM não é a ideal e recomendamos que o SMT seja habilitado para a VM.

#### <a name="how-to-identify-non-optimally-configured-vms"></a>Como identificar VMs configuradas de forma não ideal

Você pode identificar VMs não SMT examinando o log do sistema em Visualizador de Eventos para a ID de evento 3498 do processo de trabalho do Hyper-V, que será disparada para uma VM sempre que o número de VPSs na VM for maior que a contagem de núcleos físicos. Eventos de processo de trabalho podem ser obtidos de Visualizador de Eventos ou por meio do PowerShell.

#### <a name="querying-the-hyper-v-worker-process-vm-event-using-powershell"></a>Consultando o evento de VM do processo de trabalho do Hyper-V usando o PowerShell

Para consultar a ID de evento 3498 do processo de trabalho do Hyper-V usando o PowerShell, insira os seguintes comandos em um prompt do PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### <a name="impacts-of-guest-smt-configuaration-on-the-use-of-hypervisor-enlightenments-for-guest-operating-systems"></a>Impactos do convidado SMT configuração no uso de esclarecimentos do hipervisor para sistemas operacionais convidados

O hipervisor da Microsoft oferece vários esclarecimentos, ou dicas, que o sistema operacional em execução em uma VM convidada pode consultar e usar para disparar otimizações, como aquelas que podem beneficiar o desempenho ou melhorar o tratamento de várias condições durante a execução virtualizado. Uma apresentação introduzida recentemente aborda a manipulação do agendamento do processador virtual e o uso de atenuações do sistema operacional para ataques de canal lateral que exploram o SMT.

>[!NOTE]
>A Microsoft recomenda que os administradores de host habilitem o SMT para VMs convidadas para otimizar o desempenho da carga de trabalho.

Os detalhes dessa contratação de convidado são fornecidos abaixo. no entanto, o principal argumento para administradores de host de virtualização é que as máquinas virtuais devem ter HwThreadCountPerCore configurado para corresponder à configuração de SMT física do host. Isso permite que o hipervisor relate que não há nenhum compartilhamento principal não-arquitetônico. Portanto, qualquer sistema operacional convidado com suporte a otimizações que exigem o esclarecimento pode estar habilitado. No Windows Server 2019, crie novas VMs e deixe o valor padrão de HwThreadCountPerCore (0). VMs mais antigas migradas de hosts do Windows Server 2016 podem ser atualizadas para a versão de configuração do Windows Server 2019. Depois de fazer isso, a Microsoft recomenda a configuração de HwThreadCountPerCore = 0.  No Windows Server 2016, a Microsoft recomenda definir HwThreadCountPerCore para corresponder à configuração do host (normalmente 2).

### <a name="nononarchitecturalcoresharing-enlightenment-details"></a>Detalhes da NoNonArchitecturalCoreSharing de esclarecimento

A partir do Windows Server 2016, o hipervisor define um novo esclarecimento para descrever seu manuseio de planejamento e posicionamento de VP para o sistema operacional convidado. Esse esclarecimento é definido na [especificação funcional do nível superior do hipervisor v 5.0 c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

CPUID sintética do hipervisor CPUID. 0x40000004. EAX: 18 [NoNonArchitecturalCoreSharing = 1] indica que um processador virtual nunca compartilhará um núcleo físico com outro processador virtual, exceto os processadores virtuais que são relatados como irmãos SMT threads. Por exemplo, um vice-presidente convidado nunca será executado em um thread SMT junto com um VP raiz em execução simultânea em um thread SMT irmão no mesmo núcleo do processador. Essa condição só é possível durante a execução virtualizada e, portanto, representa um comportamento de SMT sem arquitetura que também tem implicações de segurança sérias. O sistema operacional convidado pode usar NoNonArchitecturalCoreSharing = 1 como uma indicação de que é seguro habilitar otimizações, o que pode ajudar a evitar a sobrecarga de desempenho da configuração de STIBP.

Em determinadas configurações, o hipervisor não indicará que NoNonArchitecturalCoreSharing = 1. Por exemplo, se um host tiver o SMT habilitado e estiver configurado para usar o Agendador clássico do hipervisor, NoNonArchitecturalCoreSharing será 0. Isso pode impedir que convidados esclarecidos habilitem determinadas otimizações. Portanto, a Microsoft recomenda que os administradores de host usando o SMT dependam do Agendador de núcleos do hipervisor e verifique se as máquinas virtuais estão configuradas para herdar a configuração do SMT do host para garantir o desempenho ideal da carga de trabalho.

## <a name="summary"></a>Resumo

O panorama de ameaças à segurança continua a evoluir. Para garantir que nossos clientes sejam protegidos por padrão, a Microsoft está alterando a configuração padrão para o hipervisor e as máquinas virtuais a partir do Windows Server 2019 Hyper-V e fornecendo diretrizes e recomendações atualizadas para clientes que executam o Windows Server 2016 Hyper-V. Os administradores de host de virtualização devem:

* Leia e entenda as diretrizes fornecidas neste documento

* Avalie e ajuste cuidadosamente suas implantações de virtualização para garantir que atendam às metas de segurança, desempenho, densidade de virtualização e capacidade de resposta de carga de trabalho para seus requisitos exclusivos

* Considere reconfigurar os hosts do Windows Server 2016 Hyper-V existentes para aproveitar os benefícios de segurança fortes oferecidos pelo Agendador principal do hipervisor

* Atualize VMs não SMT existentes para reduzir os impactos de desempenho das restrições de agendamento impostas pelo VP de isolamento que aborda vulnerabilidades de segurança de hardware
