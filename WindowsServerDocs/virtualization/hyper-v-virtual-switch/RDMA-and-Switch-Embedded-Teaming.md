---
title: Acesso Remoto Direto à Memória (RDMA) e Agrupamento incorporado do comutador (SET)
description: Este tópico fornece informações sobre a configuração de interfaces RDMA (acesso remoto direto à memória) com o Hyper-V no Windows Server 2016, além de informações sobre o switch Embedded reteaming (SET).
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: lizross
author: eross-msft
ms.openlocfilehash: b0f11e67467521a8cfa98f4035435bbed537eda2
ms.sourcegitcommit: acfdb7b2ad283d74f526972b47c371de903d2a3d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/05/2020
ms.locfileid: "87769564"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>\( \) Conjunto de agrupamentos RDMA e Comutador incorporado de acesso remoto direto à memória \(\)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece informações sobre como configurar interfaces RDMA de acesso remoto direto à memória \( \) com o Hyper-V no Windows Server 2016, além de informações sobre o conjunto de agrupamentos inseridos \( \) .

> [!NOTE]
> Além deste tópico, o conteúdo de agrupamento inserido do comutador a seguir está disponível.
> - Download da galeria do TechNet: [Guia do usuário da NIC do Windows Server 2016 e do Comutador incorporado](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="configuring-rdma-interfaces-with-hyper-v"></a><a name="bkmk_rdma"></a>Configurando interfaces RDMA com o Hyper-V

No Windows Server 2012 R2, o uso do RDMA e do Hyper-V no mesmo computador que os adaptadores de rede que fornecem serviços RDMA não podem ser associados a um comutador virtual do Hyper-V. Isso aumenta o número de adaptadores de rede física que precisam ser instalados no host do Hyper-V.

>[!TIP]
>Em edições do Windows Server anteriores ao Windows Server 2016, não é possível configurar o RDMA em adaptadores de rede que estão associados a uma equipe NIC ou a um comutador virtual Hyper-V. No Windows Server 2016, você pode habilitar o RDMA em adaptadores de rede que estão associados a um comutador virtual do Hyper-V com ou sem conjunto.

No Windows Server 2016, você pode usar menos adaptadores de rede ao usar o RDMA com ou sem SET.

A imagem abaixo ilustra as alterações de arquitetura de software entre o Windows Server 2012 R2 e o Windows Server 2016.

![Alterações de arquitetura](../media/RDMA-and-SET/rdma_over.jpg)

As seções a seguir fornecem instruções sobre como usar comandos do Windows PowerShell para habilitar a ponte do Data Center (DCB), criar um comutador virtual do Hyper-V com um vNIC de NIC virtual RDMA \( \) e criar um comutador virtual do Hyper-v com set e RDMA vNICs.

### <a name="enable-data-center-bridging-dcb"></a>Habilitar DCB de ponte do Data Center \(\)

Antes de usar qualquer RDMA sobre a \( versão de RoCE Ethernet convergida \) de RDMA, você deve habilitar o DCB.  Embora não seja necessário para redes iWARP do protocolo RDMA de longa distância da Internet \( \) , o teste determinou que todas as tecnologias RDMA baseadas em Ethernet funcionam melhor com o DCB. Por isso, você deve considerar o uso de DCB até mesmo para implantações iWARP RDMA.

Os seguintes comandos de exemplo do Windows PowerShell demonstram como habilitar e configurar o DCB para SMB Direct.

Ativar DCB

```powershell
Install-WindowsFeature Data-Center-Bridging
```

Definir uma política para o SMB – Direct:

```powershell
New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
```

Ativar o controle de fluxo para SMB:

```powershell
Enable-NetQosFlowControl  -Priority 3
```

Verifique se o controle de fluxo está desativado para outro tráfego:

```powershell
Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7
```

Aplicar política aos adaptadores de destino:

```powershell
Enable-NetAdapterQos  -Name "SLOT 2"
```

Dê ao SMB Direct 30% da largura de banda mínima:

```powershell
New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS
```

Se você tiver um depurador de kernel instalado no sistema, deverá configurar o depurador para permitir que o QoS seja definido executando o comando a seguir.

Substituir o depurador – por padrão, o depurador bloqueia NetQos:

```powershell
Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force
```

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>Criar um comutador virtual do Hyper-V com um vNIC RDMA

Se SET não for necessário para sua implantação, você poderá usar os seguintes comandos do Windows PowerShell para criar um comutador virtual do Hyper-V com um vNIC RDMA.

> [!NOTE]
> O uso de equipes definidas com NICs físicas compatíveis com RDMA fornece mais recursos RDMA para que o vNICs consuma.

```powershell
New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"
```

Adicionar host vNICs e torná-los compatíveis com RDMA:

```powershell
Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"
```
Verifique os recursos RDMA:

```powershell
Get-NetAdapterRdma
```

###  <a name="create-a-hyper-v-virtual-switch-with-set-and-rdma-vnics"></a><a name="bkmk_set-rdma"></a>Criar um comutador virtual do Hyper-V com SET e RDMA vNICs

Para usar o RDMA capabilies em adaptadores de rede virtual do host Hyper-V \( vNICs \) em um comutador virtual do Hyper-v que dá suporte ao agrupamento RDMA, você pode usar estes comandos de exemplo do Windows PowerShell.

```powershell
New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true
```

Adicionar vNICs do host:

```powershell
Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS
```

Muitas opções não passam informações de classe de tráfego no tráfego de VLAN não marcado, portanto, certifique-se de que os adaptadores de host para RDMA estejam em VLANs. Este exemplo atribui os dois adaptadores virtuais de host SMB_ * para a VLAN 42.

```powershell
Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
```

Habilitar RDMA no host vNICs:

```powershell
Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"
```

Verificar os recursos RDMA; Verifique se os recursos são diferentes de zero:

```powershell
Get-NetAdapterRdma | fl *
```

## <a name="switch-embedded-teaming-set"></a>Alternar equipe inserida (SET)

Esta seção fornece uma visão geral do switch Embedded Integration (SET) no Windows Server 2016 e contém as seções a seguir.

- [DEFINIR visão geral](#bkmk_over)

- [DEFINIR disponibilidade](#bkmk_avail)

- [NICs com suporte e sem suporte para SET](#bkmk_nics)

- [DEFINIR a compatibilidade com as tecnologias de rede do Windows Server](#bkmk_compat)

- [Definir modos e configurações](#bkmk_modes)

- [DEFINIR e filas de máquina virtual (VMQs)](#bkmk_vmq)

- [SET e virtualização de rede Hyper-V (HNV)](#bkmk_hnv)

- [DEFINIR e Migração ao Vivo](#bkmk_live)

- [Uso de endereço MAC em pacotes transmitidos](#bkmk_mac)

- [Gerenciando uma equipe de conjunto](#bkmk_manage)

## <a name="set-overview"></a><a name="bkmk_over"></a>DEFINIR visão geral

O conjunto é uma solução de agrupamento NIC alternativa que você pode usar em ambientes que incluem o Hyper-V e a pilha de Sdn de rede definida pelo software \( \) no Windows Server 2016. O conjunto integra algumas funcionalidades de agrupamento NIC ao comutador virtual Hyper-V.

SET permite que você agrupe entre um e oito adaptadores de rede Ethernet física em um ou mais adaptadores de rede virtual baseados em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.

O conjunto de adaptadores de rede de membros deve ser instalado no mesmo host do Hyper-V físico a ser colocado em uma equipe.

> [!NOTE]
> O uso de SET tem suporte apenas no comutador virtual Hyper-V no Windows Server 2016. Não é possível implantar o conjunto no Windows Server 2012 R2.

Você pode conectar suas NICs agrupadas ao mesmo comutador físico ou a diferentes comutadores físicos. Se você conectar NICs a diferentes comutadores, ambos os comutadores deverão estar na mesma sub-rede.

A ilustração a seguir descreve a arquitetura do conjunto.

![DEFINIR arquitetura](../media/RDMA-and-SET/set_architecture.jpg)

Como o conjunto é integrado ao comutador virtual do Hyper-V, não é possível usar SET dentro de uma VM (máquina virtual). Você pode, no entanto, usar agrupamento NIC em VMs.

Para obter mais informações, consulte [agrupamento NIC em VMS (máquinas virtuais)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms).

Além disso, a arquitetura SET não expõe as interfaces da equipe. Em vez disso, você deve configurar portas do comutador virtual do Hyper-V.

## <a name="set-availability"></a><a name="bkmk_avail"></a>DEFINIR disponibilidade

O conjunto está disponível em todas as versões do Windows Server 2016 que incluem o Hyper-V e a pilha SDN. Além disso, você pode usar comandos do Windows PowerShell e conexões Área de Trabalho Remota para gerenciar o conjunto de computadores remotos que estão executando um sistema operacional cliente no qual as ferramentas têm suporte.

## <a name="supported-nics-for-set"></a><a name="bkmk_nics"></a>NICs com suporte para SET

Você pode usar qualquer NIC Ethernet que tenha passado o teste de qualificação de hardware do Windows e logotipo \( \) de WHQL em uma equipe definida no Windows Server 2016. SET requer que todos os adaptadores de rede que são membros de uma equipe definida devam ser idênticos, \( ou seja, mesmo fabricante, mesmo modelo, mesmo firmware e driver \) . O conjunto oferece suporte entre um e oito adaptadores de rede em uma equipe.

## <a name="set-compatibility-with-windows-server-networking-technologies"></a><a name="bkmk_compat"></a>DEFINIR a compatibilidade com as tecnologias de rede do Windows Server

O conjunto é compatível com as seguintes tecnologias de rede no Windows Server 2016.

- DCB de ponte do datacenter \(\)

- Virtualização de rede Hyper-V-NV-GRE e VxLAN têm suporte no Windows Server 2016.
- Descarregamentos de soma de verificação do lado do recebimento \( IPv4, IPv6, TCP \) -esses são suportados se qualquer um dos membros da equipe do conjunto oferecer suporte a eles.

- Acesso remoto direto à memória \( RDMA\)

- Sr-IOV de virtualização de e/s de raiz única \(\)

- Descarregamentos de soma de verificação do lado de transmissão \( IPv4, IPv6, TCP \) -esses são suportados se todos os membros da equipe do conjunto dão suporte a eles.

- VMQ de filas da máquina virtual \(\)

- RSS de escala lateral de recebimento virtual \(\)

SET não é compatível com as seguintes tecnologias de rede no Windows Server 2016.

- autenticação 802.1 x. os pacotes EAP do protocolo de autenticação extensível 802.1 x \( \) são descartados automaticamente pelo \- comutador virtual Hyper-V em cenários de conjunto.

- IPSec de descarregamento de tarefa IPsecto \( \) . Essa é uma tecnologia herdada que não é suportada pela maioria dos adaptadores de rede e onde ela existe, está desabilitada por padrão.

- Usando \(pacer.exe\) de QoS no host ou em sistemas operacionais nativos. Esses cenários de QoS não são \- cenários do Hyper-V, portanto, as tecnologias não se interseccionam. Além disso, a QoS está disponível, mas não está habilitada por padrão – você deve habilitar intencionalmente a QoS.

- Receber União do lado a \( RSC \) . O RSC é desabilitado automaticamente pelo \- comutador virtual Hyper-V.

- RSS de escala lateral de recebimento \( \) . Como o Hyper-V usa as filas para VMQ e VMMQ, o RSS é sempre desabilitado quando você cria um comutador virtual.

- Descarregamento de Chimney TCP. Essa tecnologia é desabilitada por padrão.

- VM QoS de máquina virtual \( -QoS \) . A QoS de VM está disponível, mas desabilitada por padrão. Se você configurar a QoS de VM em um ambiente definido, as configurações de QoS causarão resultados imprevisíveis.

## <a name="set-modes-and-settings"></a><a name="bkmk_modes"></a>Definir modos e configurações

Ao contrário do agrupamento NIC, quando você cria uma equipe de conjunto, não é possível configurar um nome de equipe. Além disso, o uso de um adaptador em espera é suportado no agrupamento NIC, mas não tem suporte no conjunto. Quando você implanta SET, todos os adaptadores de rede estão ativos e nenhum está no modo de espera.

Outra diferença importante entre o agrupamento de NIC e o conjunto é que o agrupamento NIC fornece a opção de três diferentes modos de agrupamento, enquanto SET oferece suporte apenas ao modo **independente de comutador** . Com o modo independente de comutador, o comutador ou comutadores para os quais os membros da equipe do conjunto estão conectados não sabem a presença da equipe definida e não determinam como distribuir o tráfego de rede para definir os membros da equipe. em vez disso, a equipe de conjunto distribui o tráfego de rede de entrada entre os membros da equipe do conjunto.

Ao criar uma nova equipe do conjunto, você deve configurar as seguintes propriedades da equipe.

- Adaptadores de membro

- Modo de balanceamento de carga

### <a name="member-adapters"></a>Adaptadores de membro

Ao criar uma equipe do conjunto, você deve especificar até oito adaptadores de rede idênticos associados ao comutador virtual do Hyper-V como definir adaptadores de membro da equipe.

### <a name="load-balancing-mode"></a>Modo de balanceamento de carga

As opções para definir o modo de distribuição de balanceamento de carga de equipe são **a porta Hyper-V** e a **dinâmica**.

**Porta Hyper-V**

As VMs estão conectadas a uma porta no comutador virtual Hyper-V. Ao usar o modo de porta do Hyper-V para o SET Teams, a porta do comutador virtual do Hyper-V e o endereço MAC associado são usados para dividir o tráfego de rede entre os membros da equipe do conjunto.

> [!NOTE]
> Quando você usa SET em conjunto com o pacote direto, a opção de modo de agrupamento é **independente** e a **porta Hyper-V** do modo de balanceamento de carga é necessária.

Como a opção adjacente sempre vê um endereço MAC específico em uma determinada porta, o comutador distribui a carga de entrada (o tráfego da mudança para o host) para a porta na qual o endereço MAC está localizado. Isso é particularmente útil quando as filas de máquina virtual (VMQs) são usadas, porque uma fila pode ser colocada na NIC específica em que se espera que o tráfego chegue.

No entanto, se o host tiver apenas algumas VMs, esse modo poderá não ser granular o suficiente para alcançar uma distribuição bem balanceada. Esse modo também limitará sempre uma única VM (ou seja, o tráfego de uma única porta de comutador) para a largura de banda disponível em uma única interface.

**Dinâmicos**

Esse modo de balanceamento de carga oferece as seguintes vantagens.

- As cargas de saída são distribuídas com base em um hash de portas TCP e endereços IP.  O modo dinâmico também balanceia os carregamentos em tempo real para que um determinado fluxo de saída possa se mover de volta e para trás entre os membros da equipe de conjunto.

- As cargas de entrada são distribuídas da mesma maneira que o modo de porta do Hyper-V.

As cargas de saída nesse modo são balanceadas dinamicamente com base no conceito de flowlets. Assim como a fala humana tem quebras naturais nas extremidades de palavras e sentenças, os fluxos de TCP (fluxos de comunicação TCP) também apresentam quebras naturalmente. A parte de um fluxo de TCP entre duas dessas interrupções é chamada de flowlet.

Quando o algoritmo de modo dinâmico detecta que um limite de flowlet foi encontrado-por exemplo, quando ocorreu uma interrupção de comprimento suficiente no fluxo de TCP, o algoritmo reequilibra automaticamente o fluxo para outro membro da equipe, se apropriado.  Em algumas circunstâncias incomuns, o algoritmo também pode reequilibrar periodicamente os fluxos que não contêm nenhum flowlets. Por isso, a afinidade entre o fluxo TCP e o membro da equipe pode mudar a qualquer momento, pois o algoritmo de balanceamento dinâmico funciona para balancear a carga de trabalho dos membros da equipe.

## <a name="set-and-virtual-machine-queues-vmqs"></a><a name="bkmk_vmq"></a>DEFINIR e filas de máquina virtual (VMQs)

A VMQ e a SET funcionam bem em conjunto, e você deve habilitar a VMQ sempre que estiver usando o Hyper-V e o SET.

> [!NOTE]
> SET sempre apresenta o número total de filas que estão disponíveis em todos os membros da equipe do conjunto. No agrupamento NIC, isso é chamado de modo de soma de filas.

A maioria dos adaptadores de rede tem filas que podem ser usadas para receber \( RSS \) ou VMQ em escala lateral, mas não ambas ao mesmo tempo.

Algumas configurações de VMQ parecem ser configurações para filas RSS, mas são realmente configurações nas filas genéricas que o RSS e a VMQ usam, dependendo de qual recurso está atualmente em uso. Cada NIC tem, em suas propriedades avançadas, valores para `*RssBaseProcNumber` e `*MaxRssProcessors` .

A seguir estão algumas configurações de VMQ que fornecem melhor desempenho do sistema.

- Idealmente, cada NIC deve ter o `*RssBaseProcNumber` conjunto definido como um número par maior ou igual a dois (2). Isso ocorre porque o primeiro processador físico, os \( processadores lógicos de núcleo 0 0 e 1 \) , normalmente faz a maior parte do processamento do sistema, de modo que o processamento de rede deve ser direcionado para fora desse processador físico.

>[!NOTE]
>Algumas arquiteturas de computador não têm dois processadores lógicos por processador físico, portanto, para essas máquinas, o processador base deve ser maior ou igual a 1. Em caso de dúvida, suponha que o host esteja usando um processador lógico 2 por arquitetura de processador físico.

- Os processadores da equipe devem estar, na medida em que é prático, sem sobreposição. Por exemplo, em um host de 4 núcleos, \( dois processadores lógicos \) com uma equipe de 2 NICs 10 Gbps, você pode definir o primeiro para usar o processador base 2 e usar 4 núcleos; o segundo seria configurado para usar o processador base 6 e usar dois núcleos.

## <a name="set-and-hyper-v-network-virtualization-hnv"></a><a name="bkmk_hnv"></a>SET e HNV de virtualização de rede Hyper-V \(\)

O conjunto é totalmente compatível com a virtualização de rede Hyper-V no Windows Server 2016. O sistema de gerenciamento HNV fornece informações para o driver de conjunto que permite que o configure o distribua a carga de tráfego de rede de uma maneira que é otimizada para o tráfego HNV.

## <a name="set-and-live-migration"></a><a name="bkmk_live"></a>DEFINIR e Migração ao Vivo

O Migração ao Vivo tem suporte no Windows Server 2016.

## <a name="mac-address-use-on-transmitted-packets"></a><a name="bkmk_mac"></a>Uso de endereço MAC em pacotes transmitidos

Quando você configura uma equipe de conjunto com a distribuição de carga dinâmica, os pacotes de uma única fonte, como \( uma única VM, \) são distribuídos simultaneamente entre vários membros da equipe.

Para evitar que os interruptores fiquem confusos e evitar alarmes de oscilação de MAC, SET substitui o endereço MAC de origem por um endereço MAC diferente nos quadros que são transmitidos em membros da equipe que não sejam o membro da equipe do relacionados. Por isso, cada membro da equipe usa um endereço MAC diferente e os conflitos de endereço MAC são evitados, a menos que e até que ocorra uma falha.

Quando uma falha é detectada na NIC primária, o software de agrupamento de conjunto é iniciado usando o endereço MAC da VM no membro da equipe que é escolhido para servir como o membro da equipe de relacionados temporário, \( ou seja, aquele que agora será exibido para a opção como a interface da VM \) .

Essa alteração se aplica somente ao tráfego que será enviado no membro da equipe relacionados da VM com o endereço MAC próprio da VM como seu endereço MAC de origem. Outro tráfego continua a ser enviado com qualquer endereço MAC de origem que ele teria usado antes da falha.

Veja a seguir as listas que descrevem o comportamento de substituição do endereço MAC de agrupamento, com base em como a equipe está configurada:

- Em Alternar modo independente com a distribuição de porta do Hyper-V

    - Cada porta vmSwitch é relacionados a um membro da equipe

    - Cada pacote é enviado no membro da equipe para o qual a porta está relacionados

    - Nenhuma substituição de MAC de origem foi feita

- Em modo independente de opção com distribuição dinâmica

    - Cada porta vmSwitch é relacionados a um membro da equipe

    - Todos os pacotes ARP/NS são enviados no membro da equipe para o qual a porta está relacionados

    - Os pacotes enviados no membro da equipe que é o membro da equipe do relacionados não têm nenhuma substituição de endereço MAC de origem concluída

    - Os pacotes enviados em um membro da equipe que não seja o membro da equipe do relacionados terão a substituição do endereço MAC de origem concluída

## <a name="managing-a-set-team"></a><a name="bkmk_manage"></a>Gerenciando uma equipe de conjunto

É recomendável que você use System Center Virtual Machine Manager \( VMM \) para gerenciar equipes definidas, no entanto, você também pode usar o Windows PowerShell para gerenciar o conjunto. As seções a seguir fornecem os comandos do Windows PowerShell que você pode usar para gerenciar o conjunto.

Para obter informações sobre como criar uma equipe de conjunto usando o VMM, consulte a seção "configurar um comutador lógico" no tópico da biblioteca do System Center VMM [criar comutadores lógicos](https://docs.microsoft.com/system-center/vmm/network-switch).

### <a name="create-a-set-team"></a>Criar uma equipe de conjunto

Você deve criar uma equipe de conjunto ao mesmo tempo em que cria o comutador virtual do Hyper-V usando o comando **New-VMSwitch** do Windows PowerShell.

Ao criar o comutador virtual do Hyper-V, você deve incluir o novo parâmetro **EnableEmbeddedTeaming** na sintaxe do comando. No exemplo a seguir, uma opção do Hyper-V chamada **TeamedvSwitch** com o agrupamento incorporado e dois membros iniciais da equipe é criada.

```
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true
```

O parâmetro **EnableEmbeddedTeaming** é assumido pelo Windows PowerShell quando o argumento para **netadaptadorname** é uma matriz de NICs em vez de uma única NIC. Como resultado, você pode revisar o comando anterior da seguinte maneira.

```
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"
```

Se você quiser criar um comutador com capacidade de conjunto com um único membro da equipe para que você possa adicionar um membro da equipe em um momento posterior, você deverá usar o parâmetro EnableEmbeddedTeaming.

```
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true
```

### <a name="adding-or-removing-a-set-team-member"></a>Adicionando ou removendo um membro da equipe de conjunto

O comando **set-VMSwitchTeam** inclui a opção **netadaptername** . Para alterar os membros da equipe em uma equipe definida, insira a lista desejada de membros da equipe após a opção **Netadaptername** . Se **TeamedvSwitch** tiver sido originalmente criado com NIC 1 e NIC 2, o comando de exemplo a seguir excluirá o membro da equipe "NIC 2" e adicionará um novo membro da equipe "NIC 3" do conjunto.

```
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"
```

### <a name="removing-a-set-team"></a>Removendo uma equipe de conjunto

Você pode remover uma equipe de conjunto apenas removendo o comutador virtual do Hyper-V que contém a equipe definida.  Use o tópico [Remove-VMSwitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch) para obter informações sobre como remover o comutador virtual Hyper-V. O exemplo a seguir remove um comutador virtual chamado **SETvSwitch**.

```
Remove-VMSwitch "SETvSwitch"
```

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>Alterando o algoritmo de distribuição de carga para uma equipe definida

O cmdlet **set-VMSwitchTeam** tem uma opção **LoadBalancingAlgorithm** . Essa opção usa um dos dois valores possíveis: **HyperVPort** ou **Dynamic**. Para definir ou alterar o algoritmo de distribuição de carga para uma equipe do switch-Embedded, use essa opção.

No exemplo a seguir, o VMSwitchTeam chamado **TeamedvSwitch** usa o algoritmo de balanceamento de carga **dinâmico** .
```
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic
```
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>Ajustando interfaces virtuais a membros físicos da equipe

SET permite que você crie uma afinidade entre uma interface virtual \( , ou seja, a porta do comutador virtual do Hyper-V \) e uma das NICs físicas na equipe.

Por exemplo, se você criar dois vNICs de host para SMB \- Direct, como na seção [criar um comutador virtual Hyper-V com set e RDMA vNICs](#bkmk_set-rdma), você poderá garantir que os dois vNICs usem diferentes membros da equipe.

Adicionando ao script nessa seção, você pode usar os seguintes comandos do Windows PowerShell.

```powershell
Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”
```

Este tópico é examinado em mais detalhes na seção 4.2.5 da [NIC do Windows Server 2016 e no guia do usuário do conjunto incorporado de comutador](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0).
