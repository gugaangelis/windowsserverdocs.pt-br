---
title: Configurar as listas de controle de acesso (ACLs) do firewall do datacenter
description: Você pode aplicar ACLs específicas aos adaptadores de rede.  Se as ACLs também são definidas na sub-rede virtual ao qual o adaptador de rede está conectado, tanto as ACLs são aplicadas, mas a interface de rede ACLs são priorizadas acima a ACLs de sub-rede virtual.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 77a7706e39da265eedd65342a0ccf2174ab050ea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853397"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurar o firewall do datacenter listas de controle de acesso (ACLs)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois que você criou uma ACL e atribuiu a uma sub-rede virtual, você talvez queira substituir esse padrão de ACL na sub-rede virtual com uma ACL específica para uma interface de rede individuais.  Nesse caso, aplicar ACLs específicas diretamente para os adaptadores de rede anexados a VLANs, em vez da rede virtual. Se você tiver definido na sub-rede virtual conectada ao adaptador de rede de ACLs, ambas as ACLs são aplicadas e prioriza a interface de rede ACLs acima as ACLs de sub-rede virtual.

>[!IMPORTANT]
>Se você não tiver criado uma ACL e atribuiu a uma rede virtual, consulte [uso Access Control Lists (ACLs) para gerenciar o data center rede tráfego fluir](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) para criar uma ACL e atribuí-lo a uma sub-rede virtual.  

Neste tópico, mostramos como adicionar uma ACL a uma interface de rede. Também mostramos como remover uma ACL de um adaptador de rede usando o Windows PowerShell e a API de REST do controlador de rede.

- [Exemplo: Adicione uma ACL a uma interface de rede](#example-add-an-acl-to-a-network-interface)
- [Exemplo: Remover uma ACL de um adaptador de rede usando o Windows Powershell e a API de REST do controlador de rede](#example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api)


## <a name="example-add-an-acl-to-a-network-interface"></a>Exemplo: Adicione uma ACL a uma interface de rede
Este exemplo demonstra como adicionar uma ACL a uma rede virtual. 

>[!TIP]
>Também é possível adicionar uma ACL ao mesmo tempo que você criar a interface de rede.

1. Obter ou criar a interface de rede à qual você adicionará a ACL.
 
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Obter ou criar a ACL, você adicionará ao adaptador de rede.
 
   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```
 
3. Atribuir a ACL para a propriedade AccessControlList da interface de rede
 
   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```
 
4. Adicionar a interface de rede no controlador de rede
 
   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
 
## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>Exemplo: Remover uma ACL de um adaptador de rede usando o Windows Powershell e a API de REST do controlador de rede
Neste exemplo, mostramos como remover uma ACL. Remover uma ACL se aplica o conjunto de regras padrão para o adaptador de rede. O conjunto de regras padrão permite todo o tráfego de saída, mas bloqueia todo o tráfego de entrada.

>[!NOTE]
>Se você quiser permitir que todo o tráfego de entrada, você deve seguir anterior [exemplo](#example-add-an-acl-to-a-network-interface) para adicionar uma ACL que permite o tráfego de todas as entradas e todas as saídas.


1. Obtenha a interface de rede do qual você irá remover a ACL.<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```
 
2. Atribua $NULL à propriedade AccessControlList da ipConfiguration.<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```
 
3. Adicione o objeto de interface de rede no controlador de rede.<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```
