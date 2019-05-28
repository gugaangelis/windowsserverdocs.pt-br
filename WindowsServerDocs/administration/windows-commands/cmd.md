---
title: Cmd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d9b99dbe7e26190e87c5dfc9de29980b9cb2f43
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192591"
---
# <a name="cmd"></a>Cmd

Inicia uma nova instância do interpretador de comandos, Cmd.exe. Se usado sem parâmetros, **cmd** exibe as informações de versão e copyright do sistema operacional.

## <a name="syntax"></a>Sintaxe

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/c|Executa o comando especificado pelo *cadeia de caracteres* e, em seguida, para.|
|/k|Executa o comando especificado pelo *cadeia de caracteres* e continua.|
|/s|Modifica o tratamento de *cadeia de caracteres* após **/c** ou **/k**.|
|/q|Desativa o eco.|
|/d|Desabilita a execução de comandos de execução automática.|
|/a|Formata a saída do comando interno para um pipe ou um arquivo como ANSI American National Standards Institute ().|
|/u|Formata a saída do comando interno para um pipe ou um arquivo como Unicode.|
|/t:{\<B\>\<F\>\|\<F\>}|Define o plano de fundo (*B*) e de primeiro plano (*F*) cores.|
|/e:on|Permite que as extensões de comando.|
|/e:off|Desabilita extensões de comandos.|
|/f:on|Permite a conclusão de nome de arquivo e pasta.|
|/f:off|Desabilita a conclusão de nome de arquivo e pasta.|
|/v:on|Habilita atrasada expansão de variáveis de ambiente.|
|/v:off|Desabilita atrasada expansão de variáveis de ambiente.|
|\<cadeia de caracteres >|Especifica o comando que você quer realizar.|
|/?|Exibe a ajuda no prompt de comando.|

A tabela a seguir lista os dígitos hexadecimais válidos que podem ser usados como valores para \<B\> e \<F\>

|Valor|Cor|
|-----|-----|
|0|Preto|
|1|Azul|
|2|Verde|
|3|Aqua|
|4|Vermelho|
|5|Roxo|
|6|Amarelo|
|7|Branco|
|8|Cinza|
|9|Azul-claro|
|$nbsp;|Verde claro|
|b|Azul-piscina claro|
|c|Vermelho-claro|
|d.|Roxo-claro|
|e|Amarelo-claro|
|f|Branco brilhante|

## <a name="remarks"></a>Comentários

-   Usando vários comandos

    Para usar vários comandos para \<cadeia de caracteres >, separe-os pelo separador de comando **&&** e coloque-os entre aspas. Por exemplo:   
    ```
    "<Command>&&<Command>&&<Command>"
    ```  
-   Aspas de processamento

    Se você especificar **/c** ou **/k**, **cmd** processa o restante do *cadeia de caracteres,* e as aspas só serão preservadas se todas as das seguintes opções condições forem atendidas:  
    -   Você não usar **/s**.
    -   Use exatamente um conjunto de aspas.
    -   Você não usar caracteres especiais dentro das aspas (por exemplo: & () de < > @ ^ |).
    -   Você usar um ou mais caracteres de espaço em branco dentro das aspas.
    -   O *cadeia de caracteres* dentro das aspas é o nome de um arquivo executável.

    Se as condições anteriores não forem atendidas, *cadeia de caracteres* é processado, examinando o primeiro caractere para verificar se ele é uma aspa de abertura. Se o primeiro caractere for uma aspa de abertura, ela é removida, juntamente com a marca de aspas de fechamento. Qualquer texto após as aspas de fechamento é preservado.
-   Executando subchaves do registro

    Se você não especificar **/d** na *cadeia de caracteres*, Cmd.exe procura as seguintes subchaves do registro:

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\AutoRun\REG_SZ**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\AutoRun\REG_EXPAND_SZ**

    Se uma ou ambas as subchaves do registro estiverem presentes, eles serão executados antes de todas as outras variáveis.

> [!CAUTION]
> A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

-   Habilitando e desabilitando as extensões de comando

    As extensões de comando são habilitadas por padrão no Windows XP. Você pode desabilitá-los para um processo específico usando **/e: off**. Você pode habilitar ou desabilitar as extensões para todos os **cmd** opções de linha de comando em um computador ou sessão de usuário definindo o seguinte **REG_DWORD** valores:

    **Precedência Processor\EnableExtensions\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\EnableExtensions\REG_DWORD**

    Defina a **REG_DWORD** valor para um **0 × 1** (habilitado) ou **0 × 0** (desabilitada) no registro usando Regedit.exe. Configurações especificadas pelo usuário têm precedência sobre configurações de computador e opções de linha de comando têm precedência sobre as configurações do registro.

> [!CAUTION]
> A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

    When you enable command extensions, the following commands are affected:  
    -  **assoc**
    -  **call**
    -  **chdir (cd)**
    -  **color**
    -  **del (erase)**
    -  **endlocal**
    -  **for**
    -  **ftype**
    -  **goto**
    -  **if**
    -  **mkdir (md)**
    -  **popd**
    -  **prompt**
    -  **pushd**
    -  **set**
    -  **setlocal**
    -  **shift**
    -  **start** (also includes changes to external command processes)

-   Habilitando a expansão de variáveis de ambiente atrasada

    Se você habilitar a expansão de variáveis de ambiente atrasada, você pode usar o caractere de ponto de exclamação para substituir o valor de uma variável de ambiente em tempo de execução.
-   Habilitando a conclusão de nome de arquivo e pasta

    Conclusão de nome de arquivo e pasta não está habilitado por padrão. Você pode habilitar ou desabilitar a conclusão de nome de arquivo para um determinado processo do **cmd** comando **/f:** {**na**|**off**}. Você pode habilitar ou desabilitar a conclusão de nome de arquivo e pasta para todos os processos do **cmd** comando em um computador ou para uma sessão de logon do usuário definindo o seguinte **REG_DWORD** valores:

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_LOCAL_MACHINE\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\CompletionChar\REG_DWORD**

    **HKEY_CURRENT_USER\Software\Microsoft\Command Processor\PathCompletionChar\REG_DWORD**

    Para definir a **REG_DWORD** valor, execute Regedit.exe e use o valor hexadecimal de um caractere de controle para uma função específica (por exemplo, **0 × 9** é o guia e **0 × 08** é BACKSPACE). Configurações especificadas pelo usuário têm precedência sobre configurações de computador e opções de linha de comando têm precedência sobre as configurações do registro.

> [!CAUTION]
> A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

Se você habilitar a conclusão de nome de arquivo e pasta, usando **/f: em**, use CTRL + D para a conclusão de nome de diretório e CTRL + F para a conclusão de nome de arquivo. Para desabilitar um caractere de conclusão específico no registro, use o valor de espaço em branco [**0 × 20**] porque ele não é um caractere de controle válido.

Quando você pressiona CTRL + D ou CTRL + F **cmd** processa a conclusão de nome de arquivo e pasta. Essas funções de combinação de teclas acrescentam um caractere curinga *cadeia de caracteres* (se um não estiver presente), crie uma lista de caminhos que corresponder e, em seguida, exibir o primeiro caminho correspondente. Se nenhum dos caminhos corresponderem, a função de conclusão de nome de arquivo e diretório emite um aviso sonoro e não altera a exibição. Para percorrer a lista de caminhos correspondentes, pressione CTRL + D ou CTRL + F repetidamente. Para percorrer a lista de versões anteriores, pressione a tecla SHIFT e CTRL + D ou CTRL + F simultaneamente. Para descartar a lista salva de caminhos correspondentes e gerar uma nova lista, edite *cadeia de caracteres* e pressione CTRL + D ou CTRL + F. Se você alternar entre CTRL + D e CTRL + F, a lista salva de caminhos correspondentes será descartada e uma nova lista é gerada. A única diferença entre as combinações de teclas CTRL + D e CTRL + F é que CTRL + D corresponde apenas a nomes de diretório e CTRL + F corresponde aos nomes de arquivo e diretório. Se você usar a conclusão de nome de arquivo e pasta em qualquer um dos comandos internas do diretório (ou seja, **CD**, **MD**, ou **área de trabalho remota**), conclusão de pasta será assumido.

Conclusão de nome de arquivo e pasta processa corretamente os nomes de arquivo que contêm espaços em branco ou caracteres especiais, se você colocar o caminho correspondente entre aspas.

Os seguintes caracteres especiais precisam vir entre aspas: & < > [] {} ^ =;! '+', ~ [espaço em branco].

Se as informações que você fornece contiverem espaços, use aspas ao redor do texto (por exemplo, "nome do computador").

Se você processar a conclusão de nome de arquivo e pasta de dentro *cadeia de caracteres*, qualquer parte do *caminho* à direita do cursor será descartada (no ponto no *cadeia de caracteres* onde o conclusão foi processada).

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
