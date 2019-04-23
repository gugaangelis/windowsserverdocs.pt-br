---
title: Criar, excluir ou atualizar a rede virtual do locatário
description: Neste tópico, você aprenderá a criar, excluir e atualizar redes virtuais do Hyper-V Network Virtualization depois de implantar o Software Defined Networking (SDN). Virtualização de rede do Hyper-V ajuda a isolar redes de locatário para que cada rede de locatário é uma entidade separada. Cada entidade não tem nenhuma possibilidade de conexão cruzada, a menos que você configura cargas de trabalho de acesso público.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: pashort
author: shortpatti
ms.date: 08/24/2018
ms.openlocfilehash: a125ec220b4769a57a6be30f1425283afb7f0fe6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838347"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Criar, excluir ou atualizar redes virtuais do locatário

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você aprenderá a criar, excluir e atualizar redes virtuais do Hyper-V Network Virtualization depois de implantar o Software Defined Networking (SDN). Virtualização de rede do Hyper-V ajuda a isolar redes de locatário para que cada rede de locatário é uma entidade separada. Cada entidade não tem nenhuma possibilidade de conexão cruzada, a menos que você configura cargas de trabalho de acesso público.   
  
## <a name="create-a-new-virtual-network"></a>Criar uma nova rede virtual  
Criar uma rede virtual para um locatário coloca-o dentro de um domínio de roteamento exclusivo no host Hyper-V. Abaixo de cada rede virtual, há pelo menos uma sub-rede virtual. Subredes virtuais obtém definidas por um prefixo IP e fazer referência a uma ACL definida anteriormente.  

As etapas para criar uma nova rede virtual são:

1. Identifique os prefixos de endereço IP do qual você deseja criar sub-redes virtuais.   
2. Identifique a rede lógica do provedor no qual o tráfego de locatário é encapsulado.   
3. Crie pelo menos uma sub-rede virtual para cada prefixo IP que você identificou na etapa 1. 
4. (Opcional) Adicionar as ACLs criadas anteriormente para as sub-redes virtuais ou adicionar conectividade de gateway para locatários. 

A tabela a seguir inclui as IDs de sub-rede de exemplo e prefixos para dois locatários fictícios. O locatário Fabrikam tem duas sub-redes virtuais, enquanto o locatário Contoso tem três sub-redes virtuais.  
 
  
Nome do locatário  |ID de Sub-rede Virtual  |Prefixo de sub-rede virtual    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
O script de exemplo a seguir usa comandos do Windows PowerShell exportados do **NetworkController** módulo para criar a rede virtual da Contoso e a uma sub-rede:   
  
```Powershell  
import-module networkcontroller  
$URI = "https://ncrest.contoso.local"  
  
#Find the HNV Provider Logical Network  
  
$logicalnetworks = Get-NetworkControllerLogicalNetwork -ConnectionUri $uri  
foreach ($ln in $logicalnetworks) {  
   if ($ln.Properties.NetworkVirtualizationEnabled -eq "True") {  
      $HNVProviderLogicalNetwork = $ln  
   }  
}   
  
#Find the Access Control List to user per virtual subnet  
  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
#Create the Virtual Subnet  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_WebTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.1.0/24"  
  
#Create the Virtual Network  
  
$vnetproperties = new-object Microsoft.Windows.NetworkController.VirtualNetworkProperties  
$vnetproperties.AddressSpace = new-object Microsoft.Windows.NetworkController.AddressSpace  
$vnetproperties.AddressSpace.AddressPrefixes = @("24.30.1.0/24")  
$vnetproperties.LogicalNetwork = $HNVProviderLogicalNetwork  
$vnetproperties.Subnets = @($vsubnet)  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -Properties $vnetproperties  
  
```  
  
## <a name="modify-an-existing-virtual-network"></a>Modificar uma rede Virtual existente  
Você pode usar o Windows PowerShell para atualizar uma sub-rede de Virtual existente ou de rede.   
  
Quando você executa o script de exemplo a seguir, os recursos atualizados são simplesmente COLOCADOS no controlador de rede com a mesma ID de recurso. Se seu locatário Contoso quer adicionar uma nova sub-rede virtual (24.30.2.0/24) à sua rede virtual, você ou o administrador da Contoso pode usar o script a seguir.  
  
```PowerShell  
$acllist = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"  
  
$vnet = Get-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri  
  
$vnet.properties.AddressSpace.AddressPrefixes += "24.30.2.0/24"  
  
$vsubnet = new-object Microsoft.Windows.NetworkController.VirtualSubnet  
$vsubnet.ResourceId = "Contoso_DBTier"  
$vsubnet.Properties = new-object Microsoft.Windows.NetworkController.VirtualSubnetProperties  
$vsubnet.Properties.AccessControlList = $acllist  
$vsubnet.Properties.AddressPrefix = "24.30.2.0/24"  
  
$vnet.properties.Subnets += $vsubnet  
  
New-NetworkControllerVirtualNetwork -ResourceId "Contoso_VNet1" -ConnectionUri $uri -properties $vnet.properties  
  
```  
  
## <a name="delete-a-virtual-network"></a>Excluir uma rede Virtual  
  
Você pode usar o Windows PowerShell para excluir uma rede Virtual.  
  
O Windows PowerShell exemplo a seguir exclui um locatário de rede Virtual, emitindo um HTTP delete para o URI de identificação do recurso.  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```

