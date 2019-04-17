---
title: Usar listas de controle de acesso (ACLs) para gerenciar o fluxo de tráfego de rede de Datacenter
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 64b7e1abf1ddb8132a8c6692fe82521c589f32df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Usar listas de controle de acesso (ACLs) para gerenciar o fluxo de tráfego de rede de Datacenter

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como configurar listas de controle de acesso para gerenciar o fluxo de dados usando o Firewall de Datacenter e ACLs em sub-redes virtuais.  
  
Você pode ativar e configurar o Firewall de Datacenter com a criação de ACLs que são aplicadas a uma sub-rede virtual ou uma interface de rede.  
  
Os exemplos a seguir demonstram como usar o Windows PowerShell para criar essas ACLs.  
  
## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurar o Firewall para permitir que todo o tráfego do Datacenter  
  
Depois de implantar SDN, é recomendável que você teste a conectividade de rede básica no novo ambiente.  
  
Para fazer isso, você pode criar uma regra de Firewall de Datacenter que permite que todo o tráfego de rede, sem restrição.   
  
Você pode usar as entradas na tabela a seguir para criar um conjunto de regras que permitem que todo o tráfego de rede de entrada e saída.  
  
  
  
IP de origem|Destino IP|Protocolo|Porta de origem|Porta de destino|Direção|Ação|Prioridade   
---------|---------|---------|---------|---------|---------|---------|---------  
*    |   *      |   Todos os      |    *     |      *   |     Entrada    |   Permitir      |   100        
*    |     *    |     Todos os    |     *    |     *    |   Saída      |  Permitir       |  110         
  
  
Este script de exemplo cria uma ACL que contém duas regras:    
- A primeira regra "AllowAll_Inbound" permite que todo o tráfego de rede para passar para a interface de rede onde essa ACL está configurado.    
- A segunda regra, "AllowAllOutbound" permite que todo o tráfego passar para fora da interface de rede.  
Essa ACL, identificado pela id do recurso "AllowAll-1" agora está pronto para ser usado em sub-redes virtuais e interfaces de rede.  
  
O script de exemplo a seguir usa os comandos do Windows PowerShell exportados do **NetworkController** module para criar esse ACL.  
  
  
```  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "100"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  
$aclrule1 = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule1.Properties = $ruleproperties  
$aclrule1.ResourceId = "AllowAll_Inbound"  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "110"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  
$aclrule2 = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule2.Properties = $ruleproperties  
$aclrule2.ResourceId = "AllowAll_Outbound"  
$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties  
$acllistproperties.AclRules = @($aclrule1, $aclrule2)  
New-NetworkControllerAccessControlList -ResourceId "AllowAll" -Properties $acllistproperties -ConnectionUri <NC REST FQDN>  
```  
  
>[!NOTE]  
>A referência de comando do Windows PowerShell para o controlador de rede está localizada no tópico [Cmdlets de controlador de rede](https://technet.microsoft.com/library/mt576401.aspx).  
  
## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Usar ACLs para limitar o tráfego em uma sub-rede  
  
Você pode usar este exemplo para criar uma ACL que impede que VMs (máquinas virtuais) dentro da sub-rede 192.168.0.0/24 de se comunicar uns com os outros.   
  
Esse tipo de ACL é útil para limitar a capacidade de um invasor a disseminação lateralmente dentro da sub-rede, permitindo ainda VMs para receber solicitações de fora da sub-rede, bem como para se comunicar com outros serviços em outras sub-redes.  
  
  
IP de origem|Destino IP|Protocolo|Porta de origem|Porta de destino|Direção|Ação|Prioridade   
---------|---------|---------|---------|---------|---------|---------|---------  
192.168.0.1    | * | Todos os | * | * | Entrada|   Permitir      |   100        
* |192.168.0.1 | Todos os | * | * | Saída|  Permitir       |  101         
192.168.0.0/24 | * | Todos os | * | * | Entrada| Bloco | 102  
* | 192.168.0.0/24 |Todos os| * | * | Saída | Bloco |103  
* | *  |Todos os| * | * | Entrada | Permitir |104  
* | *  |Todos os| * | * | Saída | Permitir |105  
  
A ACL criado pelo script de exemplo abaixo, identificado pela identificação de recurso **sub-rede-192-168-0-0**, agora podem ser aplicadas a uma sub-rede da rede virtual que usa o endereço de sub-rede "192.168.0.0/24".  Qualquer interface de rede que está conectada à sub-rede dessa rede virtual automaticamente obtém as regras ACL acima aplicadas.  
  
A seguir está um exemplo de script usando comandos do Windows Powershell para criar esse ACL usando a API de REST de controlador de rede:  
  
```  
import-module networkcontroller  
$ncURI = "https://mync.contoso.local"  
$aclrules = @()  
  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "192.168.0.1"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "100"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  
  
$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowRouter_Inbound"  
$aclrules += $aclrule  
  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "192.168.0.1"  
$ruleproperties.Priority = "101"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  
  
$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowRouter_Outbound"  
$aclrules += $aclrule  
  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Deny"  
$ruleproperties.SourceAddressPrefix = "192.168.0.0/24"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "102"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  
  
$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "DenySubnet_Inbound"  
$aclrules += $aclrule  
  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Deny"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "192.168.0.0/24"  
$ruleproperties.Priority = "103"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  
  
$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "DenySubnet_Outbound"  
  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "104"  
$ruleproperties.Type = "Inbound"  
$ruleproperties.Logging = "Enabled"  
  
$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowAll_Inbound"  
$aclrules += $aclrule  
  
$ruleproperties = new-object Microsoft.Windows.NetworkController.AclRuleProperties  
$ruleproperties.Protocol = "All"  
$ruleproperties.SourcePortRange = "0-65535"  
$ruleproperties.DestinationPortRange = "0-65535"  
$ruleproperties.Action = "Allow"  
$ruleproperties.SourceAddressPrefix = "*"  
$ruleproperties.DestinationAddressPrefix = "*"  
$ruleproperties.Priority = "105"  
$ruleproperties.Type = "Outbound"  
$ruleproperties.Logging = "Enabled"  
  
$aclrule = new-object Microsoft.Windows.NetworkController.AclRule  
$aclrule.Properties = $ruleproperties  
$aclrule.ResourceId = "AllowAll_Outbound"  
$aclrules += $aclrule  
  
$acllistproperties = new-object Microsoft.Windows.NetworkController.AccessControlListProperties  
$acllistproperties.AclRules = $aclrules  
  
New-NetworkControllerAccessControlList -ResourceId "Subnet-192-168-0-0" -Properties $acllistproperties -ConnectionUri $ncURI   
```  
  
