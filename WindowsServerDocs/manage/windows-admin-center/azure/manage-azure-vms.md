---
title: Gerenciar máquinas virtuais de IaaS do Azure
description: Gerenciar VMs de IaaS do Azure com Windows Admin Center (projeto Paulo)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ac98f42c4ad5606cc8d2b142f209f9bdb2b9611c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445905"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>Gerenciar máquinas virtuais de IaaS do Azure com o Windows Admin Center

Você pode usar o Windows Admin Center para gerenciar suas VMs do Azure, bem como as máquinas locais. Há várias configurações diferentes possíveis – escolha a configuração que faz sentido para o seu ambiente:
- [Gerenciar VMs do Azure de um gateway do Windows Admin Center no local](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Gerenciar VMs do Azure de um gateway do Windows Admin Center instalado em uma VM do Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>Gerenciar com um gateway do Windows Admin Center no local

Se você já tiver instalado o Windows Admin Center em um gateway local (seja no Windows 10 ou Windows Server 2016), você pode usar esse mesmo gateway para gerenciar o Windows 10 ou Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 VMs no Azure. 

### <a name="connecting-to-vms-with-a-public-ip"></a>Conectar-se às VMs com um IP público

Se o destino de VMs (as VMs que você deseja gerenciar com o Windows Admin Center) tem IPs públicos, adicioná-los ao seu gateway do Windows Admin Center por endereço IP ou FQDN. Há algumas considerações para levar em conta:

- Você deve habilitar o acesso do WinRM para VM de destino executando o seguinte no Prompt de comando ou do PowerShell na VM de destino: `winrm quickconfig`
- Se você ainda não tiver ingressado no domínio na VM do Azure, a VM se comporta como um servidor no grupo de trabalho, portanto, você precisará certificar-se de que você é responsável [usando o Windows Admin Center em um grupo de trabalho](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- Você também deve habilitar conexões de entrada para a porta 5985 para WinRM via HTTP para que Windows Admin Center gerenciar a VM de destino:
  1. Execute o seguinte script do PowerShell na VM para habilitar conexões de entrada para a porta 5985 no SO convidado de destino:   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. Você também deve abrir a porta na rede do Azure:

     - Selecione sua VM do Azure, selecione **Networking**, em seguida, **Adicionar regra de porta de entrada**. 
     - Certifique-se **básicas** está selecionada na parte superior das **Adicionar regra de segurança de entrada** painel.
     - No **intervalos de porta** , insira **5985**.
    
     Se o gateway do Windows Admin Center tem um endereço IP estático, você pode selecionar para permitir que somente o WinRM acesso de entrada de seu gateway do Windows Admin Center para aumentar a segurança.
     Para fazer isso, selecione **Advanced** na parte superior das **Adicionar regra de segurança de entrada** painel.

     Para **fonte**, selecione **endereços IP**, em seguida, insira o endereço IP de origem correspondente ao seu gateway do Windows Admin Center.

     - Para **protocolo** selecionar **TCP**.
     - O restante pode ser deixado como padrão.

> [!NOTE]
> Você deve criar uma regra de porta personalizada. A regra de porta de WinRM fornecida pela rede do Azure usa a porta 5986 (via HTTPS), em vez de 5985 (via HTTP). 

### <a name="connecting-to-vms-without-a-public-ip"></a>Conectar-se às VMs sem um IP público

Se suas VMs do Azure de destino não tem IPs públicos, e você deseja gerenciar essas VMs de um gateway do Windows Admin Center implantado em sua rede local, você precisa configurar sua rede local para ter conectividade com a rede virtual na qual as VMs de destino são conectado. Há 3 maneiras que você pode fazer isso: VPN ponto a Site, VPN Site a Site ou ExpressRoute. [Saiba qual opção de conectividade faz sentido em seu ambiente.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Se você deseja usar uma VPN ponto a Site para conectar o gateway do Windows Admin Center a uma rede virtual do Azure para gerenciar VMs do Azure em que a rede virtual, você pode usar o [adaptador de rede do Azure](https://aka.ms/WACNetworkAdapter) recurso no Windows Admin Center. Para fazer isso, conecte-se ao servidor no qual o Windows Admin Center está instalado, navegue até a ferramenta de rede e selecione "Adicionar adaptador de rede do Azure". Quando você fornece os detalhes necessários e clique em "configurar", Windows Admin Center irá configurar uma VPN ponto a Site para a rede virtual do Azure que você especificar, depois disso, você pode se conectar a e gerenciar VMs do Azure em seu gateway do Windows Admin Center local.

Verifique se o que WinRM está em execução no seu destino VMs executando o seguinte no Prompt de comando ou do PowerShell na VM de destino: `winrm quickconfig`

Se você ainda não tiver ingressado no domínio na VM do Azure, a VM se comporta como um servidor no grupo de trabalho, portanto, você precisará certificar-se de que você é responsável [usando o Windows Admin Center em um grupo de trabalho](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Se você tiver algum problema, consulte [solucione o problema Windows Admin Center](../support/troubleshooting.md) para ver se etapas adicionais são necessárias para configuração (por exemplo, se você estiver se conectando usando uma conta de administrador local ou não está associados ao domínio).

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>Usar um gateway do Windows Admin Center implantado no Azure

Você pode gerenciar VMs do Azure sem qualquer dependência no local com a implantação do Windows Admin Center na rede virtual em que suas VMs de destino estão conectados. 

Para gerenciar VMs fora da VNet na qual o gateway do Windows Admin Center é implantado, você deve estabelecer conectividade de VNet para VNet entre a rede virtual do gateway do Windows Admin Center e a rede virtual dos servidores de destino. Você pode estabelecer essa conectividade com o emparelhamento VNet, conexão de rede virtual para rede virtual ou uma conexão Site a Site. [Saiba mais sobre o que a conectividade VNet a opção faz sentido em seu ambiente.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows Admin Center pode ser instalado em uma VM existente ou recentemente implantada em seu ambiente. A VM que você escolher para a instalação do Windows Admin Center deve ter um nome IP e DNS público.

[Saiba mais sobre como implantar um gateway do Windows Admin Center no Azure](deploy-wac-in-azure.md)