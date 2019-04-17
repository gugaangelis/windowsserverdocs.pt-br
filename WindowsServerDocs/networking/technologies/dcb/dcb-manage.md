---
title: Gerenciar Data Center ponte (DCB)
description: Este tópico fornece instruções sobre como usar comandos do Windows PowerShell para gerenciar dados central ponte do Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 1575cc7c-62a7-4add-8f78-e5d93effe93f
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dbe9e5e4af2ddd834b5b8f38e9ffd1b403e92793
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="manage-data-center-bridging-dcb"></a>Gerenciar Data Center ponte (DCB)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico fornece instruções sobre como usar comandos do Windows PowerShell para configurar \(DCB\) ponte do Centro de dados em um adaptador de rede compatíveis com o DCB\ que é instalado em um computador que executa o Windows Server 2016 ou Windows 10.

## <a name="install-dcb-in-windows-server-2016-or-windows-10"></a>Instalar DCB no Windows Server 2016 ou Windows 10

Para obter informações sobre os pré-requisitos para usando e como instalar DCB, consulte [instalar dados central ponte (DCB) no Windows Server 2016 ou o Windows 10](dcb-install.md).


## <a name="dcb-configurations"></a>Configurações de DCB 

Antes do Windows Server 2016, todas as configurações de DCB foi aplicada universalmente para todos os adaptadores de rede que suporte DCB. 

No Windows Server 2016, você pode aplicar configurações DCB ao repositório de política Global ou para Store\(s\) individuais de política. Quando políticas individuais são aplicadas, elas substituirão todas as configurações de política Global.

As configurações de atribuição de prioridade classe, PFC e aplicativo de tráfego no nível do sistema não é aplicada em adaptadores de rede até que você faça o seguinte.

1. Ative o bit de disposto DCBX como false

2. Habilite DCB nos adaptadores de rede. Consulte [habilitar e exibir configurações DCB em adaptadores de rede](#bkmk_enabledcb).

>[!NOTE]
>Se você deseja configurar DCB do switch por meio de DCBX, consulte [configurações DCBX](#BKMK_DCBX_Settings)

O bit disposto DCBX é descrito na especificação DCB. Se o bit disposto em um dispositivo é definido como true, o dispositivo está prestes a aceitar as configurações de um dispositivo remoto por meio de DCBX. Se o bit disposto em um dispositivo é definido como false, o dispositivo rejeitar todas as tentativas de configuração de dispositivos remotos e impor apenas as configurações de locais.

Se DCB não está instalado no Windows Server 2016 o valor do bit disposto é irrelevante quanto o sistema operacional está preocupado porque o sistema operacional não possui locais configurações se aplicam a adaptadores de rede. Após a instalação DCB, o valor padrão do bit disposto é true. Esse design permite que os adaptadores de rede manter qualquer configurações que ele podem ter recebido da seus pares remotos.

Se um adaptador de rede não dá suporte a DCBX, ele nunca receberá configurações de um dispositivo remoto. Ele receberá configurações do sistema operacional, mas somente após o DCBX bit disposto é definido como false.

## <a name="set-the-willing-bit"></a>Definir o bit disposto

Para aplicar configurações do sistema operacional de classe de tráfego, PFC e atribuição de prioridade de aplicativo em adaptadores de rede, ou para simplesmente substituir as configurações de dispositivos remotos \ — se houver alguma \ — você pode executar o comando a seguir.

>[!NOTE]
>Nomes de comando do Windows PowerShell DCB incluem "QoS" em vez de "DCB" na cadeia de caracteres de nome. Isso ocorre porque QoS e DCB são integrados no Windows Server 2016 para fornecer uma experiência perfeita de gerenciamento de QoS.

    
    Set-NetQosDcbxSetting -Willing $FALSE
    
    Confirm
    Are you sure you want to perform this action?
    Set-NetQosDcbxSetting -Willing $false
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
    

Para exibir o estado do bit disposto configuração, você pode usar o seguinte comando:

    
    Get-NetQosDcbxSetting
    
    Willing PolicySetIfIndex IfAlias
    ------- ---------------- -------
    False   Global  
    

## <a name="dcb-configuration-on-network-adapters"></a>Configuração do DCB em adaptadores de rede

Habilitar DCB em um adaptador de rede permite que você veja a configuração propagada de um comutador para o adaptador de rede.

Configurações de DCB incluem as seguintes etapas.

1.  Defina configurações de DCB no nível do sistema, que inclui:

    um. Gerenciamento de classe de tráfego
    
    b. Configurações de controle (PFC) do fluxo de prioridade
    
    c. Atribuição de prioridade de aplicativo
    
    d. Configurações de DCBX

2. Configure DCB no adaptador de rede.



##  <a name="dcb-traffic-class-management"></a>Gerenciamento de classe de tráfego DCB

A seguir estão os comandos do exemplo do Windows PowerShell para gerenciamento de classe de tráfego.

### <a name="create-a-traffic-class"></a>Crie uma classe de tráfego

Você pode usar o **nova NetQosTrafficClass** comando para criar uma classe de tráfego.

    
    New-NetQosTrafficClass -Name SMB -Priority 4 -BandwidthPercentage 30 -Algorithm ETS
    
    Name Algorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ---- --------- ------------ -------- ---------------- -------
    SMB  ETS   30   4Global
      

Por padrão, todos os valores de 802.1p são mapeados para uma classe de tráfego padrão, que tem 100% da largura de banda da conexão física. O **nova NetQosTrafficClass** comando cria uma nova classe de tráfego, para que qualquer pacote é marcado com a prioridade 802.1p valor 4 é mapeado. O algoritmo de seleção de transmissão \(TSA\) é ETS e tem 30% da largura de banda.

Você pode criar até 7 novas classes de tráfego. Incluindo a classe de tráfego padrão, pode haver no máximo 8 classes de tráfego no sistema. No entanto, um adaptador de rede compatíveis com DCB pode não ser compatíveis que muitas classes no hardware de tráfego. Se você criar mais classes de tráfego que podem ser acomodadas em um adaptador de rede e habilitar DCB no adaptador de rede, o driver de miniporta reporta um erro ao sistema operacional. O erro é registrado no log de eventos.

A soma das reservas de largura de banda para todas as classes de tráfego criado não pode exceder 99% da largura de banda. A classe de tráfego padrão sempre tem pelo menos de 1% da largura de banda reservado para si próprio.

### <a name="display-traffic-classes"></a>Exibir Classes de tráfego

Você pode usar o **Get-NetQosTrafficClass** comando para exibir as classes de tráfego.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   70   0-3,5-7  Global
    SMB ETS   30   4Global  
    
### <a name="modify-a-traffic-class"></a>Modificar uma classe de tráfego

Você pode usar o **conjunto NetQosTrafficClass** comando para criar uma classe de tráfego. 

    Set-NetQosTrafficClass -Name SMB -BandwidthPercentage 50

Você pode usar o **Get-NetQosTrafficClass** comando para exibir as configurações.

    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   50   0-3,5-7  Global
    SMB ETS   50   4Global   
    

Depois de criar uma classe de tráfego, você pode alterar suas configurações de forma independente. As configurações que você pode alterar incluem:

1. Largura de banda alocação \(-BandwidthPercentage\)

2. TSA (\-Algorithm\)

3. Prioridade mapeamento \(-Priority\)

### <a name="remove-a-traffic-class"></a>Remover uma classe de tráfego

Você pode usar o **NetQosTrafficClass remover** comando para excluir uma classe de tráfego.

>[!IMPORTANT]
>Você não pode remover a classe de tráfego padrão.


    Remove-NetQosTrafficClass -Name SMB

Você pode usar o **Get-NetQosTrafficClass** comando para exibir as configurações.
    
    Get-NetQosTrafficClass
    
    NameAlgorithm Bandwidth(%) Priority PolicySetIfIndex IfAlias
    ------------- ------------ -------- ---------------- -------
    [Default]   ETS   100  0-7  Global
    

Depois de remover uma classe de tráfego, o valor 802.1p mapeados para que a classe de tráfego é remapeado para a classe de tráfego padrão. Qualquer largura de banda que foi reservada para uma classe de tráfego é retornada para a alocação de classe de tráfego padrão quando a classe de tráfego é removida.

## <a name="per-network-interface-policies"></a>Políticas da Interface por rede

Todos os exemplos acima de definir políticas globais. A seguir estão exemplos de como você pode definir e obter políticas por NIC. 

O campo "PolicySet" muda de Global para AdapterSpecific. Quando AdapterSpecific políticas forem mostradas, o índice de Interface \(ifIndex\) e nome da Interface \(ifAlias\) também são exibidas.

```
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

## <a name="priority-flow-control-settings"></a>Configurações de fluxo de controle de prioridade:

A seguir estão exemplos de comando para configurações de controle de fluxo de prioridade. Essas configurações também podem ser especificadas para adaptadores individuais.

### <a name="enable-and-display-priority-flow-control-for-global-and-interface-specific-use-cases"></a>Casos de uso de habilitar e controle de fluxo de prioridade de exibição para Global e Interface específica

```
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


### <a name="disable-priority-flow-control-global-and-interface-specific"></a>Desabilitar o controle de prioridade de fluxo (Global e específicas para a Interface)

```
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

A seguir estão exemplos de atribuição de prioridade.

### <a name="create-qos-policy"></a>Crie a política de QoS

```
PS C:\> New-NetQosPolicy -Name "SMB Policy" -PriorityValue8021Action 4

Name           : SMB Policy
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
JobObject      :
PriorityValue  : 4

```

O comando anterior cria uma nova política para SMB. – SMB é um filtro de caixa de entrada que corresponde a porta TCP 445 (reservado para SMB). Se um pacote é enviado para a porta TCP 445 que serão marcado pelo sistema operacional com valor 802.1p de 4 antes do pacote é passado para um driver de miniporta de rede.

Além de – SMB, outros filtros padrão incluem – iSCSI (correspondentes a porta TCP 3260), - NFS (correspondente porta TCP 2049), - LiveMigration (correspondente porta TCP 6600), - FCOE (correspondência EtherType 0x8906) e – NetworkDirect.

NetworkDirect é uma camada abstrata que criamos na parte superior de qualquer implementação de RDMA em um adaptador de rede. – NetworkDirect deve ser seguido por uma porta de rede direta.

Além dos filtros padrão, você pode classificar o tráfego por nome do arquivo executável do aplicativo (como o primeiro exemplo abaixo) ou pelo endereço IP, porta ou protocolo (como mostrado no segundo exemplo):

**Pelo nome do executável**

```
PS C:\> New-NetQosPolicy -Name background -AppPathNameMatchCondition "C:\Program files (x86)\backup.exe" -PriorityValue8021Action 1

Name           : background
Owner          : Group Policy (Machine)
NetworkProfile : All
Precedence     : 127
AppPathName    : C:\Program files (x86)\backup.exe
JobObject      :
PriorityValue  : 1

```


**Porta do endereço IP ou protocolo**

```
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

### <a name="display-qos-policy"></a>Exibir política QoS

```
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
JobObject      :
PriorityValue  : 4

```

### <a name="modify-qos-policy"></a>Modificar a política de QoS

Você pode modificar as políticas de QoS conforme mostrado abaixo.


```
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

### <a name="remove-qos-policy"></a>Remover a política de QoS

```
PS C:\> Remove-NetQosPolicy -Name "Network Management"

Confirm
Are you sure you want to perform this action?
Remove-NetQosPolicy -Name "Network Management" -Store GPO:localhost
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y  

```

## <a name="dcb-configuration-on-network-adapters"></a>Configuração do DCB em adaptadores de rede

Configuração do DCB em adaptadores de rede é independente da configuração DCB no nível do sistema descrito acima. 

Independentemente de se DCB está instalado no Windows Server 2016, você sempre pode executar os comandos a seguir. 

Se você configurar DCB em uma chave e depende de DCBX para propagar as configurações para adaptadores de rede, você pode examinar quais configurações são recebidas e impostas em adaptadores de rede do lado do sistema operacional depois de habilitar DCB nos adaptadores de rede.

###  <a name="bkmk_enabledcb"></a>Habilite e exibir configurações DCB em adaptadores de rede

```
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

```
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
## <a name="bkmk_wps"></a>Comandos do Windows PowerShell para DCB

Existem comandos DCB Windows PowerShell para Windows Server 2016 e Windows Server 2012 R2. Você pode usar todos os comandos do Windows Server 2012 R2 no Windows Server 2016.

### <a name="windows-server-2016-windows-powershell-commands-for-dcb"></a>Comandos do Windows Server 2016 Windows PowerShell para DCB

O seguinte tópico para o Windows Server 2016 fornece sintaxe e as descrições do cmdlet do Windows PowerShell para todos os dados central ponte \(DCB\) qualidade de serviço \ cmdlets de \-specific (QoS\). Ela lista os cmdlets em ordem alfabética, com base no verbo no início do cmdlet.

- [Módulo DcbQoS](https://technet.microsoft.com/itpro/powershell/windows/dcbqos/dcbqos)

### <a name="windows-server-2012-r2-windows-powershell-commands-for-dcb"></a>Comandos do Windows Server 2012 R2 Windows PowerShell para DCB

O seguinte tópico para o Windows Server 2012 R2 fornece sintaxe e as descrições do cmdlet do Windows PowerShell para todos os dados central ponte \(DCB\) qualidade de serviço \ cmdlets de \-specific (QoS\). Ela lista os cmdlets em ordem alfabética, com base no verbo no início do cmdlet.

- [Centro de dados ponte (DCB) qualidade de serviço (QoS) Cmdlets do Windows PowerShell](https://technet.microsoft.com/library/hh967440.aspx)
