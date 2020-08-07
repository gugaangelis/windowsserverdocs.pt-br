---
title: Gerenciar a ponte do Data Center (DCB)
description: Este tópico fornece instruções sobre como usar comandos do Windows PowerShell para gerenciar a ponte do Data Center no Windows Server 2016.
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: bc012fd6a685b005b9f7f2b055d49050ef3c009a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971773"
---
# <a name="manage-data-center-bridging-dcb"></a>Gerenciar a ponte do Data Center (DCB)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece instruções sobre como usar comandos do Windows PowerShell para configurar a ponte do Data Center \( DCB \) em um \- adaptador de rede compatível com DCB que está instalado em um computador que executa o Windows Server 2016 ou o Windows 10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Instalar o DCB no Windows Server 2016 ou no Windows 10

Para obter informações sobre os pré-requisitos para usar e como instalar o DCB, consulte [instalar a ponte do Data Center (DCB) no Windows Server 2016 ou Windows 10](dcb-install.md).


## <a name="dcb-configurations"></a>Configurações do DCB

Antes do Windows Server 2016, todas as configurações de DCB foram aplicadas universalmente a todos os adaptadores de rede que suportavam DCB.

No Windows Server 2016, você pode aplicar configurações de DCB ao repositório de política global ou ao repositório de política individual \( s \) . Quando as políticas individuais são aplicadas, elas substituem todas as configurações de política global.

As configurações de classe de tráfego, PFC e atribuição de prioridade de aplicativo no nível do sistema não são aplicadas em adaptadores de rede até que você faça o seguinte.

1. Transforme o bit de DCBX disposto em false

2. Habilite o DCB nos adaptadores de rede. Consulte [habilitar e exibir configurações de DCB em adaptadores de rede](#bkmk_enabledcb).

>[!NOTE]
>Se você quiser configurar o DCB de um comutador por meio de DCBX, consulte [configurações de DCBX](#dcb-configuration-on-network-adapters).

O bit DCBX disposto é descrito na especificação DCB. Se o bit disposto em um dispositivo for definido como true, o dispositivo estará disposto a aceitar configurações de um dispositivo remoto por meio do DCBX. Se o bit disposto em um dispositivo for definido como false, o dispositivo rejeitará todas as tentativas de configuração de dispositivos remotos e impedirá apenas as configurações locais.

Se o DCB não estiver instalado no Windows Server 2016, o valor do bit disposto é irrelevante no que diz respeito ao sistema operacional, pois o sistema operacional não tem configurações locais aplicadas aos adaptadores de rede. Após a instalação do DCB, o valor padrão do bit disposto é true. Esse design permite que os adaptadores de rede mantenham quaisquer configurações que possam ter recebido de seus colegas remotos.

Se um adaptador de rede não oferecer suporte a DCBX, ele nunca receberá configurações de um dispositivo remoto. Ele recebe configurações do sistema operacional, mas somente depois que o bit DCBX disposto é definido como false.

## <a name="set-the-willing-bit"></a>Definir o bit disposto

Para impor as configurações do sistema operacional de classe de tráfego, PFC e atribuição de prioridade de aplicativo em adaptadores de rede, ou simplesmente substituir as configurações de dispositivos remotos \ — se houver algum, você pode executar o comando a seguir.

>[!NOTE]
>Os nomes de comando DCB do Windows PowerShell incluem "QoS" em vez de "DCB" na cadeia de caracteres de nome. Isso ocorre porque o QoS e o DCB são integrados no Windows Server 2016 para fornecer uma experiência de gerenciamento de QoS direta.

```powershell
    Set-NetQosDcbxSetting -Willing $FALSE

    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

Para exibir o estado da configuração de bit dedisposta, você pode usar o seguinte comando:

```powershell
    Get-NetQosDcbxSetting

    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global
```

## <a name="dcb-configuration-on-network-adapters"></a>Configuração do DCB em adaptadores de rede

Habilitar o DCB em um adaptador de rede permite que você veja a configuração propagada de um comutador para o adaptador de rede.

As configurações do DCB incluem as etapas a seguir.

1.  Defina as configurações de DCB no nível do sistema, que inclui:

    a. Gerenciamento de classe de tráfego

    b. Configurações de controle de fluxo prioritário (PFC)

    c. Atribuição de prioridade de aplicativo

    d. Configurações de DCBX

2. Configure o DCB no adaptador de rede.

##  <a name="dcb-traffic-class-management"></a>Gerenciamento de classe de tráfego DCB

A seguir estão exemplos de comandos do Windows PowerShell para o gerenciamento de classe de tráfego.

### <a name="create-a-traffic-class"></a>Criar uma classe de tráfego

Você pode usar o comando **New-NetQosTrafficClass** para criar uma classe de tráfego.

```powershell
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS

    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
```

Por padrão, todos os valores 802.1 p são mapeados para uma classe de tráfego padrão, que tem 100% da largura de banda do link físico. O comando **New-NetQosTrafficClass** cria uma nova classe de tráfego, para a qual qualquer pacote marcado com o valor 4 de prioridade 802.1 p é mapeado. O algoritmo de seleção de transmissão \( TSA \) é ETs e tem 30% da largura de banda.

Você pode criar até sete novas classes de tráfego. Incluindo a classe de tráfego padrão, pode haver no máximo oito classes de tráfego no sistema. No entanto, um adaptador de rede compatível com DCB pode não oferecer suporte a muitas classes de tráfego no hardware. Se você criar mais classes de tráfego do que o pode ser acomodado em um adaptador de rede e habilitar o DCB nesse adaptador de rede, o driver de miniporta relatará um erro ao sistema operacional. O erro é registrado no log de eventos.

A soma das reservas de largura de banda para todas as classes de tráfego criadas não pode exceder 99% da largura de banda. A classe de tráfego padrão sempre tem pelo menos 1% da largura de banda reservada para si mesma.

### <a name="display-traffic-classes"></a>Exibir classes de tráfego

Você pode usar o comando **Get-NetQosTrafficClass** para exibir classes de tráfego.

```powershell
    Get-NetQosTrafficClass

    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global
```

### <a name="modify-a-traffic-class"></a>Modificar uma classe de tráfego

Você pode usar o comando **set-NetQosTrafficClass** para criar uma classe de tráfego.

```powershell
    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50
```

Em seguida, você pode usar o comando **Get-NetQosTrafficClass** para exibir as configurações.

```powershell
    Get-NetQosTrafficClass

    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global
```

Depois de criar uma classe de tráfego, você pode alterar suas configurações de forma independente. As configurações que podem ser alteradas incluem:

1. Alocação de largura de banda (-BandwidthPercentage)

2. TSA (-Algorithm)

3. Mapeamento de prioridade (-Priority)

### <a name="remove-a-traffic-class"></a>Remover uma classe de tráfego

Você pode usar o comando **Remove-NetQosTrafficClass** para excluir uma classe de tráfego.

>[!IMPORTANT]
>Não é possível remover a classe de tráfego padrão.

```powershell
    Remove-NetQosTrafficClass -Name SMB

You can then use the **Get-NetQosTrafficClass** command to view settings.

    Get-NetQosTrafficClass

    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
```

Depois de remover uma classe de tráfego, o valor de 802.1 p mapeado para essa classe de tráfego é remapeado para a classe de tráfego padrão. Qualquer largura de banda reservada para uma classe de tráfego é retornada para a alocação de classe de tráfego padrão quando a classe de tráfego é removida.

## <a name="per-network-interface-policies"></a>Políticas de interface por rede

Todos os exemplos acima definem as políticas globais. Veja a seguir exemplos de como você pode definir e obter políticas por NIC.

O campo "Policyset" muda de global para AdapterSpecific. Quando as políticas AdapterSpecific são mostradas, o índice de interface \( ifIndex \) e o nome de interface \( ifAlias \) também são exibidos.

```powershell
PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       100          0-7              AdapterSpecific  4       M1


PS C:\> New-NetQosTrafficClass -Name SMBGlobal -BandwidthPercentage 30 -Priority 4 -Algorithm ETS

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBGlobal   ETS       30           4                Global


PS C:\> New-NetQosTrafficClass -Name SMBforM1 -BandwidthPercentage 30 -Priority 4 -Algorithm ETS -Interfac


Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
SMBforM1    ETS       30           4                AdapterSpecific  4       M1


PS C:\> Get-NetQosTrafficClass

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          Global
SMBGlobal   ETS       30           4                Global


PS C:\> Get-NetQosTrafficClass -InterfaceAlias M1

Name        Algorithm Bandwidth(%) Priority         PolicySet        IfIndex IfAlias
----        --------- ------------ --------         ---------        ------- -------
[Default]   ETS       70           0-3,5-7          AdapterSpecific  4       M1
SMBforM1    ETS       30           4                AdapterSpecific  4       M1

```

## <a name="priority-flow-control-settings"></a>Configurações de controle de fluxo de prioridade:

A seguir estão exemplos de comando para configurações de controle de fluxo de prioridade. Essas configurações também podem ser especificadas para adaptadores individuais.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Habilitar e exibir o controle de fluxo de prioridade para casos de uso globais e específicos de interface

```powershell
PS C:\> Enable-NetQosFlowControl -Priority 4
PS C:\> Enable-NetQosFlowControl -Priority 3 -InterfaceAlias M1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          True       Global
5          False      Global
6          False      Global
7          False      Global

PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          True       AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1
```

### <a name="disable-priority-flow-control-global-and-interface-specific"></a>Desabilitar o controle de fluxo de prioridade (global e específico da interface)

```powershell
PS C:\> Disable-NetQosFlowControl -Priority 4
PS C:\> Disable-NetQosFlowControl -Priority 3 -InterfaceAlias m1
PS C:\> Get-NetQosFlowControl

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      Global
1          False      Global
2          False      Global
3          False      Global
4          False      Global
5          False      Global
6          False      Global
7          False      Global

PS C:\> Get-NetQosFlowControl -InterfaceAlias M1

Priority   Enabled    PolicySet        IfIndex IfAlias
--------   -------    ---------        ------- -------
0          False      AdapterSpecific  4       M1
1          False      AdapterSpecific  4       M1
2          False      AdapterSpecific  4       M1
3          False      AdapterSpecific  4       M1
4          False      AdapterSpecific  4       M1
5          False      AdapterSpecific  4       M1
6          False      AdapterSpecific  4       M1
7          False      AdapterSpecific  4       M1
```

##  <a name="application-priority-assignment"></a>Atribuição de prioridade de aplicativo

Veja a seguir exemplos de atribuição de prioridade.

### <a name="create-qos-policy"></a>Criar política de QoS

```powershell
PS C:\> New-NetQosPolicy -Name "SMB Policy" -SMB -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
Template       : SMB
PriorityValue  : 4
```

O comando anterior cria uma nova política para SMB. – O SMB é um filtro de caixa de entrada que corresponde à porta TCP 445 (reservada para SMB). Se um pacote for enviado para a porta TCP 445, ele será marcado pelo sistema operacional com o valor de 4 do 802.1 p antes que o pacote seja passado para um driver de miniporta de rede.

Além de – SMB, outros filtros padrão incluem – iSCSI (correspondência da porta TCP 3260),-NFS (correspondência da porta TCP 2049),-LiveMigration (correspondência da porta TCP 6600),-FCOE (Matching EtherType 0x8906) e – NetworkDirect.

NetworkDirect é uma camada abstrata que criamos sobre qualquer implementação de RDMA em um adaptador de rede. – NetworkDirect deve ser seguido por uma porta direta da rede.

Além dos filtros padrão, você pode classificar o tráfego pelo nome do executável do aplicativo (como no primeiro exemplo abaixo) ou por endereço IP, porta ou protocolo (como mostrado no segundo exemplo):

**Por nome do executável**

```powershell
PS C:\> New-NetQosPolicy -Name background -AppPathNameMatchCondition "C:\Program files (x86)\backup.exe" -PriorityValue8021Action 1

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1
```

**Por porta de endereço IP ou protocolo**

```powershell
PS C:\> New-NetQosPolicy -Name "Network Management" -IPDstPrefixMatchCondition 10.240.1.0/24 -IPProtocolMatchCondition both -NetworkProfile all -PriorityValue8021Action 7

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7
```

### <a name="display-qos-policy"></a>Exibir política de QoS

```powershell
PS C:\> Get-NetQosPolicy

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
Template       : SMB
JobObject      :
PriorityValue  : 4
```

### <a name="modify-qos-policy"></a>Modificar política de QoS

Você pode modificar as políticas de QoS, conforme mostrado abaixo.

```powershell
PS C:\> Set-NetQosPolicy -Name "Network Management" -IPSrcPrefixMatchCondition 10.235.2.0/24 -IPProtocolMatchCondition both -PriorityValue8021Action 7
PS C:\> Get-NetQosPolicy

Name           : Network Management
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
IPProtocol     : Both
IPSrcPrefix    : 10.235.2.0/24
IPDstPrefix    : 10.240.1.0/24
PriorityValue  : 7
```

### <a name="remove-qos-policy"></a>Remover política de QoS

```powershell
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="dcb-configuration-on-network-adapters"></a>Configuração do DCB em adaptadores de rede

A configuração do DCB em adaptadores de rede é independente da configuração do DCB no nível do sistema descrito acima.

Independentemente de o DCB estar instalado no Windows Server 2016, você sempre poderá executar os comandos a seguir.

Se você configurar o DCB de um comutador e confiar no DCBX para propagar as configurações para adaptadores de rede, poderá examinar quais configurações são recebidas e impostas nos adaptadores de rede do lado do sistema operacional depois de habilitar o DCB nos adaptadores de rede.

###  <a name="enable-and-display-dcb-settings-on--network-adapters"></a><a name="bkmk_enabledcb"></a>Habilitar e exibir configurações de DCB em adaptadores de rede

```powershell
PS C:\> Enable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos

Name                       : M1
Enabled                    : True
Capabilities               :                       Hardware     Current
                                                   --------     -------
                             MacSecBypass        : NotSupported NotSupported
                             DcbxSupport         : None         None
                             NumTCs(Max/ETS/PFC) : 8/8/8        8/8/8

OperationalTrafficClasses  : TC TSA    Bandwidth Priorities
                             -- ---    --------- ----------
                              0 ETS    70%       0-3,5-7
                              1 ETS    30%       4

OperationalFlowControl     : All Priorities Disabled
OperationalClassifications : Protocol  Port/Type Priority
                             --------  --------- --------
                             Default             1
```

### <a name="disable-dcb-on-network-adapters"></a>Desabilitar DCB em adaptadores de rede

```powershell
PS C:\> Disable-NetAdapterQos M1
PS C:\> Get-NetAdapterQos M1

Name         : M1
Enabled      : False
Capabilities :                       Hardware     Current
                                     --------     -------
               MacSecBypass        : NotSupported NotSupported
               DcbxSupport         : None         None
               NumTCs(Max/ETS/PFC) : 8/8/8        0/0/0
```

## <a name="windows-powershell-commands-for-dcb"></a><a name="bkmk_wps"></a>Comandos do Windows PowerShell para DCB

Há comandos DCB do Windows PowerShell para o Windows Server 2016 e o Windows Server 2012 R2. Você pode usar todos os comandos para o Windows Server 2012 R2 no Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandos do Windows PowerShell 2016 do Windows Server para DCB

O tópico a seguir para o Windows Server 2016 fornece descrições e sintaxe de cmdlets do Windows PowerShell para toda a ponte de data center \( DCB a \) qualidade dos \( \) \- cmdlets específicos de QoS de serviço. Ela lista os cmdlets em ordem alfabética com base no verbo no início do cmdlet.

- [Módulo DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Comandos do Windows PowerShell para DCB do Windows Server 2012 R2

O tópico a seguir para o Windows Server 2012 R2 fornece descrições e sintaxe de cmdlets do Windows PowerShell para toda a ponte de data center \( DCB a \) qualidade dos \( \) \- cmdlets específicos de QoS de serviço. Ela lista os cmdlets em ordem alfabética com base no verbo no início do cmdlet.

- [Cmdlets de Qualidade de Serviço (QoS) de Ponte de Datacenter no Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
