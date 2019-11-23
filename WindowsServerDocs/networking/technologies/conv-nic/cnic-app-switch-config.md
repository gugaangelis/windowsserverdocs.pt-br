---
title: Configuração de comutador físico para NIC convergida
description: Neste tópico, fornecemos diretrizes para configurar seus comutadores físicos.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 6d53c797-fb67-4b9e-9066-1c9a8b76d2aa
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/14/2018
ms.openlocfilehash: d10e8ca6e4689b89a8b9532f77613f17280282b1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355479"
---
# <a name="physical-switch-configuration-for-converged-nic"></a>Configuração de comutador físico para NIC convergida

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, fornecemos diretrizes para configurar seus comutadores físicos. 


Esses são apenas comandos e seus usos; Você deve determinar as portas às quais as NICs estão conectadas em seu ambiente. 

>[!IMPORTANT]
>Verifique se a VLAN e a política não-drop estão definidas para a prioridade sobre a qual o SMB está configurado.

## <a name="arista-switch-dcs-7050s-64-eos-4137m"></a>Arista switch \(DCS\-7050s\-64, EOS\-4.13.7 M\)

1.  EN \(ir para o modo admin, geralmente solicita uma senha\)
2.  \(de configuração para entrar no modo de configuração\)
3.  Mostrar execução \(mostra a configuração atual em execução\)
4.  Descubra portas de comutador às quais suas NICs estão conectadas. Neste exemplo, eles são 14/1, 15/1, 16/1, 17/1.
5.  int ETH 14/1, 15/1, 16/1, 17/1 \(entrar no modo de configuração para essas portas\)
6.  modo dcbx IEEE
7.  prioridade – modo de controle de fluxo ativado
8.  VLAN nativa de switchport trunk 225
9.  VLAN do switchport com permissão 100-225
10. tronco do modo switchport
11. prioridade-controle de fluxo-prioridade 3 não-soltar
12. QoS confiável cos
13. Mostrar executar \(verificar se a configuração está configurada corretamente nas portas\)
14. WR \(para fazer com que as configurações persistam na reinicialização do comutador\)

### <a name="tips"></a>Sobre
1.  Nenhum #command # nega um comando
2.  Como adicionar uma nova VLAN: int VLAN 100 \(se a rede de armazenamento estiver em VLAN 100\)
3.  Como verificar VLANs existentes: mostrar VLAN
4.  Para obter mais informações sobre como configurar o comutador Arista, pesquise online por: Arista EOS manual
5.  Use este comando para verificar as configurações de PFC: mostrar detalhes de contadores de fluxo de controle de prioridade

--- 

## <a name="dell-switch-s4810-ftos-99-00"></a>Dell switch \(S4810, FTOS 9,9 \(0,0\)\)

    
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

## <a name="cisco-switch-nexus-3132-version-602u61"></a>Switch Cisco \(Nexus 3132, versão 6,0\(2\)U6\(1\)\)

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
    

### <a name="port-specific"></a>Específico da porta

    
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
- [Configuração NIC agrupada NIC convergida](cnic-datacenter.md)
- [Solucionando problemas de configurações de NIC convergida](cnic-app-troubleshoot.md)

--- 