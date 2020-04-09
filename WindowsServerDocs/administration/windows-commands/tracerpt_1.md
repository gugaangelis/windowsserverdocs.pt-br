---
title: tracerpt
description: O tópico de comandos do Windows para Tracerpt, que analisa logs de rastreamento de eventos, arquivos de log gerados pelo monitor de desempenho e provedores de rastreamento de eventos em tempo real.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cb9eaf86-0ef6-4197-b6c8-9cca8a1d723c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 17bed5b1cb084392ed3169ca963ce03c1ee2b0d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832679"
---
# <a name="tracerpt"></a>tracerpt

O comando **Tracerpt** pode ser usado para analisar logs de rastreamento de eventos, arquivos de log gerados pelo monitor de desempenho e provedores de rastreamento de eventos em tempo real. Ele gera arquivos de despejo, arquivos de relatório e esquemas de relatório.

Para obter exemplos de como usar o **Tracerpt**, consulte [exemplos](#BKMK_EXAMPLES).

## <a name="syntax"></a>Sintaxe

```
tracerpt <[-l] <value [value [...]]>|-rt <session_name [session_name [...]]>> [options]
```

## <a name="options"></a>{1&gt;Opções&lt;1}

|              Sinalizador de opção               |                                                                    Descrição                                                                    |
|----------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|
|                   -?                   |                                                         Exibe a ajuda contextual.                                                          |
|          -config \<nome do arquivo >           |                                                 Carregue um arquivo de configurações contendo opções de comando.                                                  |
|                   -y                   |                                                  Responda sim a todas as perguntas sem avisar.                                                   |
|            -f \<XML\|HTML >             |                                                                  Formato do relatório.                                                                   |
|         -de \<CSV\|o XML do EVTX\|>          |                                                         Formato de despejo, o padrão é XML.                                                          |
|            -DF \<nome do arquivo >             |                                            Crie um arquivo de esquema de contagem/relatório específico da Microsoft.                                            |
|            -int \<nome do arquivo >            |                                            Despeja a estrutura de evento interpretada no arquivo especificado.                                            |
|                  -RTS                  |                        Relatar carimbo de data/hora bruto no cabeçalho de rastreamento de eventos. Só pode ser usado com-o, não relatório ou-Summary.                         |
|            -TMF \<nome do arquivo >            |                                                  Especifique um arquivo de definição de formato de mensagem de rastreamento.                                                  |
|              -TP \<valor >              |                            Especifique o caminho de pesquisa do arquivo TMF. Vários caminhos podem ser usados, separados por um ponto-e-vírgula (;).                            |
|              -i \<valor >               | Especifique o caminho da imagem do provedor. O PDB correspondente estará localizado no servidor de símbolos. Vários caminhos podem ser usados, separados por um ponto-e-vírgula (;). |
|             -PDB \<valor >              |                             Especifique o caminho do servidor de símbolos. Vários caminhos podem ser usados, separados por um ponto-e-vírgula (;).                             |
|                  -GMT                  |                                              Converter carimbos de data/hora de carga de WPP em Greenwich Mean Time.                                               |
|              -RL \<valor >              |                                               Defina nível de relatório do sistema de 1 a 5. O padrão é 1.                                               |
|          -Resumo [nome do arquivo]           |                                  Gerar um arquivo de texto de relatório de resumo. Filename se não for especificado é Summary. txt.                                   |
|             -o [nome do arquivo]              |                                      Gerar um arquivo de saída de texto. Filename se não for especificado é dumpfile. xml.                                      |
|           -relatório [nome do arquivo]           |                                  Gerar um arquivo de relatório de saída de texto. Filename se não especificado for Workload. xml.                                   |
|                  -LR                   |                        Especifique menos restritivo. Isso usa os melhores esforços para eventos que não correspondem ao esquema de eventos.                         |
|           -exportar [nome do arquivo]           |                                  Gerar um arquivo de exportação de esquema de evento. Filename se não especificado for Schema. Man.                                   |
|       [-l] \<valor [valor [...]] >        |                                                   Especifique o arquivo de log de rastreamento de eventos a ser processado.                                                    |
| -RT \<session_name [session_name [...]] > |                                                Especificar fontes de dados da sessão de rastreamento de eventos em tempo real.                                                |

## <a name="examples"></a><a name=BKMK_EXAMPLES></a>Disso

- Este exemplo cria um relatório com base nos dois logs de eventos **logfile1. etl** e **logfile2. etl** e cria o arquivo de despejo **LOGDUMP. xml** no formato XML.  
  ```
  tracerpt logfile1.etl logfile2.etl -o logdump.xml -of XML
  ```  
- Este exemplo cria um relatório baseado no log de eventos **logfile. etl**, cria o arquivo de despejo **logdmp. XML** em formato XML, usa os melhores esforços para identificar eventos que não estão no esquema, produz um arquivo de relatório de resumo **logdump. txt**e produz o arquivo de relatório **logrpt. xml**.  
  ```
  tracerpt logfile.etl -o logdmp.xml -of XML -lr -summary logdmp.txt -report logrpt.xml
  ```  
- Este exemplo usa os dois logs de eventos **logfile1. etl** e **logfile2. etl** para produzir um arquivo de despejo e um arquivo de relatório com os nomes de arquivos padrão.  
  ```
  tracerpt logfile1.etl logfile2.etl -o -report
  ```  
- Este exemplo usa o log de eventos **logfile. etl** e o log de desempenho **MyFile. blg** para produzir o arquivo de relatório **logrpt. xml** e o arquivo de esquema XML específico da Microsoft **Schema. xml**.  
  ```
  tracerpt logfile.etl counterfile.blg -report logrpt.xml -df schema.xml
  ```  
- Este exemplo lê o agente de log de kernel NT de sessão de rastreamento de eventos em tempo real e produz o arquivo de despejo **logfile. csv** no formato CSV.  
  ```
  tracerpt -rt NT Kernel Logger -o logfile.csv -of CSV
  ```
