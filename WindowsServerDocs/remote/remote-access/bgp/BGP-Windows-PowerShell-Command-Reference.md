---
title: Referência de comando do Windows PowerShell do BGP
description: Você pode usar este tópico como uma referência, ao gravar scripts do Windows PowerShell no Windows Server 2016, para adicionar, configurar e remover os recursos de BGP do gateway RAS e roteadores de rede local (LAN) de acesso remoto.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4b0240a3-b927-4a1e-b241-5f8f29a9552f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7e7a32f3da4554462226fd7315708b94a8a61e19
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965598"
---
# <a name="bgp-windows-powershell-command-reference"></a>Referência de comando do Windows PowerShell do BGP

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico como uma referência, ao gravar scripts do Windows PowerShell, para adicionar, configurar e remover recursos de BGP do gateway RAS e roteadores de rede local (LAN) de acesso remoto.  
  
Esses comandos BGP fazem parte do conjunto de comandos do Windows PowerShell de acesso remoto para o Windows Server 2016. Este tópico ajuda você a localizar rapidamente os comandos BGP que você deseja usar em scripts.  
  
Para obter mais informações sobre todos os comandos de acesso remoto, consulte [cmdlets de acesso remoto](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11)).  
  
## <a name="bgp-command-reference"></a>Referência de comando BGP  
As seções a seguir fornecem o nome, a finalidade e a sintaxe do comando para cada comando BGP, bem como um link para o comando na referência de acesso remoto, que contém informações mais detalhadas sobre cada comando.  
  
Esta referência contém as seções a seguir.  
  
-   [Adicionar comandos](#bkmk_add)  
  
-   [Limpar comandos](#bkmk_clear)  
  
-   [Desabilitar e habilitar comandos](#bkmk_disable)  
  
-   [Obter comandos](#bkmk_get)  
  
-   [Instalar comandos](#bkmk_install)  
  
-   [Remover comandos](#bkmk_remove)  
  
-   [Definir comandos](#bkmk_set)  
  
-   [Comandos Iniciar e parar](#bkmk_start)  
  
-   [Comandos de desinstalação](#bkmk_uninstall)  
  
### <a name="add-commands"></a><a name="bkmk_add"></a>Adicionar comandos  
A seguir estão os comandos BGP Add.  
  
[Add-BgpCustomRoute](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Adiciona rotas personalizadas à tabela de roteamento BGP.  
  
```  
Add-BgpCustomRoute [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-Interface <String[]> ] [-Network <String[]> ] [-PassThru] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Add-BgpPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Adiciona um novo par de BGP.  
  
```  
Add-BgpPeer [-Name] <String> -LocalIPAddress <IPAddress> -PeerASN <UInt32> -PeerIPAddress <IPAddress> [-CimSession <CimSession[]> ] [-HoldTimeSec <UInt16> ] [-IdleHoldTimeSec <UInt16> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-LocalASN <UInt32> ] [-MaxAllowedPrefix <UInt32> ] [-OperationMode <OperationMode> {Mixed | Server} ] [-PassThru] [-PeeringMode <PeeringMode> {Automatic | Manual} ] [-RouteReflectorClient <Boolean> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Weight <UInt16> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Add-BgpRouteAggregate](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Adiciona uma nova rota de agregação para rotas BGP específicas.  
  
```  
Add-BgpRouteAggregate -Prefix <String> [-AttributePolicy <String[]> ] [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-PreserveASPath <PreserveASPath> ] [-RoutingDomain <String> ] [-SummaryOnly <SummaryOnly> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Add-BgpRouter](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Adiciona um roteador BGP para a ID de locatário especificada.  
  
```  
Add-BgpRouter -BgpIdentifier <IPAddress> -LocalASN <UInt32> [-CimSession <CimSession[]> ] [-ClientToClientReflection <ClientToClientReflection> ] [-ClusterId <UInt32> ] [-CompareMEDAcrossASN <Boolean> ] [-DefaultGatewayRouting <Boolean> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-IPv6Routing <IPv6RoutingState> {Disabled | Enabled} ] [-LocalIPv6Address <IPAddress> ] [-PassThru] [-RouteReflector <RouteReflector> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-TransitRouting <TransitRouting> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Add-BgpRoutingPolicy](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Adiciona uma política de roteamento BGP ao repositório de políticas.  
  
```  
Add-BgpRoutingPolicy [-Name] <String> [-PolicyType] <PolicyType> {Deny | Allow | ModifyAttribute} [-AddCommunity <String[]> ] [-CimSession <CimSession[]> ] [-ClearMED] [-Force] [-IgnorePrefix <String[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MatchASNRange <UInt32[]> ] [-MatchCommunity <String[]> ] [-MatchNextHop <IPAddress[]> ] [-MatchPrefix <String[]> ] [-NewLocalPref <UInt32]> ] [-NewMED <UInt32]> ] [-NewNextHop <IPAddress> ] [-PassThru] [-RemoveAllCommunities] [-RemoveCommunity <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Add-BgpRoutingPolicyForPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Adiciona políticas de roteamento BGP a pares de BGP.  
  
```  
Add-BgpRoutingPolicyForPeer -Direction <PolicyDirection> {Ingress | Egress} -PolicyName <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="clear-commands"></a><a name="bkmk_clear"></a>Limpar comandos  
A seguir estão os comandos Clear para BGP  
  
[Clear-BgpRouteFlapDampening](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Limpa as informações de extinção de oscilação de rota para o conjunto especificado de rotas BGP.  
  
```  
Clear-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="disable-and-enable-commands"></a><a name="bkmk_disable"></a>Desabilitar e habilitar comandos  
A seguir estão os comandos Disable e Enable para BGP  
  
[Desabilitar-BgpRouteFlapDampening](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Desabilita o retardamento de rota para as rotas BGP de oscilação.  
  
```  
Disable-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Habilitar-BgpRouteFlapDampening](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Habilita o retardamento de rota para as rotas BGP de oscilação.  
  
```  
Enable-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="get-commands"></a><a name="bkmk_get"></a>Obter comandos  
A seguir estão os comandos Get para BGP.  
  
[Get-BgpCustomRoute](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Obtém informações de rota personalizadas do roteador BGP.  
  
```  
Get-BgpCustomRoute [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Get-BgpPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Obtém informações de configuração para pares de BGP.  
  
```  
Get-BgpPeer [[-Name] <String[]> ] [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Get-BgpRouteAggregate](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Obtém todas as rotas BGP de agregação configuradas pelo administrador.  
  
```  
Get-BgpRouteAggregate [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Get-BgpRouteFlapDampening](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Recupera a configuração de um mecanismo de retardamento de rota BGP.  
  
```  
Get-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Get-BgpRouteInformation](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Recupera informações de rota BGP para um ou mais prefixos de rede da tabela de roteamento BGP.  
  
```  
Get-BgpRouteInformation [-CimSession <CimSession[]> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Network <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Type <RouteType> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Get-BgpRouter](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Obtém informações de configuração para Roteadores BGP.  
  
```  
Get-BgpRouter [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String[]> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Get-BgpRoutingPolicy](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Obtém informações de configuração de políticas de roteamento BGP.  
  
```  
Get-BgpRoutingPolicy [[-Name] <String[]> ] [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PolicyType <PolicyType> {Deny | Allow | ModifyAttribute} ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Get-BgpStatistics](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Recupera a mensagem relacionada ao emparelhamento via protocolo BGP e estatísticas de anúncio de rota.  
  
```  
Get-BgpStatistics [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="install-commands"></a><a name="bkmk_install"></a>Instalar comandos  
A seguir estão os comandos de instalação para o gateway RAS e BGP.  
  
[Instalar-RemoteAccess](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Executa verificações de pré-requisitos para o DirectAccess (DA) para garantir que ele possa ser instalado, instale o DA RA (acesso remoto) (inclui o gerenciamento de clientes remotos) ou somente para o gerenciamento de clientes remotos, instala a VPN (VPN de acesso remoto e VPN site a site) e instala o roteamento BGP.  
  
```  
Parameter Set: MultiTenant  
Install-RemoteAccess [-MultiTenancy] [-CapacityKbps <UInt64> ] [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MsgAuthenticator <String> {Enabled | Disabled} ] [-PassThru] [-RadiusPort <UInt16> ] [-RadiusScore <Byte> ] [-RadiusServer <String> ] [-RadiusTimeout <UInt32> ] [-SharedSecret <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
  
Parameter Set: Vpn  
Install-RemoteAccess [-VpnType] <String> {Vpn | VpnS2S | SstpProxy | RoutingOnly} [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-EntrypointName <String> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-IPAddressRange <String[]> ] [-IPv6Prefix <String> ] [-Legacy] [-MsgAuthenticator <String> {Enabled | Disabled} ] [-PassThru] [-RadiusPort <UInt16> ] [-RadiusScore <Byte> ] [-RadiusServer <String> ] [-RadiusTimeout <UInt32> ] [-SharedSecret <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
> [!IMPORTANT]  
> Ao instalar o gateway RAS no modo multilocatário, você deve especificar se o BGP está habilitado para cada locatário usando o comando **Enable-RemoteAccessRoutingDomain** do Windows PowerShell com o valor de parâmetro **-Type** de **todos**. O código de exemplo a seguir ilustra como instalar o RAS no modo multilocação com todos os recursos de RAS (VPN ponto a site, VPN site a site e roteamento BGP) habilitados para dois locatários, contoso e fabrikam.  
  
```  
$Contoso_RoutingDomain = "ContosoTenant"  
$Fabrikam_RoutingDomain = "FabrikamTenant"  
  
Install-RemoteAccess -MultiTenancy  
  
Enable-RemoteAccessRoutingDomain -Name $Contoso_RoutingDomain -Type All -PassThru  
Enable-RemoteAccessRoutingDomain -Name $Fabrikam_RoutingDomain -Type All -PassThru  
```  
  
Se você estiver usando o acesso remoto como um roteador de LAN em vez de como um gateway, ainda poderá usar o BGP, que fornece a vantagem de ter o roteamento dinâmico em sua intranet. Para instalar o acesso remoto como um roteador de LAN BGP, digite o seguinte comando em um prompt do Windows PowerShell e pressione ENTER.  
  
```  
Install-RemoteAccess -VpnType RoutingOnly  
```  
  
### <a name="remove-commands"></a><a name="bkmk_remove"></a>Remover comandos  
A seguir estão os comandos de remoção para BGP.  
  
[Remove-BgpCustomRoute](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Remove as rotas personalizadas do roteador BGP.  
  
```  
Remove-BgpCustomRoute [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-Interface <String[]> ] [-Network <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Remove-BgpPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Remove os pares de BGP de um roteador.  
  
```  
Remove-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Remove-BgpRouteAggregate](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Remove o conjunto de rotas BGP de agregação especificadas.  
  
```  
Remove-BgpRouteAggregate [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-Prefix <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Remove-BgpRouter](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Remove um roteador BGP.  
  
```  
Remove-BgpRouter [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String[]> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Remove-BgpRoutingPolicy](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Remove políticas de roteamento do repositório de políticas.  
  
```  
Remove-BgpRoutingPolicy [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Remove-BgpRoutingPolicyForPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Remove políticas de roteamento de pares BGP.  
  
```  
Parameter Set: Remove1  
Remove-BgpRoutingPolicyForPeer [-CimSession <CimSession[]> ] [-Direction <PolicyDirection> {Ingress | Egress} ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-PolicyName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="set-commands"></a><a name="bkmk_set"></a>Definir comandos  
A seguir estão os comandos set para BGP.  
  
[Set-BgpPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Atualiza a configuração do par de BGP especificado.  
  
```  
Set-BgpPeer [-Name] <String> [-CimSession <CimSession[]> ] [-ClearPrefixLimit] [-Force] [-HoldTimeSec <UInt16> ] [-IdleHoldTimeSec <UInt16> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-LocalASN <UInt32> ] [-LocalIPAddress <IPAddress> ] [-MaxAllowedPrefix <UInt32> ] [-OperationMode <OperationMode> {Mixed | Server} ] [-PassThru] [-PeerASN <UInt32> ] [-PeeringMode <PeeringMode> {Automatic | Manual} ] [-PeerIPAddress <IPAddress> ] [-RouteReflectorClient <Boolean> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Weight <UInt16> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Set-BgpRouteAggregate](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Atualiza as propriedades da rota BGP de agregação especificada.  
  
```  
Set-BgpRouteAggregate -Prefix <String> [-AttributePolicy <String[]> ] [-CimSession <CimSession[]> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-PassThru] [-PreserveASPath <PreserveASPath> ] [-RoutingDomain <String> ] [-SummaryOnly <SummaryOnly> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Set-BgpRouteFlapDampening](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Configura o mecanismo de retardamento de rota BGP.  
  
```  
Set-BgpRouteFlapDampening [-CimSession <CimSession[]> ] [-Force] [-HalfLife <UInt32> ] [-HalfLifeUnreachable <UInt32> ] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-MaxSuppressTime <UInt32> ] [-PassThru] [-ReuseThreshold <UInt32> ] [-RoutingDomain <String> ] [-SuppressThreshold <UInt32> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Set-BgpRouter](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Atualiza a configuração do roteador BGP local para a ID de locatário especificada.  
  
```  
Set-BgpRouter [-BgpIdentifier <IPAddress> ] [-CimSession <CimSession[]> ] [-ClientToClientReflection <ClientToClientReflection> ] [-ClusterId <UInt32> ] [-CompareMEDAcrossASN <Boolean> ] [-DefaultGatewayRouting <Boolean> ] [-Force] [-InformationAction <ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <String> ] [-IPv6Routing <IPv6RoutingState> {Disabled | Enabled} ] [-LocalASN <UInt32> ] [-LocalIPv6Address <IPAddress> ] [-PassThru] [-RouteReflector <RouteReflector> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-TransitRouting <TransitRouting> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Set-BgpRoutingPolicy](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Modifica uma configuração de política de roteamento.  
  
```  
Set-BgpRoutingPolicy [-Name] <String> [-AddCommunity <String[]> ] [-CimSession <CimSession[]> ] [-ClearMED] [-Force] [-IgnorePrefix <String[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-MatchASNRange <UInt32[]> ] [-MatchCommunity <String[]> ] [-MatchNextHop <IPAddress[]> ] [-MatchPrefix <String[]> ] [-NewLocalPref <UInt32]> ] [-NewMED <UInt32]> ] [-NewNextHop <IPAddress> ] [-PassThru] [-PolicyType <PolicyType> {Deny | Allow | ModifyAttribute} ] [-RemoveAllCommunities] [-RemoveCommunity <String[]> ] [-RemovePolicyClause <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Set-BgpRoutingPolicyForPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Modifica as políticas de roteamento de BGP para pares de BGP.  
  
```  
Set-BgpRoutingPolicyForPeer -Direction <PolicyDirection> {Ingress | Egress} -PolicyName <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-PeerName <String[]> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="start-and-stop-commands"></a><a name="bkmk_start"></a>Comandos Iniciar e parar  
A seguir estão os comandos start e Stop para BGP.  
  
[Start-BgpPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Inicia sessões de roteamento para pares de BGP.  
  
```  
Start-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
[Stop-BgpPeer](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Interrompe as sessões de roteamento para pares de BGP.  
  
```  
Stop-BgpPeer [-Name] <String[]> [-CimSession <CimSession[]> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-RoutingDomain <String> ] [-ThrottleLimit <Int32> ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
### <a name="uninstall-commands"></a><a name="bkmk_uninstall"></a>Comandos de desinstalação  
A seguir estão os comandos de desinstalação para o gateway RAS e BGP.  
  
[Desinstalar-RemoteAccess](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd851844(v=ws.11))  
  
Desinstala o acesso remoto do computador, incluindo todos os recursos e funcionalidades de acesso remoto (gateway RAS, BGP, etc.).  
  
```  
Uninstall-RemoteAccess [-CimSession <CimSession[]> ] [-ComputerName <String> ] [-EntrypointName <String> ] [-Force] [-InformationAction <System.Management.Automation.ActionPreference> {SilentlyContinue | Stop | Continue | Inquire | Ignore | Suspend} ] [-InformationVariable <System.String> ] [-ThrottleLimit <Int32> ] [-VpnType <String> {Vpn | VpnS2S} ] [-Confirm] [-WhatIf] [ <CommonParameters>] [ <WorkflowParameters>]  
```  
  
