---
title: typeperf
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c7ca89a-03b3-4626-afcf-ef8565e90043
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cfcbac82b88c0c8d8bcc706ebfd807f96e359de7
ms.sourcegitcommit: af80963a1d16c0b836da31efd9c5caaaf6708133
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/31/2019
ms.locfileid: "66440782"
---
# <a name="typeperf"></a>typeperf



O comando **Typeperf** grava dados de desempenho na janela de comando ou em um arquivo de log. Para parar **Typeperf**, pressione CTRL + C.

Para obter exemplos de como usar **Typeperf**, consulte [exemplos](#BKMK_EXAMPLES).

## <a name="syntax"></a>Sintaxe

```
typeperf <counter [counter ...]> [options]
typeperf -cf <filename> [options]
typeperf -q [object] [options]
typeperf -qx [object] [options]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<contador [contador [...]] >|Especifica os contadores de desempenho a serem monitorados.|

> [!NOTE]
> o contador > é o nome completo de um contador de desempenho no  *\\ \\formato Computer\Object (instância) \Counter* ,  **\<**  **\\ \\como Server1\Processor (0)\%Hora do usuário**.

## <a name="options"></a>Opções

|                   Opção                   |                                                         Descrição                                                          |
|--------------------------------------------|------------------------------------------------------------------------------------------------------------------------------|
|                     -?                     |                                               Exibe a ajuda contextual.                                               |
| -f \<CSV&verbar;TSV&verbar;SQL&verbar;> |                                    Especifica o formato do arquivo de saída. O padrão é CSV.                                     |
|              -nome \<de arquivo do CF >               |              Especifica um arquivo que contém uma lista de contadores de desempenho a serem monitorados, com um contador por linha.               |
|             -si < [[hh:] mm:] SS >             |                                  Especifica o intervalo de amostragem. O padrão é um segundo.                                   |
|               -o \<nome de arquivo >               |     Especifica o caminho para o arquivo de saída ou o banco de dados SQL. O padrão é STDOUT (gravado na janela de comando).      |
|                -q [objeto]                 | Exibe uma lista de contadores instalados (sem instâncias). Para listar os contadores de um objeto, inclua o nome do objeto. \*\*\*EXEMPLO |
|                -QX [objeto]                |        Exibe uma lista de contadores instalados com instâncias. Para listar os contadores de um objeto, inclua o nome do objeto.        |
|               -SC \<amostras >               |             Especifica o número de amostras a serem coletadas. O padrão é coletar dados até que CTRL + C seja pressionado.              |
|            -config \<nome de arquivo >             |                                    Especifica um arquivo de configurações contendo opções de comando.                                     |
|            -s \<nome_do_computador >             |                   Especifica um computador remoto para monitorar se nenhum computador for especificado no caminho do contador.                    |
|                     -y                     |                                        Responda sim a todas as perguntas sem avisar.                                        |

## <a name="BKMK_EXAMPLES"></a>Disso

- O exemplo a seguir grava os valores para o tempo do processador do **\\ \\\%** contador de desempenho do computador local (_ total) para a janela de comando em um intervalo de exemplo padrão de 1 segundo até que CTRL + C seja pressionado .  
  ```
  typeperf "\Processor(_Total)\% Processor Time"
  ```  
- O exemplo a seguir grava os valores da lista de contadores no arquivo **Counters. txt** para o arquivo delimitado por tabulação **domain2. tsv** em um intervalo de exemplo de 5 segundos até que 50 amostras tenham sido coletadas.  
  ```
  typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
  ```  
- O exemplo a seguir consulta os contadores instalados com instâncias para o objeto Counter **PhysicalDisk** e grava a lista resultante no arquivo **Counters. txt**.  
  ```
  typeperf -qx PhysicalDisk -o counters.txt
  ```
