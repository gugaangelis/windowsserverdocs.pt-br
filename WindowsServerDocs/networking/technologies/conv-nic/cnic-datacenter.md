---
title: NIC convergida em uma configuração do NIC agrupado (datacenter)
description: Neste tópico, fornecemos instruções para implantar o NIC convergida em uma configuração de NIC agrupado com o Switch Embedded Teaming (SET).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/17/2018
ms.openlocfilehash: 5f99600e24c62da9bdf674897dbadde9246b7bb7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821027"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>NIC convergida em uma configuração do NIC agrupado (datacenter)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, nós fornecemos instruções para implantar o NIC convergida em uma configuração do NIC agrupado com agrupamento incorporado do comutador \(definir\). 

A configuração de exemplo neste tópico descreve dois hosts Hyper-V, **Host Hyper-V 1** e **Host Hyper-V 2**. Ambos os hosts têm dois adaptadores de rede. Em cada host, um adaptador está conectado à sub-rede 192.168.1.x/24 e um adaptador está conectado à sub-rede 192.168.2.x/24.

![Hosts do Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Etapa 1. Testar a conectividade entre a origem e destino

Certifique-se de que o físico NIC pode se conectar ao host de destino.  Esse teste demonstra a conectividade com o uso de camada 3 \(L3\) – ou a camada IP –, bem como a camada 2 \(L2\) redes locais virtuais \(VLAN\).

1. Exiba propriedades do adaptador de rede primeiro.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
  
   _**Resultados:**_

   |Nome|InterfaceDescription|ifIndex|Status|MacAddress|LinkSpeed|
   |-----|--------------------|-------|-----|----------|---------|
   |Test-40G-1|Adaptador de Ethernet de Mellanox ConnectX-3 Pro|11|Para cima|E4-1D-2D-07-43-D0|40 Gbps|
   ---
   
2. Exibir propriedades adicionais para o primeiro adaptador, incluindo o endereço IP. 

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```
   
   _**Resultados:**_

   |Parâmetro|Valor|
   |---------|-----|
   |IPAddress| 192.168.1.3|
   |InterfaceIndex|11|
   |InterfaceAlias|Test-40G-1|
   |AddressFamily|IPv4|
   |Tipo| Unicast|
   |PrefixLength|24|
   ---

3. Exiba propriedades do adaptador de rede de segundo.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```
   
   _**Resultados:**_

   |Nome |InterfaceDescription |ifIndex |Status |MacAddress |LinkSpeed|
   |----|--------------------|-------|------|----------|---------|
   |TEST-40G-2 |Ethernet do Mellanox ConnectX-3 Pro r... n º 2 |13 |Para cima |E4-1D-2D-07-40-70 |40 Gbps|
   ---
   
4. Exibir propriedades adicionais para o segundo adaptador, incluindo o endereço IP.

   ```PowerShell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```
   
   _**Resultados:**_

   |Parâmetro|Valor|
   |---------|-----|
   |IPAddress|192.168.2.3|
   |InterfaceIndex|13|
   |InterfaceAlias|TEST-40G-2|
   |AddressFamily|IPv4|
   |Tipo|Unicast|
   |PrefixLength|24|
   ---

5. Verifique se que esse outro agrupamento NIC ou conjunto membro pNICs tem um endereço IP válido.<p>Use uma sub-rede separada \(xxx.xxx. **2**xxx.xxx de vs. xxx. **1**xxx\), para facilitar o envio desse adaptador para o destino. Caso contrário, se você localizar os dois pNICs na mesma sub-rede, faz o balanceamento de carga de pilha de TCP/IP do Windows entre as interfaces e simples de validação se torna mais complicado.


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Etapa 2. Certifique-se de origem e destino podem se comunicar

Nesta etapa, usamos o **Test-NetConnection** comando do Windows PowerShell, mas se você pode usar o **ping** comando se você preferir. 

1. Verifique se a comunicação bidirecional.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```
   
   _**Resultados:**_

   |Parâmetro|Valor|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|Test-40G-1|
   |SourceAddress|192.168.1.3|
   |PingSucceeded|False|
   |PingReplyDetails \(RTT\)|0 ms|
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
   
   _**Resultados:**_

   |Parâmetro|Valor|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|Test-40G-1|
   |SourceAddress|192.168.1.3|
   |PingSucceeded|False|
   |PingReplyDetails \(RTT\)|0 ms|
   ---
   
   
4. Verifique a conectividade para NICs adicionais. Repita as etapas anteriores para todos os pNICs subsequentes na equipe NIC ou um conjunto.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```
   
   _**Resultados:**_

   |Parâmetro|Valor|
   |---------|-----|
   |ComputerName|192.168.2.5|
   |RemoteAddress|192.168.2.5|
   |InterfaceAlias|Test-40G-2|
   |SourceAddress|192.168.2.3|
   |PingSucceeded|False|
   |PingReplyDetails \(RTT\)|0 ms|
   ---

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Etapa 3. Configurar as IDs de VLAN para NICs instaladas em seus hosts do Hyper-V

Várias configurações de rede faça uso de VLANs e se você estiver planejando usar VLANs em sua rede, você deve repetir o teste anterior com VLANs configurados.

Para esta etapa, as NICs estão em **acesso** modo. No entanto, quando você cria um comutador Virtual do Hyper-V \(vSwitch\) neste documento, as propriedades VLAN são aplicadas no nível de porta vSwitch. 

Como um comutador pode hospedar várias VLANs, é necessário para o topo de Rack \(ToR\) comutador físico para ter a porta que o host está conectado ao configurado no modo de tronco.

>[!NOTE]
>Consulte a documentação do comutador ToR para obter instruções sobre como configurar o modo de tronco no comutador.

A imagem a seguir mostra dois hosts Hyper-V com dois adaptadores de rede cada que têm VLAN 101 e 102 VLAN configurado nas propriedades do adaptador de rede.

![Configurar redes locais virtuais](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>Acordo com o Institute of Electrical and Electronics Engineers \(IEEE\) padrões, a qualidade do serviço de rede \(QoS\) propriedades na NIC físico atuam no cabeçalho 802.1 p incorporado dentro de 802.1Q \(VLAN\) cabeçalho quando você configura o ID. VLAN

1. Configure a ID de VLAN em teste 1 de 40G, a primeira NIC.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**Resultados:**_   

   |Nome |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |TEST-40G-1|ID DA VLAN|101|VlanID|{101}|
   ---

2. Reinicie o adaptador de rede para aplicar o ID. VLAN

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-1"
   ```
   
3. Verificar o Status é **backup**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
   
   _**Resultados:**_

   |Nome|InterfaceDescription|ifIndex| Status|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |Test-40G-1|Ethernet de Mellanox ConnectX-3 Pro Ada...|11|Para cima|E4-1D-2D-07-43-D0|40 Gbps|
   ---
    
4. Configure a ID de VLAN na segunda NIC, teste-40G-2.

   ```PowerShell    
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ``` 

   _**Resultados:**_

   |Nome |DisplayName| DisplayValue| RegistryKeyword |RegistryValue|
   |----|-----------|------------|---------------|-------------|
   |TEST-40G-2|ID DA VLAN|102|VlanID|{102}|
   ---
   
5. Reinicie o adaptador de rede para aplicar o ID. VLAN

   ```PowerShell
   Restart-NetAdapter -Name "Test-40G-2" 
   ```
   
6. Verificar o Status é **backup**.

   ```PowerShell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```
   
   _**Resultados:**_

   |Nome|InterfaceDescription|ifIndex| Status|MacAddress|LinkSpeed|
   |----|--------------------|-------|------|----------| ---------|
   |Test-40G-2 |Ethernet de Mellanox ConnectX-3 Pro Ada... |11 |Para cima |E4-1D-2D-07-43-D1 |40 Gbps|
   ---

   >[!IMPORTANT]
   >Pode levar vários segundos para o dispositivo reiniciar e se tornar disponíveis na rede. 
   
7. Verifique a conectividade para o primeiro adaptador de rede, teste-40G-1.<p>Se a conectividade falhar, inspecione o participação de configuração ou o destino de VLAN na mesma VLAN do comutador. 

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```

   _**Resultados:**_   

   |Parâmetro|Valor|
   |---------|-----|
   |ComputerName|192.168.1.5|
   |RemoteAddress|192.168.1.5|
   |InterfaceAlias|Test-40G-1|
   |SourceAddress|192.168.1.5|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 ms|
   ---
   
8. Verifique a conectividade para o primeiro adaptador de rede, teste-40G-2.<p>Se a conectividade falhar, inspecione o participação de configuração ou o destino de VLAN na mesma VLAN do comutador.

   ```PowerShell    
   Test-NetConnection 192.168.2.5
   ```

   _**Resultados:**_    

   |Parâmetro|Valor|
   |---------|-----|
   |ComputerName|192.168.2.5|
   |RemoteAddress|192.168.2.5|
   |InterfaceAlias|Test-40G-2|
   |SourceAddress|192.168.2.3|
   |PingSucceeded|True|
   |PingReplyDetails \(RTT\)|0 ms|
   ---
   
   >[!IMPORTANT]
   >Não é incomum para um **Test-NetConnection** ou a falha de ping para ocorrer imediatamente após a realização **Restart-NetAdapter**.  Portanto, aguarde até que o adaptador de rede ao inicializar completamente e, em seguida, tente novamente.
   >
   >Se as conexões de VLAN 101 funcionam, mas não as conexões de 102 VLAN, o problema pode ser que a opção precisa ser configurado para permitir o tráfego da porta na VLAN desejada. Você pode verificar isso temporariamente definindo os adaptadores de falha para 101 VLAN e repetir o teste de conectividade.

   
   A imagem a seguir mostra os hosts do Hyper-V depois de configurar com êxito a VLANs.

   ![Configurar qualidade de serviço](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)
   
   
## <a name="step-4-configure-quality-of-service-qos"></a>Etapa 4. Configurar qualidade de serviço \(QoS\)

>[!NOTE]
>Você deve executar todas as seguintes etapas de configuração do DCB e QoS em todos os hosts que se destinam a se comunicar entre si.

1. Instalar o Data Center Bridging \(DCB\) em cada um dos seus hosts Hyper-V.

   - **Opcional** para as configurações de rede que usam iWarp.
   - **Exigido** para as configurações de rede que usam RoCE \(qualquer versão\) para serviços RDMA.

   ```PowerShell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**Resultados:**_

   |Êxito |Reinicialização necessária |Código de Saída|Resultado do recurso|
   |------- |-------------- |--------- |-------------- |
   |True |Não |Êxito| {Data Center Bridging}|
   ---
   
2. Defina as políticas de QoS para SMB Direct:

   - **Opcional** para as configurações de rede que usam iWarp.
   - **Exigido** para as configurações de rede que usam RoCE \(qualquer versão\) para serviços RDMA.
   
   No comando de exemplo a seguir, o valor "3" é arbitrário. Você pode usar qualquer valor entre 1 e 7 desde que você usar consistentemente o mesmo valor em toda a configuração das políticas de QoS.

   ```PowerShell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```
   
   _**Resultados:**_

   |Parâmetro|Valor|
   |---------|-----|
   |Nome |SMB|
   |Proprietário|Política de grupo \(máquina\)|
   |NetworkProfile|Todas|
   |Precedência|127|
   |JobObject|&nbsp;| 
   |NetDirectPort|445
   |PriorityValue|3
   ---

3. Defina políticas adicionais de QoS para outro tráfego na interface.   

   ```PowerShell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**Resultados:**_   

   |Parâmetro|Valor|
   |---------|-----|
   |Nome | DEFAULT|
   |Proprietário|Política de grupo \(máquina\)|
   |NetworkProfile|Todas|
   |Precedência|127|
   |Modelo| Padrão|
   |JobObject| &nbsp;|
   |PriorityValue|0|
   ---
   
4. Ative **controle de fluxo de prioridade** para o tráfego SMB, que não é necessário para iWarp.

   ```PowerShell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```
   
   _**Resultados:**_
   
   |Priority|Enabled|PolicySet|IfIndex|IfAlias|
   |---------|-----|--------- |-------| -------|
   |0 |False |Global|&nbsp;|&nbsp;|
   |1 |False |Global|&nbsp;|&nbsp;|
   |2 |False |Global|&nbsp;|&nbsp;|
   |3 |True |Global|&nbsp;|&nbsp;|
   |4 |False |Global|&nbsp;|&nbsp;|
   |5 |False |Global|&nbsp;|&nbsp;|
   |6 |False |Global|&nbsp;|&nbsp;|
   |7 |False |Global|&nbsp;|&nbsp;|
   ---
   
   >**IMPORTANTE** se os resultados não corresponderem a esses resultados como itens que não sejam 3 têm um valor Enabled True, você deve desabilitar **controle de fluxo** para essas classes.
   >
   >```PowerShell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >Em configurações mais complexas, outras classes de tráfego podem exigir o controle de fluxo, no entanto, esses cenários estão fora do escopo deste guia.


5. Habilite a QoS para o primeiro adaptador de rede, teste-40G-1.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1 
   Enabled: True
   ```
   _**Recursos**:_   

   |Parâmetro|Hardware|Atual|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|Nenhuma|Nenhuma|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---
 
   _**OperationalTrafficClasses**:_    

   |TC|TSA|Largura de banda|Prioridades|
   |----|-----|--------|-------|
   |0| Estrito|&nbsp;|0-7|
   ---

   _**OperationalFlowControl**:_  

   Prioridade 3 habilitado  

   _**OperationalClassifications**:_  

   |Protocolo|Porta/tipo|Priority|
   |--------|---------|--------|
   |Padrão|&nbsp;|0|
   |NetDirect| 445|3|
   ---
   
6. Habilite a QoS para a segunda NIC, teste-40G-2.

   ```PowerShell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2 
   Enabled: True 
   ```

   _**Recursos**:_ 

   |Parâmetro|Hardware|Atual|
   |---------|--------|-------|
   |MacSecBypass|NotSupported|NotSupported|
   |DcbxSupport|Nenhuma|Nenhuma|
   |NumTCs(Max/ETS/PFC)|8/8/8|8/8/8|
   ---

   _**OperationalTrafficClasses**:_  

   |TC|TSA|Largura de banda|Prioridades|
   |----|-----|--------|-------|
   |0| Estrito|&nbsp;|0-7|
   ---
   
    _**OperationalFlowControl**:_  

    Prioridade 3 habilitado  
   
   _**OperationalClassifications**:_  

   |Protocolo|Porta/tipo|Priority|
   |--------|---------|--------|
   |Padrão|&nbsp;|0|
   |NetDirect| 445|3|
   ---

   
7. Reserva de meia largura de banda para SMB Direct \(RDMA\)

   ```PowerShell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```
   
   _**Resultados:**_  
   
   |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |SMB | ETS     | 50 |3 |Global |&nbsp;|&nbsp;|   
   ---

8. Exiba as configurações de reserva de largura de banda:   

   ```PowerShell
   Get-NetQosTrafficClass | ft -AutoSize
   ```
   
   _**Resultados:**_  

   |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |[Padrão]| ETS|50 |0-2,4-7|  Global|&nbsp;|&nbsp;| 
   |SMB |ETS|50 |3 |Global|&nbsp;|&nbsp;| 
   ---
   
9. (Opcional) Crie duas classes de tráfego adicional para o tráfego IP de locatário. 

   >[!TIP]
   >Você pode omitir os valores "IP1" e "IP2".

   ```PowerShell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```
   
   _**Resultados:**_
   
   |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |IP1 |ETS |10 |1 |Global|&nbsp;|&nbsp;|
   ---
   
   ```PowerShell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```
   
   _**Resultados:**_

   |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
   |----|---------| ------------ |--------| ---------|------- |------- |
   |IP2 |ETS |10 |2 |Global|&nbsp;|&nbsp;|
   ---
   
10. Exiba as classes de tráfego de QoS.

    ```PowerShell
    Get-NetQosTrafficClass | ft -AutoSize
    ```
    
    _**Resultados:**_

    |Nome|Algoritmo |Bandwidth(%)| Priority |PolicySet |IfIndex |IfAlias |
    |----|---------| ------------ |--------| ---------|------- |------- |
    |[Padrão] |ETS |30 |0,4-7 |Global|&nbsp;|&nbsp;|
    |SMB |ETS |50 |3 |Global|&nbsp;|&nbsp;|
    |IP1 |ETS |10 |1 |Global|&nbsp;|&nbsp;|
    |IP2 |ETS |10 |2 |Global|&nbsp;|&nbsp;|
    ---
   
11. (Opcional) Substitua o depurador.<p>Por padrão, o depurador anexado bloqueia NetQos. 

    ```PowerShell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**Resultados:**_  

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>Etapa 5. Verifique a configuração do RDMA \(modo 1\) 

Você deseja garantir que o fabric está configurado corretamente antes da criação de um vSwitch e fazer a transição para RDMA \(modo 2\).

A imagem a seguir mostra o estado atual dos hosts do Hyper-V.

![Teste RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. Verifique a configuração do RDMA.

   ```PowerShell
   Get-NetAdapterRdma | ft -AutoSize
   ```
   
   _**Resultados:**_

   |Nome |InterfaceDescription |Enabled|
   |----|--------------------|-------|
   |TEST-40G-1| Adaptador Mellanox ConnectX-4 VPI #2 |True|
   |TEST-40G-2| Adaptador Mellanox ConnectX-4 VPI |True|
   ---

2. Determinar a **ifIndex** valor dos seus adaptadores de destino.<p>Você pode usar esse valor nas próximas etapas quando você executa o script que você baixar.   

   ```PowerShell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```
   
   _**Resultados:**_

   |InterfaceAlias |InterfaceIndex |IPv4Address|
   |-------------- |-------------- |-----------|
   |TEST-40G-1 |14 |{192.168.1.3}|
   |TEST-40G-2 | 13 |{192.168.2.3}|
   ---
   
3. Baixe o [DiskSpd.exe utilitário](https://aka.ms/diskspd) e extraia-o em c:\test.\.

4. Baixe o [script do PowerShell Test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) para uma pasta de teste em sua unidade local, por exemplo, C:\TEST\.

5. Execute o **Rdma.ps1 teste** script do PowerShell para passar o valor de ifIndex para o script, junto com o endereço IP do primeiro adaptador remoto na mesma VLAN.<p>Neste exemplo, o script passa a **ifIndex** valor de 14 no endereço IP de adaptador de rede remota 192.168.1.5.
   
   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Resultados:**_ 
   
   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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

6. Execute o **Rdma.ps1 teste** script do PowerShell para passar o valor de ifIndex para o script, junto com o endereço IP do segundo adaptador remoto na mesma VLAN.<p>Neste exemplo, o script passa a **ifIndex** valor de 13 no endereço IP de adaptador de rede remota 192.168.2.5.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Resultados:**_ 
   
   ```   
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ``` 

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Etapa 6. Criar um vSwitch do Hyper-V em seus hosts do Hyper-V


A imagem a seguir mostra o Host Hyper-V 1 com um vSwitch.

![Criar um comutador Virtual do Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. Crie um vSwitch no modo de conjunto no host do Hyper-V 1.

   ```PowerShell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```
   
   _**Resultado:**_

   |Nome |SwitchType |NetAdapterInterfaceDescription|
   |---- |---------- |------------------------------|
   |VMSTEST |Externo |Teamed-Interface|
   ---
   
2. Exiba a equipe de adaptador físico no conjunto.

   ```PowerShell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```
   
   _**Resultados:**_  
   
   ```
   Name: VMSTEST  
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec  
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}  
   TeamingMode: SwitchIndependent  
   LoadBalancingAlgorithm: Dynamic   
   ```
   
3. Exibir dois modos de exibição da vNIC no host

   ```PowerShell
    Get-NetAdapter
   ```
   
   _**Resultados:**_

   |Nome |InterfaceDescription |ifIndex |Status |MacAddress |LinkSpeed|
   |---- |--------------------|-------|------|----------|---------|
   |vEthernet (VMSTEST)|Adaptador de Ethernet Virtual do Hyper-V #2 |28 |Para cima|E4-1D-2D-07-40-71|80 Gbps|
   ---
   
4. Exibir propriedades adicionais da vNIC no host. 

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Resultados:**_

   |Nome |IsManagementOs |VMName |SwitchName |MacAddress |Status |IPAddresses|
   |----|--------------|------|----------|----------|------|-----------|
   |VMSTEST|True |VMSTEST |E41D2D074071| {Ok}|&nbsp;|
   ---
   

5. Teste a conexão de rede ao adaptador de VLAN 101 remoto.

   ```PowerShell
   Test-NetConnection 192.168.1.5 
   ```
   
   _**Resultados:**_  
   
   ```
   WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable
    
   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (CORP-External-Switch)
   SourceAddress  : 10.199.48.170
   PingSucceeded  : False
   PingReplyDetails (RTT) : 0 ms
   ```
   
## <a name="step-7-remove-the-access-vlan-setting"></a>Etapa 7. Remova a configuração de VLAN de acesso

Nesta etapa, você remover a configuração de VLAN de acesso da NIC físico e para definir o VLANID usando vSwitch.

Você deve remover a configuração de VLAN de acesso para impedir que os dois marcação automática o tráfego de saída com a ID de VLAN incorreta e da filtragem de entrada de tráfego que não corresponde à ID da VLAN o acesso.

1. Remova a configuração.
    
   ```PowerShell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
   ```

2. Definir a ID de VLAN.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```
   
   _**Resultados:**_  
   
   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101     
   ```
   
   
3. Teste a conexão de rede.

   ```PowerShell
   Test-NetConnection 192.168.1.5
   ```
   
   _**Resultados:**_   
   
   ```
   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (VMSTEST)
   SourceAddress  : 192.168.1.3
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

   >**IMPORTANTE** se seus resultados não são semelhantes aos resultados do exemplo e ping falha com a mensagem "Aviso: Falha de ping para 192.168.1.5--Status: DestinationHostUnreachable,"confirme se o"vEthernet (VMSTEST)"tem o endereço IP apropriado.
   >
   >```PowerShell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >Se o endereço IP não for definido, corrija o problema.
   >
   >```PowerShell
   >New-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)" -IPAddress 192.168.1.3 -PrefixLength 24
   >
   >IPAddress : 192.168.1.3
   >InterfaceIndex: 37
   >InterfaceAlias: vEthernet (VMSTEST)
   >AddressFamily : IPv4
   >Type  : Unicast
   >PrefixLength  : 24
   >PrefixOrigin  : Manual
   >SuffixOrigin  : Manual
   >AddressState  : Tentative
   >ValidLifetime : Infinite ([TimeSpan]::MaxValue)
   >PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
   >SkipAsSource  : False
   >PolicyStore   : ActiveStore
   >```  
   

4. Renomeie o gerenciamento de NIC.

   ```PowerShell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Resultados:**_ 
   
   |Nome |IsManagementOs |VMName |SwitchName |MacAddress |Status |IPAddresses
   |----|--------------|------|----------|----------|------|-----------|
   |CORP-External-Switch |True |&nbsp;|CORP-External-Switch |001B785768AA |{Ok}|&nbsp;|
   |MGT |True |&nbsp;|VMSTEST |E41D2D074071 |{Ok}|&nbsp;|
   ---
   
5. Exibir propriedades adicionais da NIC.

   ```PowerShell
   Get-NetAdapter
   ```
   
   _**Resultados:**_

   |Nome |InterfaceDescription |ifIndex |Status |MacAddress |LinkSpeed|
   |----|--------------------|------|----------|---------|------|
   |vEthernet (MGT) |Adaptador de Ethernet Virtual do Hyper-V #2 |28 |Para cima | E4-1D-2D-07-40-71 |80 Gbps|
   ---
   
## <a name="step-8-test-hyper-v-vswitch-rdma"></a>Etapa 8. Teste do Hyper-V vSwitch RDMA

A imagem a seguir mostra o estado atual de seus hosts do Hyper-V, incluindo o vSwitch no Host Hyper-V 1.

![Testar o comutador Virtual do Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. Defina a prioridade de marcação na vNIC do Host para complementar as configurações de VLAN anteriores.

   ```PowerShell    
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```
   
   _**Resultados:**_  
      
   nome: MGT  
   IeeePriorityTag:  Ativado  
    
2. Crie dois vNICs do host para RDMA e conectá-los ao vSwitch VMSTEST.

   ```PowerShell    
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```
   
3. Exiba as propriedades de NIC de gerenciamento.

   ```PowerShell    
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Resultados:**_ 

   |Nome |IsManagementOs |VMName |SwitchName |MacAddress |Status |IPAddresses|
   |----|--------------|------|----------|----------|------|-----------|
   |CORP-External-Switch |True |CORP-External-Switch |001B785768AA|{Ok} |&nbsp;| 
   |Mgt |True |VMSTEST |E41D2D074071 |{Ok} |&nbsp;|
   |SMB1 |True |VMSTEST |00155D30AA00 |{Ok} |&nbsp;|
   |SMB2 |True |VMSTEST |00155D30AA01 |{Ok} |&nbsp;|
   ---
   
## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>Etapa 9. Atribuir um endereço IP para o Host SMB vNICs vEthernet \(SMB1\) e vEthernet \(SMB2\)

O teste 1 de 40G e adaptadores físicos de teste-40G-2 ainda terão uma VLAN de acesso de 101 e 102 configurado. Por isso, os adaptadores de marcam o tráfego de - e ping tiver êxito. Anteriormente, você configurou ambos os IDs de VLAN pNIC como zero e defina o vSwitch VMSTEST para VLAN 101. Depois disso, era possível executar ping o adaptador de VLAN 101 remoto usando a vNIC MGT ainda, mas no momento, existem membros 102 VLAN.



1. Remova a configuração de VLAN de acesso a NIC física para impedir a ambos os marcação automática o tráfego de saída com a ID de VLAN incorreta e para impedir a filtragem de tráfego de entrada que não coincide com a ID de VLAN de acesso.

   ```PowerShell    
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**Resultados:**_  

   ```   
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
   ```

2. Teste o adaptador de VLAN 102 remoto.
    
   ```PowerShell
   Test-NetConnection 192.168.2.5 
   ```
   
   _**Resultados:**_  
   
   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```
    
3. Adicionar um novo endereço IP para interface vEthernet \(SMB2\).

   ```PowerShell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24 
   ```
   
   _**Resultados:**_ 
   
   ```
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
   ```
   
4. Teste a conexão novamente.    


5. Coloque as vNICs RDMA Host na 102 VLAN já existente.

   ```PowerShell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS
    
   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**Resultados:**_ 

   ```   
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102 
      Mgt  Access   101 
      SMB2 Access   102 
      CORP-External-Switch Untagged
   ```
   
6. Inspecione o mapeamento de SMB1 e SMB2 para as NICs físicas subjacentes em definir equipe vSwitch.<p>A associação de vNIC do Host a NICs físicas é aleatório e sujeitos a rebalanceamento durante a criação e destruição. Nessa circunstância, você pode usar um mecanismo indireto para verificar a associação atual. Os endereços MAC SMB1 e SMB2 estão associados com o membro da equipe de NIC teste 2 de 40G. Isso não é ideal porque o teste 1 de 40G não tem uma vNIC do Host SMB associada e não permitirá para utilização de tráfego RDMA sobre o link até que uma vNIC do Host SMB é mapeada para ele.

   ```PowerShell    
   Get-NetAdapterVPort (Preferred)
    
   Get-NetAdapterVmqQueue
   ```
   
   _**Resultados:**_ 
   
   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. Exiba propriedades do adaptador de rede de VM.

   ```PowerShell
   Get-VMNetworkAdapter -ManagementOS
   ```
   
   _**Resultados:**_ 
   
   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}  
   Mgt  True  VMSTEST  E41D2D074071 {Ok}  
   SMB1 True  VMSTEST  00155D30AA00 {Ok}  
   SMB2 True  VMSTEST  00155D30AA01 {Ok}  
   ```

8. Exiba o mapeamento de equipe de adaptador de rede.<p>Os resultados não devem retornar informações porque você ainda não executaram mapeamento.
    
   ```PowerShell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```
   
   
9. Mapear SMB1 e SMB2 para separar os membros da equipe NIC físicos e para exibir os resultados de suas ações.

   >[!IMPORTANT]
   >Certifique-se de que você concluir esta etapa antes de prosseguir, ou falha de sua implementação.
    
   ```PowerShell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"
    
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**Resultados:**_ 
   
   ```   
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
   ```
   
10. Confirme se as associações de MAC criadas anteriormente.

    ```PowerShell    
    Get-NetAdapterVmqQueue
    ```

    _**Resultados:**_ 
   
    ```   
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. Testar a conexão do sistema remoto, pois ambas as vNICs do Host residem na mesma sub-rede e têm a mesma ID de VLAN \(102\).

    ```PowerShell 
    Test-NetConnection 192.168.2.111
    ```

    _**Resultados:**_   

    ```
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    ```
    
    ```PowerShell   
    Test-NetConnection 192.168.2.222
    ```

    _**Resultados:**_   

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms 
    ```
12. Defina o nome, nome do comutador e marcas de prioridade.   

    ```PowerShell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**Resultados:**_   
    
    ```
    Name: SMB1
    SwitchName  : VMSTEST
    IeeePriorityTag : On 
    Status  : {Ok}
   
    Name: SMB2
    SwitchName  : VMSTEST
    IeeePriorityTag : On
    Status  : {Ok}
    ```

13. Exiba as propriedades do adaptador de rede vEthernet.
    
    ```PowerShell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**Resultados:**_   

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False  
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False   
    ```

14. Habilite os adaptadores de rede vEthernet.  
    
    ```PowerShell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**Resultados:**_   
    
    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True   
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True  
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>Etapa 10. Valide a funcionalidade RDMA.

Você deseja validar a funcionalidade RDMA do sistema remoto ao sistema local, que tem um vSwitch, para ambos os membros da equipe do conjunto de vSwitch.<p>Porque ambas as vNICs do Host \(SMB1 e SMB2\) são atribuídos a 102 de VLAN, você pode selecionar o adaptador de VLAN 102 no sistema remoto. <p>Neste exemplo, o NIC Test-40G-2 faz o RDMA para SMB1 (192.168.2.111) e SMB2 (192.168.2.222).

>[!TIP]
>Você talvez precise desabilitar o Firewall neste sistema.  Consulte a política de malha para obter detalhes.
>
>```PowerShell
>Set-NetFirewallProfile -All -Enabled False
>    
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**Resultados:**_ 
>   
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue  
>----  -----------------------   --------------- -------------  
> .
> .
>Test-40G-2VLAN ID102VlanID  {102} 
>```
    
1. Exiba as propriedades do adaptador de rede.

   ```PowerShell
   Get-NetAdapter
   ```
    
   _**Resultados:**_ 
    
   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```
   
2. Exiba informações de RDMA do adaptador de rede.

   ```PowerShell
   Get-NetAdapterRdma
   ```
    
   _**Resultados:**_  
    
   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True   
   ```

3. Execute o teste de tráfego RDMA no adaptador físico primeiro.

   ```PowerShell 
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Resultados:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```

4. Execute o teste de tráfego RDMA no segundo adaptador físico.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Resultados:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```
    
5. Teste para o tráfego RDMA do local para o computador remoto.

    ```PowerShell
    Get-NetAdapter | ft –AutoSize
    ```
    
    _**Resultados:**_ 
    
    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. Execute o teste de tráfego RDMA no adaptador virtual de primeira.    
    
   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Resultados:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```

7. Execute o teste de tráfego RDMA no adaptador virtual de segundo.

   ```PowerShell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```
    
   _**Resultados:**_ 
    
   ```
   VERBOSE: Diskspd.exe found at C:\TEST\Diskspd-v2.0.17\amd64fre\diskspd.exe
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
   ```
    
A linha final nessa saída, "teste de tráfego RDMA bem-sucedida: Tráfego RDMA foi enviado para 192.168.2.5,"mostra que você configurou com êxito NIC convergida no adaptador.

## <a name="related-topics"></a>Tópicos relacionados 

- [Configuração de NIC convergida com um único adaptador de rede](cnic-single.md)
- [Configuração de comutador físico para NIC convergida](cnic-app-switch-config.md)
- [Solução de problemas convergido configurações de NIC](cnic-app-troubleshoot.md)
