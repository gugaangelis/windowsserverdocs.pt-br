---
title: Conjunto de subcomandos-TransportServer
description: Artigo de referência para o subcomando Set-TransportServer, que define as definições de configuração para um servidor de transporte.
ms.topic: reference
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59833701a88af6280d7f033a8430d2fec882decd
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024770"
---
# <a name="subcommand-set-transportserver"></a>Subcomando: Set-TransportServer

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Esse pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome do servidor de transporte for especificado, o servidor local será usado.|
|[/ObtainIpv4From: {DHCP &#124; intervalo}]|Define a origem dos endereços IPv4 da seguinte maneira:<p>-[/start: <IP address> ] define o início do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/End: <IP address> ] define o final do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/startPort: <port> ] define o início do intervalo de portas.<br />-[/EndPort: <port> ] define o final do intervalo de portas.|
|[/ObtainIpv6From: intervalo]|Especifica a origem dos endereços IPv6. Essa opção se aplica somente ao Windows Server 2008 R2 e o único valor com suporte é **Range**.<p>-[/start: <IP address> ] define o início do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/End: <IP address> ] define o final do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/startPort: <port> ] define o início do intervalo de portas.<br />-[/EndPort: <port> ] define o final do intervalo de portas.|
|[/Profile: {10 Mbps &#124; 100 Mbps &#124; 1 Gbps &#124; personalizado}]|Especifica o perfil de rede a ser usado. Essa opção só está disponível para servidores que executam o Windows Server 2008 ou o Windows Server 2003.|
|[/MulticastSessionPolicy]|Define as configurações de transferência para transmissões multicast. Este comando só está disponível para o Windows Server 2008 R2.<p>-[/Policy: {None &#124; a desconexão automática &#124; MultiStream}] determina como lidar com clientes lentos. **Nenhum** significa manter todos os clientes em uma sessão na mesma velocidade. A **desconexão automática** significa que todos os clientes que estiverem abaixo do **/Threshold** especificado serão desconectados. **MultiStream** significa que os clientes serão separados em várias sessões, conforme especificado por **/StreamCount**.<br />-[/Threshold: <Speed in KBps> ] define a taxa de transferência mínima em Kbps para **/Policy: undisconnect**. Os clientes que ficam abaixo dessa taxa são desconectados das transmissões multicast.<br />-[/StreamCount: {2 &#124; 3}] [/fallback: {Yes &#124; no}] determina o número de sessões para **/Policy: MultiStream**. **2** significa duas sessões (rápidas e lentas) e **3** significa três sessões (lenta, média, rápida).<br />-[/Fallback: {Yes &#124; no}] determina se os clientes tha serão desconectados continuarão a transferência usando outro método (se houver suporte do cliente). Se você estiver usando o cliente WDS, o computador voltará ao unicasting. Wdsmcast.exe não dá suporte a um mecanismo de fallback. Essa opção também se aplica a clientes que não dão suporte a **MultiStream**. Nesse caso, o computador voltará para outro método em vez de mover para uma sessão de transferência mais lenta.|
## <a name="examples"></a>Exemplos
Para definir o intervalo de endereços IPv4 para o servidor, digite:
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
Para definir o intervalo de endereços IPv4, o intervalo de portas e o perfil para o servidor, digite:
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
## <a name="additional-references"></a>Referências adicionais
- Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
 [Usando o comando](using-the-disable-transportserver-command.md) 
 Disable-TransportServer [Usando o comando](using-the-enable-transportserver-command.md) 
 Enable-TransportServer [Usando o comando](using-the-get-transportserver-command.md) 
 Get-TransportServer [Subcomando: Start-TransportServer](subcommand-start-transportserver.md) 
 [Subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
