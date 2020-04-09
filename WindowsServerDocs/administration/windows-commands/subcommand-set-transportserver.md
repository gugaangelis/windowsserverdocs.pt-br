---
title: Conjunto de subcomandos-TransportServer
description: O tópico comandos do Windows para o subcomando Set-TransportServer, que define as definições de configuração para um servidor de transporte.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c73567c58ff920b67f6e37a6f284d58517f5565
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833809"
---
# <a name="subcommand-set-transportserver"></a>Subcomando: Set-TransportServer

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
|[/ObtainIpv4From: {intervalo &#124; DHCP}]|Define a origem dos endereços IPv4 da seguinte maneira:<p>-[/start: <IP address>] define o início do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/End: <IP address>] define o final do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/startPort: <port>] define o início do intervalo de portas.<br />-[/EndPort: <port>] define o fim do intervalo de portas.|
|[/ObtainIpv6From: intervalo]|Especifica a origem dos endereços IPv6. Essa opção se aplica somente ao Windows Server 2008 R2 e o único valor com suporte é **Range**.<p>-[/start: <IP address>] define o início do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/End: <IP address>] define o final do intervalo de endereços IP. Isso é necessário e válido somente se essa opção estiver definida como **intervalo**.<br />-[/startPort: <port>] define o início do intervalo de portas.<br />-[/EndPort: <port>] define o fim do intervalo de portas.|
|[/Profile: {10 Mbps &#124; 100 &#124; Mbps &#124; 1 Gbps Custom}]|Especifica o perfil de rede a ser usado. Essa opção só está disponível para servidores que executam o Windows Server 2008 ou o Windows Server 2003.|
|[/MulticastSessionPolicy]|Define as configurações de transferência para transmissões multicast. Este comando só está disponível para o Windows Server 2008 R2.<p>-[/Policy: {None &#124; autodisconnect &#124; MultiStream}] determina como lidar com clientes lentos. **Nenhum** significa manter todos os clientes em uma sessão na mesma velocidade. A **desconexão automática** significa que todos os clientes que estiverem abaixo do **/Threshold** especificado serão desconectados. **MultiStream** significa que os clientes serão separados em várias sessões, conforme especificado por **/StreamCount**.<br />-[/Threshold:<Speed in KBps>] define a taxa de transferência mínima em KBps para **/Policy: undisconnect**. Os clientes que ficam abaixo dessa taxa são desconectados das transmissões multicast.<br />-[/StreamCount: {2 &#124; 3}] [/fallback: {yes &#124; no}] determina o número de sessões para **/Policy: MultiStream**. **2** significa duas sessões (rápidas e lentas) e **3** significa três sessões (lenta, média, rápida).<br />-[/Fallback: {yes &#124; no}] determina se os clientes tha serão desconectados continuarão a transferência usando outro método (se houver suporte do cliente). Se você estiver usando o cliente WDS, o computador voltará ao unicasting. O Wdsmcast. exe não dá suporte a um mecanismo de fallback. Essa opção também se aplica a clientes que não dão suporte a **MultiStream**. Nesse caso, o computador voltará para outro método em vez de mover para uma sessão de transferência mais lenta.|
## <a name="examples"></a><a name=BKMK_examples></a>Disso
Para definir o intervalo de endereços IPv4 para o servidor, digite:
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
Para definir o intervalo de endereços IPv4, o intervalo de portas e o perfil para o servidor, digite:
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
## <a name="additional-references"></a>Referências adicionais
- [A chave de sintaxe de linha de comando](command-line-syntax-key.md)
usando o [comando disable-TransportServer](using-the-disable-transportserver-command.md)
[usando o comando Enable-TransportServer](using-the-enable-transportserver-command.md)
[usando o comando Get-TransportServer](using-the-get-transportserver-command.md)
subcomando [: Start-TransportServer](subcommand-start-transportserver.md)
[subcomando: Stop-TransportServer](subcommand-stop-transportserver.md)
