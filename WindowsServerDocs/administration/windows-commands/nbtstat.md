---
title: nbtstat
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d2ea99e-72f1-471f-9525-d2c49bf3be82
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0193152674dc934aa4f2d3be4dec54afc3066951
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867697"
---
# <a name="nbtstat"></a>nbtstat

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cache de nomes NetBIOS de exibe sobre estatísticas de protocolo TCP/IP (NetBT), as tabelas de nomes NetBIOS para o computador local e a computadores remotos e o NetBIOS. **nbtstat** permite que uma atualização do cache de nomes NetBIOS e os nomes registrados com o Windows Internet Name Service (WINS). Usado sem parâmetros, **nbtstat** exibe a Ajuda. 

## <a name="syntax"></a>Sintaxe

```
nbtstat [/a <remoteName>] [/A <IPaddress>] [/c] [/n] [/r] [/R] [/RR] [/s] [/S] [<Interval>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|/a <remoteName>|Exibe a tabela de nomes NetBIOS de um computador remoto, onde *remoteName* é o nome do computador NetBIOS do computador remoto. A tabela de nomes NetBIOS é a lista de nomes NetBIOS que corresponde aos aplicativos em execução nesse computador.|
|/A <IPaddress>|Exibe a tabela de nomes NetBIOS de um computador remoto, especificado pelo endereço IP (em notação de ponto decimal) do computador remoto.|
|/c|Exibir o conteúdo de NetBIOS nome do cache, a tabela de nomes NetBIOS e seus endereços IP resolvidos.|
|/n|Exibe a tabela de nomes NetBIOS do computador local. O status dos **registrado** indica que o nome é registrado por difusão ou com um servidor WINS.|
|/r|Exibe as estatísticas de resolução de nome NetBIOS. Em um computador executando o Windows XP ou Windows Server 2003 que está configurado para usar o WINS, esse parâmetro retorna o número de nomes que foram resolvidos e registrados por difusão ou WINS.|
|/R|Limpa o conteúdo do cache de nomes NetBIOS e recarrega o #tagged previamente as entradas a **Lmhosts** arquivo.|
|/RR|Libera e, em seguida, atualiza nomes NetBIOS para o computador local que está registrado com servidores WINS.|
|/s|Exibe sessões de cliente e servidor NetBIOS, a tentativa de converter o endereço IP de destino com um nome.|
|/S|Exibe sessões de cliente e servidor NetBIOS, listando os computadores remotos por apenas o endereço IP de destino.|
|<Interval>|Exibe novamente estatísticas selecionadas, pausando o número de segundos especificado no *intervalo* entre cada exibição. Pressione CTRL + C para parar de reexibir estatísticas. Se esse parâmetro for omitido, **nbtstat** imprime as informações de configuração atuais apenas uma vez.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   **nbtstat** parâmetros de linha de comando diferenciam maiusculas de minúsculas.

-   A tabela a seguir descreve os cabeçalhos de coluna que são gerados pelo **nbtstat**:

    |Direcionamento|Descrição|
    |------|--------|
    |Entrada|O número de bytes recebidos.|
    |Saída|O número de bytes enviados.|
    |Entrada/saída|Se a conexão é do computador (saído) ou de outro computador para o computador local (entrada).|
    |Vida útil|O tempo restante que uma entrada de cache da tabela de nome durará antes de ser apagada.|
    |Nome local|O nome NetBIOS local associado com a conexão.|
    |remote Host|O nome ou endereço IP associado ao computador remoto.|
    |<03>|O último byte de um nome NetBIOS são convertidos em hexadecimal. Cada nome de NetBIOS é 16 caracteres. Este último byte frequentemente tem significado especial, pois o mesmo nome pode estar presente várias vezes em um computador, com diferença apenas no último byte. Por exemplo, < 20 > é um espaço em texto ASCII.|
    |type|O tipo de nome. Um nome pode ser um nome exclusivo ou um nome de grupo.|
    |Status|Se o serviço NetBIOS no computador remoto está em execução (registrado) ou um nome de computador duplicado registrou o mesmo serviço (conflito).|
    |Estado|O estado das conexões do NetBIOS.|

-   A tabela a seguir descreve os possíveis estados de conexão de NetBIOS:

    |Estado|Descrição|
    |-----|--------|
    |Conectado|Uma sessão foi estabelecida.|
    |Associados|Um ponto de extremidade de conexão foi criado e associado com um endereço IP.|
    |escutando|Esse ponto de extremidade está disponível para uma conexão de entrada.|
    |Idle|Esse ponto de extremidade foi aberto mas não pode receber conexões.|
    |Conectar-se|Uma sessão está na fase de conexão e o mapeamento de nome-para endereço IP de destino está sendo resolvido.|
    |Aceitar|Uma sessão de entrada está atualmente sendo aceita e logo será conectada.|
    |Reconectar-se|Uma sessão é tentar reconectar (ele falha ao conectar-se na primeira tentativa).|
    |Saída|Uma sessão está na fase de conexão e a conexão TCP está sendo criada.|
    |Entrada|Uma sessão de entrada está em fase de conexão.|
    |Desconectando|Uma sessão está no processo de desconexão.|
    |Disconnected|O computador local tiver emitido uma desconexão e está aguardando a confirmação do sistema remoto.|

-   Esse comando está disponível somente se o protocolo de Internet Protocol (TCP/IP) é instalado como um componente nas propriedades de um adaptador de rede em conexões de rede.

## <a name="BKMK_Examples"></a>Exemplos
Para exibir a tabela de nomes NetBIOS do computador remoto com o nome do computador NetBIOS do CORP07, digite:

```
nbtstat /a CORP07
```

Para exibir a tabela de nomes NetBIOS do computador remoto que recebe o endereço IP 10.0.0.99, digite:

```
nbtstat /A 10.0.0.99
```

Para exibir a tabela de nomes NetBIOS do computador local, digite:

```
nbtstat /n
```

Para exibir o conteúdo do cache de nomes NetBIOS de computador local, digite:

```
nbtstat /c
```

Para limpar o cache de nomes NetBIOS e recarregar o # previamente tagged entradas no arquivo Lmhosts local, digite:

```
nbtstat /R
```

Para liberar os nomes NetBIOS registrados com o servidor WINS e registrá-las novamente, digite:

```
nbtstat /RR
```

Para exibir estatísticas de sessão NetBIOS pelo endereço IP a cada cinco segundos, digite:

```
nbtstat /S 5
```

## <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)


