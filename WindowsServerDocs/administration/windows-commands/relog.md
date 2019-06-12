---
title: relog
description: Saiba como extrair informações do contador de desempenho dos arquivos de log de coutner de desempenho.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7480f6c0-9953-4d70-9b1c-b27e09d8db13
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 51b01f80259a7b83e1999b47164108dbe174b887
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441898"
---
# <a name="relog"></a>relog

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Extrai os contadores de desempenho de logs do contador de desempenho para outros formatos, como texto-TSV (para texto delimitado por tabulação), texto CSV (para texto delimitado por vírgula), binário BIN ou SQL.   

## <a name="syntax"></a>Sintaxe  
```  
relog [<FileName> [<FileName> ...]] [/a] [/c <path> [<path> ...]] [/cf <FileName>] [/f  {bin|csv|tsv|SQL}] [/t <Value>] [/o {OutputFile|DSN!CounterLog}] [/b <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/e <M/D/YYYY> [[<HH>:] <MM>:] <SS>] [/config {<FileName>|i}] [/q]  
```  

### <a name="parameters"></a>Parâmetros  

|                                         Parâmetro                                          |                                                                                                                                                                  Descrição                                                                                                                                                                   |
|--------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                *FileName* [*FileName ...* ]                                 |                                                                                                                      Especifica o nome do caminho de um log de contador de desempenho existente. Você pode especificar vários arquivos de entrada.                                                                                                                      |
|                                             -a                                             |                                                                                                          Acrescenta o arquivo de saída em vez de substituir. Essa opção não se aplica ao formato SQL, onde o padrão é sempre a acrescentar.                                                                                                           |
|                                   -c *path* [*path ...* ]                                   |                                                       Especifica o caminho do contador de desempenho para fazer logon. Para especificar vários caminhos de contador, separe-as com um espaço e coloque os caminhos de contador entre aspas (por exemplo, **"** <em>caminho_do_contador_1</em> <em>caminho_do_contador_2</em> **"** )                                                       |
|                                       -cf *FileName*                                       |                                            Especifica o nome do caminho do arquivo de texto que lista os contadores de desempenho a serem incluídos em um arquivo relog. Use esta opção para caminhos de contador de lista em um arquivo de entrada, um por linha. Configuração padrão é que todos os contadores no arquivo de log original são registrados.                                            |
|                                  -f {bin\| csv\|tsv\|SQL}                                  |                                       Especifica o nome do caminho do arquivo de formato de saída. O formato padrão é **bin**. Para um banco de dados SQL, o arquivo de saída Especifica o *DSN! LogDoContador*. Você pode especificar o local do banco de dados usando o Gerenciador de ODBC para configurar o DSN (nome de sistema de banco de dados).                                        |
|                                         -t *valor*                                         |                                                                                                           Especifica intervalos de amostra em "*N*" registros. Inclui todos os pontos de dados enésimo no arquivo relog. O padrão é todos os pontos de dados.                                                                                                           |
| -o {*OutputFile* \| *"SQL:DSN! Counter_Log*} em que o DSN é um DSN ODMC definidos no sistema. |                                                   Especifica o nome do caminho do arquivo de saída ou banco de dados do SQL onde os contadores serão gravados. <br>Observação: Para as versões de 64 bits e 32 bits do Relog.exe, você precisa definir um DSN na fonte de dados ODBC (64 bits e 32 bits, respectivamente)                                                   |
|                          -b \<*M*/*D*/*YYYY*> [[*HH*:]*MM*:]*SS*                           |                                                                          Especifica hora de início para a cópia do primeiro registro do arquivo de entrada. Data e hora devem estar no seguinte formato exato <em>M</em> **/** <em>1!d</em> **/** <em>YYYYHH</em> **:** <em>MM</em> **:** <em>SS</em>.                                                                          |
|                          -eletrônico \< *M*/*1!d*/*aaaa*> [[*HH*:]*MM*:]*SS*                           |                                                                           Especifica a hora de término para a cópia do último registro do arquivo de entrada. Data e hora devem estar no seguinte formato exato <em>M</em> **/** <em>1!d</em> **/** <em>YYYYHH</em> **:** <em>MM</em> **:** <em>SS</em>.                                                                            |
|                                -config {*FileName* \| *i*}                                 | Especifica o nome do caminho do arquivo de configurações que contém parâmetros de linha de comando. Use *-i* no arquivo de configuração como um espaço reservado para uma lista de arquivos de entrada que pode ser colocado na linha de comando. Na linha de comando, no entanto, você não precisa usar *eu*. Você também pode usar curingas como \*. blg para especificar vários nomes de arquivo de entrada. |
|                                             -q                                             |                                                                                                                          Exibe os contadores de desempenho e intervalos de tempo de arquivos de log especificados no arquivo de entrada.                                                                                                                           |
|                                             -y                                             |                                                                                                                                            Ignora o prompt respondendo "Sim" para todas as perguntas.                                                                                                                                             |
|                                             /?                                             |                                                                                                                                                      Exibe a ajuda no prompt de comando.                                                                                                                                                      |

## <a name="remarks"></a>Comentários  
Formato de caminho de contador:  
- O formato geral de caminhos de contador é da seguinte maneira: [\\\<computador >] \\ \<objeto > [\<pai >\\< instância #Index >] \\ \< Contador >] no qual o pai, instância, índice e componentes de contador do formato podem conter um nome válido ou um caractere curinga. O computador, pai, instância e componentes do índice não são necessárias para todos os contadores.  
- Você a determinar os caminhos de contador para usar com base no próprio contador. Por exemplo, o objeto LogicalDisk tem uma instância <Index>, portanto, você deve fornecer o < #index > ou um caractere curinga. Portanto, você pode usar o seguinte formato: **\LogicalDisk (\*/\*#\*)\\\\** *  
- Em comparação, o objeto de processo não requer uma instância \<índice >. Portanto, você pode usar o seguinte formato: **\Process (\*) \ID processo**  
- Se um caractere curinga é especificado no nome do pai, todas as instâncias do objeto especificado que correspondem aos campos de contador e instância especificada serão retornadas.  
- Se for especificado um caractere curinga no nome da instância, todas as instâncias do objeto especificado e do objeto pai serão retornadas se o caractere curinga corresponderem a todos os nomes de instância correspondente ao índice especificado.  
- Se um caractere curinga é especificado no nome do contador, todos os contadores do objeto especificado são retornados.  
- Não há suporte para correspondências de cadeia de caracteres de caminho de contador parcial (por exemplo, pro *).  

Arquivos de contador:  
-   Contador são arquivos de texto que listam um ou mais dos contadores de desempenho no log existente. Copie o nome completo do contador do log ou o **/q** de saída na \<computador >\\\<objeto >\\\<instância >\\ \< Contador > formato. Liste um caminho de contador em cada linha.  

Copiando contadores:  
-   Quando executado, **relog** copia contadores de todos os registros especificados no arquivo de entrada, converter o formato, se necessário. Caminhos de curinga são permitidos no arquivo do contador.  
Salvando subconjuntos do arquivo de entrada:  
-   Use o **/t** arquivos em intervalos de saída do parâmetro para especificar que os arquivos de entrada são inseridos em cada <n>registro th. Por padrão, os dados são novamente registrados de todos os registros.  
Usando o **/b** e **/e** parâmetros com arquivos de log  
-   Você pode especificar que os logs de saída incluam registros antes da hora de início (ou seja, **/b**) para fornecer dados para contadores que exigem valores de computação do valor formatado. O arquivo de saída terá os últimos registros dos arquivos de entrada com carimbos de hora menor do que o **/e** (ou seja, hora de término) parâmetro.  
Usando o **/config** opção:  
-   O conteúdo do arquivo de configuração usado com o **/config** opção deve ter o seguinte formato:  
    -   \<CommandOption >\\\<valor >, onde \<CommandOption > é uma opção de linha de comando e \<valor > especifica seu valor.

Para obter mais informações sobre como incorporar **relog** em seus scripts do Windows Management Instrumentation (WMI), consulte "Scripts WMI" no [site do Microsoft Windows Resource Kits](https://go.microsoft.com/fwlink/?LinkId=4665).  

## <a name="BKMK_Examples"></a>Exemplos  
Criar nova amostra dos logs de rastreamento existentes a intervalos fixos de 30, lista de caminhos de contador, arquivos e formatos de saída:  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.csv /t 30 /f csv  
```  
Para criar nova amostra dos logs de rastreamento existentes a intervalos fixos de 30, listar caminhos de contador e o arquivo de saída:  
```  
relog c:\perflogs\daily_trace_log.blg /cf counter_file.txt /o c:\perflogs\reduced_log.blg /t 30  
```
Para criar nova amostra dos logs de rastreamento existentes em um banco de dados, use:
```
relog "c:\perflogs\daily_trace_log.blg" -f sql -o "SQL:sql2016x64odbc!counter_log"
```

## <a name="additional-references"></a>Referências adicionais  
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)  
  
<!---
-   The following is a list of the possible formats:  
    -   \<computer>\\\<Object>(\<Parent>/\<Instance#Index>)\<Counter>  
    -   \<computer>\<Object>(<Parent>/<Instance>)\\<Counter>  
    -   \\\\<computer>\\<Object>(<Instance#Index>)\\<Counter>  
    -   \\\\<computer>\\<Object>(<Instance>)\\<Counter>  
    -   \\\\<computer>\\<Object>\\<Counter>  
    -   \\<Object>(<Parent>/<Instance#Index>)\\<Counter>  
    -   \\<Object>(<Parent>/<Instance>)<Counter>  
    -   \\<Object>(<Instance#Index>)\\<Counter>  
    -   \\<Object>(<Instance>)\\<Counter>  
    -   \\<Object>\\<Counter>  
--->