---
title: netstat
description: Artigo de referência para o comando netstat, que exibe conexões TCP ativas, portas nas quais o computador está escutando, estatísticas de Ethernet, a tabela de roteamento de IP, estatísticas de IPv4 e estatísticas de IPv6.
ms.topic: reference
ms.assetid: 60e2718f-93cc-4ceb-bf0e-58a6a6e4fc8b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 17c2251fd493041b0b39665a785d6aad8010e1d9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635836"
---
# <a name="netstat"></a>netstat

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe conexões TCP ativas, portas nas quais o computador está escutando, estatísticas de Ethernet, a tabela de roteamento de IP, estatísticas de IPv4 (para os protocolos IP, ICMP, TCP e UDP) e estatísticas de IPv6 (para os protocolos IPv6, ICMPv6, TCP sobre IPv6 e UDP sobre IPv6). Usado sem parâmetros, esse comando exibe conexões TCP ativas.

> [!IMPORTANT]
> Esse comando estará disponível somente se o protocolo TCP/IP estiver instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.

## <a name="syntax"></a>Sintaxe

```
netstat [-a] [-b] [-e] [-n] [-o] [-p <Protocol>] [-r] [-s] [<interval>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -a | Exibe todas as conexões TCP ativas e as portas TCP e UDP nas quais o computador está escutando. |
| -b | Exibe o executável envolvido na criação de cada conexão ou porta de escuta. Em alguns casos, executáveis bem conhecidos hospedam vários componentes independentes e, nesses casos, a sequência de componentes envolvidos na criação da conexão ou porta de escuta é exibida. Nesse caso, o nome do executável é [] na parte inferior, na parte superior, o componente que ele chamou e assim por diante até que o TCP/IP fosse atingido. Observe que essa opção pode ser demorada e irá falhar, a menos que você tenha permissões suficientes.
| -E | Exibe estatísticas de Ethernet, como o número de bytes e de pacotes enviados e recebidos. Esse parâmetro pode ser combinado com **-s**. |
| -n | Exibe conexões TCP ativas, no entanto, endereços e números de porta são expressos numericamente e nenhuma tentativa é feita para determinar nomes. |
| -o | Exibe conexões TCP ativas e inclui a ID do processo (PID) para cada conexão. Você pode encontrar o aplicativo com base na PID na guia processos no Gerenciador de tarefas do Windows. Esse parâmetro pode ser combinado com **-a**, **-n**e **-p**. |
| -p `<Protocol>` | Mostra conexões para o protocolo especificado pelo *protocolo*. Nesse caso, o *protocolo* pode ser TCP, UDP, TCPv6 ou UDPv6. Se esse parâmetro for usado com **-s** para exibir estatísticas por protocolo, o *protocolo* poderá ser TCP, UDP, ICMP, IP, TCPv6, UDPv6, ICMPv6 ou IPv6. |
| -S | Exibe estatísticas por protocolo. Por padrão, as estatísticas são mostradas para os protocolos TCP, UDP, ICMP e IP. Se o protocolo IPv6 estiver instalado, as estatísticas serão mostradas para os protocolos TCP sobre IPv6, UDP sobre IPv6, ICMPv6 e IPv6. O parâmetro **-p** pode ser usado para especificar um conjunto de protocolos. |
| -r | Exibe o conteúdo da tabela de roteamento de IP. Isso é equivalente ao comando Route Print. |
| `<interval>` | Exibe novamente as informações selecionadas a cada *intervalo* em segundos. Pressione CTRL + C para parar a reexibição. Se esse parâmetro for omitido, esse comando imprime as informações selecionadas apenas uma vez. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O comando **netstat** fornece estatísticas para o seguinte:

    | Parâmetro | Descrição |
    | --------- | ----------- |
    | Proto | O nome do protocolo (TCP ou UDP). |
    | Endereço local | O endereço IP do computador local e o número da porta que está sendo usado. O nome do computador local que corresponde ao endereço IP e o nome da porta é mostrado, a menos que o parâmetro **-n** seja especificado. Se a porta ainda não tiver sido estabelecida, o número da porta será mostrado como um asterisco (*). |
    | Endereço externo | O endereço IP e o número da porta do computador remoto ao qual o soquete está conectado. Os nomes que correspondem ao endereço IP e à porta são mostrados, a menos que o parâmetro **-n** seja especificado. Se a porta ainda não tiver sido estabelecida, o número da porta será mostrado como um asterisco (*). |
    | Estado | Indica o estado de uma conexão TCP, incluindo:<ul><li>CLOSE_WAIT</li><li>CLOSED</li><li>ESTABELECIDA</li><li>FIN_WAIT_1</li><li>FIN_WAIT_2</li><li>LAST_ACK</li><li>OUVIR</li><li>SYN_RECEIVED</li><li>SYN_SEND</li><li>TIMED_WAIT</li></ul> |

### <a name="examples"></a>Exemplos

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

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
