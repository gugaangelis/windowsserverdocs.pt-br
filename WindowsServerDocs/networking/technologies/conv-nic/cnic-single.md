---
title: Configuração de NIC convergida com um único adaptador de rede
description: Neste tópico, fornecemos as instruções para configurar o NIC convergida com uma única NIC no host do Hyper-V.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: 93d317534af46c87c4b2e874a5a5475687e2efa0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447060"
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configuração de NIC convergida com um único adaptador de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, fornecemos as instruções para configurar o NIC convergida com uma única NIC no host do Hyper-V.

A configuração de exemplo neste tópico descreve dois hosts Hyper-V, **um Host Hyper-V**, e **Hyper-V Host B**. Ambos os hosts têm uma NIC física única (pNIC) instalada e as NICs estão conectadas a um topo de rack \(ToR\) comutador físico. Além disso, os hosts estão localizados na mesma sub-rede, que é 192.168.1.x/24.

![Hosts do Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Etapa 1. Testar a conectividade entre a origem e destino

Certifique-se de que o físico NIC pode se conectar ao host de destino. Esse teste demonstra a conectividade com o uso de camada 3 \(L3\) – ou a camada IP –, bem como a camada 2 \(L2\).

1. Exiba as propriedades do adaptador de rede.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Resultados:** _  


   | Nome |    InterfaceDescription     | ifIndex | Status |    MacAddress     | LinkSpeed |
   |------|-----------------------------|---------|--------|-------------------|-----------|
   |  M1  | Mellanox ConnectX-3 Pro... |    4    |   Para cima   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

2. Exiba as propriedades do adaptador adicionais, incluindo o endereço IP.

   ```PowerShell
   Get-NetAdapter M1 | fl *
   ```

   _**Resultados:** _

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

## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Etapa 2. Certifique-se de origem e destino podem se comunicar

Nesta etapa, usamos o **Test-NetConnection** comando do Windows PowerShell, mas se você pode usar o **ping** comando se você preferir. 

>[!TIP]
>Se você tiver certeza de que os hosts podem se comunicar entre si, você pode ignorar esta etapa.

1. Verifique se a comunicação bidirecional.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados:** _


   |        Parâmetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      |     M1      |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    True     |
   | PingReplyDetails \(RTT\) |    0 ms     |

   ---

   Em alguns casos, você talvez precise desabilitar o Firewall do Windows com segurança avançada para executar com êxito nesse teste. Se você desabilitar o firewall, mantêm a segurança em mente e certifique-se de que sua configuração atende aos requisitos de segurança da sua organização.

2. Desabilite todos os perfis de firewall.

   ```PowerShell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. Depois de desabilitar os perfis de firewall, teste a conexão novamente. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados:** _


   |        Parâmetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      InterfaceAlias      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    False    |
   | PingReplyDetails \(RTT\) |    0 ms     |

   ---



## <a name="step-3-optional-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Etapa 3. (Opcional) Configurar as IDs de VLAN para NICs instaladas em seus hosts do Hyper-V

Várias configurações de rede faça uso de VLANs e se você estiver planejando usar VLANs em sua rede, você deve repetir o teste anterior com VLANs configurados. Além disso, se você estiver planejando usar RoCE para serviços RDMA, você deve habilitar VLANs.

Para esta etapa, as NICs estão em **acesso** modo. No entanto, quando você cria um comutador Virtual do Hyper-V \(vSwitch\) neste documento, as propriedades VLAN são aplicadas no nível de porta vSwitch. 

Como um comutador pode hospedar várias VLANs, é necessário para o topo de Rack \(ToR\) comutador físico para ter a porta que o host está conectado ao configurado no modo de tronco.

>[!NOTE]
>Consulte a documentação do comutador ToR para obter instruções sobre como configurar o modo de tronco no comutador.

A imagem a seguir mostra dois hosts Hyper-V, cada um com um adaptador de rede física, e cada um configurado para se comunicar na VLAN 101.

![Configurar redes locais virtuais](../../media/Converged-NIC/2-single-configure-vlans.jpg)


>[!IMPORTANT]
>Execute esse procedimento em servidores de local e de destino. Se o servidor de destino não está configurado com a mesma ID de VLAN como o servidor local, os dois não podem se comunicar.


1. Configure a ID de VLAN para NICs instaladas em seus hosts do Hyper-V.

   >[!IMPORTANT]
   >Não execute esse comando se você estiver conectado ao host remotamente através desta interface, porque isso resulta na perda do acesso ao host.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
   ```

   _**Resultados:** _


   | Nome | DisplayName | DisplayValue | RegistryKeyword | RegistryValue |
   |------|-------------|--------------|-----------------|---------------|
   |  M1  |   ID DA VLAN   |     101      |     VlanID      |     {101}     |

   ---

2. Reinicie o adaptador de rede para aplicar o ID. VLAN

   ```PowerShell
   Restart-NetAdapter -Name "M1"
   ```

3. Verificar o Status é **backup**.

   ```PowerShell
   Get-NetAdapter -Name "M1"
   ```

   _**Resultados:** _


   | Nome |          InterfaceDescription           | ifIndex | Status |    MacAddress     | LinkSpeed |
   |------|-----------------------------------------|---------|--------|-------------------|-----------|
   |  M1  | Ethernet de Mellanox ConnectX-3 Pro Ada... |    4    |   Para cima   | 7C-FE-90-93-8F-A1 |  40 Gbps  |

   ---

   >[!IMPORTANT]
   >Pode levar vários segundos para o dispositivo reiniciar e se tornar disponíveis na rede. 

4. Verifique a conectividade.<p>Se a conectividade falhar, inspecione o participação de configuração ou o destino de VLAN na mesma VLAN do comutador. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

## <a name="step-4-configure-quality-of-service-qos"></a>Etapa 4. Configurar qualidade de serviço \(QoS\)

>[!NOTE]
>Você deve executar todas as seguintes etapas de configuração do DCB e QoS em todos os hosts que se destinam a se comunicar entre si.

1. Instalar o Data Center Bridging \(DCB\) em cada um dos seus hosts Hyper-V.

   - **Opcional** para configurações de rede que usam iWarp para serviços RDMA.
   - **Exigido** para as configurações de rede que usam RoCE \(qualquer versão\) para serviços RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

2. Defina as políticas de QoS para SMB Direct:

   - **Opcional** para as configurações de rede que usam iWarp.
   - **Necessário** para as configurações de rede que usam RoCE.

   No comando de exemplo a seguir, o valor "3" é arbitrário. Você pode usar qualquer valor entre 1 e 7 desde que você usar consistentemente o mesmo valor em toda a configuração das políticas de QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Resultados:** _


   |   Parâmetro    |          Valor           |
   |----------------|--------------------------|
   |      Nome      |           SMB            |
   |     Proprietário      | Política de grupo \(máquina\) |
   | NetworkProfile |           Todas            |
   |   Precedência   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | PriorityValue  |            3             |

   ---

3. Para implantações de RoCE, ative **controle de fluxo de prioridade** para o tráfego SMB, que não é necessário para iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Resultados:** _


   | Priority | Enabled | PolicySet | IfIndex | IfAlias |
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

   - **Não é necessário** para as configurações de rede que usam iWarp.
   - **Necessário** para as configurações de rede que usam RoCE.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "M1"
   Get-NetAdapterQos -Name "M1"
   ```

   _**Resultados:** _

   **Nome**: M1  
   **Habilitado**: True  

   _**Recursos:** _   


   |      Parâmetro      |   Hardware   |   Atual    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Nenhuma     |     Nenhuma     |
   | NumTCs(Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   ---

   _**OperationalTrafficClasses:** _ 


   | TC | TSA | Largura de banda | Prioridades |
   |----|-----|-----------|------------|
   | 0  | ETS |    70%    |  0-2,4-7   |
   | 1  | ETS |    30%    |     3      |

   ---

   _**OperationalFlowControl:** _  

   Prioridade 3 habilitado  

   _**OperationalClassifications:** _  


   | Protocol  | Porta/tipo | Priority |
   |-----------|-----------|----------|
   |  Padrão  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

   ---

5. Reservar uma porcentagem da largura de banda para SMB Direct \(RDMA\).

    Neste exemplo, uma reserva de largura de banda de 30% é usada. Você deve selecionar um valor que representa o que você espera que seu tráfego de armazenamento requer. 

   ```PowerShell
   New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS
   ```

   _**Resultados:** _


   | Nome | Algoritmo | Bandwidth(%) | Priority | PolicySet | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      30      |    3     |  Global   | &nbsp;  | &nbsp;  |

   ---                                      

6. Exiba as configurações de reserva de largura de banda.  

   ```PowerShell
   Get-NetQosTrafficClass
   ```

   _**Resultados:** _


   |   Nome    | Algoritmo | Bandwidth(%) | Priority | PolicySet | IfIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | [Padrão] |    ETS    |      70      | 0-2,4-7  |  Global   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      30      |    3     |  Global   | &nbsp;  | &nbsp;  |

   ---

## <a name="step-5-optional-resolve-the-mellanox-adapter-debugger-conflict"></a>Etapa 5. (Opcional) Resolver o conflito de depurador de adaptador Mellanox 

Por padrão, ao usar um adaptador Mellanox, o depurador anexado bloqueia NetQos, o que é um problema conhecido. Portanto, se você estiver usando um adaptador de Mellanox e pretende anexar um depurador, use o seguinte comando para resolver esse problema. Essa etapa não será necessária se você não pretende anexar um depurador ou se você não estiver usando um adaptador Mellanox.

   ```PowerShell    
   Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
   ``` 

## <a name="step-6-verify-the-rdma-configuration-native-host"></a>Etapa 6. Verifique a configuração do RDMA (host nativo)

Você deseja garantir que o fabric está configurado corretamente antes da criação de um vSwitch e fazer a transição para o RDMA (NIC convergida). 

A imagem a seguir mostra o estado atual dos hosts do Hyper-V.

![Teste RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

1. Verifique a configuração do RDMA.

   ```PowerShell
   Get-NetAdapterRdma
   ```
   _**Resultados:** _


   | Nome |           InterfaceDescription           | Enabled |
   |------|------------------------------------------|---------|
   |  M1  | Adaptador de Ethernet de Mellanox ConnectX-3 Pro |  True   |

   ---

2. Determinar a **ifIndex** valor do seu adaptador de destino.<p>Você pode usar esse valor nas próximas etapas quando você executa o script que você baixar.

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Resultados:** _ 


   | InterfaceAlias | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |       M2       |       14       | {192.168.1.5} |

   ---

3. Baixe o [DiskSpd.exe utilitário](https://aka.ms/diskspd) e extraia-o em c:\test.\.

4. Baixe o [script do powershell Test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) para uma pasta de teste em sua unidade local, por exemplo, C:\TEST\.

5. Execute o **Rdma.ps1 teste** script do PowerShell para passar o valor de ifIndex para o script, junto com o endereço IP do adaptador remoto na mesma VLAN.<p>Neste exemplo, o script passa a **ifIndex** valor de 14 no endereço IP de adaptador de rede remota 192.168.1.5.

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
   >Se o tráfego RDMA falhar, para o caso de RoCE especificamente, consulte sua configuração de ToR Switch para definições de PFC/ETS apropriadas que devem corresponder as configurações de Host. Consulte a seção de QoS neste documento para valores de referência.

## <a name="step-7-remove-the-access-vlan-setting"></a>Etapa 7. Remova a configuração de VLAN de acesso

Em preparação para a criação de Hyper-V switch, você deve remover as configurações de VLAN instalado acima.  

1. Remova a configuração de VLAN de acesso a NIC física para impedir que a NIC de identificação automática o tráfego de saída com a ID de VLAN incorreta.<p>Remover essa configuração também impede a filtragem de tráfego de entrada que não coincide com a ID da VLAN de acesso.

   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
   ```    

2. Confirme se o **VlanID configuração** mostra o valor da ID de VLAN como zero.

   ```PowerShell    
   Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
   ```  


## <a name="step-8-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Etapa 8. Criar um vSwitch do Hyper-V em seus hosts do Hyper-V

A imagem a seguir ilustra o Host Hyper-V 1 com um vSwitch.

![Criar um comutador Virtual do Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


1. Criar um vSwitch externo do Hyper-V no Hyper-V no Host do Hyper-V A. <p>Neste exemplo, o comutador é denominado VMSTEST. Além disso, o parâmetro **AllowManagementOS** cria uma vNIC do host que herda os endereços MAC e IP da NIC físico.

   ```PowerShell
   New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true
   ```
   _**Resultados:** _


   |  Nome   | SwitchType |      NetAdapterInterfaceDescription      |
   |---------|------------|------------------------------------------|
   | VMSTEST |  Externo  | Adaptador de Ethernet de Mellanox ConnectX-3 Pro |

   ---

2. Exiba as propriedades do adaptador de rede.

   ```PowerShell
   Get-NetAdapter | ft -AutoSize
   ```

   _**Resultados:** _


   |         Nome          |        InterfaceDescription         | ifIndex | Status |    MacAddress     | LinkSpeed |
   |-----------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet \(VMSTEST\) | Adaptador de Ethernet Virtual do Hyper-V #2 |   27    |   Para cima   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---

3. Gerencie o vNIC do host em uma das duas maneiras. 

   - **NetAdapter** exibição opera com base no "vEthernet \(VMSTEST\)" nome. Nem todas as propriedades do adaptador de rede exibem nesta exibição.
   - **VMNetworkAdapter** view descarta o prefixo "vEthernet" e simplesmente usa o nome do vmswitch. (Recomendado) 

   ```PowerShell
   Get-VMNetworkAdapter –ManagementOS | ft -AutoSize
   ```

   _**Resultados:** _


   |         Nome         | IsManagementOs |        VMName        |  SwitchName  | MacAddress | Status | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-External-Switch |      True      | CORP-External-Switch | 001B785768AA |    {Ok}    | &nbsp; |             |
   |       VMSTEST        |      True      |       VMSTEST        | E41D2D074071 |    {Ok}    | &nbsp; |             |

   ---

4. Teste a conexão.

   ```Powershell    
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados:** _ 

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

5. Atribuir e exibir as configurações de VLAN do adaptador de rede.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```    

   _**Resultados:** _


   | VMName | VMNetworkAdapterName |  Modo  | VlanList |
   |--------|----------------------|--------|----------|
   | &nbsp; |       VMSTEST        | Acesso |   101    |

   ---  

6. Teste a conexão.<p>Pode levar alguns segundos para ser concluída antes que você pode fazer um ping bem-sucedido o outro adaptador.  

   ```PowerShell    
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados:** _

   ```
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-9-test-hyper-v-virtual-switch-rdma-mode-2"></a>Etapa 9. Teste do Hyper-V Virtual Switch RDMA (modo 2)

A imagem a seguir ilustra o estado atual de seus hosts do Hyper-V, incluindo o vSwitch no Host Hyper-V 1.

![Testar o comutador Virtual do Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


1. Defina a prioridade de marcação na vNIC do host.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
   ```  

   _**Resultados:** _

    Nome: VMSTEST IeeePriorityTag: Ativado


2. Exiba informações de RDMA do adaptador de rede. 

   ```PowerShell
   Get-NetAdapterRdma
   ```   

   _**Resultados:** _


   |         Nome          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST\) | Adaptador de Ethernet Virtual do Hyper-V #2 |  False  |

   ---

   >[!NOTE]
   >Se o parâmetro **Enabled** tem o valor **falso**, isso significa que o RDMA não está habilitado.


3. Exiba as informações de adaptador de rede.

   ```PowerShell
   Get-NetAdapter
   ```

   _**Resultados:** _   


   |        Nome         |        InterfaceDescription         | ifIndex | Status |    MacAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | Adaptador de Ethernet Virtual do Hyper-V #2 |   27    |   Para cima   | E4-1D-2D-07-40-71 |  40 Gbps  |

   ---


4. Habilite o RDMA na vNIC do host.

   ```PowerShell
   Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"
   ```

   _**Resultados:** _


   |         Nome          |        InterfaceDescription         | Enabled |
   |-----------------------|-------------------------------------|---------|
   | vEthernet \(VMSTEST\) | Adaptador de Ethernet Virtual do Hyper-V #2 |  True   |

   ---

   >[!NOTE]
   >Se o parâmetro **Enabled** tem o valor **verdadeiro**, isso significa que o RDMA é habilitado.

5. Execute teste de tráfego RDMA.

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

A linha final nessa saída, "teste de tráfego RDMA bem-sucedida: Tráfego RDMA foi enviado para 192.168.1.5,"mostra que você configurou com êxito NIC convergida no adaptador.

## <a name="related-topics"></a>Tópicos relacionados
- [Configuração do NIC agrupado NIC convergida](cnic-datacenter.md)
- [Configuração de comutador físico para NIC convergida](cnic-app-switch-config.md)
- [Solução de problemas convergido configurações de NIC](cnic-app-troubleshoot.md)
