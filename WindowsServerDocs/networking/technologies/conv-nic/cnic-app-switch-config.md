---
title: Configuração de comutador físico para NIC convergida
description: Neste tópico, nós fornecemos você com diretrizes para configurar seus comutadores físicos.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: e31d7b83fee84d9055d938f77b49389205786244
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829397"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configuração de comutador físico para NIC convergida

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, nós fornecemos você com diretrizes para configurar seus comutadores físicos. 


Esses são apenas os comandos e seus usos; Você deve determinar as portas para o qual as NICs estão conectadas em seu ambiente. 

>[!IMPORTANT]
>Certifique-se de que a VLAN e a política de cancelamento não está definido para a prioridade sobre o qual o SMB é configurado.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Comutador arista \(dcs\-7050s\-EOS 64,\-4.13.7M\)

1.  en \(ir para o modo de administrador, geralmente solicita uma senha\)
2.  config \(para entrar no modo de configuração\)
3.  Mostrar execução \(mostra a configuração de execução atual\)
4.  Descubra as portas de comutador ao qual seu NICs estão conectadas. Esses exemplo, eles são 14/1,15/1,16/1,17/1.
5.  int eth 14/1,15/1,16/1,17/1 \(entrar no modo de configuração para essas portas\)
6.  modo de dcbx ieee
7.  modo de controle de fluxo de prioridade em
8.  vlan nativa do tronco de porta de switch 225
9.  tronco da porta de switch permitido vlan 100 e 225
10. tronco de modo de porta de switch
11. prioridade de controle de fluxo de prioridade 3 não soltar
12. QoS de confiança cos
13. Mostrar execução \(verificar que a configuração está configurada corretamente nas portas\)
14. wr \(tornar as configurações de persiste entre a opção de reinicialização\)

### <a name="tips"></a>Dicas:
1.  Não há command # # nega um comando
2.  Como adicionar uma nova VLAN: int vlan 100 \(se a rede de armazenamento está na VLAN 100\)
3.  Como verificar VLANs existentes: Mostrar vlan
4.  Para obter mais informações sobre como configurar o comutador Arista, pesquise online por: Arista EOS Manual
5.  Use este comando para verificar as configurações de PFC: Mostrar detalhes de contadores de prioridade de fluxo de controle

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Comutador Dell \(S4810, 9.9 FTOS \(0,0\)\)

    
    !
    dcb enable
    ! put pfc control on qos class 3
    configure
    dcb-map dcb-smb
    priority group 0 bandwidth 90 pfc on
    priority group 1 bandwidth 10 pfc off
    priority-pgid 1 1 1 0 1 1 1 1
    exit
    ! apply map to ports 0-31
    configure
    interface range ten 0/0-31
    dcb-map dcb-smb
    exit
    
--- 

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Switch da Cisco \(Nexus 3132, versão 6.0\(2\)U6\(1\)\)

### <a name="global"></a>Global
    
    class-map type qos match-all RDMA
    match cos 3
    class-map type queuing RDMA
    match qos-group 3
    policy-map type qos QOS_MARKING
    class RDMA
    set qos-group 3
    class class-default
    policy-map type queuing QOS_QUEUEING
    class type queuing RDMA
    bandwidth percent 50
    class type queuing class-default
    bandwidth percent 50
    class-map type network-qos RDMA
    match qos-group 3
    policy-map type network-qos QOS_NETWORK
    class type network-qos RDMA
    mtu 2240
    pause no-drop
    class type network-qos class-default
    mtu 9216
    system qos
    service-policy type qos input QOS_MARKING
    service-policy type queuing output QOS_QUEUEING
    service-policy type network-qos QOS_NETWORK
    

### <a name="port-specific"></a>Porta específica

    
    switchport mode trunk
    switchport trunk native vlan 99
    switchport trunk allowed vlan 99,2000,2050   çuse VLANs that already exists
    spanning-tree port type edge
    flowcontrol receive on (not supported with PFC in Cisco NX-OS)
    flowcontrol send on (not supported with PFC in Cisco NX-OS)
    no shutdown
    priority-flow-control mode on
    
--- 

## <a name="related-topics"></a>Tópicos relacionados

- [Configuração de NIC convergida com um único adaptador de rede](cnic-single.md)
- [Configuração do NIC agrupado NIC convergida](cnic-datacenter.md)
- [Solução de problemas convergido configurações de NIC](cnic-app-troubleshoot.md)

--- 