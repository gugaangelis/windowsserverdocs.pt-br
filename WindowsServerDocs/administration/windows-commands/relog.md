---
title: relog
description: Saiba como extrair informações do contador de desempenho dos arquivos de log do coutner de desempenho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7480f6c0-9953-4d70-9b1c-b27e09d8db13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 00e3f4e67faa951466c59dc60bab1d75c8b2e80f
ms.sourcegitcommit: d050f4d82f462572ccf26437be03bf9ec43ed60e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84436554"
---
# <a name="relog"></a>relog

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Extrai contadores de desempenho dos logs do contador de desempenho em outros formatos, como Text-TSV (para texto delimitado por tabulação), Text-CSV (para texto delimitado por vírgula), binary-BIN ou SQL.

## <a name="syntax"></a>Sintaxe
```
relog [<FileName> [<FileName> ...]] [/a] [/c <path> [<path> ...]] [/cf <FileName>] [/f  {bin|csv|tsv|SQL}] [/t <Value>] [/o {OutputFile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<FileName>|i}] [/q]
```

#### <a name="parameters"></a>Parâmetros

|                                         Parâmetro                                          |                                                                                                                                                                  Descrição                                                                                                                                                                   |
|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                *Nome do arquivo* [*nome do arquivo...*]                                 |                                                                                                                      Especifica o nome de caminho de um log de contador de desempenho existente. Você pode especificar vários arquivos de entrada.                                                                                                                      |
|                                             -a                                             |                                                                                                          Anexa o arquivo de saída em vez de substituir. Essa opção não se aplica ao formato SQL, em que o padrão é sempre acrescentar.                                                                                                           |
|                                   -c *caminho* [*caminho...*]                                   |                                                       Especifica o caminho do contador de desempenho para o log. Para especificar vários caminhos de contador, separe-os com um espaço e coloque os caminhos do contador entre aspas (por exemplo, **"**<em>Counterpath1</em> <em>Counterpath2</em>**"**)                                                       |
|                                       -CF *nome do arquivo*                                       |                                            Especifica o nome do caminho do arquivo de texto que lista os contadores de desempenho a serem incluídos em um arquivo relog. Use esta opção para listar caminhos de contador em um arquivo de entrada, um por linha. A configuração padrão é que todos os contadores no arquivo de log original sejam registrados novamente.                                            |
|                                  -f {bin \| CSV \| \| SQL}                                  |                                       Especifica o nome do caminho do formato de arquivo de saída. O formato padrão é **bin**. Para um banco de dados SQL, o arquivo de saída especifica o *DSN! CounterLog*. Você pode especificar o local do banco de dados usando o Gerenciador ODBC para configurar o DSN (nome do sistema de banco de dados).                                        |
|                                         *valor* -t                                         |                                                                                                           Especifica os intervalos de exemplo em registros "*N*". Inclui todos os enésimo pontos de dados no arquivo relog. O padrão é todos os pontos de dados.                                                                                                           |
| -o {*OutputFile* \| *"SQL: DSN! Counter_Log*}, em que DSN é um DSN ODBC definido no sistema. |                                                   Especifica o nome do caminho do arquivo de saída ou do banco de dados SQL em que os contadores serão gravados. <br>Observação: para as versões de 64 bits e 32 bits do relog. exe, você precisa definir um DSN na fonte de dados ODBC (64-bit e 32-bit respectivamente). Usar o driver ODBC "SQL Server" para definir um DSN                                                   |
|                          -b \<*M*/*D*/*YYYY*> [[*hh*:]*mm*:]*SS*                           |                                                                          Especifica a hora de início para copiar o primeiro registro do arquivo de entrada. a data e a hora devem estar <em>nesse formato exato</em> **/** <em>D</em> **/** <em>YYYYHH</em>**:**<em>mm</em>**:**<em>SS</em>.                                                                          |
|                          -e \<*M*/*D*/*YYYY*> [[*hh*:]*mm*:]*SS*                           |                                                                           Especifica a hora de término para copiar o último registro do arquivo de entrada. a data e a hora devem estar <em>nesse formato exato</em> **/** <em>D</em> **/** <em>YYYYHH</em>**:**<em>mm</em>**:**<em>SS</em>.                                                                            |
|                                -config {*filename* \| *i*}                                 | Especifica o nome do caminho do arquivo de configurações que contém parâmetros de linha de comando. Use *-i* no arquivo de configuração como um espaço reservado para uma lista de arquivos de entrada que podem ser colocados na linha de comando. No entanto, na linha de comando, você não precisa usar *i*. Você também pode usar caracteres curinga como \* . blg para especificar muitos nomes de arquivo de entrada. |
|                                             -Q                                             |                                                                                                                          Exibe os contadores de desempenho e intervalos de tempo dos arquivos de log especificados no arquivo de entrada.                                                                                                                           |
|                                             -y                                             |                                                                                                                                            Ignora a solicitação respondendo "Sim" a todas as perguntas.                                                                                                                                             |
|                                             /?                                             |                                                                                                                                                      Exibe a ajuda no prompt de comando.                                                                                                                                                      |

## <a name="remarks"></a>Comentários
Formato do caminho do contador:
- O formato geral para caminhos de contador é o seguinte: [ \\ \<computer> ] \\ \<Object> [ \<Parent> \\<instância # index>] \\ \<Counter> ] em que os componentes pai, instância, índice e contador do formato podem conter um nome válido ou um caractere curinga. Os componentes computador, pai, instância e índice não são necessários para todos os contadores.
- Você determina os caminhos do contador a serem usados com base no próprio contador. Por exemplo, o objeto LogicalDisk tem uma instância <Index> , portanto, você deve fornecer o < # index> ou um curinga. Portanto, você poderia usar o seguinte formato: **\LogicalDisk ( \* / \* # \* ) \\ \\ ***
- Em comparação, o objeto de processo não requer uma instância \<Index> . Portanto, você pode usar o seguinte formato: **\Process ( \* ) \ID Process**
- Se um caractere curinga for especificado no nome do pai, todas as instâncias do objeto especificado que correspondem à instância especificada e aos campos de contador serão retornados.
- Se um caractere curinga for especificado no nome da instância, todas as instâncias do objeto especificado e do objeto pai serão retornadas se todos os nomes de instância correspondentes ao índice especificado corresponderem ao caractere curinga.
- Se um caractere curinga for especificado no nome do contador, todos os contadores do objeto especificado serão retornados.
- Não há suporte para correspondências de cadeias de caracteres de caminho de contador parcial (por exemplo, pro *).

Arquivos do contador:
-   Os arquivos de contador são arquivos de texto que listam um ou mais dos contadores de desempenho no log existente. Copie o nome completo do contador do log ou a saída de **/q** no \<computer> \\ \<Object> \\ \<Instance> \\ \<Counter> formato. Liste um caminho de contador em cada linha.

Copiando contadores:
-   Quando executado, o **relog** copia os contadores especificados de cada registro no arquivo de entrada, convertendo o formato, se necessário. Caminhos curinga são permitidos no arquivo do contador.
Salvando subconjuntos de arquivos de entrada:
-   Use o parâmetro **/t** para especificar que os arquivos de entrada sejam inseridos em arquivos de saída em intervalos de cada <n> registro th. Por padrão, os dados são registrados novamente de cada registro.
Usando os parâmetros **/b** e **/e** com arquivos de log
-   Você pode especificar que os logs de saída incluem registros de antes do tempo de início (ou seja, **/b**) para fornecer dados para os contadores que exigem valores de computação do valor formatado. O arquivo de saída terá os últimos registros de arquivos de entrada com carimbos de data/hora menores do que o parâmetro **/e** (ou seja, hora de término).
Usando a opção **/config** :
-   O conteúdo do arquivo de configuração usado com a opção **/config** deve ter o seguinte formato:
    -   \<CommandOption>\\\<Value>, em que \<CommandOption> é uma opção de linha de comando e \<Value> especifica seu valor.

Para obter mais informações sobre como incorporar o **relog** em seus scripts de instrumentação de gerenciamento do Windows (WMI), consulte "scripting WMI" no [site Microsoft Windows Resource Kits](https://go.microsoft.com/fwlink/?LinkId=4665).

## <a name="examples"></a>Exemplos
Para reamostrar os logs de rastreamento existentes em intervalos fixos de 30, listar caminhos de contador, arquivos de saída e formatos:
```
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv
```
Para reamostrar os logs de rastreamento existentes em intervalos fixos de 30, listar caminhos do contador e arquivo de saída:
```
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30
```
Para reamostrar os logs de rastreamento existentes em um banco de dados, use:
```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
