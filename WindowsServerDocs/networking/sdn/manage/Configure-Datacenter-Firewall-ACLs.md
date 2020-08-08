---
title: Configurar as listas de controle de acesso (ACLs) do firewall do datacenter
description: Você pode aplicar ACLs específicas a interfaces de rede.  Se as ACLs também forem definidas na sub-rede virtual à qual a interface de rede está conectada, ambas as ACLs serão aplicadas, mas as ACLs de interface de rede serão priorizadas acima das ACLs de sub-rede virtual.
manager: grcusanz
ms.topic: article
ms.assetid: 25f18927-a63e-44f3-b02a-81ed51933187
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/23/2018
ms.openlocfilehash: da5b34556f0a9fd65a4a56adc778666f6911b449
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995175"
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurar listas de controle de acesso (ACLs) do firewall do datacenter

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Depois de criar uma ACL e atribuí-la a uma sub-rede virtual, talvez você queira substituir essa ACL padrão na sub-rede virtual por uma ACL específica para uma interface de rede individual.  Nesse caso, você aplica ACLs específicas diretamente às interfaces de rede anexadas a VLANs, em vez da rede virtual. Se você tiver ACLs definidas na sub-rede virtual conectada à interface de rede, ambas as ACLs serão aplicadas e priorizará as ACLs de interface de rede acima das ACLs de sub-rede virtual.

>[!IMPORTANT]
>Se você não criou uma ACL e a atribuiu a uma rede virtual, consulte [usar ACLs (listas de controle de acesso) para gerenciar o fluxo de tráfego de rede do datacenter](./use-acls-for-traffic-flow.md) para criar uma ACL e atribuí-la a uma sub-rede virtual.

Neste tópico, mostramos como adicionar uma ACL a uma interface de rede. Também mostramos como remover uma ACL de uma interface de rede usando o Windows PowerShell e a API REST do controlador de rede.

- [Exemplo: adicionar uma ACL a uma interface de rede](#example-add-an-acl-to-a-network-interface)
- [Exemplo: remover uma ACL de uma interface de rede usando o Windows PowerShell e a API REST do controlador de rede](#example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api)


## <a name="example-add-an-acl-to-a-network-interface"></a>Exemplo: adicionar uma ACL a uma interface de rede
Neste exemplo, demonstramos como adicionar uma ACL a uma rede virtual.

>[!TIP]
>Também é possível adicionar uma ACL ao mesmo tempo em que você cria a interface de rede.

1. Obtenha ou crie a interface de rede à qual você adicionará a ACL.

   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

2. Obtenha ou crie a ACL que você adicionará à interface de rede.

   ```PowerShell
   $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"
   ```

3. Atribua a ACL à propriedade AccessControllist da interface de rede

   ```PowerShell
    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl
   ```

4. Adicionar a interface de rede no controlador de rede

   ```
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```

## <a name="example-remove-an-acl-from-a-network-interface-by-using-windows-powershell-and-the-network-controller-rest-api"></a>Exemplo: remover uma ACL de uma interface de rede usando o Windows PowerShell e a API REST do controlador de rede
Neste exemplo, mostramos como remover uma ACL. A remoção de uma ACL aplica o conjunto de regras padrão à interface de rede. O conjunto padrão de regras permite todo o tráfego de saída, mas bloqueia todo o tráfego de entrada.

>[!NOTE]
>Se você quiser permitir todo o tráfego de entrada, deverá seguir o [exemplo](#example-add-an-acl-to-a-network-interface) anterior para adicionar uma ACL que permite todos os tráfegos de entrada e de saída.


1. Obtenha a interface de rede da qual você removerá a ACL.<br>
   ```PowerShell
   $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

2. Atribua $NULL à propriedade AccessControllist da ipConfiguration.<br>
   ```PowerShell
   $nic.properties.ipconfigurations[0].properties.AccessControlList = $null
   ```

3. Adicione o objeto de interface de rede no controlador de rede.<br>
   ```PowerShell
   new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid
   ```