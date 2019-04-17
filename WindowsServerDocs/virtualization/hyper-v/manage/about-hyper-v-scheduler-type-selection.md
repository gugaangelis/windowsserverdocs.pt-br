---
title: Sobre seleção de tipo de Agendador de hipervisor do Hyper-V
description: Fornece informações para os administradores de host do Hyper-V no uso do Agendador do Hyper-V modos
author: allenma
ms.author: allenma
ms.date: 08/14/2018
ms.topic: article
ms.prod: windows-server-hyper-v
ms.technology: virtualization
ms.localizationpriority: low
ms.assetid: 5fe163d4-2595-43b0-ba2f-7fad6e4ae069
ms.openlocfilehash: c5360d8e2fdc23f8b05c6be0f665407eebedeba2
ms.sourcegitcommit: 546229d6b5fa7e16f725c6c35f4dcc272711b811
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/18/2018
ms.locfileid: "4905123"
---
# Sobre seleção de tipo de Agendador de hipervisor do Hyper-V

Aplicável a:

* Windows Server 2016
* Windows Server, versão 1709
* Windows Server, versão 1803
* Windows Server 2019

Este documento descreve alterações importantes para o padrão do Hyper-V e tipos de Agendador de uso do hipervisor recomendado. Essas alterações afetam os dois desempenho de segurança e a virtualização do sistema. Os administradores de host de virtualização devem revisar e entender as alterações e as implicações descritas neste documento e avalie cuidadosamente o impactos, orientações sobre implantação sugeridos e fatores de riscos envolvidos entender melhor como implantar e gerenciar Hosts Hyper-V no caso do cenário de segurança mutação.

>[!IMPORTANT]
>Atualmente conhecido vulnerabilidades cego em várias arquiteturas de processador podem ser exploradas por um convidado VM mal-intencionado por meio do comportamento de agendamento do tipo Agendador clássico hipervisor herdados quando executado em hosts com acesso simultâneo de segurança de canal Multithreading (SMT) habilitado.  Se explorado com êxito, uma carga de trabalho mal-intencionado poderia observar dados fora de seu limite de partição. Essa classe de ataques pode ser atenuado, configurando o hipervisor Hyper-V para utilizar o tipo de Agendador do hipervisor core e o convidado reconfigurar VMs. Com o Agendador de núcleo, o hipervisor restringe VPs da VM um convidado para ser executado no mesmo núcleo do processador físico, portanto, isolando altamente capacidade da VM para acessar os dados para os limites do núcleo físico no qual ele é executado.  Isso é uma mitigação altamente eficaz contra esses ataques de canal, que impede que a VM observar quaisquer artefatos de outras partições, se a raiz ou outra partição convidada.  Portanto, a Microsoft está mudando o padrão e configuração configurações recomendadas para hosts de virtualização e VMs convidadas.

## Tela de fundo

Começando com o Windows Server 2016, o Hyper-V oferece suporte a vários métodos de agendamento e gerenciamento de processadores virtuais, conhecidos como tipos de Agendador do hipervisor.  Uma descrição detalhada de todos os tipos de Agendador de hipervisor pode ser encontrada na [compreensão e usando tipos de Agendador de hipervisor do Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).

>[!NOTE]
>Novos tipos de Agendador de hipervisor introduzidos com o Windows Server 2016 e não estão disponíveis nas versões anteriores. Todas as versões do Hyper-V antes do Windows Server 2016 suportam somente o Agendador clássico. Suporte para o Agendador de núcleo apenas recentemente foi publicado.

## Sobre tipos de Agendador de hipervisor

Este artigo concentra-se especificamente sobre o uso do novo hipervisor core Agendador tipo de versus o Agendador "clássico" herdado e como esses tipos de Agendador interceptam o uso de vários segmento simétrico ou SMT.  É importante entender as diferenças entre os programadores core e clássico e como cada coloca o trabalho de VMs convidadas nos processadores do sistema subjacente.

### O Agendador clássico

O Agendador clássico se refere ao método por fração justa, rodízio de agendamento de trabalho em processadores virtuais (VPs) em todo o sistema - incluindo VPs raiz, bem como VPs pertencentes a VMs convidadas. O Agendador clássico tem sido o tipo de agendador padrão usado em todas as versões do Hyper-V (até o Windows Server 2019, conforme descrito aqui).  As características de desempenho do Agendador clássico sejam bem entendidas e o Agendador clássico é demonstrado para dar suporte a sobreposição de cargas de trabalho - ou seja, sobreposição de proporção de VP:LP do host por uma margem razoável ably (dependendo do tipos de cargas de trabalho que está sendo virtualizadas, geral utilização de recursos, etc.).

Quando executado em um host de virtualização com SMT habilitado, o Agendador clássico programará VPs convidado de qualquer VM em cada thread SMT pertencentes a um núcleo de forma independente. Portanto, VMs diferentes podem estar executando no mesmo núcleo ao mesmo tempo (uma VM em execução em um thread de um núcleo enquanto outra VM está em execução em outro thread).

### O Agendador de core

O Agendador de núcleo aproveita as propriedades de SMT para fornecer isolamento de cargas de trabalho de convidado, que afeta a segurança e desempenho do sistema. O Agendador de núcleo garante que VPs de uma VM sejam agendados em threads SMT irmão. Isso é feito simétrica para que se LPs estão em grupos de dois, VPs sejam agendados em grupos de dois, e um núcleo de CPU do sistema nunca é compartilhado entre as VMs.

Ao agendar VPs de convidado em pares SMT subjacentes, o Agendador de core oferece um limite de segurança forte para o isolamento da carga de trabalho e também pode ser usado para reduzir a flexibilidade de desempenho para cargas de trabalho confidenciais latência.

Observe que, quando o VICE-PRESIDENTE está agendada para uma máquina virtual sem SMT habilitado, vice-Presidente consumirá todos os principais quando ele é executado e irmão do núcleo threads SMT fica ocioso.  Isso é necessário fornecer o isolamento da carga de trabalho correto, mas impactos no desempenho geral do sistema, especialmente à medida que o sistema LPs ficam assinados em excesso - ou seja, quando VP:LP total proporção excede 1:1. Portanto, executar VMs convidadas configuradas sem vários threads por core é uma configuração ideal.

### Benefícios do usando o Agendador de core

O Agendador de core oferece os seguintes benefícios:

* Um limite de segurança forte para o isolamento de carga de trabalho de convidado - convidado VPs são restringidos seja executado em pares de núcleo físico subjacente, reduzindo a vulnerabilidade a ataques de espionagem de canal.

* Variação de carga de trabalho reduzido - variação de taxa de transferência de carga de trabalho de convidado é reduzida significativamente, oferecendo maior consistência de carga de trabalho.

* Uso de SMT no convidado VMs - o sistema operacional e aplicativos em execução na máquina virtual convidada pode utilizar comportamento SMT e programação APIs (interfaces) para controlar e distribuir o trabalho entre threads SMT, exatamente como eles executaria quando não virtualizado.

O Agendador de núcleo atualmente é usado em hosts de virtualização Azure, especificamente para aproveitar o limite de segurança forte e variabilty de baixa carga de trabalho. A Microsoft acredita que o tipo de Agendador principal deve ser e continuará a ser o hipervisor padrão agendamento de tipo para a maioria dos cenários de virtualização.  Portanto, para garantir que nossos clientes são seguros por padrão, a Microsoft está fazendo essa alteração para o Windows Server 2019 agora.

### Impactos no desempenho de Agendador Core cargas de trabalho de convidado

Embora seja necessário para atenuar efetivamente determinadas classes de vulnerabilidades, o Agendador de núcleo também potencialmente pode reduzir o desempenho. Os clientes podem ver uma diferença nas características de desempenho com suas VMs e impactos com a capacidade de carga de trabalho geral de seus hosts de virtualização. Em casos onde o Agendador de núcleo deve executar um não - SMT vice-Presidente, apenas um dos fluxos instrução no núcleo lógico subjacente é executado enquanto o outro deve ser deixado ocioso. Isso limitará a capacidade total de host para cargas de trabalho de convidado.

Esses impactos no desempenho podem ser minimizados, seguindo as diretrizes de implantação neste documento. Os administradores de host cuidadosamente devem considerar específicas de virtualização cenários de implantação e equilibrar sua tolerância a riscos de segurança em relação a necessidade de carga de trabalho máxima densidade, visados consolidação de hosts de virtualização, etc.

## Alterações no padrão e as configurações recomendadas para o Windows Server 2016 e Windows Server 2019

Implantar hosts Hyper-V com a postura de segurança máximo requer o uso do tipo hipervisor core Agendador. Para garantir que nossos clientes são seguros por padrão, a Microsoft está mudando o seguinte formato padrão e as configurações recomendadas.

>[!NOTE]
>Enquanto o suporte interno do hipervisor para os tipos de Agendador foi incluída na versão inicial do Windows Server 2016, Windows Server 1709 e Windows Server 1803, as atualizações são necessárias para acessar o controle de configuração que permite selecionar o tipo de Agendador do hipervisor.  Consulte [Entendendo](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types) e usando tipos de Agendador de hipervisor do Hyper-V para obter detalhes sobre essas atualizações.

### Alterações de host de virtualização

* O hipervisor usará o Agendador core pelo Windows Server 2019 a partir do padrão.

* Microsoft reccommends Configurando o Agendador core no Windows Server 2016. O tipo de Agendador de núcleo do hipervisor é suportado no Windows Server 2016, no entanto, o padrão é o Agendador clássico. O Agendador de core é opcional e deve ser habilitado explicitamente pelo administrador do host do Hyper-V.

### Alterações de configuração de máquina virtual

* No Windows Server 2019, o novas máquinas virtuais criadas usando a versão VM padrão 9.0 herdarão automaticamente as propriedades SMT (ativado ou desativado) do host de virtualização. Ou seja, se SMT está habilitado no host físico, recém-criados VMs também terá SMT habilitado e herdará a topologia SMT do host por padrão, com a VM ter o mesmo número de segmentos de hardware por núcleo que o sistema subjacente. Isso será refletido na configuração da VM com HwThreadCountPerCore = 0, onde 0 indica que a máquina virtual deve herdam as configurações de SMT do host.

* As máquinas virtuais existentes com uma versão VM do 8.2 ou anterior será manter sua configuração de processador VM original para HwThreadCountPerCore e o padrão para 8.2 convidados de versão VM é HwThreadCountPerCore = 1. Quando esses convidados executado em um host do Windows Server 2019, ele serão tratados da seguinte maneira:

    1. Se a VM tem uma contagem de vice-Presidente for menor ou igual à contagem de núcleos LP, a VM serão ser tratada como um não - SMT VM pelo Agendador de núcleo. Quando o VICE-PRESIDENTE convidado é executado em um único thread SMT, irmão do núcleo threads SMT será ociosos. Isso é não ideal e resultará na perda geral de desempenho.

    2. Se a VM tem mais VPs que núcleos LP, a VM será tratada como uma VM SMT pelo Agendador de núcleo. No entanto, a VM não observará outras indicações que se trata de uma VM SMT. Por exemplo, o uso da instrução CPUID ou APIs do Windows para consultar a topologia de CPU pelo sistema operacional ou aplicativos não indicará que SMT esteja habilitado.

* Quando uma VM existente explicitamente é atualizada de versões VM eariler para a versão 9.0 por meio da operação de atualização-VM, a VM manterá seu valor atual de HwThreadCountPerCore.  A VM não terão SMT força habilitado.

* No Windows Server 2016, a Microsoft recomenda habilitar SMT para VMs convidadas.  Por padrão, as VMs criadas no Windows Server 2016 seria ter SMT desabilitado, que é HwThreadCountPerCore é definida como 1, a menos que explicitamente alterado.

>[!NOTE]
>Windows Server 2016 não dá suporte a configuração HwThreadCountPerCore como 0.

#### Gerenciamento de configuração da máquina virtual SMT

A configuração de SMT de máquina virtual convidada é definida em uma base por VM. O administrador do host pode inspecionar e definir a configuração de SMT da VM para selecionar dentre as seguintes opções:

    1. Configurar VMs para ser executado como SMT habilitado, opcionalmente herdar automaticamente a topologia SMT host

    2. Configurar VMs para execução como não-SMT

O configuaration SMT para uma VM é exibido nos painéis de resumo no console do Gerenciador do Hyper-V.  Definir configurações de SMT da VM pode ser feito usando as configurações de VM ou o PowerShell.

#### Definir configurações de VM SMT usando o PowerShell

Para definir as configurações de SMT para uma máquina virtual convidada, abra uma janela do PowerShell com permissões suficientes e digite:

``` powershell
Set-VMProcessor -VMName <VMName> -HwThreadCountPerCore <0, 1, 2>
```

Onde:

    0 = Inherit SMT topology from the host (this setting of HwThreadCountPerCore=0 is not supported on Windows Server 2016)

    1 = Non-SMT

    Values > 1 = the desired number of SMT threads per core. May not exceed the number of physical SMT threads per core.

Para ler as configurações de SMT para uma máquina virtual convidada, abra uma janela do PowerShell com permissões suficientes e digite:

``` powershell
(Get-VMProcessor -VMName <VMName>).HwThreadCountPerCore
```

Observe que convidado VMs configurados com HwThreadCountPerCore = 0 indica que a SMT será habilitada para o convidado e expõe o mesmo número de threads SMT para o convidado como eles estão no host virtualização subjacente, normalmente 2.

### VMs convidadas pode observar mudanças à topologia de CPU em todos os cenários de mobilidade VM

O sistema operacional e aplicativos em uma VM poderão ver as alterações para o host e configurações de VM antes e depois de eventos de ciclo de vida VM, como a migração dinâmica ou salvar e restaurar operações. Durante uma operação em qual VM estado é salvo e restaurado, configuração de HwThreadCountPerCore da VM e o valor realizado (ou seja, a combinação calculado de configuração da VM e configuração do host de origem) são migrados. A VM continuará sendo executada com essas configurações no host de destino. No ponto de VM é desligado e iniciado novamente, é possível que o valor realizado observado pela VM será alterado. Isso deve ser benigno, como o sistema operacional e aplicativo software de camada deve procurar por informações de topologia de CPU como parte de seus fluxos de código de inicialização e inicialização normais. No entanto, porque esses inicialização do tempo de inicialização sequências são ignoradas durante as operações em tempo real de migração ou salvar/restaurar, o VMs que passar por essas transições de estado poderia observar originalmente calculado valor obtido até que eles estão desligados e começar novamente.  

### Alertas sobre configurações de VM não ideal

Máquinas virtuais configuradas com VPs mais que o número de núcleos físicos no resultado host em uma configuração não ideal. O Agendador de hipervisor tratará essas VMs como se fossem SMT reconhecimento. No entanto, sistema operacional e software de aplicativo na VM será apresentada uma topologia de CPU mostrando SMT está desabilitada. Quando essa condição for detectada, o processo de trabalho do Hyper-V registrará um evento no host de virtualização aviso o administrador do host que a configuração da VM é não ideal e recomendar SMT ser habilitado para a VM.

#### Como identificar forma não otimizada configurado VMs

Você pode identificar não - SMT VMs examinando o Log do sistema no Visualizador de eventos para o evento de processo de trabalho do Hyper-V 3498 ID, que será disparada para uma VM sempre que o número de VPs na VM é maior do que a contagem de núcleo físico. Eventos de processo de trabalho podem ser obtidos do Visualizador de eventos, ou por meio do PowerShell.

#### Consultando o evento de VM do Hyper-V trabalhador processo usando o PowerShell

A consulta de evento 3498 ID do processo de trabalhador do Hyper-V usando o PowerShell, digite os seguintes comandos em um prompt do PowerShell.

``` powershell
Get-WinEvent -FilterHashTable @{ProviderName="Microsoft-Windows-Hyper-V-Worker"; ID=3498}
```

### Impactos de convidado SMT configuaration sobre o uso de esclarecimentos de hipervisor para sistemas operacionais convidados

O hipervisor da Microsoft oferece vários esclarecimentos ou dicas, que pode ser consultadas e usar disparar otimizações, como aqueles que podem se beneficiar de desempenho ou melhorar caso contrário, a manipulação de várias condições ao executar o sistema operacional em execução em um convidado VM virtualizado. Habilitação recentemente introduzida um diz respeito a manipulação de agendamento de processador virtual e o uso de mitigações de sistema operacional para ataques de canal que explorar SMT.

>[!NOTE]
>A Microsoft recomenda que os administradores de host habilitar SMT para VMs convidadas otimizar o desempenho de carga de trabalho.

Os detalhes dessa habilitação de convidado são fornecidos abaixo, porém o principal argumento para os administradores de host de virtualização é que as máquinas virtuais deve ter HwThreadCountPerCore configurada para corresponder a configuração de SMT física do host. Isso permite que o hipervisor para relatar que não há nenhuma core não arquitetônicas compartilhamento. Portanto, qualquer otimizações de suporte do sistema operacional convidado que exigem a habilitação podem ser ativadas. No Windows Server 2019, crie novas VMs e deixe o valor padrão de HwThreadCountPerCore (0). Migraram VMs mais antigas do Windows Server 2016 hosts podem ser atualizados para a versão de configuração do Windows Server 2019. Após fazer isso, a Microsoft recomenda definir HwThreadCountPerCore = 0.  No Windows Server 2016, a Microsoft recomenda a configuração HwThreadCountPerCore para corresponder à configuração de host (normalmente 2).

### Detalhes de habilitação de NoNonArchitecturalCoreSharing

A partir do Windows Server 2016, o hipervisor define uma novo habilitação para descrever seu tratamento de agendamento vice-Presidente e posicionamento para o sistema operacional convidado. Essa habilitação é definida na [especificação funcional do nível do hipervisor superior v5.0c](https://docs.microsoft.com/virtualization/hyper-v-on-windows/reference/tlfs).

Folha CPUID sintética hipervisor CPUID.0x40000004.EAX:18[NoNonArchitecturalCoreSharing = 1] indica que um processador virtual nunca compartilharão um núcleo físico com outro processador virtual, com exceção de processadores virtuais que são relatados como irmão SMT threads. Por exemplo, um VICE-PRESIDENTE convidado nunca será executada em um thread SMT junto com um VICE-PRESIDENTE raiz executando simultaneamente em um irmão threads SMT no mesmo núcleo do processador. Essa condição é possível apenas durante a execução virtualizado e então representa um comportamento SMT não arquitetura que também tem sérias implicações de segurança. O sistema operacional convidado pode usar NoNonArchitecturalCoreSharing = 1 como uma indicação de que é seguro habilitar otimizações, que podem ajudá--lo a evitar a sobrecarga de desempenho de configuração STIBP.

Em determinadas configurações, o hipervisor não indicará que NoNonArchitecturalCoreSharing = 1. Por exemplo, se um host foi SMT habilitado e está configurado para usar o Agendador clássico do hipervisor, NoNonArchitecturalCoreSharing será 0. Isso pode impedir que convidados capacitados habilitando determinadas otimizações. Portanto, a Microsoft recomenda que os administradores de host usando SMT contam com o Agendador de núcleo do hipervisor e garantem que as máquinas virtuais são configuradas para herdar sua configuração SMT do host para garantir o desempenho ideal de carga de trabalho.

## Resumo

O cenário de ameaças de segurança continua a evoluir. Para garantir que nossos clientes são seguros por padrão, a Microsoft está mudando a configuração padrão para o hipervisor e máquinas virtuais a partir do Windows Server 2019 Hyper-V, e fornecendo atualizado diretrizes e recomendações para clientes que executam o Windows Server 2016 Hyper-V. Os administradores de host de virtualização devem:

* Ler e entender as orientações fornecidas neste documento

* Avalie cuidadosamente e ajustar suas implantações de virtualização para garantir que eles atendem a segurança, desempenho, densidade de virtualização e as metas de capacidade de resposta de carga de trabalho para seus requisitos exclusivos

* Considere reconfigurando hosts existentes do Windows Server 2016 Hyper-V para aproveitar os benefícios de segurança forte oferecidos pelo Agendador de núcleo de hipervisor

* Atualizar existente não - SMT VMs para reduzir o impacto de desempenho de agendamento restrições impostas por isolamento vice-Presidente que aborda as vulnerabilidades de segurança de hardware
