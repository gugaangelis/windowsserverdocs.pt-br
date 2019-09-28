---
title: Gerenciar máquinas virtuais IaaS do Azure
description: Gerenciando VMs IaaS do Azure com o centro de administração do Windows (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 7b85f64d108283d4865b718b565ad3ba40f14f02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357343"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>Gerenciar máquinas virtuais IaaS do Azure com o centro de administração do Windows

É possível usar o Windows Admin Center para gerenciar suas VMs do Azure, bem como seus computadores locais. Há várias configurações diferentes possíveis: escolha a configuração que faz sentido para o seu ambiente:
- [Gerenciar VMs do Azure de um gateway do centro de administração do Windows local](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Gerenciar VMs do Azure de um gateway do centro de administração do Windows instalado em uma VM do Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>Gerenciar com um gateway do centro de administração do Windows local

Se você já tiver instalado o centro de administração do Windows em um gateway local (no Windows 10 ou no Windows Server 2016), poderá usar esse mesmo gateway para gerenciar o Windows 10 ou o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012 ou o Windows Server 2008 R2 VMs no Azure. 

### <a name="connecting-to-vms-with-a-public-ip"></a>Conectando-se a VMs com um IP público

Se as VMs de destino (as VMs que você deseja gerenciar com o centro de administração do Windows) tiverem IPs públicos, adicione-as ao seu gateway do centro de administração do Windows pelo endereço IP ou pelo FQDN. Há algumas considerações a serem levadas em conta:

- Você deve habilitar o acesso do WinRM à sua VM de destino executando o seguinte no PowerShell ou o prompt de comando na VM de destino: `winrm quickconfig`
- Se você não ingressou no domínio na VM do Azure, a VM se comporta como um servidor no grupo de trabalho, portanto, você precisará se certificar de [usar o centro de administração do Windows em um grupo de trabalho](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- Você também deve habilitar conexões de entrada para a porta 5985 para o WinRM sobre HTTP para que o centro de administração do Windows gerencie a VM de destino:
  1. Execute o seguinte script do PowerShell na VM de destino para habilitar conexões de entrada para a porta 5985 no sistema operacional convidado:   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. Você também deve abrir a porta na rede do Azure:

     - Selecione sua VM do Azure, selecione **rede**e, em seguida, **Adicionar regra de porta de entrada**. 
     - Verifique se **básico** está selecionado na parte superior do painel **Adicionar regra de segurança de entrada** .
     - No campo **intervalos de porta** , insira **5985**.
    
     Se o seu gateway do centro de administração do Windows tiver um IP estático, você poderá selecionar para permitir apenas o acesso de WinRM de entrada do seu gateway do centro de administração do Windows para aumentar a segurança.
     Para fazer isso, selecione **avançado** na parte superior do painel **Adicionar regra de segurança de entrada** .

     Para **origem**, selecione **endereços IP**e, em seguida, insira o endereço IP de origem correspondente ao seu gateway do centro de administração do Windows.

     - Para **protocolo** , selecione **TCP**.
     - O restante pode ser deixado como padrão.

> [!NOTE]
> Você deve criar uma regra de porta personalizada. A regra de porta do WinRM fornecida pela rede do Azure usa a porta 5986 (via HTTPS) em vez de 5985 (sobre HTTP). 

### <a name="connecting-to-vms-without-a-public-ip"></a>Conectando-se a VMs sem um IP público

Se suas VMs do Azure de destino não tiverem IPs públicos e você quiser gerenciar essas VMs de um gateway do centro de administração do Windows implantado em sua rede local, você precisará configurar sua rede local para ter conectividade com a VNet na qual as VMs de destino são Connected. Há três maneiras de fazer isso: ExpressRoute, VPN site a site ou VPN ponto a site. [Saiba qual opção de conectividade faz sentido em seu ambiente.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Se você quiser usar uma VPN ponto a site para conectar o gateway do centro de administração do Windows a uma VNet do Azure para gerenciar VMs do Azure nessa VNet, você pode usar o recurso do [adaptador de rede do Azure](https://aka.ms/WACNetworkAdapter) no centro de administração do Windows. Para fazer isso, conecte-se ao servidor no qual o centro de administração do Windows está instalado, navegue até a ferramenta de rede e selecione "Adicionar adaptador de rede do Azure". Ao fornecer os detalhes necessários e clicar em "configurar", o centro de administração do Windows configurará uma VPN ponto a site para a VNet do Azure que você especificar, após o qual, você poderá se conectar e gerenciar as VMs do Azure do seu gateway do centro de administração do Windows local.

Verifique se o WinRM está em execução nas VMs de destino executando o seguinte no PowerShell ou no prompt de comando na VM de destino: `winrm quickconfig`

Se você não ingressou no domínio na VM do Azure, a VM se comporta como um servidor no grupo de trabalho, portanto, você precisará se certificar de [usar o centro de administração do Windows em um grupo de trabalho](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Se você tiver problemas, consulte [solucionar problemas do centro de administração do Windows](../support/troubleshooting.md) para ver se etapas adicionais são necessárias para a configuração (por exemplo, se você estiver se conectando usando uma conta de administrador local ou não ingressado no domínio).

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>Usar um gateway do centro de administração do Windows implantado no Azure

Você pode gerenciar VMs do Azure sem nenhuma dependência local implantando o centro de administração do Windows na VNet em que as VMs de destino estão conectadas. 

Para gerenciar VMs fora da VNet na qual o gateway do centro de administração do Windows está implantado, você deve estabelecer a conectividade de VNet para VNet entre a VNet do gateway do centro de administração do Windows e a VNet dos servidores de destino. Você pode estabelecer essa conectividade com o emparelhamento VNet, conexão VNet a VNet ou uma conexão site a site. [Saiba mais sobre qual opção de conectividade VNet a VNet faz sentido em seu ambiente.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

O centro de administração do Windows pode ser instalado em uma VM existente ou recentemente implantada em seu ambiente. A VM que você escolher para a instalação do centro de administração do Windows deve ter um IP público e um nome DNS.

[Saiba mais sobre como implantar um gateway do centro de administração do Windows no Azure](deploy-wac-in-azure.md)