---
title: cmd
description: Artigo de referência para o comando cmd, que inicia uma nova instância do interpretador de comando, Cmd.exe.
ms.topic: reference
ms.assetid: 6ec588db-31a9-4a73-a970-65a2c6f4abbe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b782a93d4c61f43bbe45497871fe66f29ef972a4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030974"
---
# <a name="cmd"></a>cmd

Inicia uma nova instância do interpretador de comando, Cmd.exe. Se usado sem parâmetros, o **cmd** exibirá a versão e as informações de direitos autorais do sistema operacional.

## <a name="syntax"></a>Sintaxe

```
cmd [/c|/k] [/s] [/q] [/d] [/a|/u] [/t:{<b><f> | <f>}] [/e:{on | off}] [/f:{on | off}] [/v:{on | off}] [<string>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /c | Executa o comando especificado pela *cadeia de caracteres* e, em seguida, para. |
| /k | Executa o comando especificado por *cadeia de caracteres* e continua. |
| /s | Modifica o tratamento da *cadeia de caracteres* após **/c** ou **/k**. |
| /q | Desativa o eco. |
| /d | Desabilita a execução de comandos de AutoRun. |
| /a | Formata a saída de comando interno para um pipe ou um arquivo como American National Standards Institute (ANSI). |
| /u | Formata a saída de comando interno para um pipe ou um arquivo como Unicode. |
| /t: {`<b><f>` | `<f>`} | Define as cores de plano de fundo (*b*) e de primeiro plano (*f*). |
| /e: on | Habilita extensões de comando. |
| /e: desativado | Desabilita extensões de comandos. |
| /f: ativado | Habilita a conclusão do nome de arquivo e diretório. |
| /f: desativado | Desabilita a conclusão do nome de arquivo e diretório. |
| /v: ativado | Habilita a expansão de variável de ambiente atrasada. |
| /v: desativado | Desabilita a expansão da variável de ambiente atrasada. |
| `<string>` | Especifica o comando que você deseja executar. |
| /? | Exibe a ajuda no prompt de comando. |

A tabela a seguir lista os dígitos hexadecimais válidos que você pode usar como valores para `<b>` e `<f>` :

| Valor | Cor |
| ----- | ----- |
| 0 | Preto |
| 1 | Azul |
| 2 | Verde |
| 3 | Aqua |
| 4 | Vermelho |
| 5 | Roxo |
| 6 | Amarelo |
| 7 | Branco |
| 8 | Cinza |
| 9 | Azul-claro |
| um | verde-claro |
| b | Azul-claro |
| c | Vermelho-claro |
| d | Roxo-claro |
| e | Amarelo-claro |
| f | Branco brilhante |

## <a name="remarks"></a>Comentários

- Para usar vários comandos para `<string>` , separe-os pelo separador de comandos **&&** e coloque-os entre aspas. Por exemplo:

    ```
    "<command1>&&<command2>&&<command3>"
    ```

- Se você especificar **/c** ou **/k**, os processos **cmd** , o restante da *cadeia de caracteres*e as aspas serão preservadas somente se todas as condições a seguir forem atendidas:

    - Você também não usa **/s**.

    - Você usa exatamente um conjunto de aspas.

    - Você não usa nenhum caractere especial dentro das aspas (por exemplo:  & < >  () @ ^ |).

    - Você usa um ou mais caracteres de espaço em branco dentro das aspas.

    - A *cadeia de caracteres* entre aspas é o nome de um arquivo executável.

    Se as condições anteriores não forem atendidas, a *cadeia de caracteres* será processada examinando o primeiro caractere para verificar se é uma aspa de abertura. Se o primeiro caractere for uma aspa de abertura, ele será retirado junto com as aspas de fechamento. Qualquer texto após as aspas de fechamento é preservado.

- Se você não especificar **/d** na *cadeia de caracteres*, Cmd.exe procurará as seguintes subchaves do registro:

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\AutoRun\ REG_SZ**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\AutoRun\ REG_EXPAND_SZ**

    Se uma ou ambas as subchaves do registro estiverem presentes, elas serão executadas antes de todas as outras variáveis.

    > [!CAUTION]
    > A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

- Você pode desabilitar as extensões de comando para um processo específico usando **/e: off**. Você pode habilitar ou desabilitar extensões para todas as opções de linha de comando **cmd** em uma sessão de computador ou de usuário definindo os seguintes valores de **REG_DWORD** :

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\EnableExtensions\ REG_DWORD**

    Defina o valor de **REG_DWORD** como **0 × 1** (habilitado) ou **0 × 0** (desabilitado) no registro usando Regedit.exe. As configurações especificadas pelo usuário têm precedência sobre as configurações do computador, e as opções de linha de comando têm precedência sobre as configurações do registro.

    > [!CAUTION]
    > A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

    Quando você habilita extensões de comando, os seguintes comandos são afetados:
    - **assoc**

    - **call**

    - **chdir (CD)**

    - **color**

    - **del (apagar)**

    - **endlocal**

    - **for**

    - **ftype**

    - **goto**

    - **if**

    - **mkdir (MD)**

    - **popd**

    - **prompt**

    - **pushd**

    - **set**

    - **setlocal**

    - **shift**

    - **Iniciar** (também inclui alterações em processos de comando externo)

- Se você habilitar a expansão de variável de ambiente atrasada, poderá usar o caractere de ponto de exclamação para substituir o valor de uma variável de ambiente em tempo de execução.

- A conclusão do nome de arquivo e diretório não está habilitada por padrão. Você pode habilitar ou desabilitar a conclusão do nome de arquivo para um processo específico do comando **cmd** com **/f:**{**on**  |  **off**}. Você pode habilitar ou desabilitar a conclusão de nome de arquivo e diretório para todos os processos do comando **cmd** em um computador ou para uma sessão de logon de usuário definindo os seguintes valores de **REG_DWORD** :

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    - **HKEY_LOCAL_MACHINE \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\CompletionChar\ REG_DWORD**

    - **HKEY_CURRENT_USER \Software\Microsoft\Command Processor\PathCompletionChar\ REG_DWORD**

    Para definir o valor de **REG_DWORD** , execute Regedit.exe e use o valor hexadecimal de um caractere de controle para uma função específica (por exemplo, **0 × 9** é Tab e **0 × 08** é Backspace). As configurações especificadas pelo usuário têm precedência sobre as configurações do computador, e as opções de linha de comando têm precedência sobre as configurações do registro.

    > [!CAUTION]
    > A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.

- Se você habilitar a conclusão de nome de arquivo e diretório usando **/f: on**, use **Ctrl + D** para a conclusão de nome de diretório e **Ctrl + f** para a conclusão do nome de arquivo. Para desabilitar um caractere de conclusão específico no registro, use o valor para espaço em branco [**0 × 20**] porque ele não é um caractere de controle válido.

  - Pressionar **Ctrl + D** ou **Ctrl + F**processa a conclusão do nome do arquivo e do diretório. Essas funções de combinação de teclas anexam um caractere curinga à *cadeia de caracteres* (se não houver uma), o cria uma lista de caminhos que correspondem e, em seguida, exibe o primeiro caminho correspondente.<p>Se nenhum dos caminhos corresponder, a função de conclusão de nome de arquivo e diretório emitirá um alarme sonoro e não alterará a exibição. Para percorrer a lista de caminhos correspondentes, pressione **Ctrl + D** ou **Ctrl + F** repetidamente. Para percorrer a lista com versões anteriores, pressione a tecla **Shift** e **Ctrl + D** ou **Ctrl + F** simultaneamente. Para descartar a lista salva de caminhos correspondentes e gerar uma nova lista, edite a *cadeia de caracteres* e pressione **Ctrl + D** ou **Ctrl + F**. Se você alternar entre **Ctrl + D** e **Ctrl + F**, a lista salva de caminhos correspondentes será descartada e uma nova lista será gerada. A única diferença entre as combinações de teclas **Ctrl + d** e **Ctrl + f** é que **Ctrl + d** só corresponde a nomes de diretório e **Ctrl + f** corresponde a nomes de arquivos e diretórios. Se você usar a conclusão de nome de arquivo e diretório em qualquer um dos comandos de diretório internos (ou seja, **CD**, **MD**ou **RD**), a conclusão do diretório será assumida.

  - A conclusão de nome de arquivo e diretório processa corretamente os nomes de arquivo que contêm espaços em branco ou caracteres especiais, se você colocar o caminho de correspondência entre aspas.

  - Você deve usar aspas ao contrário dos seguintes caracteres especiais:  & < >  [] {} ^ =;! ' + ' ~ [espaço em branco].

  - Se as informações fornecidas contiverem espaços, você deverá usar aspas em volta do texto (por exemplo, "nome do computador").

  - Se você processar a conclusão do nome do arquivo e do diretório de dentro da *cadeia de caracteres*, qualquer parte do *caminho* à direita do cursor será descartada (no ponto na *cadeia de caracteres* em que a conclusão foi processada).

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
