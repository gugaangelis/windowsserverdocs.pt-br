---
title: Acesso Remoto Direto à Memória (RDMA) e Agrupamento incorporado do comutador (SET)
description: Este tópico fornece informações sobre como configurar interfaces direto memória RDMA (acesso remoto) com o Hyper-V no Windows Server 2016, além de informações sobre o Switch Embedded Teaming (SET).
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: 68c35b64-4d24-42be-90c9-184f2b5f19be
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c945144194ac86623bfa8ce60a306f0202df0874
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881657"
---
# <a name="remote-direct-memory-access-rdma-and-switch-embedded-teaming-set"></a>Acesso remoto direto à memória \(RDMA\) e agrupamento incorporado do comutador \(definido\)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico fornece informações sobre como configurar o acesso de memória direto remoto \(RDMA\) interfaces com o Hyper-V no Windows Server 2016, além disso, para obter informações sobre agrupamento incorporado do comutador \(definir\).  

> [!NOTE]
> Além deste tópico, o seguinte conteúdo agrupamento incorporado do comutador está disponível. 
> - Baixe da Galeria do TechNet: [NIC do Windows Server 2016 e o guia do usuário agrupamento incorporado do comutador](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)

## <a name="bkmk_rdma"></a>Configurando Interfaces RDMA com o Hyper-V  

No Windows Server 2012 R2, usando o Hyper-V e o RDMA no mesmo computador que os adaptadores de rede que fornecem serviços RDMA não podem ser associados a um comutador Virtual do Hyper-V. Isso aumenta o número de adaptadores de rede física que são necessários para ser instalado no host do Hyper-V.

>[!TIP]
>Nas edições do Windows Server anteriores ao Windows Server 2016, não é possível configurar o RDMA nos adaptadores de rede que estão associados a uma equipe NIC ou a um comutador Virtual do Hyper-V. No Windows Server 2016, você pode habilitar o RDMA nos adaptadores de rede que estão associados a um comutador Virtual do Hyper-V com ou sem um conjunto.

No Windows Server 2016, você pode usar menos adaptadores de rede ao usar RDMA com ou sem um conjunto.

A imagem a seguir ilustra as alterações de arquitetura de software entre o Windows Server 2012 R2 e Windows Server 2016.

![Alterações de arquitetura](../media/RDMA-and-SET/rdma_over.jpg)

As seções a seguir fornecem instruções sobre como usar comandos do Windows PowerShell para habilitar ponte DCB (Data Center), crie um comutador Virtual do Hyper-V com uma NIC virtual RDMA \(vNIC\)e crie um comutador Virtual do Hyper-V com o conjunto e vNICs RDMA.

### <a name="enable-data-center-bridging-dcb"></a>Habilitar Data Center Bridging \(DCB\)

Antes de usar qualquer RDMA over Converged Ethernet \(RoCE\) versão de RDMA, você deve habilitar o DCB.  Enquanto não é necessário para o protocolo do IP ampla área RDMA \(iWARP\) redes, teste determinou que todas as tecnologias RDMA baseada em Ethernet funcionam melhor com DCB. Por isso, você deve considerar usar o DCB até mesmo para implantações de RDMA iWARP.

Os comandos de exemplo do Windows PowerShell a seguir demonstram como habilitar e configurar o DCB para SMB Direct.

Ativar o DCB

    Install-WindowsFeature Data-Center-Bridging

Defina uma política para SMB Direct:

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

Ative o controle de fluxo para SMB:

    Enable-NetQosFlowControl  -Priority 3

Verifique se o controle de fluxo é desativado para outro tráfego:

    Disable-NetQosFlowControl  -Priority 0,1,2,4,5,6,7

Aplica a política para os adaptadores de destino:

    Enable-NetAdapterQos  -Name "SLOT 2"

Dê ao SMB Direct 30% da largura de banda mínima:

`New-NetQosTrafficClass "SMB"  -Priority 3  -BandwidthPercentage 30  -Algorithm ETS`  

Se você tiver um depurador de kernel instalado no sistema, você deve configurar o depurador para permitir o QoS ser definido ao executar o comando a seguir.

Substituir o depurador - por padrão, o depurador bloqueia NetQos:
 
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 -Force

### <a name="create-a-hyper-v-virtual-switch-with-an-rdma-vnic"></a>Criar um comutador Virtual do Hyper-V com uma vNIC RDMA

Se o conjunto não é necessário para sua implantação, você pode usar os seguintes comandos do Windows PowerShell para criar um comutador Virtual do Hyper-V com uma vNIC RDMA.

> [!NOTE]
> Usando o conjunto de equipes com NICs físicas compatíveis com RDMA fornece mais recursos RDMA para as vNICs consumir.

    New-VMSwitch -Name RDMAswitch -NetAdapterName "SLOT 2"

Adicione os vNICs do host e torná-los RDMA compatível com:

    Add-VMNetworkAdapter -SwitchName RDMAswitch -Name SMB_1
    Enable-NetAdapterRDMA "vEthernet (SMB_1)" "SLOT 2"

Verifique se os recursos de RDMA:

    Get-NetAdapterRdma

###  <a name="bkmk_set-rdma"></a>Criar um comutador Virtual do Hyper-V com vNICs RDMA e SET

Ao fazer uso de RDMA funcionalidades no Hyper-V hospedam adaptadores de rede virtual \(vNICs\) em um comutador Virtual de Hyper-V que oferece suporte ao agrupamento de RDMA, você pode usar esses comandos do Windows PowerShell de exemplo.

    New-VMSwitch -Name SETswitch -NetAdapterName "SLOT 2","SLOT 3" -EnableEmbeddedTeaming $true

Adicione os vNICs do host:

    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_1 -managementOS
    Add-VMNetworkAdapter -SwitchName SETswitch -Name SMB_2 -managementOS

Muitos switches não passar informações de classe de tráfego no tráfego sem marcas de VLAN, portanto certifique-se de que os adaptadores de host para RDMA estão em VLANs. Este exemplo atribui dois SMB_ * adaptadores de host virtuais que 42 VLAN.
    
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_1  -IsolationMode VLAN -DefaultIsolationID 42
    Set-VMNetworkAdapterIsolation -ManagementOS -VMNetworkAdapterName SMB_2  -IsolationMode VLAN -DefaultIsolationID 42
    

Habilite o RDMA vNICs do Host:

    Enable-NetAdapterRDMA "vEthernet (SMB_1)","vEthernet (SMB_2)" "SLOT 2", "SLOT 3"

Verifique se os recursos RDMA; Certifique-se de que os recursos são diferentes de zero:

    Get-NetAdapterRdma | fl *


## <a name="bkmk_sswitchembedded"></a>Alternar agrupamento incorporado (SET)  

Esta seção fornece uma visão geral de Switch Embedded Teaming (SET) no Windows Server 2016 e contém as seções a seguir.

- [Visão geral do conjunto](#bkmk_over)

- [CONJUNTO de disponibilidade](#bkmk_avail)

- [NICs compatíveis e sem suportadas para o conjunto](#bkmk_nics)

- [DEFINIR a compatibilidade com as tecnologias de rede do Windows Server](#bkmk_compat)

- [Configurações e modos de conjunto](#bkmk_modes)

- [CONJUNTO e filas de máquina Virtual (VMQs)](#bkmk_vmq)

- [CONJUNTO e virtualização de rede Hyper-V (HNV)](#bkmk_hnv)

- [Migração ao vivo e defina](#bkmk_live)

- [Uso do endereço MAC em pacotes transmitidos](#bkmk_mac)

- [Gerenciar uma equipe de conjunto](#bkmk_manage)

## <a name="bkmk_over"></a>Visão geral do conjunto

CONJUNTO é uma solução alternativa do agrupamento NIC que você pode usar em ambientes que incluem o Hyper-V e a rede definida pelo Software \(SDN\) stack no Windows Server 2016. CONJUNTO integra algumas funcionalidades de agrupamento NIC no comutador Virtual do Hyper-V.

CONJUNTO permite que você agrupe entre uma e oito Ethernet adaptadores de rede física em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.

Todos os adaptadores de rede do conjunto de membro devem ser instalados no mesmo host do Hyper-V físico a ser colocado em uma equipe.

> [!NOTE]
> Somente há suporte para o uso do conjunto no comutador Virtual do Hyper-V no Windows Server 2016. Não é possível implantar o conjunto no Windows Server 2012 R2.

Você pode conectar os NICs agrupados para o mesmo comutador físico ou comutadores físicos diferentes. Se você conectar as NICs a diferentes comutadores, ambas as opções devem ser na mesma sub-rede.

A ilustração a seguir ilustra a arquitetura de conjunto.

![Arquitetura de conjunto](../media/RDMA-and-SET/set_architecture.jpg)

Como o conjunto é integrado ao comutador Virtual do Hyper-V, é possível usar o conjunto de dentro de uma máquina virtual (VM). Porém você pode usar o agrupamento NIC em máquinas virtuais.

Para obter mais informações, consulte [agrupamento NIC em máquinas virtuais (VMs)](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nict-vms).

Além disso, a arquitetura de conjunto não expõe as interfaces de equipe. Em vez disso, você deve configurar as portas do comutador Virtual do Hyper-V.

## <a name="bkmk_avail"></a>CONJUNTO de disponibilidade

CONJUNTO está disponível em todas as versões do Windows Server 2016 que incluem o Hyper-V e a pilha de SDN. Além disso, você pode usar comandos do Windows PowerShell e conexões de área de trabalho remota para gerenciar o conjunto de computadores remotos que estão executando um sistema operacional do cliente no qual as ferramentas são suportadas.

## <a name="bkmk_nics"></a>NICs com suporte para o conjunto

Você pode usar qualquer NIC Ethernet que passou a qualificação de Hardware do Windows e o logotipo \(WHQL\) testar em um conjunto da equipe no Windows Server 2016. CONJUNTO de requer que todos os adaptadores de rede que são membros de uma equipe de conjunto devem ser idêntico \(, ou seja, mesmo fabricante, mesmo modelo, mesmo firmware e driver\). CONJUNTO dá suporte a entre uma e oito adaptadores de rede em uma equipe.
  
## <a name="bkmk_compat"></a>DEFINIR a compatibilidade com as tecnologias de rede do Windows Server

CONJUNTO é compatível com as seguintes tecnologias de rede no Windows Server 2016.

- Ponte de Datacenter \(DCB\)
  
- Virtualização de rede do Hyper-V-NV GRE e VxLAN têm suporte no Windows Server 2016.  
- Descarregamento de soma de verificação do lado Recebedor \(IPv4, IPv6, TCP\) -eles têm suporte se qualquer um dos membros da equipe do conjunto de dar suporte a eles.

- Acesso remoto direto à memória \(RDMA\)

- Virtualização de e/s de raiz única \(SR-IOV\)

- Descarregamento de soma de verificação lado transmitir \(IPv4, IPv6, TCP\) -esses são suportados se todos os membros da equipe de conjunto de dar suporte a eles.

- Filas de máquina virtual \(VMQ\)

- Virtual Receive Side Scaling \(RSS\)

CONJUNTO não é compatível com as seguintes tecnologias de rede no Windows Server 2016.

- Autenticação 802.1X. 802.1 x Extensible Authentication Protocol \(EAP\) pacotes são descartados automaticamente pelo Hyper\-V Virtual Switch no conjunto de cenários.
 
- Descarregamento de tarefa IPsec \(IPsecTO\). Isso é uma tecnologia herdada que não é compatível com a maioria dos adaptadores de rede e, em que ele existe, ele é desabilitado por padrão.

- Usando a QoS \(pacer.exe\) no host ou sistemas de operacionais nativos. Esses cenários de QoS não são Hyper\-cenários V, portanto, as tecnologias não se cruzam. Além disso, QoS está disponível, mas não está habilitado por padrão – você deve habilitar a QoS intencionalmente.

- União de lado de recebimento \(RSC\). A RSC é desabilitada automaticamente pelo Hyper\-V Virtual Switch.

- O Receive side scaling \(RSS\). Como o Hyper-V usa as filas para VMQ e VMMQ, o RSS está sempre desabilitado quando você cria um comutador virtual.

- Descarregamento de Chimney TCP. Essa tecnologia é desabilitada por padrão.

- Máquina virtual QoS \(VM QoS\). VM QoS está disponível mas desabilitado por padrão. Se você configurar o QoS de VM em um ambiente de conjunto, as configurações de QoS fará com que a resultados imprevisíveis.

## <a name="bkmk_modes"></a>Configurações e modos de conjunto

Ao contrário de agrupamento NIC, quando você cria uma conjunto de equipe, você não pode configurar um nome de equipe. Além disso, usando um adaptador em espera é suportado no agrupamento NIC, mas não há suporte no conjunto. Quando você implanta um conjunto, todos os adaptadores de rede estão ativos e nenhum está no modo de espera.

Outra diferença importante entre o agrupamento NIC e o conjunto é que o agrupamento NIC fornece a opção de três modos diferentes de agrupamento, enquanto o conjunto dá suporte apenas **comutador independente** modo. Com o modo de comutador independente, o comutador ou comutadores à qual os membros da equipe definido estão conectados não estão cientes da presença da equipe do conjunto e não é determinar como distribuir o tráfego de rede para definir os membros da equipe – em vez disso, a equipe de conjunto distribui a rede de entrada tráfego entre os membros da equipe de conjunto.

Quando você cria um novo conjunto de equipe, você deve configurar as seguintes propriedades de equipe.

- Adaptadores de membro

- Modo de balanceamento de carga

### <a name="member-adapters"></a>Adaptadores de membro

Quando você cria uma conjunto de equipe, você deve especificar até oito adaptadores de rede idênticos que estão associados ao comutador Virtual Hyper-V como conjunto de adaptadores de membro de equipe.

### <a name="load-balancing-mode"></a>Modo de balanceamento de carga

As opções de conjunto de balanceamento de carga de modo de distribuição são de equipe **porta do Hyper-V** e **dinâmico**.

**Porta do Hyper-V**

As VMs sejam conectadas a uma porta no comutador Virtual Hyper-V. Ao usar o modo de porta Hyper-V para as equipes de conjunto, a porta do comutador Virtual Hyper-V e o endereço MAC associado são usados para dividir o tráfego de rede entre o conjunto de membros da equipe.

> [!NOTE]
> Quando você usa o conjunto em conjunto com o pacote direto, o modo de agrupamento **comutador independente** e o modo de balanceamento de carga **porta Hyper-V** são necessários.

Como sempre, o comutador adjacente vê um endereço MAC específico em uma determinada porta, o comutador distribui a carga de entrada (o tráfego do comutador para o host) para a porta em que o endereço MAC está localizado. Isso é particularmente útil quando as filas de máquina Virtual (VMQs) são usadas, porque uma fila pode ser colocada na NIC específica onde se espera o tráfego chegue.

No entanto, se o host tiver apenas algumas VMs, esse modo pode não ser suficientemente granular para alcançar uma distribuição bem equilibrada. Esse modo também sempre limitará uma única VM (ou seja, o tráfego da porta de um único comutador) para a largura de banda que está disponível em uma única interface.

**dinâmico**

Este modo de balanceamento de carga fornece as seguintes vantagens.

- Cargas de saída são distribuídas com base em um hash dos endereços IP e portas TCP.  Modo dinâmico também novamente equilibra cargas em tempo real para que um determinado fluxo de saída pode ir e voltar entre conjunto de membros da equipe.

- Cargas de entrada são distribuídas da mesma maneira que o modo da porta do Hyper-V.

As cargas de saída nesse modo dinamicamente são balanceadas com base no conceito de flowlets. Assim como fala humana tem quebras naturais nos finais de palavras e frases, fluxos de TCP (fluxos de comunicação TCP) também têm quebras que ocorre naturalmente. A parte de um fluxo TCP entre duas quebras de tais é conhecida como um flowlet.

Quando o algoritmo de modo dinâmico detecta que um limite de flowlet foi encontrado - por exemplo quando uma quebra de tamanho suficiente tiver ocorrido no fluxo de TCP - o algoritmo automaticamente balanceia novamente o fluxo para outro membro da equipe se apropriado.  Em algumas circunstâncias incomuns, o algoritmo também periodicamente pode reequilibrar fluxos que não contêm nenhum flowlets. Por isso, a afinidade entre o membro de equipe e o fluxo TCP pode alterar a qualquer momento como o algoritmo de balanceamento dinâmico funciona para balancear a carga de trabalho dos membros da equipe.

## <a name="bkmk_vmq"></a>CONJUNTO e filas de máquina Virtual (VMQs)

VMQ e o conjunto funcionam bem em conjunto, e você deve habilitar VMQ, sempre que você estiver usando o Hyper-V e defina.

> [!NOTE]
> Defina sempre apresenta o número total de filas que estão disponíveis em conjunto todos os membros da equipe. No agrupamento NIC, isso é chamado de modo de soma de filas.

A maioria dos adaptadores de rede têm as filas que podem ser usadas para qualquer um dos Receive Side Scaling \(RSS\) ou VMQ, mas não ambos ao mesmo tempo.
  
Algumas configurações de VMQ parecem ser as configurações para as filas RSS, mas são realmente as configurações nas filas genéricas que uso RSS e VMQ, dependendo de qual recurso está atualmente em uso. Cada NIC tem, em suas propriedades avançadas, os valores para `*RssBaseProcNumber` e `*MaxRssProcessors`.

A seguir estão algumas configurações de VMQ que fornecem melhor desempenho do sistema.

- O ideal é que cada NIC deve ter o `*RssBaseProcNumber` definido como um número par, maior que ou igual a dois (2). Isso ocorre porque o primeiro processador físico, Core 0 \(processadores lógicos, 0 e 1\), normalmente faz a maior parte do sistema de processamento para que o processamento de rede deve ser direcionado para longe desse processador físico. 

>[!NOTE]
>Algumas arquiteturas de computador não tem dois processadores lógicos por processador físico, portanto, tais máquinas o processador de base deve ser maior que ou igual a 1. Em caso de dúvida, suponha que seu host está usando um processador lógico 2 por arquitetura de processador físico.

- Processadores de membros da equipe devem ser, na medida em que é prática, não sobrepostos. Por exemplo, em um host de 4 núcleos \(8 processadores lógicos\) com uma equipe de 2 NICs de 10 Gbps, você pode definir a primeira é usar o processador de base de 2 e usar 4 núcleos; o segundo seria definido para usar o processador de base 6 e usar 2 núcleos.

## <a name="bkmk_hnv"></a>Virtualização de rede do conjunto e Hyper-V \(HNV\)

CONJUNTO é totalmente compatível com virtualização de rede do Hyper-V no Windows Server 2016. O sistema de gerenciamento de HNV fornece informações para o driver de conjunto que permite que um conjunto distribuir a carga de tráfego de rede de uma maneira que é otimizada para o tráfego da HNV.
  
## <a name="bkmk_live"></a>Migração ao vivo e defina

Migração ao vivo é suportada no Windows Server 2016.

## <a name="bkmk_mac"></a>Uso do endereço MAC em pacotes transmitidos

Quando você configura uma equipe em conjunto com a distribuição de carga dinâmico, os pacotes de uma única fonte \(como uma única VM\) simultaneamente são distribuídas entre vários membros da equipe. 

Para impedir que os comutadores confuso e para evitar alarmes oscilação do MAC, defina substitui o endereço MAC de origem com um endereço MAC diferente dos quadros que são transmitidos em membros da equipe que não seja membro da equipe de afinidade. Por isso, cada membro da equipe usa um endereço MAC diferente e conflitos de endereços MAC são impedidos até que a falha ocorre.

Quando uma falha é detectada na NIC primária, o conjunto de software de agrupamento é iniciado usando o endereço de MAC da VM no membro da equipe é escolhido para servir como um membro da equipe temporária de afinidade \(, ou seja, aquele que agora aparecerão ao comutador da VM interface\).

Essa alteração se aplica apenas ao tráfego que vai ser enviado no membro de equipe afinidade da VM com o endereço MAC da VM como seu endereço MAC de origem. Outro tráfego continua a ser enviada com qualquer fonte da qual endereço de MAC, ele poderia ter usado antes da falha.

Seguem listas que descrevem o comportamento de substituição de endereço MAC, com base em como a equipe está configurada o agrupamento de conjunto:

- No modo independente de comutador com a distribuição de porta Hyper-V

    - Cada porta vmSwitch é agrupado com um membro da equipe
  
    - Cada pacote é enviado em um membro da equipe ao qual a porta é agrupado com  
  
    - Nenhuma fonte de substituição de MAC é feita  
  
- No modo independente de comutador com a distribuição dinâmica
  
    - Cada porta vmSwitch é agrupado com um membro da equipe  
  
    - Todos os pacotes de ARP/NS são enviados em um membro da equipe ao qual a porta é agrupado com  
  
    - Pacotes enviados em um membro da equipe que é membro da equipe de afinidade não tem nenhuma fonte de substituição de endereço MAC feita  
  
    - Pacotes enviados em um membro da equipe que não seja membro da equipe afinidade terá a substituição de endereço MAC de origem concluída  
  
## <a name="bkmk_manage"></a>Gerenciar uma equipe de conjunto

É recomendável que você use o System Center Virtual Machine Manager \(VMM\) gerenciar o conjunto de equipes, porém você também pode usar Windows PowerShell para gerenciar o conjunto. As seções a seguir fornecem que o Windows PowerShell comandos que você pode usar para gerenciar o conjunto.

Para obter informações sobre como criar um conjunto de equipe usando o VMM, consulte a seção "Configurar um comutador lógico" no tópico da biblioteca do System Center VMM [criar comutadores lógicos](https://docs.microsoft.com/system-center/vmm/network-switch).
  
### <a name="create-a-set-team"></a>Criar uma equipe de conjunto

Você deve criar uma equipe de conjunto ao mesmo tempo que você cria o comutador Virtual do Hyper-V usando o **New-VMSwitch** comando do Windows PowerShell.

Quando você cria o comutador Virtual do Hyper-V, você deve incluir o novo **EnableEmbeddedTeaming** parâmetro em sua sintaxe de comando. No exemplo a seguir, um comutador de Hyper-V chamado **TeamedvSwitch** com agrupamento incorporado e equipe inicial dois membros é criado.
  
```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2" -EnableEmbeddedTeaming $true  
```  
  
O **EnableEmbeddedTeaming** parâmetro é considerado pelo Windows PowerShell quando o argumento **NetAdapterName** é uma matriz de NICs, em vez de uma única NIC. Como resultado, você pode revisar o comando anterior da seguinte maneira.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 2"  
```  

Se você quiser criar um conjunto com capacidade de alternar com um único membro da equipe para que você possa adicionar um membro da equipe em um momento posterior, você deverá usar o parâmetro EnableEmbeddedTeaming.

```  
New-VMSwitch -Name TeamedvSwitch -NetAdapterName "NIC 1" -EnableEmbeddedTeaming $true  
```  

### <a name="adding-or-removing-a-set-team-member"></a>Adicionar ou remover um membro da equipe de conjunto

O **Set-VMSwitchTeam** comando inclui o **NetAdapterName** opção. Para alterar os membros da equipe em uma equipe de conjunto, insira a lista desejada dos membros da equipe após o **NetAdapterName** opção. Se **TeamedvSwitch** foi criado originalmente com NIC 1 e 2 da NIC, em seguida, o comando de exemplo a seguir exclui conjunto membro da equipe "NIC 2" e adiciona o novo membro da equipe conjunto "NIC 3".
  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -NetAdapterName "NIC 1","NIC 3"  
```  

### <a name="removing-a-set-team"></a>Removendo uma equipe de conjunto

Você pode remover uma equipe de conjunto removendo o comutador Virtual Hyper-V que contém o conjunto de equipe.  Use o tópico [Remove-VMSwitch](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/remove-vmswitch) para obter informações sobre como remover o comutador Virtual do Hyper-V. O exemplo a seguir remove um comutador Virtual chamado **SETvSwitch**.

```  
Remove-VMSwitch "SETvSwitch"  
```  

### <a name="changing-the-load-distribution-algorithm-for-a-set-team"></a>Alterando o algoritmo de distribuição de carga para uma equipe de conjunto

O **Set-VMSwitchTeam** cmdlet tem um **LoadBalancingAlgorithm** opção. Essa opção usa um dos dois valores possíveis: **HyperVPort** ou **dinâmico**. Para definir ou alterar o algoritmo de distribuição de carga para uma equipe switch incorporado, use essa opção. 

No exemplo a seguir, chamado de VMSwitchTeam **TeamedvSwitch** usa o **dinâmico** algoritmo de balanceamento de carga.  
```  
Set-VMSwitchTeam -Name TeamedvSwitch -LoadBalancingAlgorithm Dynamic  
```  
### <a name="affinitizing-virtual-interfaces-to-physical-team-members"></a>Ajustar as interfaces virtuais aos membros da equipe físico

CONJUNTO permite que você criar uma afinidade entre uma interface virtual \(, ou seja, porta de comutador Virtual Hyper-V\) e uma das NICs físicas na equipe. 

Por exemplo, se você criar dois hospedam vNICs para SMB\-direto, como na seção [criar um comutador Virtual do Hyper-V com vNICs RDMA e SET](#bkmk_set-rdma), você pode garantir que dois vNICs usar diferentes membros da equipe. 

Adicionando ao script nesta seção, você pode usar os seguintes comandos do Windows PowerShell.

    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_1 –ManagementOS –PhysicalNetAdapterName “SLOT 2”
    Set-VMNetworkAdapterTeamMapping -VMNetworkAdapterName SMB_2 –ManagementOS –PhysicalNetAdapterName “SLOT 3”

Este tópico é examinado em mais detalhes na seção 4.2.5 a [NIC do Windows Server 2016 e Switch Embedded Teaming guia do usuário](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0).
