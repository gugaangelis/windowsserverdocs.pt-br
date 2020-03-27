---
title: Usar ACLs (listas de controle de acesso) para gerenciar o fluxo de tráfego de rede do datacenter
description: Neste tópico, você aprenderá a configurar listas de controle de acesso (ACLs) para gerenciar o fluxo de tráfego de dados usando o Firewall do datacenter e as ACLs em sub-redes virtuais. Você habilita e configura o Firewall do datacenter criando ACLs que são aplicadas a uma sub-rede virtual ou a uma interface de rede.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a7ac5af-85e9-4440-a631-6a3a38e9015d
ms.author: lizross
author: eross-msft
ms.date: 08/27/2018
ms.openlocfilehash: 1f18ad9ddb0ea1a7575f6fcb26189f36f818ada2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317483"
---
# <a name="use-access-control-lists-acls-to-manage-datacenter-network-traffic-flow"></a>Usar ACLs (listas de controle de acesso) para gerenciar o fluxo de tráfego de rede do datacenter

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você aprenderá a configurar listas de controle de acesso (ACLs) para gerenciar o fluxo de tráfego de dados usando o Firewall do datacenter e as ACLs em sub-redes virtuais. Você habilita e configura o Firewall do datacenter criando ACLs que são aplicadas a uma sub-rede virtual ou a uma interface de rede.   

Os exemplos a seguir neste tópico demonstram como usar o Windows PowerShell para criar essas ACLs.  

## <a name="configure-datacenter-firewall-to-allow-all-traffic"></a>Configurar o Firewall do datacenter para permitir todo o tráfego  

Depois de implantar o SDN, você deve testar a conectividade de rede básica em seu novo ambiente.  Para fazer isso, crie uma regra para o Firewall do datacenter que permite todo o tráfego de rede, sem restrição.   

Use as entradas na tabela a seguir para criar um conjunto de regras que permitem todo o tráfego de rede de entrada e saída.


| IP de Origem | IP de destino | Protocolo | Porta de Origem | Porta de destino | Direção | Ação | Prioridade |
|:---------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|    \*     |       \*       |   Tudo    |     \*      |        \*        |  Entrada  | Permitir  |   100    |
|    \*     |       \*       |   Tudo    |     \*      |        \*        | Saída  | Permitir  |   110    |

---       

### <a name="example-create-an-acl"></a>Exemplo: criar uma ACL 
Neste exemplo, você cria uma ACL com duas regras:

1. **AllowAll_Inbound** – permite que todo o tráfego de rede passe para o adaptador de rede em que essa ACL está configurada.    
2. **AllowAllOutbound** – permite que todo o tráfego seja transmitido da interface de rede. Essa ACL, identificada pela ID de recurso "AllowAll-1", agora está pronta para ser usada em sub-redes virtuais e interfaces de rede.  

O script de exemplo a seguir usa comandos do Windows PowerShell exportados do módulo **NetworkController** para criar essa ACL.  


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
>A referência de comando do Windows PowerShell para o controlador de rede está localizada no tópico [cmdlets do controlador de rede](https://technet.microsoft.com/library/mt576401.aspx).  

## <a name="use-acls-to-limit-traffic-on-a-subnet"></a>Usar ACLs para limitar o tráfego em uma sub-rede  
Neste exemplo, você cria uma ACL que impede que as VMs na sub-rede 192.168.0.0/24 se comuniquem entre si. Esse tipo de ACL é útil para limitar a capacidade de um invasor se espalhar de forma mais tarde dentro da sub-rede, enquanto ainda permite que as VMs recebam solicitações de fora da sub-rede, bem como se comuniquem com outros serviços em outras sub-redes.   


|   IP de Origem    | IP de destino | Protocolo | Porta de Origem | Porta de destino | Direção | Ação | Prioridade |
|:--------------:|:--------------:|:--------:|:-----------:|:----------------:|:---------:|:------:|:--------:|
|  192.168.0.1   |       \*       |   Tudo    |     \*      |        \*        |  Entrada  | Permitir  |   100    |
|       \*       |  192.168.0.1   |   Tudo    |     \*      |        \*        | Saída  | Permitir  |   101    |
| 192.168.0.0/24 |       \*       |   Tudo    |     \*      |        \*        |  Entrada  | Bloquear  |   102    |
|       \*       | 192.168.0.0/24 |   Tudo    |     \*      |        \*        | Saída  | Bloquear  |   103    |
|       \*       |       \*       |   Tudo    |     \*      |        \*        |  Entrada  | Permitir  |   104    |
|       \*       |       \*       |   Tudo    |     \*      |        \*        | Saída  | Permitir  |   105    |

--- 

A ACL criada pelo script de exemplo abaixo, identificada pela ID de recurso **subnet-192-168-0-0**, agora pode ser aplicada a uma sub-rede de rede virtual que usa o endereço de sub-rede "192.168.0.0/24".  Qualquer interface de rede que é anexada a essa sub-rede de rede virtual automaticamente Obtém as regras de ACL acima aplicadas.  

Veja a seguir um exemplo de script usando comandos do Windows PowerShell para criar essa ACL usando a API REST do controlador de rede:  

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

