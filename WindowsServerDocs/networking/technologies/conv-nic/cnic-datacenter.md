---
title: NIC convergida em uma configuração de NIC agrupada (Datacenter)
description: Neste tópico, fornecemos instruções para implantar a NIC convergida em uma configuração de NIC agrupada com o switch Embedded Integration (SET).
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: f01546f8-c495-4055-8492-8806eee99862
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/17/2018
ms.openlocfilehash: d81e4013d7cc38a15dd8b0bcd48529a2d72d0b69
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87520195"
---
# <a name="converged-nic-in-a-teamed-nic-configuration-datacenter"></a>NIC convergida em uma configuração de NIC agrupada (Datacenter)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste tópico, fornecemos instruções para implantar a NIC convergida em uma configuração de NIC agrupada com o conjunto de agrupamentos incorporados de switch \( \) .

A configuração de exemplo neste tópico descreve dois hosts Hyper-v, o host **Hyper-v 1** e o **host do Hyper-v 2**. Ambos os hosts têm dois adaptadores de rede. Em cada host, um adaptador é conectado à sub-rede 192.168.1. x/24 e um adaptador é conectado à sub-rede 192.168.2. x/24.

![Hosts do Hyper-V](../../media/Converged-NIC/1-datacenter-test-conn.jpg)

## <a name="step-1-test-the-connectivity-between-source-and-destination"></a>Etapa 1. Testar a conectividade entre a origem e o destino

Verifique se a NIC física pode se conectar ao host de destino.  Esse teste demonstra a conectividade usando a camada 3 \( L3 \) -ou a camada de IP, bem como a VLAN de rede local virtual de camada 2 \( L2 \) \( \) .

1. Exiba as primeiras propriedades do adaptador de rede.

   ```powershell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Da**_


   |    Name    |           InterfaceDescription           | ifIndex | Status |    MacAddress     | LinkSpeed |
   |------------|------------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-1 | Adaptador Mellanox ConnectX-3 pro Ethernet |   11    |   Up   | E4-1D-2D-07-43-D0 |  40 Gbps  |

2. Exiba propriedades adicionais para o primeiro adaptador, incluindo o endereço IP.

   ```powershell
   Get-NetIPAddress -InterfaceAlias "Test-40G-1"
   Get-NetIPAddress -InterfaceAlias "TEST-40G-1" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**Da**_


   |   Parâmetro    |    Valor    |
   |----------------|-------------|
   |   IPAddress    | 192.168.1.3 |
   | InterfaceIndex |     11      |
   | AliasdeInterface | Test-40G-1  |
   | AddressFamily  |    IPv4     |
   |      Tipo      |   Unicast   |
   |  PrefixLength  |     24      |

3. Exiba as propriedades do segundo adaptador de rede.

   ```powershell
   Get-NetAdapter -Name "Test-40G-2" | ft -AutoSize
   ```

   _**Da**_


   |    Name    |          InterfaceDescription           | ifIndex | Status |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | TESTE-40G-2 | Mellanox ConnectX-3 pro Ethernet A... #2 |   13    |   Up   | E4-1D-2D-07-40-70 |  40 Gbps  |

4. Exiba propriedades adicionais para o segundo adaptador, incluindo o endereço IP.

   ```powershell
   Get-NetIPAddress -InterfaceAlias "Test-40G-2"
   Get-NetIPAddress -InterfaceAlias "Test-40G-2" | Where-Object {$_.AddressFamily -eq "IPv4"} | fl InterfaceAlias,IPAddress
   ```

   _**Da**_


   |   Parâmetro    |    Valor    |
   |----------------|-------------|
   |   IPAddress    | 192.168.2.3 |
   | InterfaceIndex |     13      |
   | AliasdeInterface | TESTE-40G-2  |
   | AddressFamily  |    IPv4     |
   |      Tipo      |   Unicast   |
   |  PrefixLength  |     24      |

5. Verifique se outra equipe NIC ou conjunto de membros pNICs tem um endereço IP válido.<p>Use uma sub-rede separada, \( xxx.xxx.** 2**. xxx vs xxx.xxx. **1**. xxx \) , para facilitar o envio desse adaptador para o destino. Caso contrário, se você localizar ambos os pNICs na mesma sub-rede, os balanceamentos de carga de pilha TCP/IP do Windows entre as interfaces e a validação simples se tornarão mais complicados.


## <a name="step-2-ensure-that-source-and-destination-can-communicate"></a>Etapa 2. Garantir que a origem e o destino possam se comunicar

Nesta etapa, usamos o comando **Test-NetConnect** do Windows PowerShell, mas se você preferir, poderá usar o comando **ping** .

1. Verifique a comunicação bidirecional.

   ```powershell
   Test-NetConnection 192.168.1.5
   ```

   _**Da**_


   |        Parâmetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      AliasdeInterface      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    Falso    |
   | RTT de PingReplyDetails \(\) |    0 ms     |

   Em alguns casos, talvez seja necessário desabilitar o Firewall do Windows com segurança avançada para executar esse teste com êxito. Se você desabilitar o firewall, mantenha a segurança em mente e verifique se sua configuração atende aos requisitos de segurança de sua organização.

2. Desabilitar todos os perfis de firewall.

   ```powershell
   Set-NetFirewallProfile -All -Enabled False
   ```

3. Depois de desabilitar os perfis de firewall, teste a conexão novamente.

   ```powershell
   Test-NetConnection 192.168.1.5
   ```

   _**Da**_


   |        Parâmetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      AliasdeInterface      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.3 |
   |      PingSucceeded       |    Falso    |
   | RTT de PingReplyDetails \(\) |    0 ms     |


4. Verifique a conectividade para NICs adicionais. Repita as etapas anteriores para todos os pNICs subsequentes incluídos na NIC ou no conjunto de equipe.

   ```powershell
   Test-NetConnection 192.168.2.5
   ```

   _**Da**_


   |        Parâmetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      AliasdeInterface      | Teste-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    Falso    |
   | RTT de PingReplyDetails \(\) |    0 ms     |

## <a name="step-3-configure-the-vlan-ids-for-nics-installed-in-your-hyper-v-hosts"></a>Etapa 3. Configurar as IDs de VLAN para NICs instaladas em seus hosts Hyper-V

Muitas configurações de rede fazem uso de VLANs e, se você estiver planejando usar VLANs em sua rede, deverá repetir o teste anterior com VLANs configuradas.

Para esta etapa, as NICs estão no modo de **acesso** . No entanto, quando você cria um comutador virtual do Hyper-V \( vSwitch \) posteriormente neste guia, as propriedades de VLAN são aplicadas no nível de porta vSwitch.

Como uma opção pode hospedar várias VLANs, é necessário que a parte superior da \( chave física ToR do rack \) tenha a porta que o host está conectado para ser configurada no modo de tronco.

>[!NOTE]
>Consulte a documentação do comutador ToR para obter instruções sobre como configurar o modo de tronco no comutador.

A imagem a seguir mostra dois hosts Hyper-V com dois adaptadores de rede cada um com VLAN 101 e VLAN 102 configurados nas propriedades do adaptador de rede.

![Configurar redes de área local virtual](../../media/Converged-NIC/2-datacenter-configure-vlans.jpg)


>[!TIP]
>De acordo com os padrões de rede IEEE do Instituto de engenheiros elétricos e eletrônicos \( \) , as propriedades de QoS de qualidade de serviço \( \) na NIC física agem no cabeçalho 802.1 p que é inserido no \( cabeçalho VLAN 802.1 q \) quando você configura a ID de VLAN.

1. Configure a ID de VLAN na primeira NIC, Test-40G-1.

   ```powershell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "101"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-1" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**Da**_


   |    Name    | DisplayName | DisplayValue | RegistryKeyword | Registrovalue |
   |------------|-------------|--------------|-----------------|---------------|
   | TEST-40G-1 |   ID DA VLAN   |     101      |     VlanID      |     {101}     |

2. Reinicie o adaptador de rede para aplicar a ID de VLAN.

   ```powershell
   Restart-NetAdapter -Name "Test-40G-1"
   ```

3. Verifique se o status está **ativo**.

   ```powershell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Da**_


   |    Name    |          InterfaceDescription           | ifIndex | Status |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | Test-40G-1 | Mellanox ConnectX-3 pro Ethernet Ada... |   11    |   Up   | E4-1D-2D-07-43-D0 |  40 Gbps  |

4. Configure a ID de VLAN na segunda NIC, Test-40G-2.

   ```powershell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "102"
   Get-NetAdapterAdvancedProperty -Name "Test-40G-2" | Where-Object {$_.RegistryKeyword -eq "VlanID"} | ft -AutoSize
   ```

   _**Da**_


   |    Name    | DisplayName | DisplayValue | RegistryKeyword | Registrovalue |
   |------------|-------------|--------------|-----------------|---------------|
   | TESTE-40G-2 |   ID DA VLAN   |     102      |     VlanID      |     {102}     |

5. Reinicie o adaptador de rede para aplicar a ID de VLAN.

   ```powershell
   Restart-NetAdapter -Name "Test-40G-2"
   ```

6. Verifique se o status está **ativo**.

   ```powershell
   Get-NetAdapter -Name "Test-40G-1" | ft -AutoSize
   ```

   _**Da**_


   |    Name    |          InterfaceDescription           | ifIndex | Status |    MacAddress     | LinkSpeed |
   |------------|-----------------------------------------|---------|--------|-------------------|-----------|
   | Teste-40G-2 | Mellanox ConnectX-3 pro Ethernet Ada... |   11    |   Up   | E4-1D-2D-07-43-D1 |  40 Gbps  |

   >[!IMPORTANT]
   >Pode levar vários segundos para o dispositivo reiniciar e ficar disponível na rede.

7. Verifique a conectividade da primeira NIC, Test-40G-1.<p>Se a conectividade falhar, inspecione a configuração do switch VLAN ou a participação de destino na mesma VLAN.

   ```powershell
   Test-NetConnection 192.168.1.5
   ```

   _**Da**_


   |        Parâmetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.1.5 |
   |      RemoteAddress       | 192.168.1.5 |
   |      AliasdeInterface      | Test-40G-1  |
   |      SourceAddress       | 192.168.1.5 |
   |      PingSucceeded       |    Verdadeiro     |
   | RTT de PingReplyDetails \(\) |    0 ms     |

8. Verifique a conectividade da primeira NIC, Test-40G-2.<p>Se a conectividade falhar, inspecione a configuração do switch VLAN ou a participação de destino na mesma VLAN.

   ```powershell
   Test-NetConnection 192.168.2.5
   ```

   _**Da**_


   |        Parâmetro         |    Valor    |
   |--------------------------|-------------|
   |       ComputerName       | 192.168.2.5 |
   |      RemoteAddress       | 192.168.2.5 |
   |      AliasdeInterface      | Teste-40G-2  |
   |      SourceAddress       | 192.168.2.3 |
   |      PingSucceeded       |    Verdadeiro     |
   | RTT de PingReplyDetails \(\) |    0 ms     |

   >[!IMPORTANT]
   >Não é incomum para um **Test-NetConnection** ou a falha de ping ocorrer imediatamente após a execução **de Restart-netadapter**.  Portanto, aguarde até que o adaptador de rede seja inicializado completamente e tente novamente.
   >
   >Se as conexões de VLAN 101 funcionarem, mas as conexões de VLAN 102 não, o problema pode ser que a opção precisa ser configurada para permitir o tráfego de porta na VLAN desejada. Você pode verificar isso Configurando temporariamente os adaptadores com falha para a VLAN 101 e repetindo o teste de conectividade.


   A imagem a seguir mostra os hosts do Hyper-V após configurar com êxito as VLANs.

   ![Configurar a qualidade do serviço](../../media/Converged-NIC/3-datacenter-configure-qos.jpg)


## <a name="step-4-configure-quality-of-service-qos"></a>Etapa 4. Configurar a qualidade do serviço \( QoS\)

>[!NOTE]
>Você deve executar todas as seguintes etapas de configuração de DCB e QoS em todos os hosts que se destinam a se comunicar entre si.

1. Instale o Data Center Bridge \( DCB \) em cada um dos hosts do Hyper-V.

   - **Opcional** para configurações de rede que usam iWarp.
   - **Necessário** para configurações de rede que usam RoCE \( qualquer versão \) para serviços RDMA.

   ```powershell
   Install-WindowsFeature Data-Center-Bridging
   ```

   _**Da**_


   | Sucesso | Reinicialização necessária | Código de Saída |     Resultado do recurso     |
   |---------|----------------|-----------|------------------------|
   |  Verdadeiro   |       Não       |  Sucesso  | {Ponte do Data Center} |

2. Defina as políticas de QoS para o SMB – Direct:

   - **Opcional** para configurações de rede que usam iWarp.
   - **Necessário** para configurações de rede que usam RoCE \( qualquer versão \) para serviços RDMA.

   No comando de exemplo abaixo, o valor "3" é arbitrário. Você pode usar qualquer valor entre 1 e 7, desde que você use consistentemente o mesmo valor em toda a configuração de políticas de QoS.

   ```powershell
   New-NetQosPolicy "SMB" -NetDirectPortMatchCondition 445 -PriorityValue8021Action 3
   ```

   _**Da**_


   |   Parâmetro    |          Valor           |
   |----------------|--------------------------|
   |      Nome      |           SMB            |
   |     Proprietário      | \(Máquina política de grupo\) |
   | NetworkProfile |           Tudo            |
   |   Precedência   |           127            |
   |   JobObject    |          &nbsp;          |
   | NetDirectPort  |           445            |
   | Prioridadevalue  |            3             |

3. Defina políticas de QoS adicionais para outro tráfego na interface.

   ```powershell
   New-NetQosPolicy "DEFAULT" -Default -PriorityValue8021Action 0
   ```

   _**Da**_


   |   Parâmetro    |          Valor           |
   |----------------|--------------------------|
   |      Nome      |         DEFAULT          |
   |     Proprietário      | \(Máquina política de grupo\) |
   | NetworkProfile |           Tudo            |
   |   Precedência   |           127            |
   |    Modelo    |         Padrão          |
   |   JobObject    |          &nbsp;          |
   | Prioridadevalue  |            0             |

4. Ative o **controle de fluxo de prioridade** para o tráfego SMB, o que não é necessário para iWarp.

   ```powershell
   Enable-NetQosFlowControl -priority 3
   Get-NetQosFlowControl
   ```

   _**Da**_


   | Prioridade | habilitado | Política de | IfIndex | IfAlias |
   |----------|---------|-----------|---------|---------|
   |    0     |  Falso  |  Global   | &nbsp;  | &nbsp;  |
   |    1     |  Falso  |  Global   | &nbsp;  | &nbsp;  |
   |    2     |  Falso  |  Global   | &nbsp;  | &nbsp;  |
   |    3     |  Verdadeiro   |  Global   | &nbsp;  | &nbsp;  |
   |    4     |  Falso  |  Global   | &nbsp;  | &nbsp;  |
   |    5     |  Falso  |  Global   | &nbsp;  | &nbsp;  |
   |    6     |  Falso  |  Global   | &nbsp;  | &nbsp;  |
   |    7     |  Falso  |  Global   | &nbsp;  | &nbsp;  |

   >**Importante** Se os resultados não corresponderem a esses resultados porque itens diferentes de 3 têm um valor habilitado de true, você deve desabilitar **FlowControl** para essas classes.
   >
   >```powershell
   >Disable-NetQosFlowControl -priority 0,1,2,4,5,6,7
   >Get-NetQosFlowControl
   >```
   >Em configurações mais complexas, as outras classes de tráfego podem exigir controle de fluxo, no entanto, esses cenários estão fora do escopo deste guia.


5. Habilite a QoS para a primeira NIC, Test-40G-1.

   ```powershell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-1"
   Get-NetAdapterQos -Name "Test-40G-1"

   Name: TEST-40G-1
   Enabled: True
   ```

   _**Recursos**:_


   |      Parâmetro      |   Hardware   |   Current    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Nenhum     |     Nenhum     |
   | NumTCs (Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   _**OperationalTrafficClasses**:_


   | TC |  TSA   | Largura de banda | Prioridades |
   |----|--------|-----------|------------|
   | 0  | Rigoroso |  &nbsp;   |    0-7     |

   _**OperationalFlowControl**:_

   Prioridade 3 habilitada

   _**OperationalClassifications**:_


   | Protocolo  | Porta/tipo | Prioridade |
   |-----------|-----------|----------|
   |  Padrão  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |

6. Habilite a QoS para a segunda NIC, Test-40G-2.

   ```powershell
   Enable-NetAdapterQos -InterfaceAlias "Test-40G-2"
   Get-NetAdapterQos -Name "Test-40G-2"

   Name: TEST-40G-2
   Enabled: True
   ```

   _**Recursos**:_


   |      Parâmetro      |   Hardware   |   Current    |
   |---------------------|--------------|--------------|
   |    MacSecBypass     | NotSupported | NotSupported |
   |     DcbxSupport     |     Nenhum     |     Nenhum     |
   | NumTCs (Max/ETS/PFC) |    8/8/8     |    8/8/8     |

   _**OperationalTrafficClasses**:_


   | TC |  TSA   | Largura de banda | Prioridades |
   |----|--------|-----------|------------|
   | 0  | Rigoroso |  &nbsp;   |    0-7     |

    _**OperationalFlowControl**:_

    Prioridade 3 habilitada

   _**OperationalClassifications**:_


   | Protocolo  | Porta/tipo | Prioridade |
   |-----------|-----------|----------|
   |  Padrão  |  &nbsp;   |    0     |
   | NetDirect |    445    |    3     |


7. Reserve metade da largura de banda para o RDMA do SMB Direct \(\)

   ```powershell
   New-NetQosTrafficClass "SMB" -priority 3 -bandwidthpercentage 50 -algorithm ETS
   ```

   _**Da**_


   | Name | Algoritmo | Largura de banda (%) | Prioridade | Política de | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | SMB  |    ETS    |      50      |    3     |  Global   | &nbsp;  | &nbsp;  |

8. Exibir as configurações de reserva de largura de banda:

   ```powershell
   Get-NetQosTrafficClass | ft -AutoSize
   ```

   _**Da**_

   |   Name    | Algoritmo | Largura de banda (%) | Prioridade | Política de | IfIndex | IfAlias |
   |-----------|-----------|--------------|----------|-----------|---------|---------|
   | Os |    ETS    |      50      | 0-2, 4-7  |  Global   | &nbsp;  | &nbsp;  |
   |    SMB    |    ETS    |      50      |    3     |  Global   | &nbsp;  | &nbsp;  |

9. Adicional Crie duas classes de tráfego adicionais para o tráfego IP de locatário.

   >[!TIP]
   >Você pode omitir os valores "IP1" e "IP2".

   ```powershell
   New-NetQosTrafficClass "IP1" -Priority 1 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**Da**_

   | Name | Algoritmo | Largura de banda (%) | Prioridade | Política de | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP1  |    ETS    |      10      |    1     |  Global   | &nbsp;  | &nbsp;  |

   ```powershell
   New-NetQosTrafficClass "IP2" -Priority 2 -bandwidthpercentage 10 -algorithm ETS
   ```

   _**Da**_


   | Name | Algoritmo | Largura de banda (%) | Prioridade | Política de | IfIndex | IfAlias |
   |------|-----------|--------------|----------|-----------|---------|---------|
   | IP2  |    ETS    |      10      |    2     |  Global   | &nbsp;  | &nbsp;  |

1.  Exiba as classes de tráfego de QoS.

    ```powershell
    Get-NetQosTrafficClass | ft -AutoSize
    ```

    _**Da**_


    |   Name    | Algoritmo | Largura de banda (%) | Prioridade | Política de | IfIndex | IfAlias |
    |-----------|-----------|--------------|----------|-----------|---------|---------|
    | Os |    ETS    |      30      |  0, 4-7   |  Global   | &nbsp;  | &nbsp;  |
    |    SMB    |    ETS    |      50      |    3     |  Global   | &nbsp;  | &nbsp;  |
    |    IP1    |    ETS    |      10      |    1     |  Global   | &nbsp;  | &nbsp;  |
    |    IP2    |    ETS    |      10      |    2     |  Global   | &nbsp;  | &nbsp;  |

    ---

2.  Adicional Substitua o depurador.<p>Por padrão, o depurador anexado bloqueia NetQos.

    ```powershell
    Set-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" AllowFlowControlUnderDebugger -type DWORD -Value 1 –Force
    Get-ItemProperty HKLM:"\SYSTEM\CurrentControlSet\Services\NDIS\Parameters" | ft AllowFlowControlUnderDebugger
    ```

    _**Da**_

    ```
    AllowFlowControlUnderDebugger
    -----------------------------
    1
    ```

## <a name="step-5-verify-the-rdma-configuration-mode-1"></a>Etapa 5. Verifique o modo de configuração RDMA \( 1\)

Você deseja garantir que a malha esteja configurada corretamente antes de criar um vSwitch e fazer a transição para o \( modo RDMA 2 \) .

A imagem a seguir mostra o estado atual dos hosts Hyper-V.

![Testar RDMA](../../media/Converged-NIC/4-datacenter-test-rdma.jpg)


1. Verifique a configuração de RDMA.

   ```powershell
   Get-NetAdapterRdma | ft -AutoSize
   ```

   _**Da**_


   |    Name    |        InterfaceDescription        | habilitado |
   |------------|------------------------------------|---------|
   | TEST-40G-1 | Adaptador de VPI Mellanox ConnectX-4 #2 |  Verdadeiro   |
   | TESTE-40G-2 |  Adaptador de VPI Mellanox ConnectX-4   |  Verdadeiro   |

2. Determine o valor **ifIndex** de seus adaptadores de destino.<p>Você usará esse valor em etapas subsequentes ao executar o script baixado.

   ```powershell
   Get-NetIPConfiguration -InterfaceAlias "TEST*" | ft InterfaceAlias,InterfaceIndex,IPv4Address
   ```

   _**Da**_


   | AliasdeInterface | InterfaceIndex |  IPv4Address  |
   |----------------|----------------|---------------|
   |   TEST-40G-1   |       14       | 192.168.1.3 |
   |   TESTE-40G-2   |       13       | {192.168.2.3} |

3. Baixe o [utilitárioDiskSpd.exe](https://aka.ms/diskspd) e extraia-o em C:\test\.

4. Baixe o [script do PowerShell Test-RDMA](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1) para uma pasta de teste em sua unidade local, por exemplo, C:\test\.

5. Execute o **Test-Rdma.ps1** script do PowerShell para passar o valor ifIndex para o script, junto com o endereço IP do primeiro adaptador remoto na mesma VLAN.<p>Neste exemplo, o script passa o valor **ifIndex** de 14 no endereço IP do adaptador de rede remoto 192.168.1.5.

   ```powershell
   C:\TEST\Test-RDMA.PS1 -IfIndex 14 -IsRoCE $true -RemoteIpAddress 192.168.1.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Da**_

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
   >Se o tráfego RDMA falhar, para o caso RoCE especificamente, consulte a configuração do comutador ToR para configurações apropriadas de PFC/ETS que devem corresponder às configurações do host. Consulte a seção QoS neste documento para obter os valores de referência.

6. Execute o **Test-Rdma.ps1** script do PowerShell para passar o valor ifIndex para o script, junto com o endereço IP do segundo adaptador remoto na mesma VLAN.<p>Neste exemplo, o script passa o valor **ifIndex** de 13 no endereço IP do adaptador de rede remoto 192.168.2.5.

   ```powershell
   C:\TEST\Test-RDMA.PS1 -IfIndex 13 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Da**_

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

## <a name="step-6-create-a-hyper-v-vswitch-on-your-hyper-v-hosts"></a>Etapa 6. Criar um vSwitch do Hyper-V em seus hosts Hyper-V


A imagem a seguir mostra o host do Hyper-V 1 com um vSwitch.

![Criar um comutador virtual do Hyper-V](../../media/Converged-NIC/5-datacenter-create-vswitch.jpg)

1. Crie um vSwitch no modo SET no host do Hyper-V 1.

   ```powershell
   New-VMSwitch –Name "VMSTEST" –NetAdapterName "TEST-40G-1","TEST-40G-2" -EnableEmbeddedTeaming $true -AllowManagementOS $true
   ```

   _**Disso**_


   |  Name   | SwitchType | NetAdapterInterfaceDescription |
   |---------|------------|--------------------------------|
   | VMSTEST |  Externo  |        Agrupado-interface        |

2. Exiba a equipe do adaptador físico no conjunto.

   ```powershell
   Get-VMSwitchTeam -Name "VMSTEST" | fl
   ```

   _**Da**_

   ```
   Name: VMSTEST
   Id: ad9bb542-dda2-4450-a00e-f96d44bdfbec
   NetAdapterInterfaceDescription: {Mellanox ConnectX-3 Pro Ethernet Adapter, Mellanox ConnectX-3 Pro Ethernet Adapter #2}
   TeamingMode: SwitchIndependent
   LoadBalancingAlgorithm: Dynamic
   ```

3. Exibir duas exibições do host vNIC

   ```powershell
    Get-NetAdapter
   ```

   _**Da**_


   |        Name         |        InterfaceDescription         | ifIndex | Status |    MacAddress     | LinkSpeed |
   |---------------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (VMSTEST) | #2 de adaptador Ethernet virtual do Hyper-V |   28    |   Up   | E4-1D-2D-07-40-71 |  80 Gbps  |

4. Exiba as propriedades adicionais do host vNIC.

   ```powershell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Da**_


   |  Name   | IsManagementOs | VMName  |  SwitchName  | MacAddress | Status | IPAddresses |
   |---------|----------------|---------|--------------|------------|--------|-------------|
   | VMSTEST |      Verdadeiro      | VMSTEST | E41D2D074071 |    Problemas    | &nbsp; |             |


5. Teste a conexão de rede com o adaptador remoto VLAN 101.

   ```powershell
   Test-NetConnection 192.168.1.5
   ```

   _**Da**_

   ```
   WARNING: Ping to 192.168.1.5 failed -- Status: DestinationHostUnreachable

   ComputerName   : 192.168.1.5
   RemoteAddress  : 192.168.1.5
   InterfaceAlias : vEthernet (CORP-External-Switch)
   SourceAddress  : 10.199.48.170
   PingSucceeded  : False
   PingReplyDetails (RTT) : 0 ms
   ```

## <a name="step-7-remove-the-access-vlan-setting"></a>Etapa 7. Remover a configuração de VLAN de acesso

Nesta etapa, você remove a configuração de VLAN de acesso da NIC física e para definir a VLANid usando o vSwitch.

Você deve remover a configuração de VLAN de acesso para impedir a marcação automática do tráfego de saída com a ID de VLAN incorreta e a filtragem do tráfego de entrada que não corresponde à ID de VLAN de acesso.

1. Remova a configuração.

   ```powershell
   Set-NetAdapterAdvancedProperty -Name "Test-40G-1" -RegistryKeyword VlanID -RegistryValue "0"
   Set-NetAdapterAdvancedProperty -Name "Test-40G-2" -RegistryKeyword VlanID -RegistryValue "0"
   ```

2. Defina a ID da VLAN.

   ```powershell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "VMSTEST" -VlanId "101" -Access -ManagementOS
   Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName "VMSTEST"
   ```

   _**Da**_

   ```
   VMName VMNetworkAdapterName Mode   VlanList
   ------ -------------------- ----   --------
          VMSTEST              Access 101
   ```


3. Teste a conexão de rede.

   ```powershell
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

   >**Importante** Se os resultados não forem semelhantes aos resultados de exemplo e o ping falhar com a mensagem "aviso: ping para 192.168.1.5 falhou--status: DestinationHostUnreachable", confirme se o "vEthernet (VMSTEST)" tem o endereço IP adequado.
   >
   >```powershell
   >Get-NetIPAddress -InterfaceAlias "vEthernet (VMSTEST)"
   >```
   >
   >Se o endereço IP não estiver definido, corrija o problema.
   >
   >```powershell
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


4. Renomeie a NIC de gerenciamento.

   ```powershell
   Rename-VMNetworkAdapter -ManagementOS -Name “VMSTEST” -NewName “MGT”
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Da**_


   |         Name         | IsManagementOs | VMName |      SwitchName      |  MacAddress  | Status | IPAddresses |
   |----------------------|----------------|--------|----------------------|--------------|--------|-------------|
   | CORP-external-switch |      Verdadeiro      | &nbsp; | CORP-external-switch | 001B785768AA |  Problemas  |   &nbsp;    |
   |         GERENCIAMENTO          |      Verdadeiro      | &nbsp; |       VMSTEST        | E41D2D074071 |  Problemas  |   &nbsp;    |

5. Exibir propriedades adicionais da NIC.

   ```powershell
   Get-NetAdapter
   ```

   _**Da**_


   |      Name       |        InterfaceDescription         | ifIndex | Status |    MacAddress     | LinkSpeed |
   |-----------------|-------------------------------------|---------|--------|-------------------|-----------|
   | vEthernet (gerenciamento) | #2 de adaptador Ethernet virtual do Hyper-V |   28    |   Up   | E4-1D-2D-07-40-71 |  80 Gbps  |

## <a name="step-8-test-hyper-v-vswitch-rdma"></a>Etapa 8. Testar o RDMA do Hyper-V vSwitch

A imagem a seguir mostra o estado atual dos hosts do Hyper-V, incluindo o vSwitch no host do Hyper-V 1.

![Testar o comutador virtual do Hyper-V](../../media/Converged-NIC/6-datacenter-test-vswitch-rdma.jpg)

1. Defina a marcação de prioridade no host vNIC para complementar as configurações de VLAN anteriores.

   ```powershell
   Set-VMNetworkAdapter -ManagementOS -Name "MGT" -IeeePriorityTag on
   Get-VMNetworkAdapter -ManagementOS -Name "MGT" | fl Name,IeeePriorityTag
   ```

   _**Da**_

   Nome: IeeePriorityTag de gerenciamento: ativado

2. Crie dois vNICs de host para RDMA e conecte-os ao VMSTEST vSwitch.

   ```powershell
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB1 –ManagementOS
   Add-VMNetworkAdapter –SwitchName "VMSTEST" –Name SMB2 –ManagementOS
   ```

3. Exiba as propriedades da NIC de gerenciamento.

   ```powershell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Da**_


   |         Name         | IsManagementOs |        VMName        |  SwitchName  | MacAddress | Status | IPAddresses |
   |----------------------|----------------|----------------------|--------------|------------|--------|-------------|
   | CORP-external-switch |      Verdadeiro      | CORP-external-switch | 001B785768AA |    Problemas    | &nbsp; |             |
   |         Gerenciamento          |      Verdadeiro      |       VMSTEST        | E41D2D074071 |    Problemas    | &nbsp; |             |
   |         SMB1         |      Verdadeiro      |       VMSTEST        | 00155D30AA00 |    Problemas    | &nbsp; |             |
   |         SMB2         |      Verdadeiro      |       VMSTEST        | 00155D30AA01 |    Problemas    | &nbsp; |             |

## <a name="step-9-assign-an-ip-address-to-the-smb-host-vnics-vethernet-smb1-and-vethernet-smb2"></a>Etapa 9. Atribuir um endereço IP ao host SMB vNICs vEthernet \( SMB1 \) e vEthernet \( SMB2\)

Os adaptadores físicos TEST-40G-1 e TEST-40G-2 ainda têm uma VLAN de acesso de 101 e 102 configurados. Por isso, os adaptadores marcam o tráfego e o ping são bem-sucedidos. Anteriormente, você configurou as IDs de VLAN pNIC para zero e, em seguida, definiu o VMSTEST vSwitch para a VLAN 101. Depois disso, você ainda poderá executar ping no adaptador remoto VLAN 101 usando o vNIC de gerenciamento, mas atualmente não há nenhum membro de VLAN 102.



1. Remova a configuração de VLAN de acesso da NIC física para impedi-la de marcar automaticamente o tráfego de saída com a ID de VLAN incorreta e impedir que ela filtre o tráfego de entrada que não corresponda à ID de VLAN de acesso.

   ```powershell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB1)" -IPAddress 192.168.2.111 -PrefixLength 24
   ```

   _**Da**_

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

2. Teste o adaptador remoto VLAN 102.

   ```powershell
   Test-NetConnection 192.168.2.5
   ```

   _**Da**_

   ```
   ComputerName   : 192.168.2.5
   RemoteAddress  : 192.168.2.5
   InterfaceAlias : vEthernet (SMB1)
   SourceAddress  : 192.168.2.111
   PingSucceeded  : True
   PingReplyDetails (RTT) : 0 ms
   ```

3. Adicione um novo endereço IP para a interface vEthernet \( SMB2 \) .

   ```powershell
   New-NetIPAddress -InterfaceAlias "vEthernet (SMB2)" -IPAddress 192.168.2.222 -PrefixLength 24
   ```

   _**Da**_

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


5. Coloque o host RDMA vNICs no VLAN 102 já existente.

   ```powershell
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB1" -VlanId "102" -Access -ManagementOS
   Set-VMNetworkAdapterVlan -VMNetworkAdapterName "SMB2" -VlanId "102" -Access -ManagementOS

   Get-VMNetworkAdapterVlan -ManagementOS
   ```

   _**Da**_

   ```
   VMName VMNetworkAdapterName Mode VlanList
   ------ -------------------- ---- --------
      SMB1 Access   102
      Mgt  Access   101
      SMB2 Access   102
      CORP-External-Switch Untagged
   ```

6. Inspecione o mapeamento de SMB1 e SMB2 para as NICs físicas subjacentes na equipe do conjunto de vSwitch.<p>A associação do host vNIC a NICs físicas é aleatória e está sujeita a rebalanceamento durante a criação e a destruição. Nessa circunstância, você pode usar um mecanismo indireto para verificar a associação atual. Os endereços MAC de SMB1 e SMB2 são associados ao membro da equipe NIC TEST-40G-2. Isso não é ideal porque Test-40G-1 não tem um host SMB vNIC associado e não permitirá a utilização de tráfego RDMA pelo link até que um host SMB vNIC seja mapeado para ele.

   ```powershell
   Get-NetAdapterVPort (Preferred)

   Get-NetAdapterVmqQueue
   ```

   _**Da**_

   ```
   Name   QueueID MacAddressVlanID Processor VmFriendlyName
   ----   ------- ---------------- --------- --------------
   TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
   TEST-40G-2 1   00-15-5D-30-AA-00 1020:17
   TEST-40G-2 2   00-15-5D-30-AA-01 1020:17
   ```

7. Exiba as propriedades do adaptador de rede da VM.

   ```powershell
   Get-VMNetworkAdapter -ManagementOS
   ```

   _**Da**_

   ```
   Name IsManagementOs VMName SwitchName   MacAddress   Status IPAddresses
   ---- -------------- ------ ----------   ----------   ------ -----------
   CORP-External-Switch True  CORP-External-Switch 001B785768AA {Ok}
   Mgt  True  VMSTEST  E41D2D074071 {Ok}
   SMB1 True  VMSTEST  00155D30AA00 {Ok}
   SMB2 True  VMSTEST  00155D30AA01 {Ok}
   ```

8. Exiba o mapeamento de equipe do adaptador de rede.<p>Os resultados não devem retornar informações porque você ainda não executou o mapeamento.

   ```powershell
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB1
   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName SMB2
   ```


9. Mapeie o SMB1 e o SMB2 para separar membros da equipe NIC física e para exibir os resultados de suas ações.

   >[!IMPORTANT]
   >Verifique se você concluiu esta etapa antes de continuar ou se a sua implementação falha.

   ```powershell
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB1" -PhysicalNetAdapterName "Test-40G-1"
   Set-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST -VMNetworkAdapterName "SMB2" -PhysicalNetAdapterName "Test-40G-2"

   Get-VMNetworkAdapterTeamMapping -ManagementOS -SwitchName VMSTEST
   ```

   _**Da**_

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

10. Confirme as associações MAC criadas anteriormente.

    ```powershell
    Get-NetAdapterVmqQueue
    ```

    _**Da**_

    ```
    Name   QueueID MacAddressVlanID Processor VmFriendlyName
    ----   ------- ---------------- --------- --------------
    TEST-40G-1 1   E4-1D-2D-07-40-71 1010:17
    TEST-40G-1 2   00-15-5D-30-AA-00 1020:17
    TEST-40G-2 1   00-15-5D-30-AA-01 1020:17
    ```


11. Teste a conexão do sistema remoto porque o host vNICs reside na mesma sub-rede e tem a mesma ID de VLAN \( 102 \) .

    ```powershell
    Test-NetConnection 192.168.2.111
    ```

    _**Da**_

    ```
    ComputerName   : 192.168.2.111
    RemoteAddress  : 192.168.2.111
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    ```

    ```powershell
    Test-NetConnection 192.168.2.222
    ```

    _**Da**_

    ```
    ComputerName   : 192.168.2.222
    RemoteAddress  : 192.168.2.222
    InterfaceAlias : Test-40G-2
    SourceAddress  : 192.168.2.5
    PingSucceeded  : True
    PingReplyDetails (RTT) : 0 ms
    ```
12. Defina o nome, o nome do comutador e as marcas de prioridade.

    ```powershell
    Set-VMNetworkAdapter -ManagementOS -Name "SMB1" -IeeePriorityTag on
    Set-VMNetworkAdapter -ManagementOS -Name "SMB2" -IeeePriorityTag on
    Get-VMNetworkAdapter -ManagementOS -Name "SMB*" | fl Name,SwitchName,IeeePriorityTag,Status
    ```

    _**Da**_

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

    ```powershell
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | ft -AutoSize
    ```

    _**Da**_

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  False
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  False
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

14. Habilite os adaptadores de rede vEthernet.

    ```powershell
    Enable-NetAdapterRdma -Name "vEthernet (SMB1)"
    Enable-NetAdapterRdma -Name "vEthernet (SMB2)"
    Get-NetAdapterRdma -Name "vEthernet*" | sort Name | fl *
    ```

    _**Da**_

    ```
    Name  InterfaceDescription Enabled
    ----  -------------------- -------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  True
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  True
    vEthernet (MGT)   Hyper-V Virtual Ethernet Adapter #2  False
    ```

## <a name="step-10-validate-the-rdma-functionality"></a>Etapa 10. Validar a funcionalidade RDMA

Você deseja validar a funcionalidade RDMA do sistema remoto para o sistema local, que tem um vSwitch, para ambos os membros da equipe do conjunto de vSwitch.<p>Como o host vNICs \( SMB1 e o SMB2 \) são atribuídos à VLAN 102, você pode selecionar o adaptador VLAN 102 no sistema remoto. <p>Neste exemplo, o NIC Test-40G-2 faz o RDMA para o SMB1 (192.168.2.111) e SMB2 (192.168.2.222).

>[!TIP]
>Talvez seja necessário desabilitar o firewall neste sistema.  Consulte sua política de malha para obter detalhes.
>
>```powershell
>Set-NetFirewallProfile -All -Enabled False
>
>Get-NetAdapterAdvancedProperty -Name "Test-40G-2"
>```
>
>_**Da**_
>
>```
>Name  DisplayNameDisplayValue   RegistryKeyword RegistryValue
>----  -----------------------   --------------- -------------
> .
> .
>Test-40G-2VLAN ID102VlanID  {102}
>```

1. Exiba as propriedades do adaptador de rede.

   ```powershell
   Get-NetAdapter
   ```

   _**Da**_

   ```
   Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
   ----  --------------------------- ------   ---------- ---------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet A...#3   3 Up   E4-1D-2D-07-43-D140 Gbps
   ```

2. Exiba as informações de RDMA do adaptador de rede.

   ```powershell
   Get-NetAdapterRdma
   ```

   _**Da**_

   ```
   Name  InterfaceDescription Enabled
   ----  -------------------- -------
   Test-40G-2Mellanox ConnectX-3 Pro Ethernet Adap... True
   ```

3. Execute o teste de tráfego RDMA no primeiro adaptador físico.

   ```powershell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.111 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Da**_

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

   ```powershell
   C:\TEST\Test-RDMA.PS1 -IfIndex 3 -IsRoCE $true -RemoteIpAddress 192.168.2.222 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Da**_

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

5. Teste o tráfego RDMA do local para o computador remoto.

    ```powershell
    Get-NetAdapter | ft –AutoSize
    ```

    _**Da**_

    ```
    Name  InterfaceDescriptionifIndex Status   MacAddress LinkSpeed
    ----  --------------------------- ------   ---------- ---------
    vEthernet (SMB2)  Hyper-V Virtual Ethernet Adapter #4  45 Up   00-15-5D-30-AA-0380 Gbps
    vEthernet (SMB1)  Hyper-V Virtual Ethernet Adapter #3  41 Up   00-15-5D-30-AA-0280 Gbps
    ```

6. Execute o teste de tráfego RDMA no primeiro adaptador virtual.

   ```
   C:\TEST\Test-RDMA.PS1 -IfIndex 41 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Da**_

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

7. Execute o teste de tráfego RDMA no segundo adaptador virtual.

   ```powershell
   C:\TEST\Test-RDMA.PS1 -IfIndex 45 -IsRoCE $true -RemoteIpAddress 192.168.2.5 -PathToDiskspd C:\TEST\Diskspd-v2.0.17\amd64fre\
   ```

   _**Da**_

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

A linha final nesta saída, "teste de tráfego RDMA com êxito: o tráfego RDMA foi enviado para 192.168.2.5", mostra que você configurou com êxito a NIC convergida no adaptador.

## <a name="related-topics"></a>Tópicos relacionados

- [Configuração de NIC convergida com um único adaptador de rede](cnic-single.md)
- [Configuração de comutador físico para NIC convergida](cnic-app-switch-config.md)
- [Solucionando problemas de configurações de NIC convergida](cnic-app-troubleshoot.md)
