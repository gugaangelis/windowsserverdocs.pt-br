---
title: Solução de problemas de configurações de NIC convergidos
description: Este tópico faz parte do convergidos NIC configuração guia para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 373ecd9b9fff62aaabd8caa176ff091ec98ad81c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshooting-converged-nic-configurations"></a>Solução de problemas de configurações de NIC convergidos

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar o script a seguir para verificar se a configuração RDMA está correta no host do Hyper-V.

- [Baixe. ps1 Test-Rdma de script](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

Você também pode usar os seguintes comandos do Windows PowerShell para solucionar problemas e verificar a configuração das suas NICs convergidas.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Para verificar a configuração de RDMA do adaptador de rede, execute o seguinte comando do Windows PowerShell no servidor do Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Você pode usar as seguintes esperados e resultados inesperados para identificar e resolver problemas depois de executar esse comando no host do Hyper-V.

### <a name="get-netadapterrdma-expected-results"></a>Get-NetAdapterRdma resultados esperados

VNIC host e a placa de rede física mostram recursos RDMA diferente de zero.

![Resultados do Windows PowerShell esperado](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma resultados inesperados

Execute as seguintes etapas se você receber resultados inesperados quando você executa o **Get-NetAdapterRdma** comando.

1. Verifique se a miniporta Mlnx e drivers de barramento Mlnx são mais recentes. Para Mellanox, use pelo menos soltar 42. 
2. Verifique se os drivers de miniporta e barramento Mlnx correspondem verificando a versão do driver por meio do Gerenciador de dispositivos. O driver de barramento pode ser encontrado em dispositivos do sistema. O nome deve começar com Mellanox Connect-X 3 PRO VPI, conforme ilustrado na captura de tela das propriedades do adaptador de rede.

![Propriedades do adaptador de rede](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Certifique-se de rede direta (RDMA) está habilitado no NIC físico tanto host vNIC.
5. Certifique-se de vSwitch é criada sobre o adaptador diretamente físico verificando seus recursos RDMA.
6. Verifique o log de sistema Event e filtrar por fonte "Hyper-V-VmSwitch".

## <a name="get-smbclientnetworkinterface"></a>Obter SmbClientNetworkInterface

Como uma etapa adicional para verificar a configuração de RDMA, execute o seguinte comando do Windows PowerShell no servidor do Hyper-V.


    Get SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Obtenha resultados SmbClientNetworkInterface esperado

O host vNIC deve aparecer como RDMA capaz da perspectiva do SMB também.

![Propriedades do adaptador de rede](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Obter SmbClientNetworkInterface resultados inesperados

1. Verifique se a miniporta Mlnx e drivers de barramento Mlnx são mais recentes. Para Mellanox, use pelo menos soltar 42. 
2. Verifique se os drivers de miniporta e barramento Mlnx correspondem verificando a versão do driver por meio do Gerenciador de dispositivos. O driver de barramento pode ser encontrado em dispositivos do sistema. O nome deve começar com Mellanox Connect-X 3 PRO VPI, conforme ilustrado na captura de tela das propriedades do adaptador de rede.
3. Certifique-se de rede direta (RDMA) está habilitado no NIC físico tanto host vNIC.
4. Verifique se que o comutador Virtual Hyper-V é criado sobre o adaptador diretamente físico verificando seus recursos RDMA.
5. Verifique os logs de Event para "Cliente SMB" **aplicativos e serviços | Microsoft | Windows**.

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

Você pode exibir a qualidade de adaptador de rede de configuração do serviço \(QoS\) executando o seguinte comando do Windows PowerShell.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Get-NetAdapterQos resultados esperados

Prioridades e classes de tráfego devem ser exibidos de acordo com a primeira etapa de configuração que você executou usando este guia.

![Qualidade de classes e as prioridades de serviço](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos resultados inesperados

Se seus resultados inesperados, execute as seguintes etapas.

1. Certifique-se de que o adaptador de rede física suporta \(DCB\) ponte do Centro de dados e QoS
2. Certifique-se de que os drivers de adaptador de rede são atualizados.


## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

Você pode usar o seguinte comando do Windows PowerShell para verificar se o endereço IP do nó remota é compatível com RDMA\.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Get-SmbMultiChannelConnection resultados esperados

Endereço IP do nó remota é mostrado como RDMA capaz.

![Endereço IP de nó remoto capaz de RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection resultados inesperados

Se seus resultados inesperados, execute as seguintes etapas.

1. Verifique se o ping funciona de duas maneiras.
2. Verifique se que o firewall não está bloqueando o início de conexão do SMB. Especificamente, habilite a regra de firewall para porta SMB direta 5445 para iWARP e 445 para ROCE.

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Você pode usar o seguinte comando para confirmar que o NIC virtual habilitado para RDMA é relatado como compatível com RDMA\ por SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Get-SmbClientNetworkInterface resultados esperados

NIC virtual que foi habilitado para RDMA deve ser visto como compatível com RDMA pelo SMB.

![SMB relata NICs são capazes de RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface resultados inesperados

Se seus resultados inesperados, execute as seguintes etapas.

1. Verifique se o ping funciona de duas maneiras.
2. Verifique se o firewall não está bloqueando o início de conexão do SMB.

## <a name="vstat-mellanox-specific"></a>vstat \(Mellanox specific\)

Se você estiver usando Mellanox adaptadores de rede, você pode usar o **vstat** comando para verificar se o RDMA sobre \(RoCE\) em versão Gigabit Ethernet convertida em nós Hyper-V.

### <a name="vstat-expected-results"></a>resultados de vstat esperado

A versão RoCE em ambos os nós deve ser o mesmo. Isso também é uma boa maneira de verificar se a versão do firmware em ambos os nós é mais recente.

![Exemplos de resultado de verificação de versão RoCE](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat resultados inesperados

Se seus resultados inesperados, execute as seguintes etapas.

1. Defina a versão correta do RoCE usando Set-MlnxDriverCoreSetting
2. Instale o firmware mais recente do site Mellanox.


## <a name="perfmon-counters"></a>Contadores de desempenho

Você pode revisar contadores no Monitor de desempenho para verificar a atividade RDMA de sua configuração.

![Exemplos de resultado do monitor de desempenho](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

## <a name="all-topics-in-this-guide"></a>Todos os tópicos neste guia

Este guia contém os tópicos a seguir.

- [Configuração de NIC convergido com um único adaptador de rede](cnic-single.md)
- [Configuração de NIC emparelhados NIC convergido](cnic-datacenter.md)
- [Configuração do comutador físico para NIC convergido](cnic-app-switch-config.md)
- [Solução de problemas de configurações de NIC convergidos](cnic-app-troubleshoot.md)
