---
title: Cmd
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 032fbea2039faa09753ac0c2b51e4b62004d36ac
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379339"
---
# <a name="cmd"></a>Cmd

Inicia uma nova instância do interpretador de comando, cmd. exe. Se usado sem parâmetros, o **cmd** exibirá a versão e as informações de direitos autorais do sistema operacional.

## <a name="syntax"></a>Sintaxe

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<B><F>|<F>}] [/e:{on|off}] [/f:{on|off}] [/v:{on|off}] [<String>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/c|Executa o comando especificado pela *cadeia de caracteres* e, em seguida, para.|
|/k|Executa o comando especificado por *cadeia de caracteres* e continua.|
|/s|Modifica o tratamento da *cadeia de caracteres* após **/c** ou **/k**.|
|/q|Desativa o eco.|
|/d|Desabilita a execução de comandos de AutoRun.|
|/a|Formata a saída de comando interno para um pipe ou um arquivo como American National Standards Institute (ANSI).|
|/u|Formata a saída de comando interno para um pipe ou um arquivo como Unicode.|
|/t: {\<B\>\<F\>\|\<F\>}|Define as cores de plano de fundo (*B*) e de primeiro plano (*F*).|
|/e: on|Habilita extensões de comando.|
|/e: desativado|Desabilita extensões de comandos.|
|/f: ativado|Habilita a conclusão do nome de arquivo e diretório.|
|/f: desativado|Desabilita a conclusão do nome de arquivo e diretório.|
|/v: ativado|Habilita a expansão de variável de ambiente atrasada.|
|/v: desativado|Desabilita a expansão da variável de ambiente atrasada.|
|Cadeia de caracteres \<>|Especifica o comando que você deseja executar.|
|/?|Exibe a ajuda no prompt de comando.|

A tabela a seguir lista os dígitos hexadecimais válidos que você pode usar como valores para \<B\> e \<F\>

|{1&gt;Valor&lt;1}|Cor|
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
|a|Verde-claro|
|b|Azul-claro|
|c|Vermelho-claro|
|d|Roxo-claro|
|e|Amarelo-claro|
|f|Branco brilhante|

## <a name="remarks"></a>Comentários

-   Usando vários comandos

    Para usar vários comandos para \<cadeia de caracteres >, separe-os pelo separador de comandos **&&** e coloque-os entre aspas. Por exemplo:

    ```
    "<Command>&&<Command>&&<Command>"
    ``` 
 
-   Aspas de processamento

    Se você especificar **/c** ou **/k**, **cmd** processará o restante da *cadeia de caracteres* e as aspas serão preservadas somente se todas as condições a seguir forem atendidas:  
    -   Você não usa **/s**.
    -   Você usa exatamente um conjunto de aspas.
    -   Você não usa nenhum caractere especial dentro das aspas (por exemplo: & < > () @ ^ |).
    -   Você usa um ou mais caracteres de espaço em branco dentro das aspas.
    -   A *cadeia de caracteres* entre aspas é o nome de um arquivo executável.

    Se as condições anteriores não forem atendidas, a *cadeia de caracteres* será processada examinando o primeiro caractere para verificar se é uma aspa de abertura. Se o primeiro caractere for uma aspa de abertura, ele será retirado junto com as aspas de fechamento. Qualquer texto após as aspas de fechamento é preservado.
-   Executando subchaves do registro

    Se você não especificar **/d** na *cadeia de caracteres*, o cmd. exe procurará as seguintes subchaves do registro:

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\AutoRun\ REG_SZ**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\AutoRun\ REG_EXPAND_SZ**

    Se uma ou ambas as subchaves do registro estiverem presentes, elas serão executadas antes de todas as outras variáveis.

> [!CAUTION]
> A edição correta do registro pode danificar gravemente o seu sistema. Antes de fazer mudanças no registro, você deve fazer o backup de quaisquer dados importantes no computador.

-   Habilitando e desabilitando extensões de comando

    As extensões de comando são habilitadas por padrão no Windows XP. Você pode desabilitá-los para um processo específico usando **/e: off**. Você pode habilitar ou desabilitar extensões para todas as opções de linha de comando **cmd** em uma sessão de computador ou de usuário definindo os seguintes valores de **REG_DWORD** :

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    Defina o valor de **REG_DWORD** como **0 × 1** (habilitado) ou **0 × 0** (desabilitado) no registro usando Regedit. exe. As configurações especificadas pelo usuário têm precedência sobre as configurações do computador, e as opções de linha de comando têm precedência sobre as configurações do registro.

> [!CAUTION]
> A edição correta do registro pode danificar gravemente o seu sistema. Antes de fazer mudanças no registro, você deve fazer o backup de quaisquer dados importantes no computador.

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

-   Habilitando expansão de variável de ambiente atrasada

    Se você habilitar a expansão de variável de ambiente atrasada, poderá usar o caractere de ponto de exclamação para substituir o valor de uma variável de ambiente em tempo de execução.
-   Habilitando a conclusão do nome de arquivo e diretório

    A conclusão do nome de arquivo e diretório não está habilitada por padrão. Você pode habilitar ou desabilitar a conclusão do nome de arquivo para um processo específico do comando **cmd** com **/f:** {**on**|**off**}. Você pode habilitar ou desabilitar a conclusão de nome de arquivo e diretório para todos os processos do comando **cmd** em um computador ou para uma sessão de logon de usuário definindo os seguintes valores de **REG_DWORD** :

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    Para definir o valor de **REG_DWORD** , execute regedit. exe e use o valor hexadecimal de um caractere de controle para uma função específica (por exemplo, **0 × 9** é Tab e **0 × 08** é Backspace). As configurações especificadas pelo usuário têm precedência sobre as configurações do computador, e as opções de linha de comando têm precedência sobre as configurações do registro.

> [!CAUTION]
> A edição correta do registro pode danificar gravemente o seu sistema. Antes de fazer mudanças no registro, você deve fazer o backup de quaisquer dados importantes no computador.

Se você habilitar a conclusão de nome de arquivo e diretório usando **/f: on**, use Ctrl + D para a conclusão de nome de diretório e Ctrl + f para a conclusão do nome de arquivo. Para desabilitar um caractere de conclusão específico no registro, use o valor para espaço em branco [**0 × 20**] porque ele não é um caractere de controle válido.

Quando você pressiona CTRL + D ou CTRL + F, o **cmd** processa a conclusão do nome do arquivo e do diretório. Essas funções de combinação de teclas anexam um caractere curinga à *cadeia de caracteres* (se não houver uma), criam uma lista de caminhos que correspondem e, em seguida, exibem o primeiro caminho correspondente. Se nenhum dos caminhos corresponder, a função de conclusão de nome de arquivo e diretório emitirá um alarme sonoro e não alterará a exibição. Para percorrer a lista de caminhos correspondentes, pressione CTRL + D ou CTRL + F repetidamente. Para percorrer a lista com versões anteriores, pressione a tecla SHIFT e CTRL + D ou CTRL + F simultaneamente. Para descartar a lista salva de caminhos correspondentes e gerar uma nova lista, edite a *cadeia de caracteres* e pressione CTRL + D ou CTRL + F. Se você alternar entre CTRL + D e CTRL + F, a lista salva de caminhos correspondentes será descartada e uma nova lista será gerada. A única diferença entre as combinações de teclas CTRL + D e CTRL + F é que CTRL + D só corresponde a nomes de diretório e CTRL + F corresponde a nomes de arquivos e diretórios. Se você usar a conclusão de nome de arquivo e diretório em qualquer um dos comandos de diretório internos (ou seja, **CD**, **MD**ou **RD**), a conclusão do diretório será assumida.

A conclusão de nome de arquivo e diretório processa corretamente os nomes de arquivo que contêm espaços em branco ou caracteres especiais, se você colocar o caminho de correspondência entre aspas.

Os seguintes caracteres especiais exigem aspas: & < > [] {} ^ =;! ' + ' ~ [espaço em branco].

Se as informações fornecidas contiverem espaços, use aspas ao contrário do texto (por exemplo, "nome do computador").

Se você processar a conclusão do nome do arquivo e do diretório de dentro da *cadeia de caracteres*, qualquer parte do *caminho* à direita do cursor será descartada (no ponto na *cadeia de caracteres* em que a conclusão foi processada).

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
