---
title: Configuração de NIC convergido com um único adaptador de rede
description: Este tópico fornece instruções sobre como implantar NIC convergidos com um único adaptador de rede no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: eed5c184-fa55-43a8-a879-b1610ebc70ca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d6663a966026afb301a4bad90a9573d16fc82875
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-configuration-with-a-single-network-adapter"></a>Configuração de NIC convergido com um único adaptador de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

As seções a seguir fornecem instruções para configurar NIC convergidos com uma única NIC em seu host do Hyper-V.

A configuração de exemplo neste guia mostra dois hosts Hyper-V, **um Host do Hyper-V**, e **B de Host do Hyper-V**.

## <a name="test-connectivity-between-source-and-destination"></a>Testar a conectividade entre a origem e de destino

Esta seção fornece as etapas necessárias para testar a conectividade entre a origem e destino Hosts Hyper-V.

A ilustração a seguir mostra dois Hosts Hyper-V, **um Host do Hyper-V** e **B de Host do Hyper-V**. 

Ambos os servidores têm uma única física NIC (pNIC) instalada e as NICs estão conectadas a um superior do comutador físico do rack \(ToR\). Além disso, os servidores estão localizados na mesma sub-rede, que é 192.168.1.x/24.

![Hosts Hyper-V](../../media/Converged-NIC/1-single-test-conn.jpg)


### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>Testar a conectividade NIC ao Switch Virtual Hyper\-V

Usando essa etapa, você pode garantir que a placa de rede física, para que você mais tarde criará um comutador Virtual Hyper\-V, pode se conectar ao host de destino. 

Esse teste demonstra conectividade usando \(L3\) Layer 3 - ou a camada IP - bem como \(L2\) camada 2.

Você pode usar o seguinte comando do Windows PowerShell para obter as propriedades do adaptador de rede.

    Get-NetAdapter
     
A seguir estão exemplos de resultados desse comando.

|Nome|InterfaceDescription|ifIndex|Status|MacAddress|LinkSpeed|
|-----|--------------------|-------|-----|----------|---------|
|
|M1|Pro Mellanox ConnectX-3...| 4| Para cima|7C-FE-90-93-8F-A1|40 Gbps|

Você pode usar um dos seguintes comandos para obter propriedades de adaptador adicionais, incluindo o endereço IP.

    Get-NetAdapter M1 | fl *

A seguir estão exemplos editada de resultados desse comando.
    
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
    

### <a name="ensure-that-source-and-destination-can-communicate"></a>Certifique-se de origem e destino podem se comunicar

Você pode usar esta etapa para verificar a comunicação bidirecional \ (ping da origem ao destino e vice-versa em ambos os sistemas).  No exemplo a seguir, o **teste NetConnection** comando do Windows PowerShell é usado, mas se você preferir que você pode usar o **ping** comando. 

>[!NOTE]
>Se você tiver certeza de que seu hosts podem se comunicar uns com os outros, você pode pular esta etapa.

    Test-NetConnection 192.168.1.5

A seguir estão exemplos de resultados desse comando.

|Parâmetro|Valor|
|---------|-----|
|
|ComputerName|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|M1|
|Endereço_da_origem|192.168.1.3|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0 ms|

Em alguns casos, talvez seja necessário desabilitar o Firewall do Windows com segurança avançada para executar esse teste com êxito. Se você desativar o firewall, manter a segurança em mente e certifique-se de que a configuração atende aos requisitos de segurança da sua organização.

O comando de exemplo a seguir permite desabilitar todos os perfis de firewall.

    Set-NetFirewallProfile -All -Enabled False
    
Depois que você desative o firewall, você pode usar o seguinte comando para testar a conexão.

    Test-NetConnection 192.168.1.5

A seguir estão exemplos de resultados desse comando.

|Parâmetro|Valor|
|---------|-----|
|
|ComputerName|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|Teste-40G-1|
|Endereço_da_origem|192.168.1.3|
|PingSucceeded|False|
|PingReplyDetails \(RTT\)|0 ms|

## <a name="configure-vlans-optional"></a>Configurar VLANs \(Optional\)

Várias configurações de rede fazer uso de VLANs. Se você estiver planejando usar VLANs em sua rede, você deve repetir o teste anterior com VLANs configuradas. \ (Se você estiver planejando usar RoCE para serviços RDMA você deve habilitar VLANs. \)

Nesta etapa, as NICs estão em **acesso** modo. No entanto quando você cria um comutador Virtual Hyper-V \(vSwitch\) neste manual, as propriedades VLAN são aplicadas no nível da porta vSwitch. 

Como um comutador pode hospedar vários VLANs, é necessário para o comutador físico do topo de Rack \(ToR\) com a porta na qual o host estiver conectado a configurado no modo de tronco.

>[!NOTE]
>Consulte a documentação do switch ToR para obter instruções sobre como configurar o modo de tronco no switch.

A ilustração a seguir mostra dois hosts Hyper-V, cada um com um adaptador de rede física, e cada configurados para se comunicar em VLAN 101.

![Configurar redes locais virtuais](../../media/Converged-NIC/2-single-configure-vlans.jpg)

### <a name="configure-the-vlan-id"></a>Configurar o ID de VLAN

Você pode usar esta etapa para configurar as IDs de VLAN de NICs instaladas no seu host do Hyper-V.

#### <a name="configure-nic-m1"></a>Configurar NIC M1

Com os seguintes comandos, configure o ID de VLAN para a primeira NIC, M1, e exibir a configuração resultante.

>[!IMPORTANT]
>Não execute este comando se você estiver conectado ao host remotamente pela interface, porque isso apresenta em perda de acesso para o host.
    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name M1 | Where-Object {$_.RegistryKeyword -eq "VlanID"} 
    
A seguir estão exemplos de resultados desse comando.

|Nome |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|M1|ID DE VLAN|101|VlanID|{101}|


Certifique-se de que a ID de VLAN entra em vigor independente da implementação do adaptador de rede usando o seguinte comando para reiniciar o adaptador de rede.

    Restart-NetAdapter -Name "M1"

Você pode usar o comando a seguir para garantir que o status do adaptador de rede seja **backup** antes de prosseguir.

    Get-NetAdapter -Name "M1"

A seguir estão exemplos de resultados desse comando.

|Nome|InterfaceDescription|ifIndex| Status|MacAddress|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|M1|Ethernet Pro 3 Mellanox ConnectX Ada...|4|Para cima|7C-FE-90-93-8F-A1|40 Gbps|

Certifique-se de que você executar essa etapa em servidores locais e de destino. Se o servidor de destino não está configurado com a mesma ID de VLAN como o servidor local, os dois não poderão se comunicar.

### <a name="verify-connectivity"></a>Verificar a conectividade

Você pode usar esta seção para verificar a conectividade depois que os adaptadores de rede são reiniciados. Você pode confirmar conectividade depois de aplicar a marca VLAN aos dois adaptadores. Se a conectividade falhar, você pode inspecionar o switch de participação de configuração ou de destino VLAN na mesma VLAN. 

>[!IMPORTANT]
>Depois que você executar as etapas na seção anterior, poderá levar alguns segundos para o dispositivo para reiniciar e ficam disponíveis na rede.

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>Verificar a conectividade de NIC Test-40G-1

Para verificar a conectividade para a primeira NIC, você pode executar o comando a seguir.

    Test-NetConnection 192.168.1.5
    
## <a name="configure-data-center-bridging-dcb"></a>Configure a Central de dados ligando \(DCB\)

A próxima etapa é configurar \(QoS\) DCB e qualidade de serviço que exige que você instale primeiro os recursos do Windows Server 2016 DCB.

>[!NOTE]
>Você deve executar todas as seguintes etapas de configuração DCB e QoS em todos os servidores que são projetados para se comunicar uns com os outros.

### <a name="install-data-center-bridging-dcb"></a>Instale a ponte \(DCB\) do Centro de dados

Você pode usar esta etapa para instalar e habilitar DCB. 

>[!IMPORTANT]
>- Instalar e configurar DCB são **opcional** para configurações de rede que usam iWarp para serviços RDMA.
>- Instalar e configurar DCB são **necessária** para configurações de rede que usam RoCE \(any version\) para serviços RDMA.


Você pode usar o seguinte comando para instalar DCB em cada um dos seus hosts Hyper-V. 

    Install-WindowsFeature Data-Center-Bridging

### <a name="set-the-qos-policies-for-smb-direct"></a>Definir as políticas de QoS para SMB-Fi Direct 

Você pode usar o comando a seguir para configurar as políticas de QoS de SMB direta.

>[!IMPORTANT]
>- Esta etapa é opcional para configurações de rede que usam iWarp.
>- Essa etapa é necessária para as configurações de rede que usam RoCE.
>- O comando de exemplo abaixo, o valor "3" é arbitrário. Você pode usar qualquer valor entre 1 e 7 desde que você usar o mesmo valor em toda a configuração de políticas de QoS consistentemente.

    New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3

A seguir estão exemplos de resultados desse comando.

|Parâmetro|Valor|
|---------|-----|
|
|Nome |SMB|
|Proprietário|\(Machine\) política de grupo|
|NetworkProfile|Todos os|
|Precedência|127|
|JobObject|&nbsp;| 
|NetDirectPort|445
|PriorityValue|3

### <a name="for-roce-deployments-turn-on-priority-flow-control-for-smb-traffic"></a>Para RoCE implantações ativar o controle de fluxo de prioridade para tráfego SMB 

Se você estiver usando RoCE para seus serviços RDMA, você pode usar os comandos a seguir para habilitar o controle de fluxo SMB e exibir os resultados. Controle de fluxo de prioridade é necessária para RoCE, mas é desnecessária quando você estiver usando iWarp.

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

A seguir estão exemplos de resultados do **Get-NetQosFlowControl** comando.

|Prioridade|Habilitado|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False |Global|&nbsp;|&nbsp;|
|1 |False |Global|&nbsp;|&nbsp;|
|2 |False |Global|&nbsp;|&nbsp;|
|3 |True  |Global|&nbsp;|&nbsp;|
|4 |False |Global|&nbsp;|&nbsp;|
|5 |False |Global|&nbsp;|&nbsp;|
|6 |False |Global|&nbsp;|&nbsp;|
|7 |False |Global|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>Habilitar QoS para os adaptadores de rede local e de destino
Com essa etapa, você pode habilitar DCB em adaptadores de rede específicos.

>[!IMPORTANT]
>-  Essa etapa não é necessária para as configurações de rede que usam iWarp.
>-  Essa etapa é necessária para as configurações de rede que usam RoCE.




#### <a name="enable-qos-for-nic-m1"></a>Habilitar QoS para NIC M1

Você pode usar os seguintes comandos para habilitar QoS e exibir os resultados de sua configuração.

    Enable-NetAdapterQos -InterfaceAlias "M1"
    Get-NetAdapterQos -Name "M1"

A seguir estão exemplos de resultados desse comando.

**Nome**: M1 **habilitado**: True **recursos**:   

|Parâmetro|Hardware|Atual|
|---------|--------|-------|
|
|MacSecBypass|NotSupported|NotSupported|
|DcbxSupport|None|None|
|NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
 
**OperationalTrafficClasses**: 

|TC|TSA|Largura de banda|Prioridades|
|----|-----|--------|-------|
|
|0| ETS|70%|0-2,4-7|
|1|ETS|30%|3

**OperationalFlowControl**: prioridade 3 habilitado **OperationalClassifications**:

|Protocolo|Tipo de porta /|Prioridade|
|--------|---------|--------|
|
|Padrão|&nbsp;|0|
|NetDirect| 445|3|

### <a name="reserve-a-percentage-of-the-bandwidth-for-smb-direct-rdma"></a>Reserve um percentual da largura de banda para \(RDMA\) SMB direta

Você pode usar o seguinte comando para reservar um percentual da largura de banda para SMB direta.  

Neste exemplo, é usada uma reserva de largura de banda de 30%. Você deve selecionar um valor que representa o que você espera que exigirá que o tráfego de armazenamento. O valor da **- bandwidthpercentage** parâmetro deve ser um múltiplo de 10%.

    New-NetQosTrafficClass "SMB" -Priority 3 -BandwidthPercentage 30 -Algorithm ETS

A seguir estão exemplos de resultados desse comando.

|Nome|Algoritmo |Bandwidth(%)| Prioridade |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ETS     | 30 |3 |Global |&nbsp;|&nbsp;|                                      
 
Você pode usar o seguinte comando para exibir informações de reserva de largura de banda.

    Get-NetQosTrafficClass

A seguir estão exemplos de resultados desse comando.
 
|Nome|Algoritmo |Bandwidth(%)| Prioridade |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Padrão]|ETS|70 |0-2,4-7| Global|&nbsp;|&nbsp;| 
|SMB      |ETS|30 |3 |Global|&nbsp;|&nbsp;| 

## <a name="remove-debugger-conflict-mellanox-adapter-only"></a>Remover conflito de depurador \(Mellanox adapter only\)

Se você estiver usando um adaptador de Mellanox você precisa executar essa etapa para configurar o depurador. Por padrão, quando um adaptador Mellanox é usado, o depurador anexado bloqueia NetQos. Você pode usar o comando a seguir para substituir o depurador.

    
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    

## <a name="test-rdma-native-host"></a>Teste RDMA (host nativo)

Você pode usar esta etapa para garantir que a malha é configurada corretamente antes da criação de um vSwitch e fazer a transição para RDMA \(Converged NIC\).

A ilustração a seguir mostra o estado atual dos hosts Hyper-V.

![Teste RDMA](../../media/Converged-NIC/4-single-test-rdma.jpg)

Para verificar a configuração de RDMA, você pode executar o comando a seguir.

    Get-NetAdapterRdma

A seguir estão exemplos de resultados desse comando.

|Nome |InterfaceDescription |Habilitado|
|----|--------------------|-------|
|
|M1| Adaptador Ethernet Pro Mellanox ConnectX-3 |True|

### <a name="download-diskspdexe-and-a-powershell-script"></a>Baixar DiskSpd.exe e um Script do PowerShell

Para continuar, você deve primeiro baixar os itens a seguir.

- Baixe o utilitário DiskSpd.exe e extraia o utilitário em C:\TEST\ [Diskspd utilitário: um robusto armazenamento testes ferramenta (seja substituído SQLIO)](https://gallery.technet.microsoft.com/DiskSpd-a-robust-storage-6cd2f223)

- Baixe o script do powershell Test-RDMA para C:\TEST\[https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Determinar o valor ifIndex do seu adaptador de destino

Você pode usar o seguinte comando para descobrir o valor ifIndex do adaptador de destino. Você pode usar esse valor em etapas subsequentes quando você executa o script que você baixou.

    Get-NetIPConfiguration -InterfaceAlias "M*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

A seguir estão exemplos de resultados desse comando.

|InterfaceAlias |Índice_da_interface |IPv4Address|
|-------------- |-------------- |-----------|
|
|M2 |14 |{192.168.1.5}|

### <a name="run-the-powershell-script"></a>Execute o script do PowerShell

Quando você executa o Test-Rdma. ps1 script do Windows PowerShell, você pode passar o valor ifIndex ao script, juntamente com o endereço IP do adaptador remoto na mesma VLAN.

Você pode usar o comando de exemplo a seguir para executar o script com uma ifIndex 14 no adaptador de rede 192.168.1.5.
    
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
    

>[!NOTE]
>Se o tráfego RDMA falhar, no caso de RoCE especificamente, consulte sua configuração ToR Switch para configurações PFC/ETS corretas que devem corresponder as configurações de Host. Consulte a seção QoS neste documento para valores de referência.

## <a name="remove-the-access-vlan-setting"></a>Remover a configuração de acesso VLAN

Em preparação para a criação da opção Hyper-V, você deve remover as configurações de VLAN instalado acima.  Você pode usar o seguinte comando para remover a configuração de VLAN de acesso do NIC físico. Essa ação impede que o NIC marcação automática o tráfego de saída com a ID de VLAN incorretos e também impede que ele filtrando o tráfego de entrada que não corresponda a ID de VLAN de acesso.

    
    Set-NetAdapterAdvancedProperty -Name M1 -RegistryKeyword VlanID -RegistryValue "0"
    

Você pode usar o comando de exemplo a seguir para confirmar a configuração VlanID e exibir os resultados, que mostram o valor de ID de VLAN é zero.

    
    Get-NetAdapterAdvancedProperty -name m1 | Where-Object {$_.RegistryKeyword -eq 'VlanID'} 
    


## <a name="create-a-hyper-v-virtual-switch"></a>Criar um comutador Virtual Hyper-V

Você pode usar esta seção para criar um comutador Virtual Hyper-V \(vSwitch\) nos hosts Hyper-V.

A ilustração a seguir representa 1 de Host do Hyper-V com um vSwitch.

![Criar um comutador Virtual Hyper-V](../../media/Converged-NIC/5-single-create-vswitch.jpg)


### <a name="create-an-external-hyper-v-virtual-switch"></a>Criar um comutador Virtual externo do Hyper-V

Você pode usar esta seção para criar um vSwitch externo em Hyper-V no Host do Hyper-V A.

Você pode usar o comando de exemplo a seguir para criar um botão chamado VMSTEST.

>[!NOTE]
>O parâmetro **AllowManagementOS** no comando a seguir cria um vNIC Host que herda o endereço MAC e o endereço IP do NIC físico.

    New-VMSwitch -Name VMSTEST -NetAdapterName "M1" -AllowManagementOS $true

A seguir estão exemplos de resultados desse comando.

|Nome |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |Externa |Adaptador Ethernet Pro Mellanox ConnectX-3|

Você pode usar o seguinte comando para exibir as propriedades do adaptador de rede.

    Get-NetAdapter | ft -AutoSize

A seguir estão exemplos de resultados desse comando.

|Nome |InterfaceDescription | ifIndex |Status |MacAddress |LinkSpeed|
|---- |-------------------- |-------| ------|----------|---------|
|
|vEthernet \(VMSTEST\) |Adaptador Ethernet Virtual Hyper-V #2|27 |Para cima |E4-1D-2D-07-40-71 |40 Gbps|


Você pode gerenciar vNIC um Host de duas maneiras. Um método é o **NetAdapter** exibição, que opera com base na "vEthernet \(VMSTEST\)" nome.

O outro método é o **VMNetworkAdapter** exibição, que descarta o prefixo "vEthernet" e simplesmente usa o nome vmswitch.

O **VMNetworkAdapter** exibição mostra algumas propriedades do adaptador de rede que não são exibidas com o **NetAdapter** comando.

Você pode usar o seguinte comando para exibir os resultados do **VMNetworkAdapter** método.

    Get-VMNetworkAdapter –ManagementOS | ft -AutoSize

A seguir estão exemplos de resultados desse comando.

|Nome |IsManagementOs |VMName |SwitchName |MacAddress |Status |EndereçosIP|
|----|-------------- |------ |----------|----------|------ |-----------|
|
|CORP-External-Switch |True |CORP-External-Switch| 001B785768AA |{Okey} |&nbsp;|
|VMSTEST |True |VMSTEST | E41D2D074071| {Okey} | &nbsp;| 

### <a name="test-the-connection"></a>Teste a conexão

Você pode usar o comando de exemplo a seguir para testar a conexão e exibir os resultados.
    
    Test-NetConnection 192.168.1.5

    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
Você pode usar os comandos de exemplo a seguir para atribuir e exibir configurações de VLAN do adaptador de rede.
    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

|VMName |VMNetworkAdapterName |Modo |VlanList|
|------ |-------------------- |---- |--------|
|
|&nbsp;|VMSTEST |Acesso |101     
 
### <a name="test-the-connection"></a>Teste a conexão

Lembre-se de que a alteração pode levar alguns segundos para ser concluída antes de você pode conseguir efetuar ping o outro adaptador.

Você pode usar o comando de exemplo a seguir para testar a conexão e exibir os resultados.
    
    Test-NetConnection 192.168.1.5
     
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

## <a name="test-hyper-v-virtual-switch-rdma-mode-2"></a>Teste do Hyper-V Virtual Switch RDMA (modo 2)

A ilustração a seguir mostra o estado atual do seu hosts Hyper-V, incluindo o vSwitch em 1 de Host do Hyper-V.

![Testar o comutador Virtual Hyper-V](../../media/Converged-NIC/6-single-test-vswitch-rdma.jpg)


### <a name="set-priority-tagging-on-the-host-vnic"></a>Definir a marcação em vNIC o Host de prioridade

Você pode usar os comandos de exemplo a seguir para definir a marcação em vNIC o Host de prioridade e exibir os resultados das ações.
    
    Set-VMNetworkAdapter -ManagementOS -Name "VMSTEST" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "VMSTEST" | fl Name,IeeePriorityTag
    
    Name: VMSTEST
    IeeePriorityTag : On
    
Você pode usar o comando de exemplo a seguir para exibir informações de RDMA do adaptador de rede. Nos resultados, quando o parâmetro **Enabled** tem o valor **False**, isso significa que RDMA não está habilitado.
    
    Get-NetAdapterRdma
    
|Nome |InterfaceDescription |Habilitado |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| Adaptador Ethernet Virtual Hyper-V #2|False|

### <a name="enable-rdma-on-the-host-vnic"></a>Ativar RDMA vNIC o Host

Você pode usar os comandos de exemplo a seguir para exibir propriedades do adaptador de rede, habilitar RDMA para o adaptador e, em seguida, exibir informações de RDMA do adaptador de rede.
    
    Get-NetAdapter
    
|Nome |InterfaceDescription |ifIndex |Status |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|Adaptador Ethernet Virtual Hyper-V #2|27|Para cima|E4-1D-2D-07-40-71|40 Gbps|

    Enable-NetAdapterRdma -Name "vEthernet (VMSTEST)"
    Get-NetAdapterRdma -Name "vEthernet (VMSTEST)"

Nos resultados a seguir, quando o parâmetro **Enabled** tem o valor **True**, isso significa que RDMA está habilitado.

|Nome |InterfaceDescription |Habilitado |
|---- |-------------------- |-------|
|
|vEthernet \(VMSTEST\)| Adaptador Ethernet Virtual Hyper-V #2|True|


    
    Get-NetAdapter 
    

|Nome |InterfaceDescription |ifIndex |Status |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|vEthernet (VMSTEST)|Adaptador Ethernet Virtual Hyper-V #2|27|Para cima|E4-1D-2D-07-40-71|40 Gbps|

### <a name="perform-rdma-traffic-test-by-using-the-script"></a>Execute o teste de tráfego RDMA usando o script

Você pode usar o seguinte comando para executar o script que você baixou e exibir os resultados.
    
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
    
A linha final nessa saída, "teste de tráfego RDMA bem-sucedida: RDMA tráfego foi enviado para 192.168.1.5," demonstra que você configurou o NIC convergidos com êxito no seu adaptador.

## <a name="all-topics-in-this-guide"></a>Todos os tópicos neste guia

Este guia contém os tópicos a seguir.

- [Configuração de NIC convergido com um único adaptador de rede](cnic-single.md)
- [Configuração de NIC emparelhados NIC convergido](cnic-datacenter.md)
- [Configuração do comutador físico para NIC convergido](cnic-app-switch-config.md)
- [Solução de problemas de configurações de NIC convergidos](cnic-app-troubleshoot.md)
