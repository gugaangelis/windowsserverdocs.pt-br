---
title: netstat
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afd34cca2ecd3caa7ac480b380b85ba6d2a19fcb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839009"
---
# <a name="netstat"></a>netstat

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe conexões TCP ativas, portas nas quais o computador está escutando, estatísticas de Ethernet, a tabela de roteamento de IP, estatísticas de IPv4 (para os protocolos IP, ICMP, TCP e UDP) e estatísticas de IPv6 (para os protocolos IPv6, ICMPv6, TCP sobre IPv6 e UDP sobre IPv6). Usado sem parâmetros, **netstat** EXIBE conexões TCP ativas. 

## <a name="syntax"></a>Sintaxe
```
netstat [-a] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<Interval>]
```

#### <a name="parameters"></a>Parâmetros

|   Parâmetro   |                                                                                                                                              Descrição                                                                                                                                              |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      -a       |                                                                                                   Exibe todas as conexões TCP ativas e as portas TCP e UDP nas quais o computador está escutando.                                                                                                   |
|      {1&gt;-e&lt;1}       |                                                                                 Exibe estatísticas de Ethernet, como o número de bytes e de pacotes enviados e recebidos. Esse parâmetro pode ser combinado com **-s**.                                                                                  |
|      -n       |                                                                               Exibe conexões TCP ativas, no entanto, endereços e números de porta são expressos numericamente e nenhuma tentativa é feita para determinar nomes.                                                                               |
|      -o       |                          Exibe conexões TCP ativas e inclui a ID do processo (PID) para cada conexão. Você pode encontrar o aplicativo com base na PID na guia processos no Gerenciador de tarefas do Windows. Esse parâmetro pode ser combinado com **-a**, **-n**e **-p**.                           |
| -p <Protocol> |               Mostra conexões para o protocolo especificado pelo *protocolo*. Nesse caso, o *protocolo* pode ser TCP, UDP, TCPv6 ou UDPv6. Se esse parâmetro for usado com **-s** para exibir estatísticas por protocolo, o *protocolo* poderá ser TCP, UDP, ICMP, IP, TCPv6, UDPv6, ICMPv6 ou IPv6.                |
|      -s       | Exibe estatísticas por protocolo. Por padrão, as estatísticas são mostradas para os protocolos TCP, UDP, ICMP e IP. Se o protocolo IPv6 estiver instalado, as estatísticas serão mostradas para os protocolos TCP sobre IPv6, UDP sobre IPv6, ICMPv6 e IPv6. O parâmetro **-p** pode ser usado para especificar um conjunto de protocolos. |
|      -r       |                                                                                                     Exibe o conteúdo da tabela de roteamento de IP. Isso é equivalente ao comando Route Print.                                                                                                     |
|  <Interval>   |                                                        Exibe novamente as informações selecionadas a cada *intervalo* em segundos. Pressione CTRL + C para parar a reexibição. Se esse parâmetro for omitido, o **netstat** imprime as informações selecionadas apenas uma vez.                                                         |
|      /?       |                                                                                                                                 Exibe a ajuda no prompt de comando.                                                                                                                                  |

## <a name="remarks"></a>Comentários
-   Os parâmetros usados com esse comando devem ser prefixados com um hífen ( **-** ) em vez de uma barra ( **/** ).
-   o **netstat** fornece estatísticas para o seguinte:
    -   Proto o nome do protocolo (TCP ou UDP).
    -   Endereço local o endereço IP do computador local e o número da porta que está sendo usado. O nome do computador local que corresponde ao endereço IP e o nome da porta é mostrado, a menos que o parâmetro **-n** seja especificado. Se a porta ainda não tiver sido estabelecida, o número da porta será mostrado como um asterisco (*).
    -   Endereço externo o endereço IP e o número da porta do computador remoto ao qual o soquete está conectado. Os nomes que correspondem ao endereço IP e à porta são mostrados, a menos que o parâmetro **-n** seja especificado. Se a porta ainda não tiver sido estabelecida, o número da porta será mostrado como um asterisco (*).
    -   status Indica o estado de uma conexão TCP. Os possíveis Estados são os seguintes: CLOSE_WAIT fechado estabelecido FIN_WAIT_1 FIN_WAIT_2 LAST_ACK escutar SYN_RECEIVED SYN_SEND timeD_WAIT para obter mais informações sobre os Estados de uma conexão TCP, consulte RFC 793.
-   Esse comando estará disponível somente se o protocolo TCP/IP estiver instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso
Para exibir as estatísticas de Ethernet e as estatísticas de todos os protocolos, digite:
```
netstat -e -s
```
Para exibir as estatísticas somente para os protocolos TCP e UDP, digite:
```
netstat -s -p tcp udp
```
Para exibir conexões TCP ativas e as IDs de processo a cada 5 segundos, digite:
```
netstat -o 5
```
Para exibir conexões TCP ativas e as IDs de processo usando formulário numérico, digite:
```
netstat -n -o
```

## <a name="additional-references"></a>Referências adicionais
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
