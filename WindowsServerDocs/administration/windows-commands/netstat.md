---
title: netstat
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c06684eb73639e7480b5bad39d4d679739682800
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437152"
---
# <a name="netstat"></a>netstat

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe conexões TCP ativas, as portas nas quais o computador está escutando, estatísticas de Ethernet, a tabela de roteamento de IP, as estatísticas IPv4 (para os protocolos IP, ICMP, TCP e UDP) e as estatísticas IPv6 (para o IPv6, ICMPv6, TCP sobre IPv6 e UDP por meio de protocolos IPv6). Usado sem parâmetros, **netstat** exibe conexões TCP ativas. 

## <a name="syntax"></a>Sintaxe
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

### <a name="parameters"></a>Parâmetros

|   Parâmetro   |                                                                                                                                              Descrição                                                                                                                                              |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      -a       |                                                                                                   Exibe todas as conexões TCP ativas e as portas TCP e UDP escutadas pelo computador.                                                                                                   |
|      -e       |                                                                                 Exibe estatísticas de Ethernet, como o número de bytes e pacotes enviados e recebidos. Esse parâmetro pode ser combinado com **-s**.                                                                                  |
|      -n       |                                                                               Exibe as conexões TCP ativas, no entanto, endereços e números de porta são expressos numericamente e nenhuma tentativa é feita para determinar os nomes.                                                                               |
|      -o       |                          Exibe as conexões TCP ativas e inclui o processo de identificação para cada conexão. Você pode encontrar o aplicativo com base na identificação de produto na guia processos no Gerenciador de tarefas do Windows. Esse parâmetro pode ser combinado com **- a**, **- n**, e **-p**.                           |
| -p <Protocol> |               Mostra as conexões para o protocolo especificado por *protocolo*. Nesse caso, o *protocolo* pode ser tcp, udp, tcpv6 ou udpv6. Se esse parâmetro é usado com **-s** para exibir estatísticas por protocolo *protocolo* pode ser tcp, udp, icmp, ip, tcpv6, udpv6, icmpv6 ou ipv6.                |
|      -s       | Exibe estatísticas por protocolo. Por padrão, as estatísticas são mostradas para os protocolos TCP, UDP, ICMP e IP. Se o protocolo IPv6 estiver instalado, as estatísticas são mostradas para o TCP sobre IPv6, UDP sobre IPv6, ICMPv6 e IPv6 protocolos. O **-p** parâmetro pode ser usado para especificar um conjunto de protocolos. |
|      -r       |                                                                                                     Exibe o conteúdo da tabela de roteamento de IP. Isso é equivalente ao comando de impressão de rota.                                                                                                     |
|  <Interval>   |                                                        Exibe novamente as informações selecionadas cada *intervalo* segundos. Pressione CTRL + C para interromper a nova exibição. Se esse parâmetro for omitido, **netstat** imprime as informações selecionadas somente uma vez.                                                         |
|      /?       |                                                                                                                                 Exibe a ajuda no prompt de comando.                                                                                                                                  |

## <a name="remarks"></a>Comentários
-   Os parâmetros usados com este comando devem ser prefixados com um hífen ( **-** ) em vez de uma barra ( **/** ).
-   **netstat** fornece estatísticas para o seguinte:
    -   Proto o nome do protocolo (TCP ou UDP).
    -   Endereço local o endereço IP do computador local e o número da porta que está sendo usado. O nome do computador local que corresponde ao endereço IP e o nome da porta são mostrados, a menos que o **- n** parâmetro for especificado. Se a porta ainda não foi estabelecida, o número da porta é mostrado como um asterisco (*).
    -   Endereço estrangeiro IP endereço e número da porta do computador remoto ao qual o soquete está conectado. Os nomes que corresponde ao endereço IP e a porta são mostrados, a menos que o **- n** parâmetro for especificado. Se a porta ainda não foi estabelecida, o número da porta é mostrado como um asterisco (*).
    -   (estado) Indica o estado de uma conexão TCP. Os possíveis estados são da seguinte maneira: CLOSE_WAIT fechado estabelecida FIN_WAIT_1 FIN_WAIT_2 LAST_ACK escuta SYN_RECEIVED SYN_SEND timeD_WAIT para obter mais informações sobre os estados de uma conexão TCP, consulte Rfc 793.
-   Esse comando está disponível somente se o protocolo de Internet Protocol (TCP/IP) é instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.

## <a name="BKMK_Examples"></a>Exemplos
Para exibir as estatísticas de Ethernet e as estatísticas para todos os protocolos, digite:
```
netstat -e -s
```
Para exibir as estatísticas para somente os protocolos TCP e UDP, digite:
```
netstat -s -p tcp udp
```
Para exibir conexões TCP ativas e as identificações de processos a cada 5 segundos, digite:
```
netstat -o 5
```
Para exibir o TCP ativas as conexões e o processo de IDs de formato numérico, digite:
```
netstat -n -o
```

## <a name="additional-references"></a>Referências adicionais
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
