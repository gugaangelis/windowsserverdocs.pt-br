---
title: tracerpt
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4d943da40793be0680d6ea6a4d9de0960f65c72
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861697"
---
# <a name="tracerpt"></a>tracerpt



O **tracerpt** comando pode ser usado para analisar Logs de rastreamento de eventos, arquivos de log gerados pelo Monitor de desempenho e provedores de rastreamento de eventos em tempo real. Ele gera arquivos de despejo, arquivos de relatório e esquemas de relatório.

Para obter exemplos de como usar **tracerpt**, consulte [exemplos](#BKMK_EXAMPLES).

## <a name="syntax"></a>Sintaxe

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>Opções

|Sinalizador de opção|Descrição|
|-----------|-----------|
|-?|Sensíveis ao contexto de exibe a Ajuda.|
|-config \<filename>|Carrega um arquivo de configurações que contém opções de comando.|
|-y|Responda Sim para todas as perguntas sem avisar.|
|-f \<XML | HTML>|Defina o formato de relatório.|
|-de \<CSV | EVTX | XML>|Defina o formato de despejo. O padrão é XML.|
|-df \<filename>|Criar um específico da Microsoft-arquivo de esquema de contagem/relatórios.|
|-int \<filename >|Despeje a estrutura de evento interpretada no arquivo especificado.|
|-rts|Carimbo de hora bruto no cabeçalho de rastreamento de eventos de relatório. Só pode ser usado com -o, não - report ou - summary.|
|-tmf \<filename>|Especifique um arquivo de definição de formato de mensagem de rastreamento.|
|-tp \<value>|Especifique o caminho de pesquisa do arquivo TMF. Vários caminhos podem ser usados, separados por ponto e vírgula (;).|
|-i \<value>|Especifique o caminho de imagem do provedor. O PDB correspondente estará localizado no servidor de símbolos. Vários caminhos podem ser usados, separados por ponto e vírgula (;).|
|-pdb \<value>|Especifique o caminho do servidor de símbolo. Vários caminhos podem ser usados, separados por ponto e vírgula (;).|
|-gmt|Converta WPP carga os carimbos de hora em hora de Greenwich.|
|-rl \<value>|Defina o nível de relatório do sistema de 1 a 5. O padrão é 1.|
|-Resumo [filename]|Gere um arquivo de texto do relatório de resumo. Nome do arquivo se não especificado é Summary. txt.|
|-o [filename]|Gere um arquivo de saída de texto. Nome do arquivo se não especificado é dumpfile.|
|-relatórios [filename]|Gere um arquivo de relatório de saída de texto. Nome do arquivo se não especificado é workload.xml.|
|-lr|Especifique "menos restritiva." Isso usa os melhores esforços para eventos que correspondem ao esquema de eventos.|
|-Exportar [filename]|Gere um arquivo de exportação de esquema de eventos. Nome do arquivo se não especificado é Schema. man.|
|[-l] \<value [value […]]>|Especifique o arquivo de log de rastreamento de eventos para processar.|
|-rt \<session_name [session_name [...]] >|Especifica as fontes de dados de sessão de rastreamento de eventos em tempo real.|

## <a name="BKMK_EXAMPLES"></a>Exemplos

-   Este exemplo cria um relatório com base em dois logs de eventos **logfile1.etl** e **logfile2.etl** e cria o arquivo de despejo **logdump.xml** em formato XML.  
    ```
    tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
    ```  
-   Este exemplo cria um relatório com base no log de eventos **. logfile. etl**, cria o arquivo de despejo **logdmp.xml** em formato XML, melhores esforços usa para identificar os eventos não está no esquema, produz um arquivo de relatório de resumo **logdump.txt**e produz o arquivo de relatório **logrpt.xml**.  
    ```
    tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
    ```  
-   Este exemplo usa os dois logs de eventos **logfile1.etl** e **logfile2.etl** para produzir um arquivo de despejo e o arquivo de relatório com os nomes de arquivo padrão.  
    ```
    tracerpt logfile1.etl logfile2.etl -o -report
    ```  
-   Este exemplo usa o log de eventos **. logfile. etl** e o log de desempenho **counterfile.blg** para produzir o arquivo de relatório **logrpt.xml** e o esquema XML específico da Microsoft arquivo **Schema. XML**.  
    ```
    tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
    ```  
-   Este exemplo lê a sessão de rastreamento de eventos em tempo real "Agente NT Kernel" e produz o arquivo de despejo **logfile.csv** no formato CSV.  
    ```
    tracerpt -rt "NT Kernel Logger" -o logfile.csv -of CSV
    ```