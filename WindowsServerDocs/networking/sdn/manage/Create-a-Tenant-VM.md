---
title: Criar uma máquina virtual e se conectar a uma rede virtual de locatário ou VLAN
description: Neste tópico, mostramos como criar uma VM de locatário e conectá-lo para uma rede virtual que você criou com a virtualização de rede do Hyper-V ou a uma rede Local virtual (VLAN).
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3c62f533-1815-4f08-96b1-dc271f5a2b36
ms.author: pashort
author: shortpatti
ms.date: 08/24/2018
ms.openlocfilehash: e23e6c020c12dd4900caa368daae0cc6dbeceaf4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856807"
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>Criar uma máquina virtual e se conectar a uma rede virtual de locatário ou VLAN

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você pode cria uma VM de locatário e conectá-lo para uma rede virtual que você criou com a virtualização de rede do Hyper-V ou a uma rede Local virtual (VLAN). Você pode usar o controlador de rede do Windows PowerShell cmdlets para se conectar a uma rede virtual ou NetworkControllerRESTWrappers para se conectar a uma VLAN.

Use os processos descritos neste tópico para implantar soluções de virtualização. Com algumas etapas adicionais, você pode configurar dispositivos para processar ou inspecionar os pacotes de dados que fluem para ou de outras VMs na rede Virtual.

As seções neste tópico incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para muitos parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com os valores que são apropriadas para sua implantação antes de executar esses comandos. 


## <a name="prerequisites"></a>Pré-requisitos

1. Adaptadores de rede VM criados com endereços MAC estáticos para o tempo de vida da VM.<p>Se o endereço MAC for alterado durante o tempo de vida da VM, o controlador de rede não é possível configurar a política necessária para o adaptador de rede. Não configurar a política para a rede impede que o adaptador de rede de processamento do tráfego de rede e falha de toda a comunicação com a rede.  

2. Se a VM requer acesso à rede na inicialização, não inicie a VM até depois de definir a ID de interface na VM a porta do adaptador de rede. Se você iniciar a VM antes de definir a ID de interface e a interface de rede não existe, a VM não pode se comunicar na rede no controlador de rede e todas as políticas aplicadas.

3. Se você precisar ACLs personalizadas para esse adaptador de rede, em seguida, crie a ACL agora usando instruções no tópico [Use Access Control Lists (ACLs) para gerenciar o data center tráfego fluxo de rede](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)

Certifique-se de que você já tenha criado uma rede Virtual antes de usar este exemplo de comando. Para obter mais informações, consulte [Create, Delete ou Update redes virtuais de locatário](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks).

## <a name="create-a-vm-and-connect-to-a-virtual-network-by-using-the-windows-powershell-network-controller-cmdlets"></a>Criar uma VM e conectar-se a uma rede Virtual usando os cmdlets do controlador de rede do Windows PowerShell


1. Crie uma VM com um adaptador de rede VM que tem um endereço MAC estático. 

   ```PowerShell    
   New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
   Set-VM -Name "MyVM" -ProcessorCount 4
    
   Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
   ```

2. Obtenha a rede virtual que contém a sub-rede à qual você deseja conectar o adaptador de rede.

   ```Powershell 
   $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId “Contoso_WebTier”
   ```

3. Crie um objeto de interface de rede no controlador de rede.

   >[!TIP]
   >Nesta etapa, você deve usar a ACL personalizada.

   ```PowerShell
   $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
   $vmnicproperties.PrivateMacAddress = "001122334455" 
   $vmnicproperties.PrivateMacAllocationMethod = "Static" 
   $vmnicproperties.IsPrimary = $true 

   $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
   $vmnicproperties.DnsSettings.DnsServers = @("24.30.1.11", "24.30.1.12")
    
   $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
   $ipconfiguration.resourceid = "MyVM_IP1"
   $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
   $ipconfiguration.properties.PrivateIPAddress = “24.30.1.101”
   $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"
    
   $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
   $ipconfiguration.properties.subnet.ResourceRef = $vnet.Properties.Subnets[0].ResourceRef
    
   $vmnicproperties.IpConfigurations = @($ipconfiguration)
   New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri
   ```

4. Obtenha a ID da instância para o adaptador de rede do controlador de rede.

   ```PowerShell 
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
   ```

5. Defina a ID de Interface na VM do Hyper-V a porta do adaptador de rede.

   >[!NOTE]
   >Você deve executar esses comandos no host Hyper-V em que a VM está instalada.

   ```PowerShell 
   #Do not change the hardcoded IDs in this section, because they are fixed values and must not change.
    
   $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"
    
   $vmNics = Get-VMNetworkAdapter -VMName “MyVM”
    
   $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNics
    
   if ($CurrentFeature -eq $null)
   {
   $Feature = Get-VMSystemSwitchExtensionPortFeature -FeatureId $FeatureId
    
   $Feature.SettingData.ProfileId = "{$($nic.InstanceId)}"
   $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
   $Feature.SettingData.CdnLabelString = "TestCdn"
   $Feature.SettingData.CdnLabelId = 1111
   $Feature.SettingData.ProfileName = "Testprofile"
   $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
   $Feature.SettingData.VendorName = "NetworkController"
   $Feature.SettingData.ProfileData = 1
    
   Add-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNics
   }
   else
   {
   $CurrentFeature.SettingData.ProfileId = "{$($nic.InstanceId)}"
   $CurrentFeature.SettingData.ProfileData = 1
    
   Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
   }
   ```

6. Inicie a VM.

   ```PowerShell
    Get-VM -Name “MyVM” | Start-VM 
   ```

Com êxito ter criado uma VM, conectados a VM a um locatário de rede Virtual e iniciado a VM para que ele possa processar cargas de trabalho de locatário.

## <a name="create-a-vm-and-connect-to-a-vlan-by-using-networkcontrollerrestwrappers"></a>Criar uma VM e conectar-se a uma VLAN usando NetworkControllerRESTWrappers


1. Criar a máquina virtual e atribuir um endereço MAC estático à VM.

   ```PowerShell
   New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

   Set-VM -Name "MyVM" -ProcessorCount 4

   Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
   ```

2. Defina a ID de VLAN no adaptador de rede VM.

   ```PowerShell
   Set-VMNetworkAdapterIsolation –VMName “MyVM” -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123
   ```

3. Obtenha a sub-rede de rede lógica e crie a interface de rede. 

   ```PowerShell
    $logicalnet = get-networkcontrollerLogicalNetwork -connectionuri $uri -ResourceId "00000000-2222-1111-9999-000000000002"

    $vmnicproperties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceProperties
    $vmnicproperties.PrivateMacAddress = "00-1D-C8-B7-01-02"
    $vmnicproperties.PrivateMacAllocationMethod = "Static"
    $vmnicproperties.IsPrimary = $true 
    
    $vmnicproperties.DnsSettings = new-object Microsoft.Windows.NetworkController.NetworkInterfaceDnsSettings
    $vmnicproperties.DnsSettings.DnsServers = $logicalnet.Properties.Subnets[0].DNSServers

    $ipconfiguration = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $ipconfiguration.resourceid = "MyVM_Ip1"
    $ipconfiguration.properties = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties
    $ipconfiguration.properties.PrivateIPAddress = “10.127.132.177”
    $ipconfiguration.properties.PrivateIPAllocationMethod = "Static"

    $ipconfiguration.properties.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $ipconfiguration.properties.subnet.ResourceRef = $logicalnet.Properties.Subnets[0].ResourceRef

    $vmnicproperties.IpConfigurations = @($ipconfiguration)
    $vnic = New-NetworkControllerNetworkInterface –ResourceID “MyVM_Ethernet1” –Properties $vmnicproperties –ConnectionUri $uri

    $vnic.InstanceId
   ```

4. Defina a ID da instância na porta do Hyper-V.

   ```PowerShell  
   #The hardcoded Ids in this section are fixed values and must not change.
   $FeatureId = "9940cd46-8b06-43bb-b9d5-93d50381fd56"

   $vmNics = Get-VMNetworkAdapter -VMName “MyVM”

   $CurrentFeature = Get-VMSwitchExtensionPortFeature -FeatureId $FeatureId -VMNetworkAdapter $vmNic
        
   if ($CurrentFeature -eq $null)
   {
       $Feature = Get-VMSystemSwitchExtensionFeature -FeatureId $FeatureId
        
       $Feature.SettingData.ProfileId = "{$InstanceId}"
       $Feature.SettingData.NetCfgInstanceId = "{56785678-a0e5-4a26-bc9b-c0cba27311a3}"
       $Feature.SettingData.CdnLabelString = "TestCdn"
       $Feature.SettingData.CdnLabelId = 1111
       $Feature.SettingData.ProfileName = "Testprofile"
       $Feature.SettingData.VendorId = "{1FA41B39-B444-4E43-B35A-E1F7985FD548}"
       $Feature.SettingData.VendorName = "NetworkController"
       $Feature.SettingData.ProfileData = 1
                
       Add-VMSwitchExtensionFeature -VMSwitchExtensionFeature  $Feature -VMNetworkAdapter $vmNic
   }        
   else
   {
       $CurrentFeature.SettingData.ProfileId = "{$InstanceId}"
       $CurrentFeature.SettingData.ProfileData = 1

       Set-VMSwitchExtensionPortFeature -VMSwitchExtensionFeature $CurrentFeature  -VMNetworkAdapter $vmNic
   }
   ```

5. Inicie a VM.

   ```PowerShell
   Get-VM -Name “MyVM” | Start-VM 
   ```

E com sucesso criou uma VM, conectados a VM a uma VLAN, iniciado a VM para que ele possa processar cargas de trabalho de locatário.

  

