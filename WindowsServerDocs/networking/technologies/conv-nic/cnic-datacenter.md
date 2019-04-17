---
title: Configuração de NIC emparelhados NIC convergido
description: Este tópico fornece instruções sobre como configurar NIC convertida em uma configuração de datacenter Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1ac6c2915301b1cf64486f24c197ebbf22bb5e2e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="converged-nic-teamed-nic-configuration"></a>Configuração de NIC emparelhados NIC convergido

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

As seções a seguir fornecem instruções para implantar NIC convertida em uma configuração agrupada NIC com \(SET\) Switch Embedded agrupamento. A configuração de exemplo neste guia mostra dois hosts Hyper-V, Hyper-V Host 1 e 2 de Host do Hyper-V.

## <a name="test-connectivity-between-source-and-destination"></a>Testar a conectividade entre a origem e de destino

Esta seção fornece as etapas necessárias para testar a conectividade entre hosts Hyper-V de origem e de destino.

A ilustração a seguir mostra dois hosts Hyper-V, **1 de Host do Hyper-V** e **2 de Host do Hyper-V**. 

Ambos os hosts Hyper-V tem dois adaptadores de rede. Em cada host, um adaptador está conectado à sub-rede 192.168.1.x/24 e um adaptador está conectado à sub-rede 192.168.2.x/24.

![Hosts Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

### <a name="test-nic-connectivity-to-the-hyper-v-virtual-switch"></a>Testar a conectividade NIC ao Switch Virtual Hyper-V

Usando essa etapa, você pode garantir que a placa de rede física, para que você mais tarde criará um comutador Virtual Hyper-V, pode se conectar ao host de destino. 

Esse teste demonstra conectividade usando \(L3\) Layer 3 - ou a camada IP -, bem como as camada 2 \(L2\) redes locais virtuais \(VLAN\).

Você pode usar o seguinte comando do Windows PowerShell para obter as propriedades do adaptador de rede.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
     
A seguir estão exemplos de resultados desse comando.

|Nome|InterfaceDescription|ifIndex|Status|MacAddress|LinkSpeed|
|-----|--------------------|-------|-----|----------|---------|
|
|Teste-40G-1|Adaptador Ethernet Pro Mellanox ConnectX-3|11|Para cima|E4-1D-2D-07-43-D0|40 Gbps|

Você pode usar um dos seguintes comandos para obter propriedades de adaptador adicionais, incluindo o endereço IP.

    Get-NetIPAddress -InterfaceAlias "Test-40G-1"

    Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
    
A seguir estão exemplos de resultados desses comandos.

|Parâmetro|Valor|
|---------|-----|
|
|IPAddress| 192.168.1.3|
|Índice_da_interface|11|
|InterfaceAlias|Teste-40G-1|
|AddressFamily|IPv4|
|Tipo| Unicast|
|PrefixLength|24|

### <a name="verify-that-nic-team-member-has-a-valid-ip-address"></a>Verifique se que o membro da equipe NIC tem um endereço IP válido

Você pode usar esta etapa para verificar se outra equipe NIC ou alternar Embedded equipe \(SET\) membro físico NICs \(pNICs\) também tem um endereço IP válido.

Nesta etapa, você pode usar uma sub-rede separada, \ (xxx.xxx.** 2**vs. xxx xxx.xxx. **1**. xxx\), para facilitar o envio desse adaptador para o destino. 

Caso contrário, se você localizar os dois pNICs na mesma sub-rede, a carga de pilha de TCP/IP Windows equilibra entre as interfaces e validação simple se torna mais complicada.

Você pode usar o seguinte comando do Windows PowerShell para obter as propriedades do segundo adaptador de rede.

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

A seguir estão exemplos de resultados desse comando.

|Nome |InterfaceDescription |ifIndex |Status |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|TESTE-40G-2 |Ethernet Pro Mellanox ConnectX-3 r.... \ #2 |13 |Para cima |E4-1D-2D-07-40-70 |40 Gbps|

Você pode usar um dos seguintes comandos para obter propriedades de adaptador adicionais, incluindo o endereço IP.

    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$\_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

A seguir estão exemplos de resultados desses comandos.

|Parâmetro|Valor|
|---------|-----|
|
|IPAddress|192.168.2.3|
|Índice_da_interface|13|
|InterfaceAlias|TESTE-40G-2|
|AddressFamily|IPv4|
|Tipo|Unicast|
|PrefixLength|24|

### <a name="verify-that-additional-nics-have-valid-ip-addresses"></a>Verifique se NICs adicionais têm endereços IP válidos

Você pode usar esta etapa para garantir que os outro NIC tenha um endereço IP válido.

Nesta etapa, você pode usar uma sub-rede separada, \ (xxx.xxx.** 2**vs. xxx xxx.xxx. **1**. xxx\), para facilitar o envio desse adaptador para o destino. 

Caso contrário, se você localizar os dois pNICs na mesma sub-rede, a carga de pilha de TCP/IP Windows equilibra entre as interfaces e validação simple se torna mais complicada.

Você pode usar o seguinte comando do Windows PowerShell para obter as propriedades do segundo adaptador de rede.

    Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize

A seguir estão exemplos de resultados desse comando.

|Nome |InterfaceDescription |ifIndex |Status |MacAddress |LinkSpeed|
|----|--------------------|-------|------|----------|---------|
|
|TESTE-40G-2 |Ethernet Pro Mellanox ConnectX-3 r.... \ #2 |13 |Para cima |E4-1D-2D-07-40-70 |40 Gbps|

Você pode usar um dos seguintes comandos para obter propriedades de adaptador adicionais, incluindo o endereço IP.
    
    Get-NetIPAddress -InterfaceAlias "Test-40G-2"

    Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress

A seguir estão exemplos de resultados desses comandos.

|Parâmetro|Valor|
|---------|-----|
|
|IPAddress|192.168.2.3|
|Índice_da_interface|13|
|InterfaceAlias|TESTE-40G-2|
|AddressFamily|IPv4|
|Tipo|Unicast|
|PrefixLength|24|

### <a name="ensure-that-source-and-destination-can-communicate"></a>Certifique-se de origem e destino podem se comunicar

Você pode usar esta etapa para verificar a comunicação bidirecional \ (ping da origem ao destino e vice-versa em ambos os sistemas).  No exemplo a seguir, o **teste NetConnection** comando do Windows PowerShell é usado, mas se você preferir que você pode usar o **ping** comando. 

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

### <a name="verify-connectivity-for-additional-nics"></a>Verifique se a conectividade para NICs adicionais

Com essa etapa, você pode repetir as etapas anteriores para todos os pNICs subsequentes que estão incluídos na equipe do NIC ou conjunto.

Você pode usar o seguinte comando para testar a conexão.
    
    Test-NetConnection 192.168.2.5

A seguir estão exemplos de resultados desse comando.

|Parâmetro|Valor|
|---------|-----|
|
|ComputerName|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|Teste-40G-2|
|Endereço_da_origem|192.168.2.3|
|PingSucceeded|False|
|PingReplyDetails \(RTT\)|0 ms|


## <a name="configure-vlans"></a>Configurar VLANs

Nesta etapa, as NICs estão em **acesso** modo. No entanto quando você cria um comutador Virtual Hyper-V \(vSwitch\) neste manual, as propriedades VLAN são aplicadas no nível da porta vSwitch. 

Como um comutador pode hospedar vários VLANs, é necessário para o comutador físico do topo de Rack \(ToR\) ter sua porta configurada no modo de tronco.

Consulte a documentação do switch ToR para obter instruções sobre como configurar o modo de tronco no switch.

A ilustração a seguir mostra dois hosts Hyper-V com dois adaptadores de rede cada que têm VLAN 101 e 102 VLAN configurado nas propriedades do adaptador de rede.

![Configurar redes locais virtuais](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)

### <a name="configure-the-vlan-ids-for-both-nics"></a>Configurar as IDs VLAN para ambas as NICs

Você pode usar esta etapa para configurar as IDs de VLAN de NICs instaladas no seu host do Hyper-V.

Acordo com do Institute of Electrical and \(IEEE\) engenheiros de aparelhos eletrônicos padrões de rede, as propriedades de \(QoS\) de qualidade de serviço no físico NIC agir sobre o cabeçalho de 802.1 p é inserido no 802.1 q \(VLAN\) cabeçalho quando você configura o VLAN ID.

#### <a name="configure-nic-test-40g-1"></a>Configurar NIC Test-40G-1

Com os seguintes comandos, defina a ID de VLAN para a primeira NIC, teste-40G-1, em seguida, exibir a configuração resultante.

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    
A seguir estão exemplos de resultados desse comando.

|Nome |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|TESTE-40G-1|ID DE VLAN|101|VlanID|{101}|


Certifique-se de que a ID de VLAN entra em vigor independente da implementação do adaptador de rede usando o seguinte comando para reiniciar o adaptador de rede.

    Restart-NetAdapter -Name "Test-40G-1"

Você pode usar o comando a seguir para garantir que o status do adaptador de rede seja **backup** antes de prosseguir.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

A seguir estão exemplos de resultados desse comando.

|Nome|InterfaceDescription|ifIndex| Status|MacAddress|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|Teste-40G-1|Ethernet Pro 3 Mellanox ConnectX Ada...|11|Para cima|E4-1D-2D-07-43-D0|40 Gbps|

#### <a name="configure-nic-test-40g-2"></a>Configurar NIC Test-40G-2

Com os seguintes comandos, defina a ID de VLAN para a segunda NIC, teste-40G-2, em seguida, exibir a configuração resultante.

    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
    

A seguir estão exemplos de resultados desse comando.

|Nome |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
|----|-----------|------------|---------------|-------------|
|
|TESTE-40G-2|ID DE VLAN|102|VlanID|{102}|

Certifique-se de que a ID de VLAN entra em vigor independente da implementação do adaptador de rede usando o seguinte comando para reiniciar o adaptador de rede.

`Restart-NetAdapter -Name "Test-40G-2"  `              

Você pode usar o comando a seguir para garantir que o status do adaptador de rede seja **backup** antes de prosseguir.

    Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize

A seguir estão exemplos de resultados desse comando.

|Nome|InterfaceDescription|ifIndex| Status|MacAddress|LinkSpeed|
|----|--------------------|-------|------|----------| ---------|
|
|Teste-40G-2 |Ethernet Pro 3 Mellanox ConnectX Ada... |11 |Para cima |E4-1D-2D-07-43-D1 |40 Gbps|


### <a name="verify-connectivity"></a>Verificar a conectividade

Você pode usar esta seção para verificar a conectividade depois que os adaptadores de rede são reiniciados. Você pode confirmar conectividade depois de aplicar a marca VLAN aos dois adaptadores. Se a conectividade falhar, você pode inspecionar o switch de participação de configuração ou de destino VLAN na mesma VLAN. 

>[!IMPORTANT]
>Depois que você executar as etapas na seção anterior, poderá levar alguns segundos para o dispositivo para reiniciar e ficam disponíveis na rede.

#### <a name="verify-connectivity-for-nic-test-40g-1"></a>Verificar a conectividade de NIC Test-40G-1

Para verificar a conectividade para a primeira NIC, você pode executar o comando a seguir.

    Test-NetConnection 192.168.1.5
    
A seguir estão exemplos de resultados desse comando.

|Parâmetro|Valor|
|---------|-----|
|
|ComputerName|192.168.1.5|
|RemoteAddress|192.168.1.5|
|InterfaceAlias|Teste-40G-1|
|Endereço_da_origem|192.168.1.5|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0 ms|

#### <a name="verify-connectivity-for-nic-test-40g-2"></a>Verificar a conectividade de NIC Test-40G-2

Você pode usar esta seção para testar a conectividade para NIC Test-40G-2.

Você pode usar o seguinte comando para testar a conectividade para a segunda NIC.
    
    Test-NetConnection 192.168.2.5
    
A seguir estão exemplos de resultados desse comando.

|Parâmetro|Valor|
|---------|-----|
|
|ComputerName|192.168.2.5|
|RemoteAddress|192.168.2.5|
|InterfaceAlias|Teste-40G-2|
|Endereço_da_origem|192.168.2.3|
|PingSucceeded|True|
|PingReplyDetails \(RTT\)|0 ms|

A **teste NetConnection** falha ou ping que ocorre imediatamente após a realização **NetAdapter reiniciar** não é incomum, aguarde até o adaptador de rede inicializar totalmente e tente novamente.

Se as conexões de 101 VLAN funcionar, mas as conexões VLAN 102 não funcionar, o problema pode ser que a opção precisa ser configurado para permitir o tráfego de porta no NATIVA desejada. Você pode verificar isso temporariamente definindo os adaptadores de falha para VLAN 101 e repetir o teste de conectividade.

## <a name="configure-quality-of-service-qos"></a>Configurar a qualidade de serviço \(QoS\)

A próxima etapa é configurar \(QoS\) qualidade de serviço, que requer que você instale primeiro o recurso do Windows Server 2016 \(DCB\) ponte do Centro de dados.

A ilustração a seguir mostra os hosts Hyper-V depois de configurar com êxito VLANs na etapa anterior.

![Configurar a qualidade de serviço](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)

### <a name="install-data-center-bridging-dcb"></a>Instale a ponte \(DCB\) do Centro de dados

Você pode usar esta etapa para instalar e habilitar DCB. Na maioria dos casos, essa etapa é opcional para implementações de iWarp, mas é necessário na escala malha, por exemplo, para cenários de rack cruzada.

Você pode usar o seguinte comando para instalar DCB em cada um dos seus hosts Hyper-V. 

    Install-WindowsFeature Data-Center-Bridging

A seguir estão exemplos de resultados desse comando.

|Êxito |Reinicialização necessária |Código de saída|Resultado de recurso|
|------- |-------------- |--------- |-------------- |
|
|True |Não |Êxito| {Centro de dados ligando}|

### <a name="set-the-qos-policies-for-smb-direct"></a>Definir as políticas de QoS para SMB-Fi Direct 

Você pode usar o comando a seguir para configurar as políticas de QoS de SMB direta.

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

### <a name="set-policies-for-other-traffic-on-the-interface"></a>Definir políticas para outro tráfego na interface 

Você pode usar o comando a seguir para definir políticas de QoS adicionais.

    New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
 
A seguir estão exemplos de resultados desse comando.

|Parâmetro|Valor|
|---------|-----|
|
|Nome | PADRÃO|
|Proprietário|\(Machine\) política de grupo|
|NetworkProfile|Todos os|
|Precedência|127|
|Modelo| Padrão|
|JobObject| &nbsp;|
|PriorityValue|0|

### <a name="turn-on-flow-control-for-smb"></a>Ativar o controle de fluxo para SMB 

Você pode usar os comandos a seguir para habilitar o controle de fluxo SMB e exibir os resultados.

    Enable-NetQosFlowControl -priority 3
    Get-NetQosFlowControl

A seguir estão exemplos de resultados desse comando.

|Prioridade|Habilitado|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False |Global|&nbsp;|&nbsp;|
|1 |False |Global|&nbsp;|&nbsp;|
|2 |False |Global|&nbsp;|&nbsp;|
|3 |True |Global|&nbsp;|&nbsp;|
|4 |False |Global|&nbsp;|&nbsp;|
|5 |False |Global|&nbsp;|&nbsp;|
|6 |False |Global|&nbsp;|&nbsp;|
|7 |False |Global|&nbsp;|&nbsp;|

Se os resultados não coincidem esses resultados porque os itens que não sejam 3 têm um valor de Enabled True, você deve executar a próxima etapa. Se seus resultados coincidir com esses resultados de exemplo e único item 3 tem um valor de Enabled True, você não precisa executar a próxima etapa e poderá pular para baixo para **QoS habilitar para os adaptadores de rede de destino e o destino**.

### <a name="ensure-flow-control-is-disabled-for-other-traffic-classes-optional"></a>Certifique-se de que fluxo de controle é desabilitado para outras Classes de tráfego \(Optional\)

Você não precisa executar essa etapa se **controle de fluxo** não está habilitado para classes de tráfego 0,1,2,4,5,6 e 7.

Se **controle de fluxo** já está habilitada para todo o tráfego classes que não seja 3 \ (0,1,2,4,5,6 e 7\), você deverá desabilitar **controle de fluxo** para essas classes. 

>[!NOTE]
>Em configurações mais complexas, outras classes de tráfego podem exigem controle de fluxo, mas esses cenários estão fora do escopo deste guia.

Você pode usar os comandos a seguir para desabilitar o controle de fluxo do SMB para classes de tráfego 0,1,2,4,5,6 e 7 e para exibir os resultados.

    Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
    Get-NetQosFlowControl

A seguir estão exemplos de resultados desse comando.

|Prioridade|Habilitado|PolicySet|IfIndex|IfAlias|
|---------|-----|--------- |-------| -------|
|
|0 |False |Global|&nbsp;|&nbsp;|
|1 |False |Global|&nbsp;|&nbsp;|
|2 |False |Global|&nbsp;|&nbsp;|
|3 |True |Global|&nbsp;|&nbsp;|
|4 |False |Global|&nbsp;|&nbsp;|
|5 |False |Global|&nbsp;|&nbsp;|
|6 |False |Global|&nbsp;|&nbsp;|
|7 |False |Global|&nbsp;|&nbsp;|

### <a name="enable-qos-for-the-local-and-destination-network-adapters"></a>Habilitar QoS para os adaptadores de rede local e de destino 

Com essa etapa, você pode habilitar QoS para adaptadores de rede local e de destino. Certifique-se de que você executar esses comandos em ambos os hosts Hyper-V.

#### <a name="enable-qos-for-nic-test-40g-1"></a>Habilitar QoS para NIC Test-40G-1

Você pode usar os seguintes comandos para habilitar QoS e exibir os resultados de sua configuração.

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
    Get-NetAdapterQos -Name "Test-40G-1"

A seguir estão exemplos de resultados desse comando.

**Nome**: 40G-teste-1 **habilitado**: True **recursos**:   

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
|0| Estrito|&nbsp;|0-7|

**OperationalFlowControl**: prioridade 3 habilitado **OperationalClassifications**:

|Protocolo|Tipo de porta /|Prioridade|
|--------|---------|--------|
|
|Padrão|&nbsp;|0|
|NetDirect| 445|3|

#### <a name="enable-qos-for-nic-test-40g-2"></a>Habilitar QoS para NIC Test-40G-2

Você pode usar os seguintes comandos para habilitar QoS e exibir os resultados de sua configuração.

    Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
    Get-NetAdapterQos -Name "Test-40G-2"

 
A seguir estão exemplos de resultados desse comando.

**Nome**: 40G-teste-2 **habilitado**: True **recursos**:   

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
|0| Estrito|&nbsp;|0-7|

**OperationalFlowControl**: prioridade 3 habilitado **OperationalClassifications**:

|Protocolo|Tipo de porta /|Prioridade|
|--------|---------|--------|
|
|Padrão|&nbsp;|0|
|NetDirect| 445|3|


### <a name="allocate-50-of-the-bandwidth-reservation-to-smb-direct-rdma"></a>Alocar 50% da largura de banda reserva para SMB direta \(RDMA\)

Você pode usar os seguintes comandos para atribuir metade da reserva de largura de banda para SMB direta.

    New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS

A seguir estão exemplos de resultados desse comando.

|Nome|Algoritmo |Bandwidth(%)| Prioridade |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|SMB | ETS     | 50 |3 |Global |&nbsp;|&nbsp;|                                      
 
Você pode usar o seguinte comando para exibir informações de reserva de largura de banda.

    Get-NetQosTrafficClass | ft -AutoSize

A seguir estão exemplos de resultados desse comando.
 
|Nome|Algoritmo |Bandwidth(%)| Prioridade |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Padrão]| ETS|50 |0-2,4-7|  Global|&nbsp;|&nbsp;| 
SMB |ETS|50 |3 |Global|&nbsp;|&nbsp;| 

### <a name="create-traffic-classes-for-tenant-ip-traffic-optional"></a>Criar Classes de tráfego para tráfego IP de locatário \(optional\)

Você pode usar esta etapa para criar duas classes de tráfego adicionais para tráfego IP de locatário. 

Quando você executa os comandos de exemplo a seguir, você pode omitir os valores "IP1" e "IP2" Se você preferir.

    New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS

A seguir estão exemplos de resultados desse comando.

|Nome|Algoritmo |Bandwidth(%)| Prioridade |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP1 |ETS |10 |1 |Global|&nbsp;|&nbsp;|


    New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS

A seguir estão exemplos de resultados desse comando.

|Nome|Algoritmo |Bandwidth(%)| Prioridade |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|IP2 |ETS |10 |2 |Global|&nbsp;|&nbsp;|

Você pode usar o seguinte comando para QoS tráfego classes de exibição.

    Get-NetQosTrafficClass | ft -AutoSize

A seguir estão exemplos de resultados desse comando.

|Nome|Algoritmo |Bandwidth(%)| Prioridade |PolicySet |IfIndex |IfAlias |
|----|---------| ------------ |--------| ---------|------- |------- |
|
|[Padrão] |ETS |30 |0,4-7 |Global|&nbsp;|&nbsp;|
|SMB |ETS |50 |3 |Global|&nbsp;|&nbsp;|
|IP1 |ETS |10 |1 |Global|&nbsp;|&nbsp;|
|IP2 |ETS |10 |2 |Global|&nbsp;|&nbsp;|

## <a name="configure-debugger-optional"></a>Configurar o depurador \(Optional\)

Você pode usar esta etapa para configurar o depurador.

Por padrão, o depurador anexado bloqueia NetQos. Você pode usar os comandos a seguir para substituir o depurador e exibir os resultados.

    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger

A seguir estão exemplos de resultados desse comando.

    AllowFlowControlUnderDebugger
    -----------------------------
    1

## <a name="test-rdma-mode-1"></a>Teste RDMA \(Mode 1\)

Você pode usar esta etapa para garantir que a malha é configurada corretamente antes da criação de um vSwitch e fazer a transição para RDMA \(Mode 2\).

A ilustração a seguir mostra o estado atual dos hosts Hyper-V.

![Teste RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)

Para verificar a configuração de RDMA, você pode executar o comando a seguir.

    Get-NetAdapterRdma | ft -AutoSize

A seguir estão exemplos de resultados desse comando.

|Nome |InterfaceDescription |Habilitado|
|----|--------------------|-------|
|
|TESTE-40G-1| Adaptador de 4 Mellanox ConnectX VPI #2 |True|
|TESTE-40G-2| Adaptador VPI Mellanox ConnectX-4 |True|


### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Determinar o valor ifIndex do seu adaptador de destino

Você pode usar o seguinte comando para descobrir o valor ifIndex do adaptador de destino.

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

A seguir estão exemplos de resultados desse comando.

|InterfaceAlias |Índice_da_interface |IPv4Address|
|-------------- |-------------- |-----------|
|TESTE-40G-1 |14 |{192.168.1.3}|
|TESTE-40G-2 | 13 |{192.168.2.3}|

### <a name="download-diskspdexe-and-a-powershell-script"></a>Baixar DiskSpd.exe e um Script do PowerShell

Para continuar, você deve primeiro baixar os itens a seguir.

- Baixe o utilitário DiskSpd.exe e extraia o utilitário em C:\TEST\[http://tinyurl.com/z68h3rc](http://tinyurl.com/z68h3rc)

- Baixe o script do powershell Test-RDMA para C:\TEST\[https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

### <a name="run-the-powershell-script"></a>Execute o script do PowerShell

Quando você executa o Test-Rdma. ps1 script do Windows PowerShell, você pode passar o valor ifIndex ao script, juntamente com o endereço IP do adaptador remoto na mesma VLAN.

Você pode usar o comando de exemplo a seguir para executar o script com uma ifIndex 14 no adaptador de rede 192.168.1.5.
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter TEST-40G-1 is a physical adapter
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

### <a name="determine-the-ifindex-value-of-your-target-adapter"></a>Determinar o valor ifIndex do seu adaptador de destino

Você pode usar o seguinte comando para descobrir o valor ifIndex do adaptador de destino.

    Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address

A seguir estão exemplos de resultados desse comando.

|InterfaceAlias |Índice_da_interface |IPv4Address|
|-------------- |-------------- |-----------|
|TESTE-40G-1 |14 |{192.168.1.3}|
|TESTE-40G-2 | 13 |{192.168.2.3}|

Você pode usar o comando de exemplo a seguir para executar o script com uma ifIndex de 13 anos no adaptador de rede 192.168.2.5.

    
    C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter TEST-40G-2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
    VERBOSE: Remote IP 192.168.2.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 541185606 RDMA bytes written per second
    VERBOSE: 34821478 RDMA bytes sent per second
    VERBOSE: 954717307 RDMA bytes written per second
    VERBOSE: 35040816 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
    

## <a name="create-a-hyper-v-virtual-switch"></a>Criar um comutador Virtual Hyper-V

Você pode usar esta seção para criar um comutador Virtual Hyper-V \(vSwitch\) nos hosts Hyper-V.

A ilustração a seguir representa 1 de Host do Hyper-V com um vSwitch.

![Criar um comutador Virtual Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

### <a name="create-a-vswitch-in-switch-embedded-teaming-set-mode"></a>Criar um vSwitch no modo de agrupamento Embedded do Switch \(SET\)

Você pode usar o comando de exemplo a seguir para criar uma equipe independente do switch conjunto denominada **VMSTEST** em 1 de Host do Hyper-V. Ambos os adaptadores de rede no computador, teste-40G-1 e teste-40G-2, são adicionados à equipe do conjunto com esse comando.
    
    New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
    
A seguir estão exemplos de resultados desse comando.

|Nome |SwitchType |NetAdapterInterfaceDescription|
|---- |---------- |------------------------------|
|VMSTEST |Externa |Interface agrupada|

Você pode usar esse comando para exibir a equipe de adaptador físico em conjunto.

    Get-VMSwitchTeam -Name "VMSTEST" | fl

A seguir estão exemplos de resultados desse comando.

**Nome**: VMSTEST **Id**: ad9bb542-dda2-4450-a00e-f96d44bdfbec **NetAdapterInterfaceDescription**: {adaptador Ethernet Pro 3 Mellanox ConnectX, adaptador Ethernet Pro 3 Mellanox ConnectX #2} **TeamingMode**: SwitchIndependent **LoadBalancingAlgorithm**: dinâmico

#### <a name="display-two-views-of-the-host-vnic"></a>Exibir dois modos de exibição de vNIC o Host

Você pode usar os comandos a seguir para duas exibições diferentes do vNIC Host.

Você pode usar esse comando para exibir propriedades de vNIC o Host.

    Get-NetAdapter

A seguir estão exemplos de resultados desse comando.

| Nome | InterfaceDescription | ifIndex | Status | MacAddress | LinkSpeed | |------|---|---|---|---| | | vEthernet (VMSTEST) | Hyper-V Adaptador Ethernet virtual #2 | 28 | Backup | Gbps E4-1D-2D-07-40-71|80 |

Você pode usar esse comando para exibir as propriedades adicionais do vNIC Host.

    Get-VMNetworkAdapter -ManagementOS

A seguir estão exemplos de resultados desse comando.

|Nome |IsManagementOs |VMName |SwitchName |MacAddress |Status |EndereçosIP|
|----|--------------|------|----------|----------|------|-----------|
|
|VMSTEST|True |VMSTEST |E41D2D074071| {Okey}|&nbsp;|

#### <a name="test-the-network-connection-to-the-remote-vlan-101-adapter"></a>Teste a conexão de rede ao adaptador de 101 VLAN remoto

Você pode usar esse comando para testar a conexão ao adaptador de 101 VLAN remoto.

`Test-NetConnection 192.168.1.5` 

A seguir estão exemplos de resultados desse comando.

    WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (CORP-External-Switch)
    SourceAddress  : 10.199.48.170
    PingSucceeded  : False
    PingReplyDetails (RTT) : 0 ms
    
### <a name="reconfigure-vlans"></a>Reconfigurar VLANs

Você pode usar esta etapa para remover a configuração de acesso VLAN do NIC física e definir o VLANID usando o vSwitch.

Você deve remover a configuração de acesso VLAN para impedir que os dois marcação automática o tráfego de saída com a ID de VLAN incorretos e filtragem ingresso de tráfego que não corresponde a ID de VLAN de acesso.

Você pode usar os comandos a seguir para remover a configuração de acesso VLAN.
    
    Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
    Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
    

Você pode usar os comandos a seguir para definir o VLANID usando os comandos do vSwitch específicos do Windows PowerShell e exibir os resultados dessa ação.

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
    Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
    

A seguir estão exemplos de resultados desse comando.

VMName VMNetworkAdapterName modo VlanList
------ -------------------- ----   --------
       VMSTEST              Access 101     

Você pode usar o seguinte comando para testar a conexão de rede e exibir os resultados.

    Test-NetConnection 192.168.1.5
    
    ComputerName   : 192.168.1.5
    RemoteAddress  : 192.168.1.5
    InterfaceAlias : vEthernet (VMSTEST)
    SourceAddress  : 192.168.1.3
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    

Se os resultados não são semelhantes aos exemplos de resultados e falhar com a mensagem "Aviso: Ping para 192.168.1.5 falha--Status: DestinationHostUnreachable," confirmar que o "vEthernet (VMSTEST)" tem o endereço IP adequado, executando o comando a seguir.

    
    Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
    

Se o endereço IP não estiver definido, você pode usar o comando a seguir para corrigir o problema e exibir os resultados das ações.

    
    New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
    
    IPAddress : 192.168.1.3
    InterfaceIndex: 37
    InterfaceAlias: vEthernet (VMSTEST)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Tentative
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : ActiveStore
    
#### <a name="rename-the-management-nic"></a>Renomeie o gerenciamento NIC

Você pode usar esta seção para renomear o NIC de gerenciamento e depois usar instâncias separadas de vNIC Host para RDMA.

Você pode executar os comandos a seguir para renomear o gerenciamento de NIC e exibir algumas propriedades NIC.

    Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
    Get-VMNetworkAdapter -ManagementOS

A seguir estão exemplos de resultados desse comando.

|Nome |IsManagementOs |VMName |SwitchName |MacAddress |Status |EndereçosIP
|----|--------------|------|----------|----------|------|-----------|
|
|CORP-External-Switch |True |&nbsp;|CORP-External-Switch |001B785768AA |{Okey}|&nbsp;|
|GERENCIAMENTO |True |&nbsp;|VMSTEST |E41D2D074071 |{Okey}|&nbsp;|

Você pode executar o comando a seguir para ver algumas propriedades adicionais do NIC.

    Get-NetAdapter

A seguir estão exemplos de resultados desse comando.

|Nome |InterfaceDescription |ifIndex |Status |MacAddress |LinkSpeed|
|----|--------------------|------|----------|---------|------|
|
|vEthernet (gerenciamento) |Adaptador Ethernet Virtual Hyper-V #2 |28 |Para cima | E4-1D-2D-07-40-71 |80 Gbps|

## <a name="test-hyper-v-virtual-switch-rdma"></a>Testar o comutador Virtual Hyper-V RDMA

A ilustração a seguir mostra o estado atual do seu hosts Hyper-V, incluindo o vSwitch em 1 de Host do Hyper-V.

![Testar o comutador Virtual Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

### <a name="set-priority-tagging-on-the-host-vnic-to-complement-the-previous-vlan-settings"></a>Definir prioridade a marcação em vNIC o Host para complementar as configurações de VLAN anteriores

Você pode usar os comandos de exemplo a seguir para definir a marcação em vNIC o Host de prioridade e exibir os resultados das ações.
    
    Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
    

    Name: MGT
    IeeePriorityTag : On
    
### <a name="create-two-host-vnics-for-rdma"></a>Criar dois vNICs de Host para RDMA

Você pode usar os comandos de exemplo a seguir para criar dois vNICs de host para RDMA e conectá-los ao vSwitch VMSTEST.
    
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
    Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
    
Você pode usar o comando de exemplo a seguir para exibir as propriedades de NIC de gerenciamento.
    
    Get-VMNetworkAdapter -ManagementOS
    
A seguir estão exemplos de resultados desse comando.

|Nome |IsManagementOs |VMName |SwitchName |MacAddress |Status |EndereçosIP|
|----|--------------|------|----------|----------|------|-----------|
|
|CORP-External-Switch |True |CORP-External-Switch |001B785768AA|{Okey} |&nbsp;| 
|Gerenciamento |True |VMSTEST |E41D2D074071 |{Okey} |&nbsp;|
|SMB1 |True |VMSTEST |00155D30AA00 |{Okey} |&nbsp;|
|SMB2 |True |VMSTEST |00155D30AA01 |{Okey} |&nbsp;|

### <a name="assign-an-ip-address-to-the-smb-host-vnics"></a>Atribuir um endereço IP para o Host SMB vNICs

Você pode usar esta seção para atribuir addressrd IP para o Host SMB vNICs vEthernet \(SMB1\) e vEthernet \(SMB2\).

O teste-40G-1 e adaptadores físicos 40G-teste-2 ainda tem uma VLAN de acesso de 101 e 102 configurado. Por isso, os adaptadores de marcam o tráfego - e ping tiver êxito.

Anteriormente, você configurado as IDs de VLAN pNIC como zero, e depois configurar o vSwitch VMSTEST para VLAN 101. Depois disso, você conseguiu ainda ping o adaptador VLAN 101 remoto usando o gerenciamento vNIC, mas no momento, não há nenhum membro VLAN 102.

Agora você pode usar o comando de exemplo a seguir para remover a configuração de acesso VLAN de NIC física para evitar que os dois marcação automática o tráfego de saída com a ID de VLAN incorretos e para impedir a filtragem de tráfego de entrada que não corresponda a ID de VLAN de acesso.

Você pode usar o comando de exemplo a seguir para realizar essas metas por addtion um novo endereço IP à interface vEthernet (SMB1) e exibir os resultados.

    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
    
    IPAddress : 192.168.2.111
    InterfaceIndex: 40
    InterfaceAlias: vEthernet (SMB1)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Invalid
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : PersistentStore
    

Você pode usar o comando de exemplo a seguir para testar o adaptador VLAN 102 remoto e exibir os resultados.
    
    Test-NetConnection 192.168.2.5 
    
    ComputerName   : 192.168.2.5
    RemoteAddress  : 192.168.2.5
    InterfaceAlias : vEthernet (SMB1)
    SourceAddress  : 192.168.2.111
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
Você pode usar o comando de exemplo a seguir para adicionar um novo endereço IP para interface vEthernet \(SMB2\) e exibir os resultados.
    
    New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
    
    IPAddress : 192.168.2.222
    InterfaceIndex: 44
    InterfaceAlias: vEthernet (SMB2)
    AddressFamily : IPv4
    Type  : Unicast
    PrefixLength  : 24
    PrefixOrigin  : Manual
    SuffixOrigin  : Manual
    AddressState  : Invalid
    ValidLifetime : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource  : False
    PolicyStore   : PersistentStore
    
Você não precisa testar a conexão novamente porque comunicação é estabelecida.

### <a name="place-the-rdma-host-vnics-on-vlan-102"></a>Coloque o vNICs RDMA Host em VLAN 102

Você pode usar esta seção para atribuir o vNICs RDMA Host a VLAN 102.

Porque o Host de "Gerenciamento" vNIC está localizado em VLAN 101, você pode usar os comandos de exemplo a seguir para colocar o vNICs RDMA Host na 102 VLAN preexistente e exibir os resultados das ações.

    
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
    Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS
    
    Get-VMNetworkAdapterVlan -ManagementOS
    
    VMName VMNetworkAdapterName Mode VlanList
    ------ -------------------- ---- --------
       SMB1 Access   102 
       Mgt  Access   101 
       SMB2 Access   102 
       CORP-External-Switch Untagged
    
### <a name="inspect-the-mapping-of-smb1-and-smb2-to-the-underlying-physical-nics"></a>Inspecionar o mapeamento de SMB1 e o SMB2 para as placas de NIC físicas subjacente

Você pode usar esta seção para inspecionar o mapeamento de SMB1 e o SMB2 para a placas NIC físicas subjacente sob o vSwitch equipe definido.  A associação de Host vNIC para placas NIC físicas é aleatório e sujeito a redistribuição durante a criação e destruição.

Neste caso, você pode usar um mecanismo indireto para verificar a associação atual.

Os endereços MAC de SMB1 e o SMB2 são associados o membro da equipe de NIC TEST-40G-2. Isso não é ideal porque 40G-teste-1 não tem um vNIC Host SMB associado e não permitirá para utilização de tráfego RDMA sobre o link até que um vNIC Host SMB é mapeado para ele.

Você pode usar os comandos de exemplo a seguir para exibir essas informações.

    
    Get-NetAdapterVPort (Preferred)
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
    
    Get-VMNetworkAdapter -ManagementOS
    
    Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
    ---- -------------- ------ ----------   ----------   ------ -----------
    CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
    Mgt  True  VMSTEST  E41D2D074071 {Ok}  
    SMB1 True  VMSTEST  00155D30AA00 {Ok}  
    SMB2 True  VMSTEST  00155D30AA01 {Ok}  
    

Ambos os comandos a seguir não devem retornar nenhuma informação porque você ainda não executou mapeamento.
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
    
Você pode usar os comandos de exemplo a seguir para mapear o SMB1 e o SMB2 para separar os membros da equipe NIC físicos e para exibir os resultados das ações.

>[!IMPORTANT]
>Certifique-se de que você concluir essa etapa antes de prosseguir, ou sua implementação falhará.
    
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
    Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"
    
    Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
    
    NetAdapterName : Test-40G-1
    NetAdapterDeviceId : {BAA9A00F-A844-4740-AA93-6BD838F8CFBA}
    ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB1'
    IsTemplate : False
    CimSession : CimSession: .
    ComputerName   : 27-3145G0803
    IsDeleted  : False
    
    NetAdapterName : Test-40G-2
    NetAdapterDeviceId : {B7AB5BB3-8ACB-444B-8B7E-BC882935EBC8}
    ParentAdapter  : VMInternalNetworkAdapter, Name = 'SMB2'
    IsTemplate : False
    CimSession : CimSession: .
    ComputerName   : 27-3145G0803
    IsDeleted  : False
    
### <a name="confirm-the-mac-associations"></a>Confirme as associações MAC

Você pode usar os comandos de exemplo a seguir para confirmar as associações de MAC, que você criou anteriormente.
    
    Get-NetAdapterVmqQueue
    
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    

Como ambas as vNICs Host residir na mesma sub-rede e têm o mesmo \(102\) VLAN ID, você pode validar a comunicação do sistema remoto e exibir os resultados das ações.

>[!IMPORTANT]
>Execute os seguintes comandos no computador remoto.

    
    Test-NetConnection 192.168.2.111
    
    
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    
    Test-NetConnection 192.168.2.222
    
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    
        
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    
    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    

    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    
    
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
      

### <a name="validate-rdma-functionality-from-the-remote-system"></a>Validar a funcionalidade RDMA do sistema remoto

Você pode usar esta seção para validar a funcionalidade RDMA do sistema remoto para o sistema local, que tem um vSwitch, teste, assim, ambos os adaptadores são membros da equipe do conjunto de vSwitch.

Como ambas as vNICs Host \ (SMB1 e SMB2\) são atribuídos a VLAN 102, você pode selecionar o adaptador VLAN 102 no sistema remoto.  

Nesse processo, o teste-40G-2 de NIC não RDMA a SMB1 (192.168.2.111) e o SMB2 (192.168.2.222).

Opcional: Talvez seja necessário desabilitar o Firewall no sistema.  Consulte a política de malha para obter detalhes.

    
    Set-NetFirewallProfile -All -Enabled False
    
    Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
    
    Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
    ----  -----------------------   --------------- -------------  
    .
    .
    Test-40G-2VLAN ID102VlanID  {102} 
    
    Get-NetAdapter
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
    
    
    Get-NetAdapterRdma
    
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter Test-40G-2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.111, is reachable.
    VERBOSE: Remote IP 192.168.2.111 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 34251744 RDMA bytes sent per second
    VERBOSE: 967346308 RDMA bytes written per second
    VERBOSE: 35698177 RDMA bytes sent per second
    VERBOSE: 976601842 RDMA bytes written per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.111
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter Test-40G-2 is a physical adapter
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.222, is reachable.
    VERBOSE: Remote IP 192.168.2.222 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 485137693 RDMA bytes written per second
    VERBOSE: 35200268 RDMA bytes sent per second
    VERBOSE: 939044611 RDMA bytes written per second
    VERBOSE: 34880901 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.222
    
### <a name="test-for-rdma-traffic-from-the-local-to-the-remote-computer"></a>Teste de tráfego RDMA de local para o computador remoto

Nesta seção, você pode verificar que o tráfego RDMA é enviado do computador local para o computador remoto.

Você pode usar os comandos de exemplo a seguir para testar e exibir o fluxo de tráfego.

    
    Get-NetAdapter | ft –AutoSize
    
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (SMB1) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
    VERBOSE: Remote IP 192.168.2.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 15250197 RDMA bytes sent per second
    VERBOSE: 896320913 RDMA bytes written per second
    VERBOSE: 33947559 RDMA bytes sent per second
    VERBOSE: 912160540 RDMA bytes written per second
    VERBOSE: 34091930 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
    
    
    C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
    VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\\diskspd.exe
    VERBOSE: The adapter vEthernet (SMB2) is a virtual adapter
    VERBOSE: Retrieving vSwitch bound to the virtual adapter
    VERBOSE: Found vSwitch: VMSTEST
    VERBOSE: Found the following physical adapter(s) bound to vSwitch: TEST-40G-1, TEST-40G-2
    VERBOSE: Underlying adapter is RoCE. Checking if QoS/DCB/PFC is configured on each physical adapter(s)
    VERBOSE: QoS/DCB/PFC configuration is correct.
    VERBOSE: RDMA configuration is correct.
    VERBOSE: Checking if remote IP address, 192.168.2.5, is reachable.
    VERBOSE: Remote IP 192.168.2.5 is reachable.
    VERBOSE: Disabling RDMA on adapters that are not part of this test. RDMA will be enabled on them later.
    VERBOSE: Testing RDMA traffic now for. Traffic will be sent in a parallel job. Job details:
    VERBOSE: 0 RDMA bytes written per second
    VERBOSE: 0 RDMA bytes sent per second
    VERBOSE: 385169487 RDMA bytes written per second
    VERBOSE: 33902277 RDMA bytes sent per second
    VERBOSE: 907354685 RDMA bytes written per second
    VERBOSE: 33923662 RDMA bytes sent per second
    VERBOSE: Enabling RDMA on adapters that are not part of this test. RDMA was disabled on them prior to sending RDMA traffic.
    VERBOSE: RDMA traffic test SUCCESSFUL: RDMA traffic was sent to 192.168.2.5
    
A linha final nessa saída, "teste de tráfego RDMA bem-sucedida: RDMA tráfego foi enviado para 192.168.2.5," demonstra que você configurou o NIC convergidos com êxito no seu adaptador.

## <a name="all-topics-in-this-guide"></a>Todos os tópicos neste guia

Este guia contém os tópicos a seguir.

- [Configuração de NIC convergido com um único adaptador de rede](cnic-single.md)
- [Configuração de NIC emparelhados NIC convergido](cnic-datacenter.md)
- [Configuração do comutador físico para NIC convergido](cnic-app-switch-config.md)
- [Solução de problemas de configurações de NIC convergidos](cnic-app-troubleshoot.md)
