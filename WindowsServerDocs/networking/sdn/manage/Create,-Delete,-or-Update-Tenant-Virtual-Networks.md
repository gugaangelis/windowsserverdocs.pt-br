---
title: Criar, excluir ou atualizar redes virtuais locatário
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 6ef30dcc31593e15c36f846cf6d64afcd4b85f19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="create-delete-or-update-tenant-virtual-networks"></a>Criar, excluir ou atualizar redes virtuais locatário

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como criar, excluir e atualizar redes virtuais de virtualização do Hyper-V rede após a implantação de rede definidos Software (SDN).  
  
Usando a virtualização de rede do Hyper-V, você pode isolar redes de locatário, para que cada rede de locatário é uma entidade completamente separada com nenhuma possibilidade de conexão entre a menos que você configure cargas de trabalho de acesso público.  
  
Você pode criar novas redes virtuais para locatários, você pode modificar redes virtuais existentes e se um locatário não precisa mais determinados recursos, ou se o locatário não está mais seu cliente, você pode excluir locatário redes virtuais.  
  
## <a name="create-a-new-virtual-network"></a>Criar uma nova rede Virtual  
  
Quando você cria uma rede Virtual para um locatário, ele é colocado em um domínio de roteamento exclusivo no host do Hyper-V.  
  
A seguir estão as etapas para criar uma nova rede Virtual.  
  
1. Identifique os prefixos de endereço IP do qual você deseja criar as sub-redes virtuais.   
2. Identifique a rede de provedor lógicos no qual o tráfego de locatário é encapsulado.   
3. Crie pelo menos uma sub-rede virtual para cada prefixo de IP que você definiu na etapa 1.   
  
>[!NOTE]  
>Abaixo de cada rede virtual há pelo menos uma sub-rede virtual. Virtuais sub-redes são definidos por um prefixo de IP e fazer referência a uma lista de controle de acesso definidos anteriormente.  
  
Opcionalmente, depois de concluir essas etapas, você pode também adicionar as listas de controle de acesso criado anteriormente para as sub-redes virtuais, ou adicionar conectividade de gateway para locatários.    
  
A tabela a seguir inclui exemplo sub-rede IDs e prefixos para dois locatários fictícios. O Fabrikam locatário tem duas sub-redes virtuais, e locatário da Contoso tem três sub-redes virtuais.  
  
  
  
Nome de locatário  |ID de sub-rede virtual  |Prefixo da sub-rede virtual    
---------|---------|---------  
Fabrikam    |5001         |24.30.1.0/24           
Fabrikam     |5002         | 24.30.2.0/20          
Contoso    |6001         |  24.30.1.0/24         
Contoso    | 6002        |  24.30.2.0/24         
Contoso     | 6003        | 24.30.3.0/24          
  
O script de exemplo a seguir usa os comandos do Windows PowerShell exportados do **NetworkController** module para criar uma rede virtual e uma sub-rede da Contoso:   
  
```  
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
Você pode usar o Windows PowerShell para atualizar uma sub-rede Virtual existente ou rede.   
  
Quando você executa o script de exemplo a seguir, os recursos atualizados são simplesmente colocar controlador de rede com a mesma ID de recurso. Se seu locatário Contoso quer adicionar um novo sub-rede virtual (24.30.2.0/24) para suas redes virtuais, você ou o administrador da Contoso pode usar o script a seguir.  
  
```  
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
  
O exemplo a seguir do Windows PowerShell exclui um locatário rede Virtual emitindo uma exclusão HTTP para o URI de identificação do recurso.  
  
    Remove-NetworkControllerVirtualNetwork -ResourceId "Contoso_Vnet1" -ConnectionUri $uri  


