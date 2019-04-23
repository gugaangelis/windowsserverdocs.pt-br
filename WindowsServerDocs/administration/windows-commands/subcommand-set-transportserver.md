---
title: Set-TransportServer subcomando
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d144ed7d461cbebcd351aa4347fde20f35736c26
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886597"
---
# <a name="subcommand-set-transportserver"></a>Subcommand: set-TransportServer

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Define as configurações para um servidor de transporte.
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
|[/Server:<Server name>]|Especifica o nome do servidor de transporte. Isso pode ser o nome NetBIOS ou o nome de domínio totalmente qualificado (FQDN). Se nenhum nome de servidor de transporte é especificado, o servidor local será usado.|
|[/ObtainIpv4From:{Dhcp &#124; Range}]|Define a origem dos endereços IPv4 da seguinte maneira:<br /><br />-[/ Iniciar: <IP address>] define o início do intervalo de endereços IP. Isso é necessário e válida apenas se essa opção for definida como **intervalo**.<br />-[Final: <IP address>] define o fim do intervalo de endereços IP. Isso é necessário e válida apenas se essa opção for definida como **intervalo**.<br />-[/ startPort: <port>] define o início do intervalo de porta.<br />-[/ A EndPort: <port>] define o final do intervalo de porta.|
|[/ObtainIpv6From:Range]|Especifica a origem dos endereços IPv6. Essa opção só se aplica ao Windows Server 2008 R2 e o único valor com suporte é **intervalo**.<br /><br />-[/ Iniciar: <IP address>] define o início do intervalo de endereços IP. Isso é necessário e válida apenas se essa opção for definida como **intervalo**.<br />-[Final: <IP address>] define o fim do intervalo de endereços IP. Isso é necessário e válida apenas se essa opção for definida como **intervalo**.<br />-[/ startPort: <port>] define o início do intervalo de porta.<br />-[/ A EndPort: <port>] define o final do intervalo de porta.|
|[/Profile: {10Mbps &#124; 100 Mbps &#124; 1 Gbps &#124; personalizado}]|Especifica o perfil de rede a ser usado. Essa opção só está disponível para servidores que executam o Windows Server 2008 ou Windows Server 2003.|
|[/MulticastSessionPolicy]|Define as configurações de transferência para transmissões multicast. Esse comando só está disponível para Windows Server 2008 R2.<br /><br />-[/ Política: {None &#124; desconexão automática &#124; Multistream}] determina como lidar com clientes lentos. **Nenhum** significa manter todos os clientes em uma sessão com a mesma velocidade. **Desconexão automática** significa que todos os clientes que atingem especificado **/Threshold** estão desconectados. **MultiStream** significa que os clientes serão separados em várias sessões conforme especificado por **/StreamCount**.<br />-[/ Limite:<Speed in KBps>] define a taxa de transferência mínima em KBps para **/Policy:AutoDisconnect**. Os clientes que atingem essa taxa são desconectados de transmissões multicast.<br />-[/ StreamCount: {2 &#124; 3}] [/ Fallback: {Sim &#124; não}] determina o número de sessões para **/Policy:Multistream**. **2** significa que duas sessões (rápidas e lentas) e **3** significa três sessões (lenta, média, fast).<br />-[/ Fallback: {Sim &#124; não}] determina se os clientes que estão desconectados continuarão a transferência usando outro método (se houver suporte pelo cliente). Se você estiver usando o cliente do WDS, o computador fará o fallback para unicast. Wdsmcast.exe não oferece suporte a um mecanismo de fallback. Essa opção também se aplica aos clientes que não suportam **Multistream**. Nesse caso, o computador fará o fallback para outro método em vez de mover para uma sessão de transferência mais lenta.|
## <a name="BKMK_examples"></a>Exemplos
Para definir o intervalo de endereços IPv4 para o servidor, digite:
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
Para definir o intervalo de endereços IPv4, o intervalo de porta e o perfil para o servidor, digite:
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[usando o comando disable-TransportServer](using-the-disable-transportserver-command.md)
[usando o comando enable-TransportServer](using-the-enable-transportserver-command.md) 
 [ Usando o comando get-TransportServer](using-the-get-transportserver-command.md)
[subcomando: start-TransportServer](subcommand-start-transportserver.md)
[subcomando: stop-TransportServer](subcommand-stop-transportserver.md)
