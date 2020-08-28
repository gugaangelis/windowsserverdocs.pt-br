---
title: relog
description: Artigo de referência para o comando relog, que extrai informações do contador de desempenho dos arquivos de log do contador de desempenho.
ms.topic: reference
ms.assetid: 7480f6c0-9953-4d70-9b1c-b27e09d8db13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 563bd7a460ee8809ca4020f9a83f28df435127b8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027415"
---
# <a name="relog"></a>relog

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Extrai contadores de desempenho dos logs do contador de desempenho em outros formatos, como Text-TSV (para texto delimitado por tabulação), Text-CSV (para texto delimitado por vírgula), binary-BIN ou SQL.

>[!NOTE]
>Para obter mais informações sobre como incorporar o **relog** em seus scripts de instrumentação de gerenciamento do Windows (WMI), consulte o [blog de scripts](https://devblogs.microsoft.com/scripting/).

## <a name="syntax"></a>Sintaxe

```
relog [<filename> [<filename> ...]] [/a] [/c <path> [<path> ...]] [/cf <filename>] [/f  {bin|csv|tsv|SQL}] [/t <value>] [/o {outputfile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<filename>|i}] [/q]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `filename [filename ...]` | Especifica o nome de caminho de um log de contador de desempenho existente. Você pode especificar vários arquivos de entrada. |
| -a | Anexa o arquivo de saída em vez de substituir. Essa opção não se aplica ao formato SQL, em que o padrão é sempre acrescentar. |
| -c `path [path ...]` | Especifica o caminho do contador de desempenho para o log. Para especificar vários caminhos de contador, separe-os com um espaço e coloque os caminhos de contador entre aspas (por exemplo, `"path1 path2"` ). |
| -CF nome do arquivo | Especifica o nome do caminho do arquivo de texto que lista os contadores de desempenho a serem incluídos em um arquivo relog. Use esta opção para listar caminhos de contador em um arquivo de entrada, um por linha. A configuração padrão é que todos os contadores no arquivo de log original sejam registrados novamente. |
| -f `{bin | csv | tsv | SQL}` | Especifica o nome do caminho do formato de arquivo de saída. O formato padrão é **bin**. Para um banco de dados SQL, o arquivo de saída especifica o `DSN!CounterLog` . Você pode especificar o local do banco de dados usando o Gerenciador ODBC para configurar o DSN (nome do sistema de banco de dados). |
| valor-t | Especifica intervalos de amostra em *n* registros. Inclui todos os enésimo pontos de dados no arquivo relog. O padrão é todos os pontos de dados. |
| -o `{Outputfile | SQL:DSN!Counter_Log}` | Especifica o nome do caminho do arquivo de saída ou do banco de dados SQL em que os contadores serão gravados. <P>**Observação:** Para as versões de 64 bits e 32 bits do relog.exe, você deve definir um DSN na fonte de dados ODBC (64 bits e 32-bit respectivamente) no sistema. Use o driver ODBC "SQL Server" para definir um DSN. |
| -b `<M/D/YYYY> [[<HH>:]<MM>:]<SS>]` | Especifica a hora de início para copiar o primeiro registro do arquivo de entrada. A data e a hora devem estar nesse formato exato M/D/YYYYHH: MM: SS. |
| -e `<M/D/YYYY> [[<HH>:]<MM>:]<SS>]` | Especifica a hora de término para copiar o último registro do arquivo de entrada. A data e a hora devem estar nesse formato exato M/D/YYYYHH: MM: SS. |
| -configuração `{filename | i}` | Especifica o nome do caminho do arquivo de configurações que contém parâmetros de linha de comando. Se você estiver usando um arquivo de configuração, poderá usar **-i** como um espaço reservado para uma lista de arquivos de entrada que podem ser colocados na linha de comando. Se você estiver usando a linha de comando, não use **-i**. Você também pode usar caracteres curinga, como `*.blg` para especificar vários nomes de arquivo de entrada ao mesmo tempo. |
| -Q | Exibe os contadores de desempenho e intervalos de tempo dos arquivos de log especificados no arquivo de entrada. |
| -y | Ignora a solicitação respondendo "Sim" a todas as perguntas. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- O formato geral para caminhos de contador é o seguinte: `[\<computer>] \<object>[<parent>\<instance#index>] \<counter>]` onde os componentes pai, instância, índice e contador do formato podem conter um nome válido ou um caractere curinga. Os componentes computador, pai, instância e índice não são necessários para todos os contadores.

- Você determina os caminhos do contador a serem usados com base no próprio contador. Por exemplo, o objeto **LogicalDisk** tem uma instância `<index>` , portanto, você deve fornecer o `<#index>` curinga ou. Portanto, você pode usar o seguinte formato: `\LogicalDisk(*/*#*)\\*` .

- Em comparação, o objeto de **processo** não requer uma instância `<index>` . Portanto, você pode usar o seguinte formato: `\Process(*)\ID Process` .

- Se um caractere curinga for especificado no nome do **pai** , todas as instâncias do objeto especificado que correspondem à instância especificada e aos campos de contador serão retornados.

- Se um caractere curinga for especificado no nome da **instância** , todas as instâncias do objeto especificado e do objeto pai serão retornadas se todos os nomes de instância correspondentes ao índice especificado corresponderem ao caractere curinga.

- Se um caractere curinga for especificado no nome do **contador** , todos os contadores do objeto especificado serão retornados.

- As correspondências de cadeias de caracteres de caminho parcial do contador (por exemplo, pro *) não têm suporte.

- Os arquivos de contador são arquivos de texto que listam um ou mais dos contadores de desempenho no log existente. Copie o nome completo do contador do log ou a saída de **/q** no `<computer>\<object>\<instance>\<counter>` formato. Liste um caminho de contador em cada linha.

- Quando executado, o comando **relog** copia os contadores especificados de cada registro no arquivo de entrada, convertendo o formato, se necessário. Caminhos curinga são permitidos no arquivo do contador.

- Use o parâmetro **/t** para especificar que os arquivos de entrada sejam inseridos em arquivos de saída em intervalos de cada `nth` registro. Por padrão, os dados são registrados novamente de cada registro.

- Você pode especificar que os logs de saída incluem registros de antes da hora de início (ou seja, **/b**) para fornecer dados para os contadores que exigem valores de computação do valor formatado. O arquivo de saída terá os últimos registros de arquivos de entrada com carimbos de data/hora menores do que o parâmetro **/e** (ou seja, hora de término).

- O conteúdo do arquivo de configuração usado com a opção **/config** deve ter o seguinte formato: `<commandoption>\<value>` , em que `<commandoption>` é uma opção de linha de comando e `<value>` especifica seu valor.

##<a name="q-examples"></a>Exemplos de Q #

Para reamostrar os logs de rastreamento existentes em intervalos fixos de 30, listar caminhos de contador, arquivos de saída e formatos, digite:

```
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv
```

Para reamostrar os logs de rastreamento existentes em intervalos fixos de 30, listar caminhos de contador e arquivo de saída, digite:

```
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30
```

Para reamostrar os logs de rastreamento existentes em um banco de dados, digite:

```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
