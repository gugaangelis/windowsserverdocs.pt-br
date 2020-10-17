---
title: tracerpt
description: Artigo de referência para o comando Tracerpt, que analisa logs de rastreamento de eventos, arquivos de log gerados pelo monitor de desempenho e provedores de rastreamento de eventos em tempo real.
ms.topic: reference
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1e468dab3c99219560047668f9f1bd001e8e451e
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156631"
---
# <a name="tracerpt"></a>tracerpt

O comando **Tracerpt** analisa logs de rastreamento de eventos, arquivos de log gerados pelo monitor de desempenho e provedores de rastreamento de eventos em tempo real. Ele também gera arquivos de despejo, arquivos de relatório e esquemas de relatório.

## <a name="syntax"></a>Sintaxe

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetros | Descrição |
|--|--|
| -configuração `<filename>` | Especifica qual arquivo de configurações carregar, que inclui as opções de comando. |
| -y | Especifica responder **Sim** a todas as perguntas, sem avisar. |
| -f `<XML | HTML>` | Especifica o formato do arquivo de relatório. |
| -de `<CSV | EVTX | XML>` | Especifica o formato do arquivo de despejo. O padrão é **XML*. |
| -DF `<filename>` | Especifica a criação de um arquivo de esquema de contagem/relatório específico da Microsoft. |
| -int `<filename>` | Especifica o despejo da estrutura de eventos interpretados para o arquivo especificado. |
| -RTS | Especifica a adição do carimbo de data/hora bruto do relatório no cabeçalho de rastreamento de eventos. Só pode ser usado com **-o**. Não há suporte para **-Report** ou **-Summary**. |
| -tmf `<filename>` | Especifica o arquivo de definição de formato de mensagem de rastreamento a ser usado. |
| -TP `<value>` | Especifica o caminho de pesquisa do arquivo TMF. Vários caminhos podem ser usados, separados por um ponto-e-vírgula (;). |
| -i `<value>` | Especifica o caminho da imagem do provedor. O PDB correspondente estará localizado no servidor de símbolos. Vários caminhos podem ser usados, separados por um ponto-e-vírgula (;). |
| -PDB `<value>` | Especifica o caminho do servidor de símbolos. Vários caminhos podem ser usados, separados por um ponto-e-vírgula (;). |
| -GMT | Especifica a conversão de carimbos de data/hora da carga WPP em Greenwich Mean Time. |
| -rl `<value>` | Especifica o nível de relatório do sistema de 1 a 5. O padrão é *1*. |
| -Resumo [nome do arquivo] | Especifica a criação de um arquivo de texto de relatório de resumo. O nome de arquivo, se não especificado, é *summary.txt*. |
| -o [nome do arquivo] | Especifica a criação de um arquivo de saída de texto. O nome de arquivo, se não especificado, é *dumpfile.xml*. |
| -relatório [nome do arquivo] | Especifica a criação de um arquivo de relatório de saída de texto. O nome de arquivo, se não especificado, é *workload.xml*. |
| -LR | Especifica ser menos restritivo. Isso usa os melhores esforços para eventos que não correspondem ao esquema de eventos. |
| -exportar [nome do arquivo] | Especifica a criação de um arquivo de exportação de esquema de evento. O nome do arquivo, se não for especificado, é *Schema. Man*. |
| [-l] `<value [value […]]>` | Especifica o arquivo de log de rastreamento de eventos a ser processado. |
| -RT `<session_name [session_name […]]>` | Especifica as fontes de dados da sessão de rastreamento de eventos em tempo real. |
| -? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para criar um relatório com base nos dois logs de eventos *logfile1. etl* e *logfile2. etl*, e para criar o arquivo de despejo *logdump.xml* em formato *XML* , digite:

```
tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
```

Para criar um relatório com base no log de eventos *logfile. etl*, para criar o arquivo de despejo *logdmp.xml* em formato XML, para usar os melhores esforços para identificar os eventos que não estão no esquema e para produzir um arquivo de relatório de resumo *logdump.txt* e um arquivo de relatório, *logrpt.xml*, digite:

```
tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
```

Para usar os dois logs de eventos *logfile1. etl* e *logfile2. etl* para produzir um arquivo de despejo e para relatar um arquivo com os nomes de arquivos padrão, digite:

```
tracerpt logfile1.etl logfile2.etl -o -report
```

Para usar o log de eventos *logfile. etl* e o arquivo *. blg* de log de desempenho para produzir o arquivo de relatório *logrpt.xml* e o arquivo de esquema XML específico da Microsoft *schema.xml*, digite:

```
tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
```

Para ler o agente de log de kernel NT da sessão de rastreamento de eventos em tempo real e produzir o arquivo de despejo *logfile.csv* no formato *CSV* , digite:

```
tracerpt -rt NT Kernel Logger -o logfile.csv -of CSV
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
