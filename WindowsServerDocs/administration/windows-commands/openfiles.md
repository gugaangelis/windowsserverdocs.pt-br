---
title: openfiles
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 38b1d27b86551c6d4cd9e6b1ad87bfc0e8dd221d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372503"
---
# <a name="openfiles"></a>openfiles



Permite que um administrador consulte, exiba ou desconecte arquivos e diretórios que foram abertos em um sistema. Também habilita ou desabilita o sinalizador global da lista de objetos de manutenção do sistema.

Este tópico inclui informações sobre os seguintes comandos:
-   [Openfiles/Disconnect](#BKMK_disconnect)
-   [Openfiles/Query](#BKMK_query)
-   [o Openfiles/local](#BKMK_local)

## <a name="BKMK_disconnect"></a>Openfiles/Disconnect

Permite que um administrador desconecte arquivos e pastas que foram abertos remotamente por meio de uma pasta compartilhada.

### <a name="syntax"></a>Sintaxe

```
openfiles /disconnect [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/id <OpenFileID>] | [/a <AccessedBy>] | [/o {read | write | read/write}]} [/op <OpenFile>]
```

### <a name="parameters"></a>Parâmetros

|            Parâmetro             |                                                                                                                                 Descrição                                                                                                                                  |
|----------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|           /s \<System >           | Especifica o sistema remoto ao qual se conectar (por nome ou endereço IP). Não use barras invertidas. Se você não usar a opção **/s** , o comando será executado no computador local por padrão. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando. |
|    /u [\<Domain > \] @ no__t-2     |                                                          Executa o comando usando as permissões da conta de usuário especificada. Se você não usar a opção **/u** , as permissões do sistema serão usadas por padrão.                                                           |
|         /p [\<Password >]         |                                               Especifica a senha da conta de usuário que é especificada na opção **/u** . Se você não usar a opção **/p** , um prompt de senha será exibido quando o comando for executado.                                                |
|        /ID \<OpenFileID >         |                                       Desconecta arquivos abertos pela ID de arquivo especificada. O caractere curinga ( **&#42;** ) pode ser usado com esse parâmetro.</br>Observação: Você pode usar o comando **Openfiles/Query** para localizar a ID do arquivo.                                       |
|         /a \<AccessedBy >         |                                                Desconecta todos os arquivos abertos associados ao nome de usuário especificado no parâmetro *AccessedBy* . O caractere curinga ( **&#42;** ) pode ser usado com esse parâmetro.                                                 |
| /o {leitura \| gravação \| leitura/gravação} |                                               Desconecta todos os arquivos abertos com o valor de modo aberto especificado. Os valores válidos são leitura, gravação ou leitura/gravação. O caractere curinga ( **&#42;** ) pode ser usado com esse parâmetro.                                                |
|         /op \<OpenFile >          |                                                           Desconecta todas as conexões de arquivos abertas que são criadas por um nome de arquivo aberto específico. O caractere curinga ( **&#42;** ) pode ser usado com esse parâmetro.                                                           |
|                /?                |                                                                                                                     Exibe a ajuda no prompt de comando.                                                                                                                     |

### <a name="examples"></a>Exemplos

Para desconectar todos os arquivos abertos com a ID de arquivo 26843578, digite:
```
openfiles /disconnect /id 26843578
```
Para desconectar todos os arquivos e diretórios abertos acessados pelo usuário "hiropln", digite:
```
openfiles /disconnect /a hiropln
```
Para desconectar todos os arquivos e diretórios abertos com o modo de leitura/gravação, digite:
```
openfiles /disconnect /o read/write
```
Para desconectar o diretório com o nome de arquivo aberto "C:\TestShare @ no__t-0, independentemente de quem o está acessando, digite:
```
openfiles /disconnect /a * /op "c:\testshare\"
```
Para desconectar todos os arquivos abertos no computador remoto "srvmain" que estão sendo acessados pelo usuário "hiropln", independentemente de sua ID, digite:
```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="BKMK_query"></a>Openfiles/Query

Consulta e exibe todos os arquivos abertos.

### <a name="syntax"></a>Sintaxe

```
openfiles /query [/s <System> [/u [<Domain>\]<UserName> [/p [<Password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

### <a name="parameters"></a>Parâmetros

|          Parâmetro           |                                                                                                                                 Descrição                                                                                                                                  |
|------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         /s \<System >         | Especifica o sistema remoto ao qual se conectar (por nome ou endereço IP). Não use barras invertidas. Se você não usar a opção **/s** , o comando será executado no computador local por padrão. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando. |
|  /u [\<Domain > \] @ no__t-2   |                                                          Executa o comando usando as permissões da conta de usuário especificada. Se você não usar a opção **/u** , as permissões do sistema serão usadas por padrão.                                                           |
|       /p [\<Password >]       |                                               Especifica a senha da conta de usuário que é especificada na opção **/u** . Se você não usar a opção **/p** , um prompt de senha será exibido quando o comando for executado.                                                |
| [/FO {tabela \| lista \| CSV}] |                             Exibe a saída no formato especificado. Os valores válidos para *Format* são:</br>TABELA  Exibe a saída em uma tabela.</br>LISTA Exibe a saída em uma lista.</br>CSV Exibe a saída em formato de valores separados por vírgula.                              |
|             /NH              |                                                                                Suprime o cabeçalho da coluna na saída. Válido somente quando o parâmetro **/fo** é definido como **Table** ou **CSV**.                                                                                 |
|              /v              |                                                                                                       Especifica que informações detalhadas serão exibidas na saída.                                                                                                        |
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
Para consultar e exibir todos os arquivos abertos no sistema remoto "srvmain" usando as credenciais para o usuário "hiropln" no domínio "maindom", digite:
```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> Neste exemplo, a senha é fornecida na linha de comando. Para evitar a exibição da senha, deixe a opção **/p** . Você será solicitado a fornecer a senha, que não será ecoada na tela.

## <a name="BKMK_local"></a>o Openfiles/local

Habilita ou desabilita o sinalizador global da lista de objetos de manutenção do sistema. Se usado sem parâmetros, o **Openfiles/local** exibe o status atual do sinalizador global manter a lista de objetos.

### <a name="syntax"></a>Sintaxe

```
openfiles /local [on | off]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[on \| off]|Habilita ou desabilita o sinalizador global da lista de objetos de manutenção do sistema, que controla os identificadores de arquivos locais.|
|/?|Exibe a ajuda no prompt de comando.|

### <a name="remarks"></a>Comentários

-   Habilitar o sinalizador global da lista manter objetos pode tornar o sistema mais lento.
-   As alterações feitas usando a opção Ativar ou **desativar** não entram **em** vigor até que você reinicie o sistema.

### <a name="examples"></a>Exemplos

Para verificar o status atual do sinalizador global manter a lista de objetos, digite:
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
A seguinte mensagem é exibida quando o sinalizador global está habilitado:
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