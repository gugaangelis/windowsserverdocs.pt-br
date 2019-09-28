---
title: Conjunto de subcomandos-TransportServer
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f91cca044d4c2922791ccb63b0e3cec4af685f17
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370737"
---
# <a name="subcommand-set-transportserver"></a>Subcomando: Set-TransportServer

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define definições de configuração para um servidor de transporte.
## <a name="syntax"></a>Sintaxe
```
wdsutil [Options] /Set-TransportServer [/Server:<Server name>]
     [/ObtainIpv4From:{Dhcp | Range}]
        [/start:<starting IP address>]
        [/End:<Ending IP address>]
     [/ObtainIpv6From:Range]\n\
        [/start:<start IP address>]\n\
        [/End:<End IP address>]      
     [/startPort:<starting port>
        [/EndPort:<starting port>
     [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]    
     [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome do servidor de transporte for especificado, o servidor local será usado.|
|[/ObtainIpv4From: {intervalo &#124; DHCP}]|Define a origem dos endereços IPv4 da seguinte maneira:<br /><br />-[/start: <IP address>] define o início do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/End: <IP address>] define o final do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/startPort: <port>] define o início do intervalo de portas.<br />-[/EndPort: <port>] define o fim do intervalo de portas.|
|[/ObtainIpv6From: intervalo]|Especifica a origem dos endereços IPv6. Essa opção se aplica somente ao Windows Server 2008 R2 e o único valor com suporte é **Range**.<br /><br />-[/start: <IP address>] define o início do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/End: <IP address>] define o final do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/startPort: <port>] define o início do intervalo de portas.<br />-[/EndPort: <port>] define o fim do intervalo de portas.|
|[/Profile: {10 Mbps &#124; 100 &#124; Mbps &#124; 1 Gbps Custom}]|Especifica o perfil de rede a ser usado. Essa opção só está disponível para servidores que executam o Windows Server 2008 ou o Windows Server 2003.|
|[/MulticastSessionPolicy]|Define as configurações de transferência para transmissões multicast. Este comando só está disponível para o Windows Server 2008 R2.<br /><br />-[/Policy: {None &#124; autodisconnect &#124; MultiStream}] determina como lidar com clientes lentos. **Nenhum** significa manter todos os clientes em uma sessão na mesma velocidade. A **desconexão automática** significa que todos os clientes que estiverem abaixo do **/Threshold** especificado serão desconectados. **MultiStream** significa que os clientes serão separados em várias sessões, conforme especificado por **/StreamCount**.<br />-[/Threshold: <Speed in KBps>] define a taxa de transferência mínima em KBps para **/Policy: undisconnect**. Os clientes que ficam abaixo dessa taxa são desconectados das transmissões multicast.<br />-[/StreamCount: {2 &#124; 3}] [/fallback: {yes &#124; no}] determina o número de sessões para **/Policy: MultiStream**. **2** significa duas sessões (rápidas e lentas) e **3** significa três sessões (lenta, média, rápida).<br />-[/Fallback: {yes &#124; no}] determina se os clientes tha serão desconectados continuarão a transferência usando outro método (se houver suporte do cliente). Se você estiver usando o cliente WDS, o computador voltará ao unicasting. O Wdsmcast. exe não dá suporte a um mecanismo de fallback. Essa opção também se aplica a clientes que não dão suporte a **MultiStream**. Nesse caso, o computador voltará para outro método em vez de mover para uma sessão de transferência mais lenta.|
## <a name="BKMK_examples"></a>Disso
Para definir o intervalo de endereços IPv4 para o servidor, digite:
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
Para definir o intervalo de endereços IPv4, o intervalo de portas e o perfil para o servidor, digite:
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando disable-TransportServer](using-the-disable-transportserver-command.md)
[usando o comando Enable-TransportServer](using-the-enable-transportserver-command.md)
[usando o comando Get-TransportServer do](using-the-get-transportserver-command.md)subcomando 
[: Start-TransportServer](subcommand-start-transportserver.md)
[subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
