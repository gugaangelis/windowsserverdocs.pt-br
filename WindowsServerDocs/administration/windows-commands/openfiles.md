---
title: openfiles
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0bec8cf64a3c7f261c792a07da603cba4366e1a7
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436433"
---
# <a name="openfiles"></a>openfiles



Permite que um administrador consultar, exibir ou desconectar arquivos e diretórios que foram abertos em um sistema. Também habilita ou desabilita o sinalizador global manter a lista de objetos do sistema.

Este tópico inclui informações sobre os comandos a seguir:
-   [OPENFILES /Disconnect](#BKMK_disconnect)
-   [openfiles /query](#BKMK_query)
-   [OPENFILES /local](#BKMK_local)

## <a name="BKMK_disconnect"></a>OPENFILES /Disconnect

Permite que um administrador desconecte arquivos e pastas que foram abertas remotamente por meio de uma pasta compartilhada.

### <a name="syntax"></a>Sintaxe

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

### <a name="parameters"></a>Parâmetros

|            Parâmetro             |                                                                                                                                 Descrição                                                                                                                                  |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s \<System>           | Especifica o sistema remoto para se conectar ao (por nome ou endereço IP). Não use barras invertidas. Se você não usar o **/s** opção, o comando é executada no computador local por padrão. Esse parâmetro se aplica a todos os arquivos e pastas que são especificadas no comando. |
|    /u [\<Domain>\]<UserName>     |                                                          Executa o comando usando as permissões da conta de usuário especificado. Se você não usar o **/u** opção, as permissões são usadas por padrão do sistema.                                                           |
|         /p [\<Password>]         |                                               Especifica a senha da conta de usuário que é especificada na **/u** opção. Se você não usar o **/p** opção, um prompt de senha é exibida quando o comando é executado.                                                |
|        /id \<OpenFileID>         |                                       Desconecta arquivos abertos pelo ID do arquivo especificado. O caractere curinga ( **&#42;** ) pode ser usado com esse parâmetro.</br>Observação: Você pode usar o **openfiles /query** comando para encontrar a ID do arquivo.                                       |
|         /a \<AccessedBy>         |                                                Desconecta todos os arquivos abertos associados com o nome de usuário que é especificado na *Acessado_por* parâmetro. O caractere curinga ( **&#42;** ) pode ser usado com esse parâmetro.                                                 |
| /o {leia \| gravar \| leitura/gravação} |                                               Desconecta todos os arquivos abertos com o valor especificado de modo de abertura. Valores válidos são de leitura, gravação ou leitura/gravação. O caractere curinga ( **&#42;** ) pode ser usado com esse parâmetro.                                                |
|         /op \<OpenFile>          |                                                           Desconecta todas as conexões de arquivo aberto que são criadas por um nome de arquivo abertos específicos. O caractere curinga ( **&#42;** ) pode ser usado com esse parâmetro.                                                           |
|                /?                |                                                                                                                     Exibe a ajuda no prompt de comando.                                                                                                                     |

### <a name="examples"></a>Exemplos

Para desconectar todos os arquivos abertos com o arquivo de ID 26843578, digite:
```
openfiles /disconnect /id 26843578
```
Para desconectar todos os arquivos abertos e acessados pelo usuário "hiropln" de diretórios, digite:
```
openfiles /disconnect /a hiropln
```
Para desconectar todos os arquivos abertos e diretórios com o modo de leitura/gravação, digite:
```
openfiles /disconnect /o read/write
```
Para desconectar o diretório com o nome de arquivo aberto "C:\TestShare\", independentemente de quem os está acessando, tipo:
```
openfiles /disconnect /a * /op "c:\testshare\"
```
Para desconectar todos os arquivos abertos no computador remoto "srvmain" que estão sendo acessados pelo usuário "hiropln", independentemente de sua identificação, digite:
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="BKMK_query"></a>openfiles /query

Consulta e exibe todos os arquivos abertos.

### <a name="syntax"></a>Sintaxe

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

### <a name="parameters"></a>Parâmetros

|          Parâmetro           |                                                                                                                                 Descrição                                                                                                                                  |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /s \<System>         | Especifica o sistema remoto para se conectar ao (por nome ou endereço IP). Não use barras invertidas. Se você não usar o **/s** opção, o comando é executada no computador local por padrão. Esse parâmetro se aplica a todos os arquivos e pastas que são especificadas no comando. |
|  /u [\<Domain>\]<UserName>   |                                                          Executa o comando usando as permissões da conta de usuário especificado. Se você não usar o **/u** opção, as permissões são usadas por padrão do sistema.                                                           |
|       /p [\<Password>]       |                                               Especifica a senha da conta de usuário que é especificada na **/u** opção. Se você não usar o **/p** opção, um prompt de senha é exibida quando o comando é executado.                                                |
| [/fo {TABLE \| LIST \| CSV}] |                             Exibe a saída no formato especificado. Os valores válidos para *formato* são:</br>TABELA:  Exibe a saída em uma tabela.</br>LISTA: Exibe a saída em uma lista.</br>CSV: Exibe a saída no formato de valores separados por vírgulas.                              |
|             /nh              |                                                                                Suprime o cabeçalho de coluna na saída. Válido somente quando o **/fo** parâmetro for definido como **tabela** ou **CSV**.                                                                                 |
|              /v              |                                                                                                       Especifica que informações detalhadas exibidas na saída.                                                                                                        |
|              /?              |                                                                                                                     Exibe a ajuda no prompt de comando.                                                                                                                     |

### <a name="examples"></a>Exemplos

Para consultar e exibir todos os arquivos abertos, digite:
```
openfiles /query
```
Para consultar e exibir todos os arquivos abertos em formato de tabela sem cabeçalhos, digite:
```
openfiles /query /fo table /nh
```
Para consultar e exibir todos os arquivos abertos em formato de lista com informações detalhadas, digite:
```
openfiles /query /fo list /v
```
Para consultar e exibir todos os arquivos abertos no sistema remoto "srvmain" usando as credenciais do usuário "hiropln" no domínio "maindom", digite:
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> Neste exemplo, a senha é fornecida na linha de comando. Para evitar que exibe a senha, deixe de fora o **/p** opção. Você será solicitado a senha, que não será refletido na tela.

## <a name="BKMK_local"></a>OPENFILES /local

Habilita ou desabilita o sinalizador global manter a lista de objetos do sistema. Se usado sem parâmetros, **openfiles /local** exibe o status atual do sinalizador global Manter lista de objetos.

### <a name="syntax"></a>Sintaxe

```
openfiles /local [on | off]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[em \| off]|Habilita ou desabilita o sinalizador global Manter lista de objetos, que rastreia identificadores de arquivos local do sistema.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários

-   Habilitar o sinalizador global manter a lista de objetos pode tornar o sistema mais lento.
-   As alterações feitas usando o **na** ou **off** opção não terão efeito até que você reinicie o sistema.

### <a name="examples"></a>Exemplos

Para verificar o status atual do sinalizador global Manter lista de objetos, digite:
```
openfiles /local
```
Por padrão, o sinalizador global manter a lista de objetos está desabilitado e a seguinte saída é exibida:
```
INFO: The system global flag 'maintain objects list' is currently disabled.
```
Para habilitar o sinalizador global manter a lista de objetos, digite:
```
openfiles /local on
```
A seguinte mensagem é exibida quando o sinalizador global estiver habilitado:
```
SUCCESS: The system global flag 'maintain objects list' is enabled.
         This will take effect after the system is restarted.
```
Para desabilitar o sinalizador global manter a lista de objetos, digite:
```
openfiles /local off
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)