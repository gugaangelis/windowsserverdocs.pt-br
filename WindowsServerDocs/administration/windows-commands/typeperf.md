---
title: typeperf
description: Tópico de referência para Typeperf, que grava dados de desempenho na janela de comando ou em um arquivo de log.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a996abc1417fdb6aa50370a942433716d8df2cbe
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721213"
---
# <a name="typeperf"></a>typeperf

O comando **Typeperf** grava dados de desempenho na janela de comando ou em um arquivo de log. Para parar **Typeperf**, pressione CTRL + C.

## <a name="syntax"></a>Sintaxe

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<contador [contador [...]] >|Especifica os contadores de desempenho a serem monitorados.|

> [!NOTE]
> o contador>é o nome completo de um contador de desempenho no * \\ \\formato Computer\Object (instância) \Counter* , ** \\ \\como Server1\Processor (0\% ) hora do usuário**. ** \<**

## <a name="options"></a>Opções

|                   Opção                   |                                                         Descrição                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               Exibe a ajuda contextual.                                               |
| -f \<CSV&verbar;TSV&verbar;&verbar;SQL> |                                    Especifica o formato do arquivo de saída. O padrão é CSV.                                     |
|              -nome \<de arquivo do CF>               |              Especifica um arquivo que contém uma lista de contadores de desempenho a serem monitorados, com um contador por linha.               |
|             -si < [[hh:] mm:] SS>             |                                  Especifica o intervalo de amostragem. O padrão é um segundo.                                   |
|               -o \<nome de arquivo>               |     Especifica o caminho para o arquivo de saída ou o banco de dados SQL. O padrão é STDOUT (gravado na janela de comando).      |
|                -q [objeto]                 | Exibe uma lista de contadores instalados (sem instâncias). Para listar os contadores de um objeto, inclua o nome do objeto. \*\*\*EXEMPLO |
|                -QX [objeto]                |        Exibe uma lista de contadores instalados com instâncias. Para listar os contadores de um objeto, inclua o nome do objeto.        |
|               -SC \<amostras>               |             Especifica o número de amostras a serem coletadas. O padrão é coletar dados até que CTRL + C seja pressionado.              |
|            -config \<nome de arquivo>             |                                    Especifica um arquivo de configurações contendo opções de comando.                                     |
|            -s \<computer_name>             |                   Especifica um computador remoto para monitorar se nenhum computador for especificado no caminho do contador.                    |
|                     -y                     |                                        Responda sim a todas as perguntas sem avisar.                                        |

## <a name="examples"></a>Exemplos

- Para gravar os valores do tempo de processador do processador de contador ** \\ \\de desempenho (\% _Total)** do computador local na janela de comando em um intervalo de amostragem padrão de 1 segundo até que CTRL + C seja pressionado.  
  ```
  typeperf \Processor(_Total)\% Processor Time
  ```  
- Para gravar os valores da lista de contadores no arquivo **Counters. txt** para o arquivo delimitado por tabulação **domain2. tsv** em um intervalo de exemplo de 5 segundos até que 50 amostras tenham sido coletadas.  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- Para consultar os contadores instalados com instâncias para o objeto de contador **PhysicalDisk** e grava a lista resultante no arquivo **Counters. txt**.  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
