---
title: Gerenciar máquinas virtuais de IaaS do Azure
description: Gerenciar VMs do Azure IaaS com o Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6f162294076d7e12df09f31b0bbd7d9244abe679
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296850"
---
# Gerenciar máquinas virtuais de IaaS do Azure com o Windows Admin Center

Você pode usar o Windows Admin Center para gerenciar suas VMs do Azure, bem como máquinas locais. Há várias configurações possíveis - escolher a configuração que faça sentido para seu ambiente:
- [Gerenciar VMs do Azure de um gateway do Windows Admin Center no local](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Gerenciar VMs do Azure de um gateway do Windows Admin Center instalado em uma VM do Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## Gerenciar com um gateway do Windows Admin Center no local

Se você já instalou o Windows Admin Center em um gateway local (tanto no Windows 10 ou Windows Server 2016), você pode usar esse mesmo gateway para gerenciar o Windows 10 ou Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 ou Windows Server 2008 R2 VMs no Azure. 

### Conectando-se às VMs com um IP público

Se o destino VMs (as VMs que você deseja gerenciar com o Windows Admin Center) tiver IPs público, adicioná-los ao seu gateway do Windows Admin Center pelo endereço IP ou FQDN. Há algumas considerações para levar em conta:

- Você deve habilitar o acesso do WinRM para sua VM de destino executando o seguinte no PowerShell ou do Prompt de comando no destino VM: `winrm quickconfig`
- Se você ainda não tenha ingressado no domínio VM do Azure, a VM se comporta como um servidor no grupo de trabalho, portanto, você precisará considerar para [usar o Windows Admin Center em um grupo de trabalho](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- Você também deve habilitar conexões de entrada para a porta 5985 para WinRM por HTTP em ordem para o Windows Admin Center gerenciar a VM de destino:
   1. Execute o seguinte script do PowerShell no destino VM para permitir conexões de entrada para o sistema operacional convidado da porta 5985:   
`Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

   2. Você também deve abrir a porta na rede Azure:

    - Selecione sua VM do Azure, selecione a **rede**, em seguida, **Adicionar regra de porta de entrada**. 
    - Certifique-se de **que básico** é selecionado na parte superior do painel **Adicionar regra de segurança de entrada** .
    - No campo de **intervalos de portas** , insira **5985**.
    
    Se o gateway do Windows Admin Center tem um IP estático, você pode selecionar para permitir que apenas o WinRM acesso de entrada em seu gateway do Windows Admin Center para segurança adicional.
    Para fazer isso, selecione **Avançado** na parte superior do painel **Adicionar regra de segurança de entrada** .

    Para **origem**, selecione **Os endereços IP**e insira o endereço IP de origem correspondente ao seu gateway do Windows Admin Center.

    - Selecione **protocolo** **TCP**.
    - O restante pode ser deixado como padrão.

> [!NOTE]
> Você deve criar uma regra de porta personalizada. A regra de porta WinRM fornecida pela rede Azure usa a porta 5986 (sobre HTTPS) em vez de 5985 (por HTTP). 

### Conectando-se às VMs sem um IP público

Se o destino VMs do Azure não tiver IPs público e você deseja gerenciar essas VMs de um gateway do Windows Admin Center implantado em sua rede local, você precisa configurar sua rede local para ter conectividade com a VNet no qual são o destino de VMs conectado. Há 3 maneiras de fazer isso: ExpressRoute, VPN Site a Site ou VPN Site de ponto. [Saiba qual opção de conectividade faz sentido em seu ambiente.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Se você quiser usar um Site de ponto de VPN para conectar seu gateway do Windows Admin Center para um VNet do Azure para gerenciar VMs do Azure no que VNet, você pode usar o recurso do [Adaptador de rede do Azure](https://aka.ms/WACNetworkAdapter) no Centro de administração do Windows. Para fazer isso, conecte-se ao servidor no qual o Windows Admin Center está instalado, navegue até a ferramenta de rede e selecione "Adicionar adaptador de rede Azure". Quando você fornece os detalhes necessários e clique em "configurar", o Windows Admin Center irá configurar uma VPN Site de ponto para o VNet do Azure que você especificar, após o qual você pode conectar e gerenciar VMs do Azure em seu gateway do Windows Admin Center no local.

Verifique se que está em execução no seu destino VMs WinRM executando o seguinte no PowerShell ou do Prompt de comando no destino VM: `winrm quickconfig`

Se você ainda não tenha ingressado no domínio VM do Azure, a VM se comporta como um servidor no grupo de trabalho, portanto, você precisará considerar para [usar o Windows Admin Center em um grupo de trabalho](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Se você tiver algum problema, consulte [Solucionar Windows Admin Center](../support/troubleshooting.md) para ver se etapas adicionais são necessárias para a configuração (por exemplo, se você está se conectando usando uma conta de administrador local ou não é ingressado no domínio).

## Use um gateway do Windows Admin Center implantado no Azure

Você pode gerenciar VMs do Azure sem nenhuma dependência do local Implantando o Windows Admin Center no VNet onde suas VMs de destino estão conectados. 

Para gerenciar VMs fora do VNet no qual o gateway do Windows Admin Center é implantado, você deve estabelecer conectividade VNet-VNet entre o VNet do gateway do Windows Admin Center e a VNet dos servidores de destino. Você pode estabelecer essa conectividade com o emparelhamento VNet, VNet-VNet conexão ou uma conexão de Site a Site. [Saiba mais sobre quais conectividade VNet-VNet opção faz sentido em seu ambiente.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows Admin Center pode ser instalado em uma VM recém-implantados ou existente em seu ambiente. A VM que você escolhe para instalação do Windows Admin Center deve ter um nome DNS e IP público.

[Saiba mais sobre como implantar um gateway do Windows Admin Center no Azure](deploy-wac-in-azure.md)