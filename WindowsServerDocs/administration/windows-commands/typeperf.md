---
title: typeperf
description: 'Tópico de comandos do Windows para * * *- '
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
ms.openlocfilehash: 1337066f547367b381a531dbab6627ad78d280ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848447"
---
# <a name="typeperf"></a>typeperf



O **typeperf** comando grava dados de desempenho para a janela de comando ou para um arquivo de log. Para interromper **typeperf**, pressione CTRL + C.

Para obter exemplos de como usar **typeperf**, consulte [exemplos](#BKMK_EXAMPLES).

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
|\<counter [counter […]]>|Especifica os contadores de desempenho para monitorar.|

> [!NOTE]
> **\<contador >** é o nome completo de um contador de desempenho no  *\\ \\Computer\Object (instância) \Counter* Formatar, como  **\\ \\Server1\ Processor(0)\% de tempo do usuário**.

## <a name="options"></a>Opções

|Opção|Descrição|
|---------|-----------|
|-?|Exibe contextual a Ajuda.|
|-f \<CSV&verbar;TSV&verbar;BIN&verbar;SQL>|Especifica o formato de arquivo de saída. O padrão é CSV.|
|-cf \<filename>|Especifica um arquivo que contém uma lista de contadores de desempenho para monitorar, com um contador por linha.|
|-si <[[hh:]mm:]ss>|Especifica o intervalo de amostragem. O padrão é um segundo.|
|-o \<filename>|Especifica o caminho para o arquivo de saída ou o banco de dados SQL. O padrão é STDOUT (gravada na janela de comando).|
|-q [objeto]|Exiba uma lista de contadores instalados (nenhuma instância). Para listar os contadores para um objeto, inclua o nome do objeto. EXEMPLO|
|-qx [objeto]|Exiba uma lista de contadores instalados com instâncias. Para listar os contadores para um objeto, inclua o nome do objeto.|
|-sc \<samples>|Especifica o número de amostras a serem coletadas. O padrão é coletar dados até que CTRL + C é pressionado.|
|-config \<filename>|Especifica um arquivo de configurações que contém opções de comando.|
|-s \<computer_name>|Especifica um computador remoto para monitorar se nenhum for especificado no caminho do contador.|
|-y|Responda Sim para todas as perguntas sem avisar.|

## <a name="BKMK_EXAMPLES"></a>Exemplos

-   O exemplo a seguir grava os valores de contador de desempenho do computador local  **\\ \\( total)\% tempo do processador** para a janela de comando em um intervalo de amostragem padrão de 1 segundo até que CTRL + C é pressionado.  
    ```
    typeperf "\Processor)_Total)\% Processor Time"
    ```  
-   O exemplo a seguir grava os valores para a lista de contadores no arquivo **counters.txt** no arquivo delimitado por tabulação **domain2.tsv** a um intervalo de amostragem de 5 segundos até 50 amostras foram coletadas.  
    ```
    typeperf -cf counters.txt -si 5 -sc 50 -f TSV -o domain2.tsv
    ```  
-   A exemplo a seguir consulta os contadores instalados com instâncias para o objeto de contador **PhysicalDisk** e grava a lista resultante para o arquivo **counters.txt**.  
    ```
    typeperf -qx PhysicalDisk -o counters.txt
    ```
