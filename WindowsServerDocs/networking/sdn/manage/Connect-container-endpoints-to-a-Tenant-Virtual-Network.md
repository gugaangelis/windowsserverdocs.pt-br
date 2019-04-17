---
title: Conectar contêineres a uma rede Virtual
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
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
ms.openlocfilehash: 801cf4b8f71935eb72d820d47e523a310fa64562
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Pontos de extremidade do contêiner conectar-se a uma rede virtual locatário

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico mostra como conectar os pontos de extremidade do contêiner a uma rede virtual existentes do locatário criada através da pilha do Microsoft Software definido de rede (SDN). Nós usaremos o *l2bridge* (e, opcionalmente, *l2tunnel*) driver de rede disponível com o Windows libnetwork plug-in para Docker criar uma rede de contêiner na máquina virtual contêiner host (Locatário).

Como documentado no [contêiner de rede](https://msdn.microsoft.com/en-us/virtualization/windowscontainers/management/container_networking) tópico no MSDN, vários drivers de rede estão disponíveis por meio de Docker no Windows. Os drivers mais adequados para SDN são *l2bridge* e *l2tunnel *. Para ambos os drivers, cada ponto de extremidade do contêiner está na mesma sub-rede virtual a VM do contêiner host (Locatário). Os endereços IP para pontos de extremidade de contêiner são atribuídos dinamicamente pelo Host de rede serviço (HNS) por meio do plug-in de nuvem privada. Os pontos de extremidade do contêiner têm endereços IP exclusivos, mas compartilham o mesmo endereço MAC da máquina virtual contêiner host (Locatário) devido à conversão de endereços de camada 2. Política de rede (por exemplo: ACLs, encapsulamento e QoS) para esses pontos de extremidade do contêiner são impostos no host do Hyper-V físico como recebido pelo controlador de rede e definidos em sistemas de gerenciamento de camada superior. Há uma pequena diferença entre o *l2bridge* e *l2tunnel* drivers que é explicado abaixo.

- **Ponte L2** -pontos de extremidade de contêiner que residem no mesmo contêiner host virtual computador e estão na mesma sub-rede tem todo o tráfego de rede de transição dentro do switch virtual Hyper-V. Pontos de extremidade de contêiner que residem no host do contêiner diferentes VMs ou que estão em várias sub-redes têm o tráfego encaminhado para o host do Hyper-V físico. Desde que o tráfego de rede entre contêineres no mesmo host e na mesma sub-rede não fluem para o host físico, nenhuma política de rede é aplicada. Política é aplicada somente para o tráfego de rede de contêiner entre hosts ou entre sub-rede.  
 
- **Encapsulamento de L2** - *todos os* o tráfego de rede entre dois pontos de extremidade do contêiner é encaminhado para o host do Hyper-V físico, independentemente de host ou sub-rede. Política de rede é imposta para o tráfego de rede entre sub-rede e entre hosts.   

>[!NOTE]
>Esses modos de redes não funcionam para pontos de extremidade de contêiner windows conectar a uma rede virtual locatário na nuvem pública do Azure

## <a name="prerequistes"></a>Pré-requisitos
 * Uma infraestrutura SDN com o controlador de rede foi implantada
 * Uma rede virtual locatário foi criada
 * Uma máquina virtual de locatário tenha sido implantada com o recurso de contêiner do Windows habilitado, Docker instalado e recurso Hyper-V habilitada

>[!Note]
>[Virtualização aninhada](https://msdn.microsoft.com/en-us/virtualization/hyperv_on_windows/user_guide/nesting) e expondo extensões de virtualização não é necessária, a menos que usar o recurso contêineres Hyper-V o Hyper-v é necessário para instalar vários binários para redes l2bridge e l2tunnel

```powershell
# To install HyperV feature without checks for nested virtualization
dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All 
```

 

## <a name="workflow"></a>Fluxo de trabalho

1. [Adicionar várias configurações de IP a um recurso de VM NIC existente por meio de controlador de rede](#Add) (Host do Hyper-V)
2. [Habilitar o proxy de rede no host alocar endereços IP de autoridade de certificação para pontos de extremidade do contêiner](#Enable) (Host do Hyper-V) 
3. [Instalar a plug-in para atribuir endereços IP CA a pontos de extremidade do contêiner de nuvem privada](#Install) (contêiner Host VM) 
4. [Criar um *l2bridge* ou *l2tunnel* rede usando docker](#Create) (contêiner Host VM) 
 
>[!NOTE]
>Várias configurações de IP não é compatível com recursos de VM NIC criados por meio do System Center Virtual Machine Manager. É recomendável para esses tipos de implantações que você criar o recurso de VM NIC fora de banda usando o PowerShell de controlador de rede.

### <a name="Add"></a>1. adicionar várias configurações de IP

Para este exemplo, presumimos que o VM NIC da máquina virtual locatário já tem uma configuração de IP com o endereço IP de 192.168.1.9 e é anexado a um ID de recurso VNet de 'VNet1' e recursos de sub-rede VM do 'Subnet1' na sub-rede IP 192.168.1.0/24. Adicionaremos 10 endereços IP para contêineres de 192.168.1.101 - 192.168.1.110.

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

### <a name="Enable"></a>2. Ative o Proxy de rede

[ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1>)

Executar esse script **Host do Hyper-V** que hospeda a máquina de virtual contêiner host (Locatário) para habilitar o proxy de rede alocar vários endereços IP para a VM do host de contêiner.

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="Install"></a>3. Instale o plug-in de nuvem privada

[InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1)

Executar esse script dentro do **contêiner host (Locatário) virtual machine** para permitir que o Host de rede serviço (HNS) para se comunicar com o proxy de rede no Host do Hyper-V.

```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="Create"></a>4. Crie um *l2bridge* rede de contêiner

Sobre o **contêiner host (Locatário) virtual machine** usar o `docker network create`comando para criar uma rede l2bridge

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork 
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>Não há suporte para a atribuição de IP estático com *l2bridge* ou *l2tunnel* contêiner redes quando usado com o Microsoft SDN pilha.

## <a name="more-information"></a>Obter mais informações
Para mais infortation sobre a implantação de uma infraestrutura SDN, consulte [implantar uma infraestrutura de rede definidos do Software](https://technet.microsoft.com/en-us/windows-server-docs/networking/sdn/deploy/deploy-a-software-defined-network-infrastructure).

