---
title: nbtstat
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8077228a6c72302e63a2d1b8123e7f7e0ff6b8e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839019"
---
# <a name="nbtstat"></a>nbtstat

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe as estatísticas de protocolo NetBIOS sobre TCP/IP (NetBT), as tabelas de nomes NetBIOS para o computador local e para os computadores remotos e o cache de nomes NetBIOS. o **NBTSTAT** permite uma atualização do cache de nomes NetBIOS e os nomes registrados com o WINS (serviço de cadastramento na Internet do Windows). Usado sem parâmetros, **NBTSTAT** exibe a ajuda. 

## <a name="syntax"></a>Sintaxe

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

#### <a name="parameters"></a>Parâmetros

|    Parâmetro    |                                                                                                                         Descrição                                                                                                                         |
|-----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /a <remoteName> |    Exibe a tabela de nomes NetBIOS de um computador remoto, em que *RemoteName* é o nome do computador NetBIOS do computador remoto. A tabela de nomes NetBIOS é a lista de nomes NetBIOS que corresponde a aplicativos NetBIOS em execução nesse computador.     |
| /A <IPaddress>  |                                                           Exibe a tabela de nomes NetBIOS de um computador remoto, especificada pelo endereço IP (em notação decimal pontilhada) do computador remoto.                                                            |
|       /c        |                                                                        Exibe o conteúdo do cache de nomes NetBIOS, a tabela de nomes NetBIOS e seus endereços IP resolvidos.                                                                         |
|       /n        |                                            Exibe a tabela de nomes NetBIOS do computador local. O status **registrado** indica que o nome é registrado por difusão ou com um servidor WINS.                                             |
|       /r        |      Exibe estatísticas de resolução de nomes NetBIOS. Em um computador que executa o Windows XP ou o Windows Server 2003 que está configurado para usar o WINS, esse parâmetro retorna o número de nomes que foram resolvidos e registrados usando broadcast e WINS.       |
|       /R        |                                                                      Limpa o conteúdo do cache de nomes NetBIOS e, em seguida, recarrega as entradas marcadas #PRE do arquivo **Lmhosts** .                                                                      |
|       /RR       |                                                                           Libera e atualiza nomes NetBIOS para o computador local que é registrado com servidores WINS.                                                                            |
|       /s        |                                                                          Exibe sessões de cliente e servidor NetBIOS, tentando converter o endereço IP de destino em um nome.                                                                           |
|       /S        |                                                                          Exibe as sessões de cliente e servidor NetBIOS, listando somente os computadores remotos por endereço IP de destino.                                                                          |
|   <Interval>    | Reexibe as estatísticas selecionadas, pausando o número de segundos especificado em *intervalo* entre cada exibição. Pressione CTRL + C para parar de Reexibir as estatísticas. Se esse parâmetro for omitido, o **NBTSTAT** imprime as informações de configuração atuais apenas uma vez. |
|       /?        |                                                                                                            Exibe a ajuda no prompt de comando.                                                                                                             |

## <a name="remarks"></a>Comentários

-   os parâmetros de linha de comando **NBTSTAT** diferenciam maiúsculas de minúsculas.

-   A tabela a seguir descreve os títulos de coluna gerados pelo **NBTSTAT**:

    |Direcionamento|Descrição|
    |------|--------|
    |Entrada|O número de bytes recebidos.|
    |Saída|O número de bytes enviados.|
    |Entrada/saída|Se a conexão é do computador (de saída) ou de outro computador para o computador local (entrada).|
    |Ciclo|O tempo restante em que uma entrada de cache de tabela de nome residirá antes de ser limpa.|
    |Nome local|O nome NetBIOS local associado à conexão.|
    |Host remoto|O nome ou endereço IP associado ao computador remoto.|
    |< 03 >|O último byte de um nome NetBIOS convertido em hexadecimal. Cada nome NetBIOS tem 16 caracteres. Geralmente, esse último byte tem um significado especial, pois o mesmo nome pode estar presente várias vezes em um computador, diferente somente no último byte. Por exemplo, < 20 > é um espaço em texto ASCII.|
    |type|O tipo do nome. Um nome pode ser um nome exclusivo ou um nome de grupo.|
    |Status|Se o serviço NetBIOS no computador remoto está em execução (registrado) ou se um nome de computador duplicado registrou o mesmo serviço (conflito).|
    |Estado|O estado das conexões NetBIOS.|

-   A tabela a seguir descreve os Estados de conexão NetBIOS possíveis:

    |Estado|Descrição|
    |-----|--------|
    |Conectado|Uma sessão foi estabelecida.|
    |associar|Um ponto de extremidade de conexão foi criado e associado a um endereço IP.|
    |detecta|Esse ponto de extremidade está disponível para uma conexão de entrada.|
    |Idle|Este ponto de extremidade foi aberto, mas não pode receber conexões.|
    |Conectando|Uma sessão está na fase de conexão e o mapeamento de nome para endereço IP do destino está sendo resolvido.|
    |Aceitar|Uma sessão de entrada está sendo aceita no momento e será conectada em breve.|
    |Reconexão|Uma sessão está tentando se reconectar (falha ao conectar na primeira tentativa).|
    |Saída|Uma sessão está na fase de conexão e a conexão TCP está sendo criada no momento.|
    |Entrada|Uma sessão de entrada está na fase de conexão.|
    |Desconectando|Uma sessão está no processo de desconexão.|
    |Desconectado|O computador local emitiu uma desconexão e está aguardando a confirmação do sistema remoto.|

-   Esse comando estará disponível somente se o protocolo TCP/IP estiver instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso
Para exibir a tabela de nomes NetBIOS do computador remoto com o nome do computador NetBIOS de CORP07, digite:

```
nbtstat /a CORP07
```

Para exibir a tabela de nomes NetBIOS do computador remoto que atribuiu o endereço IP de 10.0.0.99, digite:

```
nbtstat /A 10.0.0.99
```

Para exibir a tabela de nomes NetBIOS do computador local, digite:

```
nbtstat /n
```

Para exibir o conteúdo do cache de nomes NetBIOS do computador local, digite:

```
nbtstat /c
```

Para limpar o cache de nomes NetBIOS e recarregar as entradas com marca #PRE no arquivo Lmhosts local, digite:

```
nbtstat /R
```

Para liberar os nomes NetBIOS registrados com o servidor WINS e registrá-los novamente, digite:

```
nbtstat /RR
```

Para exibir estatísticas de sessão NetBIOS por endereço IP a cada cinco segundos, digite:

```
nbtstat /S 5
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)


