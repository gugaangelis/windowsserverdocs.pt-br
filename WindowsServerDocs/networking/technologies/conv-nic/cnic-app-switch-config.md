---
title: Configuração do comutador físico para NIC convergido
description: Este tópico faz parte do convergidos NIC configuração guia para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 98a2e249aea38bd4d07dc1bcbc9b1ca98b98b6d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configuração do comutador físico para NIC convergido

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar as seções a seguir como diretrizes para configurar as opções da físicas.

Esses são apenas os comandos e seus usos; Você deve determinar as portas para o qual as NICs estão conectadas em seu ambiente. 

>[!IMPORTANT]
>Certifique-se de que o VLAN e a política de soltar não está definida para a prioridade sobre quais SMB é configurada.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Alternar arista \ (dcs\-7050s\-64, EOS\-4.13.7M\)

1.  en \ (Ir para o modo de administrador, geralmente solicitará um password\)
2.  config \ (para entrar no modo de configuração \)
3.  Mostrar execução \ (mostra a configuração em execução atual)
4.  Descubra ao qual seu NICs estão conectados a portas do switch. Esses exemplo, eles são 14 1,15/1,16/1,17/1 /.
5.  int eth 14 1,15/1,16/1,17/1 / \ (inserir em modo de configuração para esses ports\)
6.  modo de dcbx ieee
7.  modo de controle de fluxo de prioridade no
8.  vlan nativa do tronco de porta do switch 225
9.  tronco de porta do switch permitido vlan 100-225
10. tronco de modo de porta do switch
11. controle de fluxo de prioridade alta prioridade 3 não soltar
12. QoS confiar cos
13. Mostrar execução \ (Verifique se essa configuração está configurado corretamente no ports\)
14. wr \ (tornar as configurações são preservados entre switch reboot\)

### <a name="tips"></a>Dicas:
1.  Nenhuma command # nega um comando
2.  Como adicionar uma nova VLAN: int vlan 100 \ (se o armazenamento de rede é em VLAN 100\)
3.  Como verificar VLANs existentes: Mostrar vlan
4.  Para obter mais informações sobre como configurar Arista Switch, procure online: Arista EOS Manual
5.  Use este comando para verificar as configurações de PFC: Mostrar detalhes de contadores de prioridade de fluxo de controle

## <a name="dell-switch-s4810-ftos-99-00"></a>Alternar Dell \ (S4810, FTOS 9,9 \(0.0\)\)

    
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
    

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Switch Cisco \ (Nexus 3132, versão 6.0\(2\)U6\(1\)\)

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
    

## <a name="all-topics-in-this-guide"></a>Todos os tópicos neste guia

Este guia contém os tópicos a seguir.

- [Configuração de NIC convergido com um único adaptador de rede](cnic-single.md)
- [Configuração de NIC emparelhados NIC convergido](cnic-datacenter.md)
- [Configuração do comutador físico para NIC convergido](cnic-app-switch-config.md)
- [Solução de problemas de configurações de NIC convergidos](cnic-app-troubleshoot.md)
