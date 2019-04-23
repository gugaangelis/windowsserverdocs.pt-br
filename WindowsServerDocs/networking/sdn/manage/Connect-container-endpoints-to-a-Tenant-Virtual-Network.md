---
title: Conectar pontos de extremidade do contêiner a uma rede virtual do locatário
description: Neste tópico, mostramos como se conectar a pontos de extremidade do contêiner a uma rede virtual existente do locatário criada por meio de SDN. Use o l2bridge (e, opcionalmente, l2tunnel) disponíveis com o plug-in do Windows libnetwork para Docker para criar uma rede de contêiner na VM de locatário do driver de rede.
manager: ravirao
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: pashort
author: jmesser81
ms.date: 08/24/2018
ms.openlocfilehash: 1968a4db9231459fe5858d9a0f3ba5e8f317ed1b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872737"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Conectar pontos de extremidade do contêiner a uma rede virtual do locatário

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, mostramos como se conectar a pontos de extremidade do contêiner a uma rede virtual existente do locatário criada por meio de SDN. Você usa o *l2bridge* (e opcionalmente *l2tunnel*) disponíveis com o plug-in do Windows libnetwork para Docker para criar uma rede de contêiner na VM de locatário do driver de rede.

No [drivers de rede de contêiner](https://docs.microsoft.com/virtualization/windowscontainers/container-networking/network-drivers-topologies) tópico, abordamos vários drivers de rede estão disponíveis por meio do Docker no Windows. Para SDN, use o *l2bridge* e *l2tunnel* drivers. Para ambos os drivers, cada ponto de extremidade do contêiner está na mesma sub-rede virtual da máquina de virtual de host (Locatário) do contêiner. 

O Host de rede HNS (serviço), com o plug-in de nuvem privada, atribui dinamicamente os endereços IP para pontos de extremidade do contêiner. Os pontos de extremidade de contêiner têm endereços IP exclusivos, mas compartilham o mesmo endereço MAC da máquina virtual de host (Locatário) do contêiner devido à conversão de endereços de camada 2. 

Diretiva de rede (ACLs, o encapsulamento e QoS) para esses pontos de extremidade do contêiner são impostas no host Hyper-V físico como recebido pelo controlador de rede e definidos em sistemas de gerenciamento de camada superior. 

A diferença entre o *l2bridge* e *l2tunnel* drivers são:

| l2bridge | l2tunnel |
| --- | --- |
|Pontos de extremidade de contêiner que residem em: <ul><li>O mesmo contêiner de host de máquina virtual e na mesma sub-rede ter todo o tráfego de rede com ponte no comutador virtual Hyper-V. </li><li>Contêiner diferente hospedar VMs ou em sub-redes diferentes e têm seu tráfego encaminhado para o host físico do Hyper-V. </li></ul>Política de rede não obter imposta, pois o tráfego de rede entre contêineres no mesmo host e na mesma sub-rede não fluem para o host físico. Política de rede aplica-se o tráfego de rede de contêiner somente como entre hosts ou entre sub-redes. | *Todos os* tráfego de rede entre dois pontos de extremidade do contêiner é encaminhado para o host do Hyper-V físico independentemente do host ou sub-rede. Política de rede se aplica ao tráfego de rede entre sub-redes e entre hosts. |
---

>[!NOTE]
>Esses modos de rede não funcionam para pontos de extremidade de contêiner windows está se conectando a uma rede virtual do locatário na nuvem pública do Azure.


## <a name="prerequisites"></a>Pré-requisitos
-  Uma infraestrutura SDN implantada com o controlador de rede.
-  Uma rede virtual do locatário foi criada.
-  Uma máquina virtual de locatário implantado com o recurso de contêiner do Windows habilitado, Docker instalado e o recurso Hyper-V habilitada. O recurso Hyper-V é necessário instalar vários binários para redes l2bridge e l2tunnel.

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
   ```

>[!Note]
>[A virtualização aninhada](https://msdn.microsoft.com/virtualization/hyperv_on_windows/user_guide/nesting) e expor extensões de virtualização não é necessária, a menos que o uso de contêineres do Hyper-V. 


## <a name="workflow"></a>Fluxo de Trabalho

[1. Adicionar várias configurações de IP a um recurso NIC da VM existente por meio do controlador de rede (Host Hyper-V)](#1-add-multiple-ip-configurations)
[2. Habilitar o proxy de rede no host para alocar endereços IP de CA para pontos de extremidade do contêiner (Host Hyper-V) ](#2-enable-the-network-proxy) 
 [3. Instalar a plug-in para atribuir endereços IP de autoridade de certificação para pontos de extremidade do contêiner (VM do Host do contêiner) de nuvem privada ](#3-install-the-private-cloud-plug-in) 
 [4. Criar uma *l2bridge* ou *l2tunnel* rede usando o docker (VM do Host do contêiner) ](#4-create-an-l2bridge-container-network)
 
>[!NOTE]
>Não há suporte para várias configurações de IP nos recursos de NIC da VM criados por meio do System Center Virtual Machine Manager. É recomendável para esses tipos de implantações que você crie o recurso NIC da VM fora da banda usando o PowerShell do controlador de rede.

### <a name="1-add-multiple-ip-configurations"></a>1. Adicionar várias configurações de IP
Nesta etapa, vamos supor que a NIC da VM da máquina virtual locatário tem uma configuração de IP com o endereço IP de 192.168.1.9 e é anexada a uma ID de recurso de rede virtual do 'VNet1' e o recurso de subrede VM de 'Subrede 1' na sub-rede IP 192.168.1.0/24. Adicionamos 10 endereços IP para contêineres do 192.168.1.101 - 192.168.1.110.

```powershell
Import-Module NetworkController

# Specify Network Controller REST IP or FQDN
$uri = "<NC REST IP or FQDN>"
$vnetResourceId = "VNet1"
$vsubnetResourceId = "Subnet1"

$vmnic= Get-NetworkControllerNetworkInterface -ConnectionUri $uri | where {$_.properties.IpConfigurations.Properties.PrivateIPAddress -eq "192.168.1.9" }
$vmsubnet = Get-NetworkControllerVirtualSubnet -VirtualNetworkId $vnetResourceId -ResourceId $vsubnetResourceId -ConnectionUri $uri

# For this demo, we will assume an ACL has already been defined; any ACL can be applied here
$allowallacl = Get-NetworkControllerAccessControlList -ConnectionUri $uri -ResourceId "AllowAll"


foreach ($i in 1..10)
{
    $newipconfig = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfiguration
    $props = new-object Microsoft.Windows.NetworkController.NetworkInterfaceIpConfigurationProperties

    $resourceid = "IP_192_168_1_1"
    if ($i -eq 10) 
    {
        $resourceid += "10"
        $ipstr = "192.168.1.110"
    }
    else
    {
        $resourceid += "0$i"
        $ipstr = "192.168.1.10$i"
    }
    
    $newipconfig.ResourceId = $resourceid
    $props.PrivateIPAddress = $ipstr    
    
    $props.PrivateIPAllocationMethod = "Static"
    $props.Subnet = new-object Microsoft.Windows.NetworkController.Subnet
    $props.Subnet.ResourceRef = $vmsubnet.ResourceRef
    $props.AccessControlList = new-object Microsoft.Windows.NetworkController.AccessControlList
    $props.AccessControlList.ResourceRef = $allowallacl.ResourceRef

    $newipconfig.Properties = $props
    $vmnic.Properties.IpConfigurations += $newipconfig
}

New-NetworkControllerNetworkInterface -ResourceId $vmnic.ResourceId -Properties $vmnic.Properties -ConnectionUri $uri
```

### <a name="2-enable-the-network-proxy"></a>2. Habilitar o proxy de rede
Nesta etapa, você deve habilitar o proxy de rede alocar vários endereços IP para a máquina virtual do host do contêiner. 

Para habilitar o proxy de rede, execute as [ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1) script na **Host Hyper-V** hospeda a máquina de virtual de host (Locatário) do contêiner.

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3. Instalar o plug-in de nuvem privada
Nesta etapa, você instala um plug-in para permitir que o HNS para se comunicar com o proxy de rede no Host Hyper-V.

Para instalar o plug-in, execute as [InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1) dentro do script as **máquina virtual do contêiner host (Locatário)**.


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4. Criar uma *l2bridge* rede de contêiner
Nesta etapa, você usa o `docker network create` comando os **máquina virtual do contêiner host (Locatário)** para criar uma rede l2bridge. 

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>Atribuição de IP estático não é compatível com *l2bridge* ou *l2tunnel* redes de contêiner quando usado com a pilha de SDN da Microsoft.

## <a name="more-information"></a>Mais informações
Para obter mais detalhes sobre como implantar uma infraestrutura SDN, consulte [implantar uma infraestrutura de rede definida pelo Software](https://docs.microsoft.com/windows-server/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

