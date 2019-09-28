---
title: Configuração de NIC convergida com um único adaptador de rede
description: Neste tópico, fornecemos as instruções para configurar a NIC convergida com uma única NIC em seu host Hyper-V.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 2ad7592fd9faf1e92893e6271daabdad907d3aaa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405793"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configuração de NIC convergida com um único adaptador de rede

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste tópico, fornecemos as instruções para configurar a NIC convergida com uma única NIC em seu host Hyper-V.

A configuração de exemplo neste tópico descreve dois hosts Hyper-v, **o host Hyper-v A e o** **host Hyper-v B**. Ambos os hosts têm uma única NIC física (pNIC) instalada e as NICs estão conectadas a uma parte superior do rack \(ToR @ no__t-1 comutador físico. Além disso, os hosts estão localizados na mesma sub-rede, que é 192.168.1. x/24.

![Hosts do Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Etapa 1. Testar a conectividade entre a origem e o destino

Verifique se a NIC física pode se conectar ao host de destino. Este teste demonstra a conectividade usando a camada 3 \(L3 @ no__t-1-ou a camada de IP, bem como a camada 2 \(L2 @ no__t-3.

1. Exiba as propriedades do adaptador de rede.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Da**_  


   | Nome |    InterfaceDescription     | IfIndex | Status |    macAddress     | LinkSpeed |
   |------|-----------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX-3 Pro... |    4    |   Para cima   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

2. Exiba as propriedades de adaptador adicionais, incluindo o endereço IP.

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**Da**_

   ```PowerShell   
    MacAddress   : 7C-FE-90-93-8F-A1
    Status   : Up
    LinkSpeed: 40 Gbps
    MediaType: 802.3
    PhysicalMediaType: 802.3
    AdminStatus  : Up
    MediaConnectionState : Connected
    DriverInformation: Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60
    DriverFileName   : mlx4eth63.sys
    NdisVersion  : 6.60
    ifOperStatus : Up
    ifAlias  : M1
    InterfaceAlias   : M1
    ifIndex  : 4
    ifDesc   : Mellanox ConnectX-3 Pro Ethernet Adapter
    ifName   : ethernet_32773
    DriverVersion: 5.25.12665.0
    LinkLayerAddress : 7C-FE-90-93-8F-A1
    Caption  :
    Description  :
    ElementName  :
    InstanceID   : {39B58B4C-8833-4ED2-A2FD-E105E7146D43}
    CommunicationStatus  :
    DetailedStatus   :
    HealthState  :
    InstallDate  :
    Name : M1
    OperatingStatus  :
    OperationalStatus:
    PrimaryStatus:
    StatusDescriptions   :
    AvailableRequestedStates :
    EnabledDefault   : 2
    EnabledState : 5
    OtherEnabledState:
    RequestedState   : 12
    TimeOfLastStateChange:
    TransitioningToState : 12
    AdditionalAvailability   :
    Availability :
    CreationClassName: MSFT_NetAdapter
   ``` 

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Etapa 2. Garantir que a origem e o destino possam se comunicar

Nesta etapa, usamos o comando **Test-NetConnect** do Windows PowerShell, mas se você preferir, poderá usar o comando **ping** . 

>[!TIP]
>Se você tiver certeza de que seus hosts podem se comunicar entre si, poderá ignorar esta etapa.

1. Verifique a comunicação bidirecional.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Da**_


   |        Parâmetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      Aliasdeinterface      |     M1      |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    True     |
   | RTT \(de PingReplyDetails\) |    0 ms     |

   ---

   Em alguns casos, talvez seja necessário desabilitar o Firewall do Windows com segurança avançada para executar esse teste com êxito. Se você desabilitar o firewall, mantenha a segurança em mente e verifique se sua configuração atende aos requisitos de segurança de sua organização.

2. Desabilitar todos os perfis de firewall.

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. Depois de desabilitar os perfis de firewall, teste a conexão novamente. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Da**_


   |        Parâmetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      Aliasdeinterface      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | RTT \(de PingReplyDetails\) |    0 ms     |

   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Etapa 3. Adicional Configurar as IDs de VLAN para NICs instaladas em seus hosts Hyper-V

Muitas configurações de rede fazem uso de VLANs e, se você estiver planejando usar VLANs em sua rede, deverá repetir o teste anterior com VLANs configuradas. Além disso, se você estiver planejando usar o RoCE para serviços RDMA, deverá habilitar VLANs.

Para esta etapa, as NICs estão no modo de **acesso** . No entanto, quando você cria um comutador \(virtual do Hyper-V vSwitch\) posteriormente neste guia, as propriedades de VLAN são aplicadas no nível de porta vSwitch. 

Como uma opção pode hospedar várias VLANs, é necessário que a parte superior da chave física \(ToR\) do rack tenha a porta que o host está conectado para ser configurada no modo de tronco.

>[!NOTE]
>Consulte a documentação do comutador ToR para obter instruções sobre como configurar o modo de tronco no comutador.

A imagem a seguir mostra dois hosts Hyper-V, cada um com um adaptador de rede física e cada um configurado para se comunicar na VLAN 101.

![Configurar redes de área local virtual](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>Execute isso nos servidores local e de destino. Se o servidor de destino não estiver configurado com a mesma ID de VLAN que o servidor local, os dois não poderão se comunicar.


1. Configure a ID de VLAN para NICs instaladas em seus hosts Hyper-V.

   >[!IMPORTANT]
   >Não execute esse comando se você estiver conectado ao host remotamente por essa interface, pois isso resultará em perda de acesso ao host.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**Da**_


   | Nome | DisplayName | TipoDeExibição | RegistryKeyword | Registrovalue |
   |------|-------------|--------------|-----------------|---------------|
   |  M1  |   ID DA VLAN   |     101      |     VlanID      |     {101}     |

   ---

2. Reinicie o adaptador de rede para aplicar a ID de VLAN.

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. Verifique se o status está **ativo**.

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```

   _**Da**_


   | Nome |          InterfaceDescription           | IfIndex | Status |    macAddress     | LinkSpeed |
   |------|-----------------------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX-3 pro Ethernet Ada... |    4    |   Para cima   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >Pode levar vários segundos para o dispositivo reiniciar e ficar disponível na rede. 

4. Verifique a conectividade.<p>Se a conectividade falhar, inspecione a configuração do switch VLAN ou a participação de destino na mesma VLAN. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

## <a name="step-4-configure-quality-of-service-qos"></a>Etapa 4. Configurar a qualidade do \(serviço QoS\)

>[!NOTE]
>Você deve executar todas as seguintes etapas de configuração de DCB e QoS em todos os hosts que se destinam a se comunicar entre si.

1. Instale o Data Center \(Bridge\) DCB em cada um dos hosts do Hyper-V.

   - **Opcional** para configurações de rede que usam iWarp para serviços RDMA.
   - **Necessário** para configurações de rede que usam \(RoCE qualquer\) versão para serviços RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. Defina as políticas de QoS para o SMB – Direct:

   - **Opcional** para configurações de rede que usam iWarp.
   - **Necessário** para configurações de rede que usam RoCE.

   No comando de exemplo abaixo, o valor "3" é arbitrário. Você pode usar qualquer valor entre 1 e 7, desde que você use consistentemente o mesmo valor em toda a configuração de políticas de QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Da**_


   |   Parâmetro    |          Valor           |
   |----------------|--------------------------|
   |      Nome      |           SMB            |
   |     Proprietário      | Máquina \(política de grupo\) |
   | NetworkProfile |           Todas            |
   |   Precedência   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | Prioridadevalue  |            3             |

   ---

3. Para implantações de RoCE, ative o **controle de fluxo de prioridade** para o tráfego SMB, o que não é necessário para iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Da**_


   | Priority | Enabled | Política de | IfIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    1     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    2     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    3     |  True   |  Global   | &nbsp;  | &nbsp;  |
   |    4     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    5     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    6     |  False  |  Global   | &nbsp;  | &nbsp;  |
   |    7     |  False  |  Global   | &nbsp;  | &nbsp;  |

   ---

4. Habilite a QoS para os adaptadores de rede local e de destino.

   - **Não é necessário** para configurações de rede que usam iWarp.
   - **Necessário** para configurações de rede que usam RoCE.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**Da**_

   **Nome**: M1  
   **Habilitado**: True  

   _**Técnicas**_   


   |      Parâmetro      |   Hardware   |   Atual    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | Sem suporte | Sem suporte |
   |     DcbxSupport     |     Nenhuma     |     Nenhuma     |
   | NumTCs (Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses:**_ 


   | TCP | TSA | Larga | Suas |
   |----|-----|-----------|------------|
   | 0  | ETS |    70%    |  0-2, 4-7   |
   | 1  | ETS |    30%    |     3      |

   ---

   _**OperationalFlowControl:**_  

   Prioridade 3 habilitada  

   _**OperationalClassifications:**_  


   | Protocol  | Porta/tipo | Priority |
   |-----------|-----------|----------|
   |  Padrão  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

5. Reserve um percentual da largura de banda para SMB Direct \(RDMA @ no__t-1.

    Neste exemplo, uma reserva de largura de banda de 30% é usada. Você deve selecionar um valor que represente o que você espera que seu tráfego de armazenamento exija. 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**Da**_


   | Nome | Algoritmo | Largura de banda (%) | Priority | Política de | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      30      |    3     |  Global   | &nbsp;  | &nbsp;  |

   ---                                      

6. Exibir as configurações de reserva de largura de banda.  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**Da**_


   |   Nome    | Algoritmo | Largura de banda (%) | Priority | Política de | IfIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | Os |    ETS    |      70      | 0-2, 4-7  |  Global   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      30      |    3     |  Global   | &nbsp;  | &nbsp;  |

   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>Etapa 5. Adicional Resolver o conflito do depurador do adaptador Mellanox 

Por padrão, ao usar um adaptador Mellanox, o depurador anexado bloqueia NetQos, que é um problema conhecido. Portanto, se você estiver usando um adaptador de Mellanox e pretender anexar um depurador, use o seguinte comando resolver esse problema. Essa etapa não será necessária se você não pretender anexar um depurador ou se não estiver usando um adaptador Mellanox.

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>Etapa 6. Verificar a configuração de RDMA (host nativo)

Você deseja garantir que a malha esteja configurada corretamente antes de criar um vSwitch e fazer a transição para RDMA (NIC convergida). 

A imagem a seguir mostra o estado atual dos hosts Hyper-V.

![Testar RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. Verifique a configuração de RDMA.

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**Da**_


   | Nome |           InterfaceDescription           | Enabled |
   |------|------------------------------------------|---------|
   |  M1  | Adaptador Mellanox ConnectX-3 pro Ethernet |  True   |

   ---

2. Determine o valor **ifIndex** do adaptador de destino.<p>Você usará esse valor em etapas subsequentes ao executar o script baixado.

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Da**_ 


   | Aliasdeinterface | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |       M2       |       14       | {192.168.1.5} |

   ---

3. Baixe o [utilitário DiskSpd. exe](https://aka.ms/diskspd) e extraia-o em C:\test\.

4. Baixe o [script do PowerShell Test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) para uma pasta de teste em sua unidade local, por exemplo, C:\test @ no__t-1

5. Execute o script do PowerShell **Test-RDMA. ps1** para passar o valor ifIndex para o script, junto com o endereço IP do adaptador remoto na mesma VLAN.<p>Neste exemplo, o script passa o valor **ifIndex** de 14 no endereço IP do adaptador de rede remoto 192.168.1.5.

   ```PowerShell
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\

    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter M2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 662979201 RDMA bytes written per second
    VERBOSE: 37561021 RDMA bytes sent per second
    VERBOSE: 1023098948 RDMA bytes written per second
    VERBOSE: 8901349 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
   ```

   >[!NOTE]
   >Se o tráfego RDMA falhar, para o caso RoCE especificamente, consulte a configuração do comutador ToR para configurações apropriadas de PFC/ETS que devem corresponder às configurações do host. Consulte a seção QoS neste documento para obter os valores de referência.

## <a name="step-7-remove-the-access-vlan-setting"></a>Etapa 7. Remover a configuração de VLAN de acesso

Em preparação para criar a opção Hyper-V, você deve remover as configurações de VLAN instaladas acima.  

1. Remova a configuração de VLAN de acesso da NIC física para impedir que a NIC marcação automaticamente o tráfego de saída com a ID de VLAN incorreta.<p>A remoção dessa configuração também impede que ela filtre o tráfego de entrada que não corresponda à ID de VLAN de acesso.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. Confirme se a **configuração de vlanid** mostra o valor da ID de VLAN como zero.

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Etapa 8. Criar um vSwitch do Hyper-V em seus hosts Hyper-V

A imagem a seguir ilustra o host do Hyper-V 1 com um vSwitch.

![Criar um comutador virtual do Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. Crie um vSwitch Hyper-V externo no Hyper-v no host A do Hyper-V. <p>Neste exemplo, a opção é denominada VMSTEST. Além disso, o parâmetro **AllowManagementOS** cria um host vNIC que herda os endereços MAC e IP da NIC física.

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**Da**_


   |  Nome   | SwitchType |      NetAdapterInterfaceDescription      |
   |---------|------------|------------------------------------------|
   | VMSTEST |  Externo  | Adaptador Mellanox ConnectX-3 pro Ethernet |

   ---

2. Exiba as propriedades do adaptador de rede.

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**Da**_


   |         Nome          |        InterfaceDescription         | IfIndex | Status |    macAddress     | LinkSpeed |
   |-----------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet \(VMSTEST @ no__t-1 | #2 de adaptador Ethernet virtual do Hyper-V |   27    |   Para cima   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---

3. Gerencie o host vNIC de uma das duas maneiras. 

   - A exibição do **netadapter** opera com base no nome "vEthernet \(VMSTEST @ no__t-2". Nem todas as propriedades do adaptador de rede são exibidas nesta exibição.
   - A exibição **VMNetworkAdapter** descarta o prefixo "vEthernet" e simplesmente usa o nome vmswitch. (Recomendado) 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**Da**_


   |         Nome         | IsManagementOs |        VMName        |  SwitchName  | macAddress | Status | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-external-switch |      True      | CORP-external-switch | 001B785768AA |    Problemas    | &nbsp; |             |
   |       VMSTEST        |      True      |       VMSTEST        | E41D2D074071 |    Problemas    | &nbsp; |             |

   ---

4. Teste a conexão.

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**Da**_ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. Atribua e exiba as configurações de VLAN do adaptador de rede.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**Da**_


   | VMName | VMNetworkAdapterName |  Modo  | Vlanlist |
   |--------|----------------------|--------|----------|
   | &nbsp; |       VMSTEST        | Access |   101    |

   ---  

6. Teste a conexão.<p>Pode levar alguns segundos para ser concluído para que você possa executar o ping com êxito no outro adaptador.  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**Da**_

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>Etapa 9. Testar o comutador virtual do Hyper-V RDMA (modo 2)

A imagem a seguir ilustra o estado atual dos hosts do Hyper-V, incluindo o vSwitch no host do Hyper-V 1.

![Testar o comutador virtual do Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. Defina a marcação de prioridade no host vNIC.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  

   _**Da**_

    Nome: VMSTEST IeeePriorityTag : Ativado


2. Exiba as informações de RDMA do adaptador de rede. 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**Da**_


   |         Nome          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST @ no__t-1 | #2 de adaptador Ethernet virtual do Hyper-V |  False  |

   ---

   >[!NOTE]
   >Se o parâmetro **habilitado** tiver o valor **false**, significa que o RDMA não está habilitado.


3. Exiba as informações do adaptador de rede.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Da**_   


   |        Nome         |        InterfaceDescription         | IfIndex | Status |    macAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | #2 de adaptador Ethernet virtual do Hyper-V |   27    |   Para cima   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---


4. Habilite o RDMA no host vNIC.

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**Da**_


   |         Nome          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST @ no__t-1 | #2 de adaptador Ethernet virtual do Hyper-V |  True   |

   ---

   >[!NOTE]
   >Se o parâmetro **habilitado** tiver o valor **true**, significa que o RDMA está habilitado.

5. Executar o teste de tráfego RDMA.

   ```PowerShell    
    C:\TEST\Test-RDMA.PS1 -IfIndex 27 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (VMSTEST) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.1.5, is reachable.
    VERBOSE: Remote IP 192.168.1.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 9162492 RDMA bytes sent per second
    VERBOSE: 938797258 RDMA bytes written per second
    VERBOSE: 34621865 RDMA bytes sent per second
    VERBOSE: 933572610 RDMA bytes written per second
    VERBOSE: 35035861 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.1.5
   ```

A linha final nesta saída, "teste de tráfego RDMA com êxito: O tráfego RDMA foi enviado para 192.168.1.5, "mostra que você configurou com êxito a NIC convergida no adaptador.

## <a name="related-topics"></a>Tópicos relacionados
- [Configuração NIC agrupada NIC convergida](cnic-datacenter.md)
- [Configuração de comutador físico para NIC convergida](cnic-app-switch-config.md)
- [Solucionando problemas de configurações de NIC convergida](cnic-app-troubleshoot.md)
