---
title: Configurar o Firewall de Datacenter listas de controle de acesso (ACLs)
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: d5f7c4baad24a720e073857cb6c835167e5b419b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-datacenter-firewall-access-control-lists-acls"></a>Configurar o Firewall de Datacenter listas de controle de acesso (ACLs)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode aplicar ACLs específicas para interfaces de rede.  Se ACLs também são definidos na sub-rede virtual à qual a interface de rede está conectada, as ACLs são aplicadas, mas ACLs de interface de rede é priorizada acima da sub-rede virtual ACLs.

Este tópico contém as seções a seguir.

- [Exemplo: Adicionar uma ACL de uma interface de rede](#bkmk_addacl)
- [Exemplo: Remover uma ACL de uma interface de rede usando o Windows Powershell e a API de REST do controlador de rede](#bkmk_removeacl)

##<a name="bkmk_addacl"></a>Exemplo: Adicionar uma ACL de uma interface de rede

No tópico [uso controle listas de acesso (ACLs) para gerenciar Datacenter rede tráfego fluir](Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md) você aprendeu como criar uma ACL e atribua a uma sub-rede virtual.  No entanto, em alguns casos, convém substituir esse padrão ACL na sub-rede virtual com uma ACL específica para uma interface de rede individuais.  Você também precisará aplicar ACLs diretamente para interfaces de rede que são anexados ao VLANs em vez de redes virtuais.

Este exemplo demonstra como adicionar uma ACL a uma rede virtual. 

>[!NOTE]
>Também é possível adicionar uma ACL ao mesmo tempo que você cria a interface de rede.

###<a name="step-1-get-or-create-the-network-interface-to-which-you-will-add-the-acl"></a>Etapa 1: Obter ou criar a interface de rede à qual você adicionará a ACL

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-get-or-create-the-acl-you-will-add-to-the-network-interface"></a>Etapa 2: Obtenha ou criar a ACL que você adicionará à interface de rede
Você pode usar o comando de exemplo a seguir para obter ou criar a ACL. 

    $acl = get-networkcontrolleraccesscontrollist -ConnectionUri $uri -resourceid "AllowAllACL"

###<a name="step-3-assign-the-acl-to-the-accesscontrollist-property-of-the-network-interface"></a>Etapa 3: Atribuir a ACL à propriedade AccessControlList da interface de rede
Você pode usar o comando de exemplo a seguir para atribuir a ACL à propriedade AccessControlList.

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $acl

###<a name="step-4-add-the-network-interface-in-network-controller"></a>Etapa 4: Adicionar a interface de rede no controlador de rede
Você pode usar o comando de exemplo a seguir para adicionar a interface de rede no controlador de rede.

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid


##<a name="bkmk_removeacl"></a>Exemplo: Remover uma ACL de uma interface de rede usando o Windows Powershell e a API de REST do controlador de rede
Você pode usar este exemplo, se você quiser remover uma ACL. Quando você remove uma ACL, o conjunto padrão de regras são aplicadas à interface de rede.

O conjunto de regras padrão permite que todo o tráfego de saída, mas bloqueia todo o tráfego.

>[!NOTE]
>Se você quiser permitir que todo o tráfego, você deve seguir o exemplo anterior para adicionar uma ACL que permite que todo o tráfego de entrada e saído tudo.

###<a name="step-1-get-the-network-interface-from-which-you-will-remove-the-acl"></a>Etapa 1: Obter a interface de rede do qual você removerá a ACL
Você pode usar o comando de exemplo a seguir para recuperar a interface de rede.

    $nic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-2-assign-null-to-the-accesscontrollist-property-of-the-ipconfiguration"></a>Etapa 2: Atribuir $NULL à propriedade AccessControlList do ipConfiguration
Você pode usar o comando de exemplo a seguir para atribuir $NULL à propriedade AccessControlList.

    $nic.properties.ipconfigurations[0].properties.AccessControlList = $null

###<a name="step-3-add-the-network-interface-object-in-network-controller"></a>Etapa 3: Adicionar o objeto de interface de rede no controlador de rede
Você pode usar o comando de exemplo a seguir para adicionar o objeto de interface de rede no controlador de rede

    new-networkcontrollernetworkinterface -ConnectionUri $uri -Properties $nic.properties -ResourceId $nic.resourceid

