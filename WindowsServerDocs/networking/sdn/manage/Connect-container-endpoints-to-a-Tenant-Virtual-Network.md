---
title: Conectar pontos de extremidade do contêiner a uma rede virtual do locatário
description: Neste tópico, mostraremos como conectar pontos de extremidade do contêiner a uma rede virtual de locatário existente criada por meio de SDN. Use o driver de rede l2bridge (e, opcionalmente, l2tunnel) disponível com o plug-in libnetwork do Windows para o Docker para criar uma rede de contêiner na VM de locatário.
manager: grcusanz
ms.topic: article
ms.assetid: f7af1eb6-d035-4f74-a25b-d4b7e4ea9329
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/24/2018
ms.openlocfilehash: af8232de75005ae295079eb2207bce303629acaa
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87995196"
---
# <a name="connect-container-endpoints-to-a-tenant-virtual-network"></a>Conectar pontos de extremidade do contêiner a uma rede virtual do locatário

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste tópico, mostraremos como conectar pontos de extremidade do contêiner a uma rede virtual de locatário existente criada por meio de SDN. Use o driver de rede *l2bridge* (e, opcionalmente, *l2tunnel*) disponível com o plug-in libnetwork do Windows para o Docker para criar uma rede de contêiner na VM de locatário.

No tópico [drivers de rede do contêiner](/virtualization/windowscontainers/container-networking/network-drivers-topologies) , discutimos que os vários drivers de rede estão disponíveis por meio do Docker no Windows. Para SDN, use os drivers *l2bridge* e *l2tunnel* . Para ambos os drivers, cada ponto de extremidade do contêiner está na mesma sub-rede virtual que a máquina virtual do host do contêiner (locatário).

O serviço de rede de host (HNS), por meio do plug-in de nuvem privada, atribui dinamicamente os endereços IP para pontos de extremidade de contêiner. Os pontos de extremidade do contêiner têm endereços IP exclusivos, mas compartilham o mesmo endereço MAC da máquina virtual do host do contêiner (locatário) devido à conversão de endereço da camada 2.

A política de rede (ACLs, encapsulamento e QoS) para esses pontos de extremidade de contêiner são impostas no host físico do Hyper-V conforme recebido pelo controlador de rede e definido em sistemas de gerenciamento de camada superior.

A diferença entre os drivers *l2bridge* e *l2tunnel* é:


|                                                                                                                                                                                                                                                                            l2bridge                                                                                                                                                                                                                                                                            |                                                                                                 l2tunnel                                                                                                  |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Pontos de extremidade do contêiner que residem em: <ul><li>A mesma máquina virtual do host do contêiner e na mesma sub-rede têm todo o tráfego de rede ponte dentro do comutador virtual do Hyper-V. </li><li>VMs de host de contêiner diferentes ou em sub-redes diferentes têm seu tráfego encaminhado para o host do Hyper-V físico. </li></ul>A política de rede não é imposta, pois o tráfego de rede entre os contêineres no mesmo host e na mesma sub-rede não flui para o host físico. A política de rede aplica-se somente ao tráfego de rede do contêiner entre hosts ou entre sub-redes. | *Todo* o tráfego de rede entre dois pontos de extremidade de contêiner é encaminhado para o host do Hyper-V físico, independentemente do host ou da sub-rede. A política de rede se aplica a tráfego de rede entre sub-redes e entre hosts. |

---

>[!NOTE]
>Esses modos de rede não funcionam para conectar pontos de extremidade de contêiner do Windows a uma rede virtual de locatário na nuvem pública do Azure.


## <a name="prerequisites"></a>Pré-requisitos
-  Uma infraestrutura de SDN implantada com o controlador de rede.
-  Uma rede virtual de locatário foi criada.
-  Uma máquina virtual de locatário implantada com o recurso de contêiner do Windows habilitado, Docker instalado e recurso do Hyper-V habilitado. O recurso Hyper-V é necessário para instalar vários binários para redes l2bridge e l2tunnel.

   ```powershell
   # To install HyperV feature without checks for nested virtualization
   dism /Online /Enable-Feature /FeatureName:Microsoft-Hyper-V /All
   ```

>[!Note]
>A [virtualização aninhada](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization) e a exposição de extensões de virtualização não são necessárias, a menos que usem contêineres Hyper-V.


## <a name="workflow"></a>Fluxo de trabalho

[1. Adicione várias configurações de IP a um recurso de NIC de VM existente por meio do controlador de rede (host Hyper-V)](#1-add-multiple-ip-configurations) 
 [2. Habilite o proxy de rede no host para alocar endereços IP de CA para pontos de extremidade de contêiner (host Hyper-V)](#2-enable-the-network-proxy) 
 [3. Instale o plug-in de nuvem privada para atribuir endereços IP de autoridade de certificação aos pontos de extremidade do contêiner (VM do host do contêiner)](#3-install-the-private-cloud-plug-in) 
 [4. Criar uma rede *l2bridge* ou *l2tunnel* usando o Docker (VM do host do contêiner)](#4-create-an-l2bridge-container-network)

>[!NOTE]
>Não há suporte para várias configurações de IP em recursos de NIC de VM criados por meio de System Center Virtual Machine Manager. É recomendável para esses tipos de implantações que você cria o recurso NIC da VM fora da banda usando o PowerShell do controlador de rede.

### <a name="1-add-multiple-ip-configurations"></a>1. adicionar várias configurações de IP
Nesta etapa, assumimos que a NIC da VM da máquina virtual de locatário tem uma configuração de IP com o endereço IP de 192.168.1.9 e está anexada a uma ID de recurso de VNet de ' VNet1 ' e recurso de sub-rede VM de ' Subnet1 ' na sub-rede IP 192.168.1.0/24. Adicionamos 10 endereços IP para contêineres de 192.168.1.101-192.168.1.110.

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

### <a name="2-enable-the-network-proxy"></a>2. habilitar o proxy de rede
Nesta etapa, você habilita o proxy de rede para alocar vários endereços IP para a máquina virtual do host do contêiner.

Para habilitar o proxy de rede, execute o script [ConfigureMCNP.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/ConfigureMCNP.ps1) no **host Hyper-V** que hospeda a máquina virtual de host (locatário) do contêiner.

```powershell
PS C:\> ConfigureMCNP.ps1
```

### <a name="3-install-the-private-cloud-plug-in"></a>3. instalar o plug-in de nuvem privada
Nesta etapa, você instala um plug-in para permitir que o HNS se comunique com o proxy de rede no host Hyper-V.

Para instalar o plug-in, execute o script de [InstallPrivateCloudPlugin.ps1](https://github.com/Microsoft/SDN/blob/master/Containers/InstallPrivateCloudPlugin.ps1) dentro da **máquina virtual de host (locatário) do contêiner**.


```powershell
PS C:\> InstallPrivateCloudPlugin.ps1
```

### <a name="4-create-an-l2bridge-container-network"></a>4. criar uma rede de contêiner *l2bridge*
Nesta etapa, você usa o `docker network create` comando na **máquina virtual do host do contêiner (locatário)** para criar uma rede l2bridge.

```powershell
# Create the container network
C:\> docker network create -d l2bridge --subnet="192.168.1.0/24" --gateway="192.168.1.1" MyContainerOverlayNetwork

# Attach a container to the MyContainerOverlayNetwork
C:\> docker run -it --network=MyContainerOverlayNetwork <image> <cmd>
```

>[!NOTE]
>A atribuição de IP estático não tem suporte com redes de contêiner *l2bridge* ou *l2tunnel* quando usadas com a pilha do Microsoft Sdn.

## <a name="more-information"></a>Mais informações
Para obter mais detalhes sobre como implantar uma infraestrutura de SDN, consulte [implantar uma infraestrutura de rede definida pelo software](../deploy/deploy-a-software-defined-network-infrastructure.md).