---
title: Criar uma VM e se conectar a um locatário Virtual, rede ou VLAN
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 001eb3efa073e4ffbdfad41f88949303159a7274
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-vm-and-connect-to-a-tenant-virtual-network-or-vlan"></a>Criar uma VM e se conectar a um locatário Virtual, rede ou VLAN

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para criar um locatário virtual \(VM\) da máquina e conectar-se a VM para uma rede Virtual que você criou com a virtualização de rede do Hyper-V ou um virtual \(VLAN\) rede Local. 

Este tópico contém as seções a seguir.

- [Criar uma VM e se conectar a uma rede Virtual usando os cmdlets do controlador de rede do Windows PowerShell](#bkmk_vn)
- [Criar uma VM e conecte obtenham uma usando NetworkControllerRESTWrappers](#bkmk_vlan)

## <a name="requirements"></a>Requisitos
Antes de executar os procedimentos nas seções a seguir, observe os requisitos a seguir.

1. Você deve criar os adaptadores de rede VM com mídia estática \(MAC\) endereços para que o endereço MAC da VM não muda durante o ciclo de vida VM do controle de acesso. 
>[!NOTE]
>Se o endereço MAC de VM muda durante o ciclo de vida VM, o controlador de rede não pode configurar a política necessária para o adaptador de rede. Se a política para o adaptador de rede não estiver configurada, o adaptador de rede é impedido de processamento de tráfego de rede, e toda a comunicação com a rede falhará. 

2. Se a VM requer acesso à rede na inicialização, é importante que você não inicie a VM até após a etapa de configuração final - definindo o ID de Interface na VM porta de adaptador de rede. Se você iniciar a VM antes de você concluir essa etapa, a VM não poderão se comunicar na rede até que a interface de rede é criada no controlador de rede e o controlador aplicou todas as políticas aplicáveis - política de rede Virtual, listas de controle de acesso \(ACLs\) e qualidade de serviço \(QoS\).

Você também pode usar os processos que estão descritos neste tópico para implantar dispositivos virtuais. Com algumas etapas adicionais, você pode configurar dispositivos para processar ou inspecionar os pacotes de dados que o fluem de ou para outras VMs em uma rede Virtual.

>[!IMPORTANT]
>As seções a seguir incluem comandos do Windows PowerShell de exemplo que contêm valores de exemplo para vários parâmetros. Certifique-se de que você substitua os valores de exemplo nesses comandos com valores que são apropriados para sua implantação antes de executar esses comandos.  

## <a name="bkmk_vn"></a>Criar uma VM e se conectar a uma rede Virtual usando os cmdlets do controlador de rede do Windows PowerShell

Esta seção inclui os tópicos a seguir.

1.  [Criar uma máquina virtual com um adaptador de rede VM que tenha um endereço MAC estático](#bkmk_create)
2.  [Obtenha a rede Virtual que contém a sub-rede ao qual você deseja se conectar o adaptador de rede](#bkmk_getvn)
3.  [Crie um objeto de interface de rede no controlador de rede](#bkmk_object)
4.  [Obtenha o InstanceId para a interface de rede do controlador de rede](#bkmk_getinstance)
5.  [Definir a ID de Interface na VM Hyper-V porta de adaptador de rede](#bkmk_setinstance)
6.  [Inicie a máquina virtual](#bkmk_start)

### <a name="bkmk_create"></a>Criar uma máquina virtual com um adaptador de rede VM que tenha um endereço MAC estático

Para criar uma máquina virtual com um adaptador de rede que tenha um endereço MAC estático, use o comando de exemplo a seguir.

    
    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 
    
    Set-VM -Name "MyVM" -ProcessorCount 4
    
    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 
    

### <a name="bkmk_getvn"></a>Obtenha a rede Virtual que contém a sub-rede ao qual você deseja se conectar o adaptador de rede
Certifique-se de que você já criou uma rede Virtual antes de usar esse comando de exemplo. Para obter mais informações, consulte [criar, excluir ou redes virtuais do locatário atualização](https://technet.microsoft.com/windows-server-docs/networking/sdn/manage/create%2c-delete%2c-or-update-tenant-virtual-networks).

Para obter a rede Virtual, use o comando de exemplo a seguir.

    
    $vnet = get-networkcontrollervirtualnetwork -connectionuri $uri -ResourceId “Contoso_WebTier”
    

>[!NOTE]
>Se você precisar ACLs personalizadas para essa interface de rede, em seguida, criar a ACL agora usando instruções no tópico [uso controle listas de acesso (ACLs) para gerenciar Datacenter rede tráfego fluir](../../sdn/manage/Use-Access-Control-Lists--ACLs--to-Manage-Datacenter-Network-Traffic-Flow.md)

### <a name="bkmk_object"></a>Crie um objeto de interface de rede no controlador de rede

Para criar um objeto de interface de rede no controlador de rede, use o comando de exemplo a seguir.

>[!NOTE]
>Se você criou uma ACL personalizado após a etapa anterior, você pode usá-lo agora.

    
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
    

### <a name="bkmk_getinstance"></a>Obtenha o InstanceId para a interface de rede do controlador de rede
Para obter o InstanceId da interface de rede do controlador de rede, use o comando de exemplo a seguir.

    
    $nic = Get-NetworkControllerNetworkInterface -ConnectionUri $uri -ResourceId "MyVM-Ethernet1"
    

### <a name="bkmk_setinstance"></a>Definir a ID de Interface na VM Hyper-V porta de adaptador de rede
Para definir a ID de Interface da VM do Hyper-V porta de adaptador de rede, use o comando de exemplo a seguir.

>[!NOTE]
>Você deve executar esses comandos no host do Hyper-V onde a VM está instalada.

    
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
    

### <a name="bkmk_start"></a>Inicie a máquina virtual
Para iniciar a VM, use o comando de exemplo a seguir.

    
    Get-VM -Name “MyVM” | Start-VM 
    
Você tem agora com êxito criou uma VM, conectado a VM a um rede Virtual locatário e começar a VM para que ele possa processar locatário cargas de trabalho.

## <a name="bkmk_vlan"></a>Criar uma VM e conecte obtenham uma usando NetworkControllerRESTWrappers

Esta seção inclui os tópicos a seguir.

1. [Crie a VM e atribua um endereço MAC estático](#bkmk_mac)
2. [Definir a ID de VLAN no adaptador de rede VM](#bkmk_vid)
3. [Obtenha a sub-rede da rede lógica e crie a interface de rede](#bkmk_subnet)
4. [Defina o InstanceId na porta Hyper-V](#bkmk_instance)
5. [Inicie a máquina virtual](#bkmk_startvlan)


###<a name="bkmk_mac"></a>Crie a VM e atribua um endereço MAC estático
Para criar uma VM e atribuir uma mídia estática \(MAC\) endereço do controle de acesso para a VM, você pode usar os comandos de exemplo a seguir.

    New-VM -Generation 2 -Name "MyVM" -Path "C:\VMs\MyVM" -MemoryStartupBytes 4GB -VHDPath "c:\VMs\MyVM\Virtual Hard Disks\WindowsServer2016.vhdx" -SwitchName "SDNvSwitch" 

    Set-VM -Name "MyVM" -ProcessorCount 4

    Set-VMNetworkAdapter -VMName "MyVM" -StaticMacAddress "00-11-22-33-44-55" 

###<a name="bkmk_vid"></a>Definir a ID de VLAN no adaptador de rede VM
Para definir a ID de VLAN no adaptador de rede, você pode usar o comando de exemplo a seguir.


    Set-VMNetworkAdapterIsolation –VMName “MyVM” -AllowUntaggedTraffic $true -IsolationMode VLAN -DefaultIsolationId 123


###<a name="bkmk_subnet"></a>Obtenha a sub-rede da rede lógica e crie a interface de rede

Para obter a sub-rede da rede lógica e criar a interface de rede usando a sub-rede da rede lógica, você pode usar os comandos de exemplo a seguir.


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
    

###<a name="bkmk_instance"></a>Defina o InstanceId na porta Hyper-V
Para definir o InstanceId na porta Hyper-V, você pode usar os comandos de exemplo a seguir no host do Hyper-V onde a VM está localizada.

  
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


###<a name="bkmk_startvlan"></a>Inicie a máquina virtual
Para iniciar a máquina virtual, você pode usar o comando de exemplo a seguir.


    Get-VM -Name “MyVM” | Start-VM 

Você tem agora com êxito criou uma VM, conectado a VM obtenham uma e começar a VM para que ele possa processar locatário cargas de trabalho.

  

