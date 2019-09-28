---
title: Criar, excluir ou atualizar a rede virtual de locatário
description: Neste tópico, você aprenderá a criar, excluir e atualizar redes virtuais de virtualização de rede Hyper-V depois de implantar o SDN (rede definida pelo software). A virtualização de rede Hyper-V ajuda você a isolar redes de locatários para que cada rede de locatários seja uma entidade separada. Cada entidade não tem nenhuma possibilidade de conexão cruzada, a menos que você configure cargas de trabalho de acesso público.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6a820826-e829-4ef2-9a20-f74235f8c25b
ms.author: pashort
author: shortpatti
ms.date: 08/24/2018
ms.openlocfilehash: 779c7bc4f6c4ff1e66fca68ced8b0eeb4d54abc5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406066"
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Criar, excluir ou atualizar redes virtuais do locatário

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste tópico, você aprenderá a criar, excluir e atualizar redes virtuais de virtualização de rede Hyper-V depois de implantar o SDN (rede definida pelo software). A virtualização de rede Hyper-V ajuda você a isolar redes de locatários para que cada rede de locatários seja uma entidade separada. Cada entidade não tem nenhuma possibilidade de conexão cruzada, a menos que você configure cargas de trabalho de acesso público.   
  
## <a name="create-a-new-virtual-network"></a>Criar uma nova rede virtual  
A criação de uma rede virtual para um locatário a coloca em um domínio de roteamento exclusivo no host do Hyper-V. Abaixo de cada rede virtual, há pelo menos uma sub-rede virtual. Sub-redes virtuais são definidas por um prefixo IP e fazem referência a uma ACL definida anteriormente.  

As etapas para criar uma nova rede virtual são:

1. Identifique os prefixos de endereço IP dos quais você deseja criar as sub-redes virtuais.   
2. Identifique a rede do provedor lógico na qual o tráfego do locatário é encapsulado.   
3. Crie pelo menos uma sub-rede virtual para cada prefixo IP que você identificou na etapa 1. 
4. Adicional Adicione as ACLs criadas anteriormente às sub-redes virtuais ou adicione conectividade de gateway para locatários. 

A tabela a seguir inclui exemplos de IDs de sub-rede e prefixos para dois locatários fictícios. A Fabrikam do locatário tem duas sub-redes virtuais, enquanto o locatário da Contoso tem três sub-redes virtuais.  
 
  
Nome do locatário  |ID de Sub-rede Virtual  |Prefixo de sub-rede virtual    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Funcionam    |6001         |  24.30.1.0/24         
Funcionam    | 6002        |  24.30.2.0/24         
Funcionam     | 6003        | 24.30.3.0/24          
  
O script de exemplo a seguir usa comandos do Windows PowerShell exportados do módulo **NetworkController** para criar a rede virtual da Contoso e uma sub-rede:   
  
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
  
## <a name="modify-an-existing-virtual-network"></a>Modificar uma rede virtual existente  
Você pode usar o Windows PowerShell para atualizar uma rede ou sub-rede virtual existente.   
  
Quando você executa o script de exemplo a seguir, os recursos atualizados são simplesmente colocados no controlador de rede com a mesma ID de recurso. Se o locatário contoso quiser adicionar uma nova sub-rede virtual (24.30.2.0/24) à sua rede virtual, você ou o administrador da Contoso poderão usar o script a seguir.  
  
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
  
## <a name="delete-a-virtual-network"></a>Excluir uma rede virtual  
  
Você pode usar o Windows PowerShell para excluir uma rede virtual.  
  
O exemplo do Windows PowerShell a seguir exclui uma rede virtual de locatário emitindo uma exclusão HTTP para o URI da ID do recurso.  

```PowerShell  
Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  
```

