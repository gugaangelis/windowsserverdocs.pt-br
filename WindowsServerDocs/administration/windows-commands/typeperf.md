---
title: typeperf
description: Artigo de referência para o comando Typeperf, que grava dados de desempenho na janela de comando ou em um arquivo de log.
ms.topic: reference
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6a06d422db5f681c3e1775facc4f8d0eee422244
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156352"
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

| Parâmetro | Descrição |
|--|--|
| `<counter [counter […]]>` | Especifica os contadores de desempenho a serem monitorados. O `<counter>` parâmetro é o nome completo de um contador de desempenho no \\ formato Computer\Object (instância) \Counter, como `\\Server1\Processor(0)\% User Time` .  |

#### <a name="options"></a>Opções

| Opção | Descrição |
|--|--|
| -f `<CSV | TSV | BIN | SQL>` | Especifica o formato do arquivo de saída. O padrão é CSV. |
| -CF `<filename>` | Especifica um arquivo que contém uma lista de contadores de desempenho a serem monitorados, com um contador por linha. |
| -si `<[[hh:]mm:]ss>` | Especifica o intervalo de amostragem. O padrão é um segundo. |
| -o `<filename>` | Especifica o caminho para o arquivo de saída ou o banco de dados SQL. O padrão é STDOUT (gravado na janela de comando). |
| -q `[object]` | Exibe uma lista de contadores instalados (sem instâncias). Para listar os contadores de um objeto, inclua o nome do objeto. EXEMPLO |
| -qx `[object]` | Exibe uma lista de contadores instalados com instâncias. Para listar os contadores de um objeto, inclua o nome do objeto. |
| -SC `<samples>` | Especifica o número de amostras a serem coletadas. O padrão é coletar dados até que CTRL + C seja pressionado. |
| -configuração `<filename>` | Especifica um arquivo de configurações contendo opções de comando. |
| -s `<computer_name>` | Especifica um computador remoto para monitorar se nenhum computador for especificado no caminho do contador. |
| -y | Responda *Sim* a todas as perguntas sem avisar. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para gravar os valores do contador de desempenho do computador local `\Processor(_Total)\% Processor Time` na janela de comando em um intervalo de amostragem padrão de 1 segundo até que CTRL + C seja pressionado, digite:

```
typeperf \Processor(_Total)\% Processor Time
```

Para gravar os valores da lista de contadores no arquivo *counters.txt* para o arquivo delimitado por tabulação *domain2. tsv* em um intervalo de exemplo de 5 segundos até que 50 amostras tenham sido coletadas, digite:

```
typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
```

Para consultar os contadores instalados com instâncias do objeto Counter *PhysicalDisk* e gravar a lista resultante no arquivo *counters.txt*, digite:

```
typeperf -qx PhysicalDisk -o counters.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
