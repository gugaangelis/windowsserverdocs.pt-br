---
title: Solucionando problemas de configurações de NIC convergida
description: Este tópico faz parte do guia de configuração da NIC convergida para o Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0bc6746f-2adb-43d8-a503-52f473833164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a732c91b1eaee870a8aeb6d33c5aa604bf4f2a7
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870122"
---
# <a name="troubleshooting-converged-nic-configurations"></a>Solucionando problemas de configurações de NIC convergida

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar o script a seguir para verificar se a configuração de RDMA está correta no host Hyper-V.

- [Baixar o script Test-Rdma. ps1](https://github.com/Microsoft/SDN/blob/master/Diagnostics/Test-Rdma.ps1)

Você também pode usar os seguintes comandos do Windows PowerShell para solucionar problemas e verificar a configuração de suas NICs convergidas.

## <a name="get-netadapterrdma"></a>Get-NetAdapterRdma

Para verificar a configuração de RDMA do adaptador de rede, execute o seguinte comando do Windows PowerShell no servidor Hyper-V.

    
    Get-NetAdapterRdma | fl *
    

Você pode usar os seguintes resultados esperados e inesperados para identificar e resolver problemas depois de executar esse comando no host do Hyper-V.

### <a name="get-netadapterrdma-expected-results"></a>Resultados esperados de Get-NetAdapterRdma

O host vNIC e a NIC física mostram recursos RDMA diferentes de zero.

![Resultados esperados do Windows PowerShell](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-01.jpg)

### <a name="get-netadapterrdma-unexpected-results"></a>Resultados inesperados do Get-NetAdapterRdma

Execute as etapas a seguir se você receber resultados inesperados ao executar o comando **Get-NetAdapterRdma** .

1. Verifique se os drivers de barramento Mlnx e miniporta Mlnx são mais recentes. Para o Mellanox, use pelo menos a remoção de 42. 
2. Verifique se a miniporta Mlnx e os drivers de barramento correspondem verificando a versão do driver por meio de Device Manager. O driver de barramento pode ser encontrado em dispositivos do sistema. O nome deve começar com Mellanox Connect-X 3 PRO VPI, conforme ilustrado na captura de tela a seguir das propriedades do adaptador de rede.

![Propriedades do adaptador de rede](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-02.jpg)

4. Verifique se a RDMA (rede direta) está habilitada na NIC física e no host vNIC.
5. Verifique se o vSwitch foi criado no adaptador físico correto verificando seus recursos de RDMA.
6. Verifique o log do sistema do visualizador e filtre por origem "Hyper-V-VmSwitch".

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Como uma etapa adicional para verificar a configuração de RDMA, execute o seguinte comando do Windows PowerShell no servidor Hyper-V.


    Get-SmbClientNetworkInterface

### <a name="get-smbclientnetworkinterface-expected-results"></a>Resultados esperados de Get-SmbClientNetworkInterface

O host vNIC também deve aparecer como RDMA habilitado da perspectiva do SMB.

![Propriedades do adaptador de rede](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-03.jpg)


### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Resultados inesperados do Get-SmbClientNetworkInterface

1. Verifique se os drivers de barramento Mlnx e miniporta Mlnx são mais recentes. Para o Mellanox, use pelo menos a remoção de 42. 
2. Verifique se a miniporta Mlnx e os drivers de barramento correspondem verificando a versão do driver por meio de Device Manager. O driver de barramento pode ser encontrado em dispositivos do sistema. O nome deve começar com Mellanox Connect-X 3 PRO VPI, conforme ilustrado na captura de tela a seguir das propriedades do adaptador de rede.
3. Verifique se a RDMA (rede direta) está habilitada na NIC física e no host vNIC.
4. Verifique se o comutador virtual do Hyper-V foi criado no adaptador físico correto, verificando seus recursos de RDMA.
5. Verifique os logs do visualizador para "cliente SMB" em **aplicativos e serviços | Microsoft | Windows**.

--- 

## <a name="get-netadapterqos"></a>Get-NetAdapterQos

Você pode exibir a qualidade do adaptador de rede \(da\) configuração de QoS do serviço executando o seguinte comando do Windows PowerShell.

    Get-NetAdapterQos

### <a name="get-netadapterqos-expected-results"></a>Resultados esperados de Get-NetAdapterQos

As prioridades e as classes de tráfego devem ser exibidas de acordo com a primeira etapa de configuração que você realizou usando este guia.

![Qualidade das prioridades e classes de serviço](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-04.jpg)

### <a name="get-netadapterqos-unexpected-results"></a>Resultados inesperados do Get-NetAdapterQos

Se os resultados forem inesperados, execute as etapas a seguir.

1. Verifique se o adaptador de rede física dá suporte a \(ponte\) de data center DCB e QoS
2. Verifique se os drivers do adaptador de rede estão atualizados.

--- 

## <a name="get-smbmultichannelconnection"></a>Get-SmbMultiChannelConnection

Você pode usar o seguinte comando do Windows PowerShell para verificar se o endereço IP do nó remoto é\-compatível com RDMA.

    Get-SmbMultiChannelConnection


### <a name="get-smbmultichannelconnection-expected-results"></a>Resultados esperados de Get-SmbMultiChannelConnection

O endereço IP do nó remoto é mostrado como habilitado para RDMA.

![Endereço IP do nó remoto compatível com RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-05.jpg)

### <a name="get-smbmultichannelconnection-unexpected-results"></a>Resultados inesperados do Get-SmbMultiChannelConnection

Se os resultados forem inesperados, execute as etapas a seguir.

1. Verifique se o ping funciona de ambas as maneiras.
2. Verifique se o firewall não está bloqueando o início da conexão SMB. Especificamente, habilite a regra de firewall para a porta SMB Direct 5445 para iWARP e 445 para ROCE.

--- 

## <a name="get-smbclientnetworkinterface"></a>Get-SmbClientNetworkInterface

Você pode usar o comando a seguir para verificar se a NIC virtual que você habilitou para RDMA é\-relatada como compatível com RDMA pelo SMB.

    Get-SmbClientNetworkInterface


### <a name="get-smbclientnetworkinterface-expected-results"></a>Resultados esperados de Get-SmbClientNetworkInterface

A NIC virtual habilitada para RDMA deve ser vista como compatível com RDMA pelo SMB.

![O SMB relata que as NICs são compatíveis com RDMA](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-06.jpg)

### <a name="get-smbclientnetworkinterface-unexpected-results"></a>Resultados inesperados do Get-SmbClientNetworkInterface

Se os resultados forem inesperados, execute as etapas a seguir.

1. Verifique se o ping funciona de ambas as maneiras.
2. Verifique se o firewall não está bloqueando o início da conexão SMB.

--- 

## <a name="vstat-mellanox-specific"></a>específico do VSTA. Mellanox \(\)

Se você estiver usando adaptadores de rede Mellanox, poderá usar o comando **vstat** para verificar a versão de RoCE \(\) Ethernet RDMA em convergente nos nós do Hyper-V.

### <a name="vstat-expected-results"></a>resultados esperados do VSTA

A versão do RoCE em ambos os nós deve ser a mesma. Essa também é uma boa maneira de verificar se a versão do firmware em ambos os nós é a mais recente.

![Exemplos de resultados da verificação de versão do RoCE](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-07.jpg)

### <a name="vstat-unexpected-results"></a>resultados inesperados do VSTA

Se os resultados forem inesperados, execute as etapas a seguir.

1. Definir a versão correta do RoCE usando Set-MlnxDriverCoreSetting
2. Instale o firmware mais recente do site da Mellanox.

--- 

## <a name="perfmon-counters"></a>Contadores Perfmon

Você pode examinar os contadores no monitor de desempenho para verificar a atividade RDMA de sua configuração.

![Exemplos de resultados do monitor de desempenho](../../media/Converged-NIC/CNIC-Troubleshooting/cnic-tshoot-08.jpg)

--- 

## <a name="related-topics"></a>Tópicos relacionados

- [Configuração de NIC convergida com um único adaptador de rede](cnic-single.md)
- [Configuração NIC agrupada NIC convergida](cnic-datacenter.md)
- [Configuração de comutador físico para NIC convergida](cnic-app-switch-config.md)

---
