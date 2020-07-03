---
title: tracerpt
description: Artigo de referência para Tracerpt, que analisa logs de rastreamento de eventos, arquivos de log gerados pelo monitor de desempenho e provedores de rastreamento de eventos em tempo real.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7060932f0b7eb996d0f0934e6945665c0c91e916
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85935584"
---
# <a name="tracerpt"></a>tracerpt

O comando **Tracerpt** pode ser usado para analisar logs de rastreamento de eventos, arquivos de log gerados pelo monitor de desempenho e provedores de rastreamento de eventos em tempo real. Ele gera arquivos de despejo, arquivos de relatório e esquemas de relatório.

## <a name="syntax"></a>Syntax

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>Opções

|              Sinalizador de opção               |                                                                    Descrição                                                                    |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|                   -?                   |                                                         Exibe a ajuda contextual.                                                          |
|          -configuração\<filename>           |                                                 Carregue um arquivo de configurações contendo opções de comando.                                                  |
|                   -y                   |                                                  Responda sim a todas as perguntas sem avisar.                                                   |
|            -f\<XML\|HTML>             |                                                                  Formato do relatório.                                                                   |
|         -de\<CSV\|EVTX\|XML>          |                                                         Formato de despejo, o padrão é XML.                                                          |
|            -DF\<filename>             |                                            Crie um arquivo de esquema de contagem/relatório específico da Microsoft.                                            |
|            -int\<filename>            |                                            Despeja a estrutura de evento interpretada no arquivo especificado.                                            |
|                  -RTS                  |                        Relatar carimbo de data/hora bruto no cabeçalho de rastreamento de eventos. Só pode ser usado com-o, não relatório ou-Summary.                         |
|            -tmf\<filename>            |                                                  Especifique um arquivo de definição de formato de mensagem de rastreamento.                                                  |
|              -TP\<value>              |                            Especifique o caminho de pesquisa do arquivo TMF. Vários caminhos podem ser usados, separados por um ponto-e-vírgula (;).                            |
|              -i\<value>               | Especifique o caminho da imagem do provedor. O PDB correspondente estará localizado no servidor de símbolos. Vários caminhos podem ser usados, separados por um ponto-e-vírgula (;). |
|             -PDB\<value>              |                             Especifique o caminho do servidor de símbolos. Vários caminhos podem ser usados, separados por um ponto-e-vírgula (;).                             |
|                  -GMT                  |                                              Converter carimbos de data/hora de carga de WPP em Greenwich Mean Time.                                               |
|              -rl \<value>              |                                               Defina nível de relatório do sistema de 1 a 5. O padrão é UTF-1.                                               |
|          -Resumo [nome do arquivo]           |                                  Gerar um arquivo de texto de relatório de resumo. O nome do arquivo, se não especificado, é summary.txt.                                   |
|             -o [nome do arquivo]              |                                      Gerar um arquivo de saída de texto. O nome do arquivo, se não especificado, é dumpfile.xml.                                      |
|           -relatório [nome do arquivo]           |                                  Gerar um arquivo de relatório de saída de texto. O nome do arquivo, se não especificado, é workload.xml.                                   |
|                  -LR                   |                        Especifique menos restritivo. Isso usa os melhores esforços para eventos que não correspondem ao esquema de eventos.                         |
|           -exportar [nome do arquivo]           |                                  Gerar um arquivo de exportação de esquema de evento. Filename se não especificado for Schema. Man.                                   |
|       [-l]\<value [value […]]>        |                                                   Especifique o arquivo de log de rastreamento de eventos a ser processado.                                                    |
| -RT\<session_name [session_name […]]> |                                                Especificar fontes de dados da sessão de rastreamento de eventos em tempo real.                                                |

## <a name="examples"></a>Exemplos

- Este exemplo cria um relatório com base nos dois logs de eventos **logfile1. etl** e **logfile2. etl** e cria o arquivo de despejo **logdump.xml** em formato XML.
  ```
  tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
  ```
- Este exemplo cria um relatório baseado no log de eventos **logfile. etl**, cria o arquivo de despejo **logdmp.xml** em formato XML, usa os melhores esforços para identificar eventos que não estão no esquema, produz um arquivo de relatório de resumo **logdump.txt**e produz o arquivo de relatório **logrpt.xml**.
  ```
  tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
  ```
- Este exemplo usa os dois logs de eventos **logfile1. etl** e **logfile2. etl** para produzir um arquivo de despejo e um arquivo de relatório com os nomes de arquivos padrão.
  ```
  tracerpt logfile1.etl logfile2.etl -o -report
  ```
- Este exemplo usa o log de eventos **logfile. etl** e o log de desempenho **MyFile. blg** para produzir o arquivo de relatório **logrpt.xml** e o arquivo de esquema XML específico da Microsoft **schema.xml**.
  ```
  tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
  ```
- Este exemplo lê o agente de log de kernel NT de sessão de rastreamento de eventos em tempo real e produz o arquivo de despejo **logfile.csv** no formato CSV.
  ```
  tracerpt -rt NT Kernel Logger -o logfile.csv -of CSV
  ```
