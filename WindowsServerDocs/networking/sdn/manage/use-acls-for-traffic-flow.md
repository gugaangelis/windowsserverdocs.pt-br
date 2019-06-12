---
title: Usar o controle de acesso (ACLs) de lista para gerenciar o fluxo de tráfego de rede do datacenter
description: Neste tópico, você aprenderá como configurar listas de controle de acesso (ACLs) para gerenciar o fluxo de dados usando o Firewall do Datacenter e ACLs em sub-redes virtuais. Habilitar e configurar o Firewall do Datacenter com a criação de ACLs que serão aplicadas a uma sub-rede virtual ou um adaptador de rede.
manager: dougkim
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
ms.date: 08/27/2018
ms.openlocfilehash: 7bfb74e0964735d357226ab1e5af826796c48d81
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446307"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Usar o controle de acesso (ACLs) de lista para gerenciar o fluxo de tráfego de rede do datacenter

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você aprenderá como configurar listas de controle de acesso (ACLs) para gerenciar o fluxo de dados usando o Firewall do Datacenter e ACLs em sub-redes virtuais. Habilitar e configurar o Firewall do Datacenter com a criação de ACLs que serão aplicadas a uma sub-rede virtual ou um adaptador de rede.   

Os exemplos a seguir neste tópico demonstram como usar o Windows PowerShell para criar essas ACLs.  

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurar o Firewall do Datacenter para permitir todo o tráfego  

Depois de implantar SDN, você deve testar a conectividade de rede básica em seu novo ambiente.  Para fazer isso, crie uma regra de Firewall do Datacenter que permite que todo o tráfego de rede, sem restrição.   

Use as entradas na tabela a seguir para criar um conjunto de regras que permitem todo tráfego de rede de entrada e saída.


| IP de Origem | IP de destino | Protocol | Porta de Origem | Porta de destino | Direction | Ação | Priority |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   Todas    |     \*      |        \*        |  Entrada  | Permitir  |   100    |
|    \*     |       \*       |   Todas    |     \*      |        \*        | Saída  | Permitir  |   110    |

---       

### <a name="example-create-an-acl"></a>Exemplo: Crie uma ACL 
Neste exemplo, você pode criar uma ACL com duas regras:

1. **AllowAll_Inbound** -permite que todo o tráfego de rede passar para a interface de rede no qual essa ACL é configurado.    
2. **AllowAllOutbound** -permite que todo o tráfego seja passado para fora da interface de rede. Essa ACL, identificado pela id de recurso "AllowAll-1" está pronto para ser usado em interfaces de rede e sub-redes virtuais.  

O script de exemplo a seguir usa comandos do Windows PowerShell exportados do **NetworkController** módulo para criar essa ACL.  


```PowerShell
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
>A referência de comando do Windows PowerShell para o controlador de rede está localizada no tópico [Cmdlets do controlador de rede](https://technet.microsoft.com/library/mt576401.aspx).  

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Usar ACLs para limitar o tráfego em uma sub-rede  
Neste exemplo, você cria uma ACL que impede que as VMs dentro da sub-rede 192.168.0.0/24 de se comunicar entre si. Esse tipo de ACL é útil para limitar a capacidade de um invasor para distribuir lateralmente dentro da sub-rede, enquanto ainda permite que as VMs para receber solicitações de fora da sub-rede, bem como para se comunicar com outros serviços em outras sub-redes.   


|   IP de Origem    | IP de destino | Protocol | Porta de Origem | Porta de destino | Direction | Ação | Priority |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   Todas    |     \*      |        \*        |  Entrada  | Permitir  |   100    |
|       \*       |  192.168.0.1   |   Todas    |     \*      |        \*        | Saída  | Permitir  |   101    |
| 192.168.0.0/24 |       \*       |   Todas    |     \*      |        \*        |  Entrada  | Bloqueio  |   102    |
|       \*       | 192.168.0.0/24 |   Todas    |     \*      |        \*        | Saída  | Bloqueio  |   103    |
|       \*       |       \*       |   Todas    |     \*      |        \*        |  Entrada  | Permitir  |   104    |
|       \*       |       \*       |   Todas    |     \*      |        \*        | Saída  | Permitir  |   105    |

--- 

A ACL criada pelo script de exemplo abaixo, identificado pela id de recurso **subrede-192-168-0-0**, agora pode ser aplicado a uma sub-rede de rede virtual que usa o endereço de sub-rede "192.168.0.0/24".  Qualquer interface de rede está conectado à sub-rede de rede virtual automaticamente obtém as regras ACL acima aplicadas.  

A seguir está um exemplo de script usando comandos do Windows Powershell para criar essa ACL usando a API de REST do controlador de rede:  

```PowerShell  
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

