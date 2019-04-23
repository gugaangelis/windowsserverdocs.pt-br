---
title: Solução de problemas convergido configurações de NIC
description: Este tópico faz parte do convergido NIC configuração guia para o Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3004ff9d6fe874410c24d174755a6d26f99f8f2a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876477"
---
# <a name="troubleshooting-converged-nic-configurations"></a>Solução de problemas convergido configurações de NIC

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar o script a seguir para verificar se a configuração do RDMA está correta no host Hyper-V.

- [Baixar script de teste Rdma.ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

Você também pode usar os seguintes comandos do Windows PowerShell para solucionar problemas e verificar a configuração de suas NICs convergidas.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Para verificar sua configuração de RDMA do adaptador de rede, execute o seguinte comando do Windows PowerShell no servidor Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Você pode usar o seguinte esperado e a resultados inesperados para identificar e resolver problemas depois de executar esse comando no host Hyper-V.

### <a name="get-netadapterrdma-expected-results"></a>Resultados de Get-NetAdapterRdma esperado

VNIC do host e a NIC física mostram os recursos de RDMA diferente de zero.

![Resultados do PowerShell do Windows esperado](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Get-NetAdapterRdma resultados inesperados

Execute as seguintes etapas se você receber resultados inesperados quando você executa o **Get-NetAdapterRdma** comando.

1. Certifique-se de miniporta Mlnx e motoristas de ônibus Mlnx são mais recentes. Para Mellanox, use pelo menos descartar 42. 
2. Verifique se os drivers de miniporta e barramento Mlnx correspondem, verificando a versão do driver pelo Gerenciador de dispositivos. O driver de barramento pode ser encontrado em dispositivos do sistema. O nome deve começar com o Mellanox Connect-X 3 PRO VPI, conforme ilustrado na captura de tela das propriedades do adaptador de rede.

![Propriedades do adaptador de rede](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Certifique-se de rede direto (RDMA) está habilitada na NIC física e vNIC do host.
5. Certifique-se de vSwitch é criado sobre o adaptador físico à direita, verificando seus recursos RDMA.
6. Verifique o log do sistema do Visualizador de eventos e filtrar por origem "Hyper-V-VmSwitch".

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Como uma etapa adicional para verificar sua configuração de RDMA, execute o seguinte comando do Windows PowerShell no servidor Hyper-V.


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Resultados de Get-SmbClientNetworkInterface esperado

O vNIC do host deve aparecer como compatível com RDMA do ponto de vista do SMB.

![Propriedades do adaptador de rede](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface resultados inesperados

1. Certifique-se de miniporta Mlnx e motoristas de ônibus Mlnx são mais recentes. Para Mellanox, use pelo menos descartar 42. 
2. Verifique se os drivers de miniporta e barramento Mlnx correspondem, verificando a versão do driver pelo Gerenciador de dispositivos. O driver de barramento pode ser encontrado em dispositivos do sistema. O nome deve começar com o Mellanox Connect-X 3 PRO VPI, conforme ilustrado na captura de tela das propriedades do adaptador de rede.
3. Certifique-se de rede direto (RDMA) está habilitada na NIC física e vNIC do host.
4. Verifique se que o comutador Virtual do Hyper-V foi criado sobre o adaptador físico à direita, verificando seus recursos RDMA.
5. Verifique os logs do Visualizador de eventos de "Cliente SMB" no **aplicativos e serviços | Microsoft | Windows**.

--- 

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

Você pode exibir a qualidade do adaptador de rede do serviço \(QoS\) configuração executando o seguinte comando do Windows PowerShell.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Resultados de Get-NetAdapterQos esperado

Prioridades e classes de tráfego devem ser exibidas de acordo com a primeira etapa de configuração que você executou usando este guia.

![Qualidade das prioridades de serviço e classes](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Get-NetAdapterQos resultados inesperados

Se seus resultados inesperados, execute as seguintes etapas.

1. Certifique-se de que o adaptador de rede física dá suporte ao Data Center Bridging \(DCB\) e QoS
2. Certifique-se de que os drivers de adaptador de rede estão atualizados.

--- 

## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

Você pode usar o seguinte comando do Windows PowerShell para verificar se o endereço IP do nó remoto é RDMA\-capaz.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Resultados de Get-SmbMultiChannelConnection esperado

Endereço IP do nó remoto é mostrado como RDMA capaz.

![Endereço IP de nó remoto compatíveis com RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Get-SmbMultiChannelConnection resultados inesperados

Se seus resultados inesperados, execute as seguintes etapas.

1. Certifique-se de ping funciona das duas formas.
2. Verifique se que o firewall não está bloqueando a iniciação de conexão do SMB. Especificamente, habilite a regra de firewall para a porta SMB Direct 5445 para iWARP e 445 para ROCE.

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Você pode usar o comando a seguir para verificar se a NIC virtual é habilitada para RDMA é relatado como RDMA\-capaz por SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Resultados de Get-SmbClientNetworkInterface esperado

NIC virtual que foi habilitada para RDMA deve ser visto como compatível com RDMA por SMB.

![SMB relata as NICs são compatíveis com RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Get-SmbClientNetworkInterface resultados inesperados

Se seus resultados inesperados, execute as seguintes etapas.

1. Certifique-se de ping funciona das duas formas.
2. Verifique se o firewall não está bloqueando a iniciação de conexão do SMB.

--- 

## <a name="vstat-mellanox-specific"></a>vstat \(Mellanox específico\)

Se você estiver usando adaptadores de rede Mellanox, você pode usar o **vstat** comando para verificar se o RDMA over Converged Ethernet \(RoCE\) versão em nós do Hyper-V.

### <a name="vstat-expected-results"></a>resultados de vstat esperado

A versão de RoCE em ambos os nós deve ser o mesmo. Isso também é uma boa maneira de verificar se a versão de firmware em ambos os nós é mais recente.

![Exemplos de resultados de verificação de versão RoCE](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>vstat resultados inesperados

Se seus resultados inesperados, execute as seguintes etapas.

1. Definir versão correta do RoCE usando Set-MlnxDriverCoreSetting
2. Instale o firmware mais recente pelo site do Mellanox.

--- 

## <a name="perfmon-counters"></a>Os contadores de desempenho

Você pode examinar os contadores no Monitor de desempenho para verificar a atividade RDMA da sua configuração.

![Exemplos de resultados do monitor de desempenho](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>Tópicos relacionados

- [Configuração de NIC convergida com um único adaptador de rede](cnic-single.md)
- [Configuração do NIC agrupado NIC convergida](cnic-datacenter.md)
- [Configuração de comutador físico para NIC convergida](cnic-app-switch-config.md)

---
