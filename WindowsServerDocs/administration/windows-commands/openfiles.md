---
title: openfiles
description: Artigo de referência para o comando openfiles, que permite que um administrador consulte, exiba ou desconecte arquivos e diretórios que foram abertos em um sistema.
ms.topic: reference
ms.assetid: c3be561d-a11f-4bf1-9835-8e4e96fe98ec
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: eb7d67d082c948c5f91d7551033143a2aac79f2a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633683"
---
# <a name="openfiles"></a>openfiles

Permite que um administrador consulte, exiba ou desconecte arquivos e diretórios que foram abertos em um sistema. Esse comando também habilita ou desabilita o sinalizador global da **lista de objetos de manutenção** do sistema.

## <a name="openfiles-disconnect"></a>Openfiles/Disconnect

Permite que um administrador desconecte arquivos e pastas que foram abertos remotamente por meio de uma pasta compartilhada.

### <a name="syntax"></a>Sintaxe

```
openfiles /disconnect [/s <system> [/u [<domain>\]<username> [/p [<password>]]]] {[/id <openfileID>] | [/a <accessedby>] | [/o {read | write | read/write}]} [/op <openfile>]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /s `<system>` | Especifica o sistema remoto ao qual se conectar (por nome ou endereço IP). Não use barras invertidas. Se você não usar a opção **/s** , o comando será executado no computador local por padrão. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando. |
| /u `[<domain>\]<username>` | Executa o comando usando as permissões da conta de usuário especificada. Se você não usar a opção **/u** , as permissões do sistema serão usadas por padrão. |
| /p `[<password>]` | Especifica a senha da conta de usuário que é especificada na opção **/u** . Se você não usar a opção **/p** , um prompt de senha será exibido quando o comando for executado. |
| /ID `<openfileID>` | Desconecta arquivos abertos pela ID de arquivo especificada. Você pode usar o caractere curinga (**&#42;**) com esse parâmetro.<p>Observação: você pode usar o comando **Openfiles/Query** para localizar a ID do arquivo. |
| SRDF `<accessedby>` | Desconecta todos os arquivos abertos associados ao nome de usuário especificado no parâmetro *accessedby* . Você pode usar o caractere curinga (**&#42;**) com esse parâmetro. |
| /o `{read | write | read/write}` | Desconecta todos os arquivos abertos com o valor de modo aberto especificado. Os valores válidos são **leitura**, **gravação**ou **leitura/gravação**. Você pode usar o caractere curinga (**&#42;**) com esse parâmetro. |
| /Op `<openfile>` | Desconecta todas as conexões de arquivos abertas que são criadas por um nome de arquivo aberto específico. Você pode usar o caractere curinga (**&#42;**) com esse parâmetro. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para desconectar todos os arquivos abertos com a *ID de arquivo 26843578*, digite:

```
openfiles /disconnect /id 26843578
```

Para desconectar todos os arquivos e diretórios abertos acessados pelo usuário *hiropln*, digite:

```
openfiles /disconnect /a hiropln
```

Para desconectar todos os arquivos e diretórios abertos com o *modo de leitura/gravação*, digite:

```
openfiles /disconnect /o read/write
```

Para desconectar o diretório com o nome do arquivo aberto * C:\testshare \* , independentemente de quem o está acessando, digite:

```
openfiles /disconnect /a * /op c:\testshare\
```

Para desconectar todos os arquivos abertos no computador remoto *srvmain* que estão sendo acessados pelo usuário *hiropln*, independentemente de sua ID, digite:

```
openfiles /disconnect /s srvmain /u maindom\hiropln /id *
```

## <a name="openfiles-query"></a>Openfiles/Query

Consulta e exibe todos os arquivos abertos.

### <a name="syntax"></a>Sintaxe

```
openfiles /query [/s <system> [/u [<domain>\]<username> [/p [<password>]]]] [/fo {TABLE | LIST | CSV}] [/nh] [/v]
```

#### <a name="parameters"></a>Parâmetros


| Parâmetro | Descrição |
|--|--|
| /s `<system>` | Especifica o sistema remoto ao qual se conectar (por nome ou endereço IP). Não use barras invertidas. Se você não usar a opção **/s** , o comando será executado no computador local por padrão. Esse parâmetro se aplica a todos os arquivos e pastas especificados no comando. |
| /u `[<domain>\]<username>` | Executa o comando usando as permissões da conta de usuário especificada. Se você não usar a opção **/u** , as permissões do sistema serão usadas por padrão. |
| /p `[<password>]` | Especifica a senha da conta de usuário que é especificada na opção **/u** . Se você não usar a opção **/p** , um prompt de senha será exibido quando o comando for executado. |
| [/fo `{TABLE | LIST | CSV}` ] | Exibe a saída no formato especificado. Os valores válidos incluem:<ul><li>**Tabela** -exibe a saída em uma tabela.</li><li>**Lista** -exibe a saída em uma lista.</li><li>**CSV** -exibe a saída no formato CSV (valores separados por vírgula).</li></ul> |
| /NH | Suprime cabeçalhos de coluna na saída. Válido somente quando o parâmetro **/fo** é definido como **Table** ou **CSV**. |
| /v | Especifica que as informações detalhadas (detalhadas) sejam exibidas na saída. |
| /? | Exibe a ajuda no prompt de comando. |

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

Para consultar e exibir todos os arquivos abertos no sistema remoto *srvmain* usando as credenciais para o usuário *hiropln* no domínio *maindom* , digite:

```
openfiles /query /s srvmain /u maindom\hiropln /p p@ssW23
```

> [!NOTE]
> Neste exemplo, a senha é fornecida na linha de comando. Para evitar a exibição da senha, deixe a opção **/p** . Você será solicitado a fornecer a senha, que não será ecoada na tela.

## <a name="openfiles-local"></a>o Openfiles/local

Habilita ou desabilita o sinalizador global da **lista de objetos de manutenção** do sistema. Se usado sem parâmetros, o **Openfiles/local** exibe o status atual do sinalizador global **manter a lista de objetos** .

> [!NOTE]
> As alterações feitas usando a opção Ativar ou **desativar** não entram **em** vigor até que você reinicie o sistema. Habilitar o sinalizador global da **lista manter objetos** pode tornar o sistema mais lento.

### <a name="syntax"></a>Sintaxe

```
openfiles /local [on | off]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `[on | off]` | Habilita ou desabilita o sinalizador global da **lista de objetos de manutenção** do sistema, que controla os identificadores de arquivos locais. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para verificar o status atual do sinalizador global **manter a lista de objetos** , digite:

```
openfiles /local
```

Por padrão, o sinalizador global **manter a lista de objetos** está desabilitado e a seguinte mensagem é exibida, `INFO: The system global flag 'maintain objects list' is currently disabled.`

Para habilitar o sinalizador global **manter a lista de objetos** , digite:

```
openfiles /local on
```

A mensagem a seguir é exibida quando o sinalizador global está habilitado, `SUCCESS: The system global flag 'maintain objects list' is enabled. This will take effect after the system is restarted.`

Para desabilitar o sinalizador global **manter a lista de objetos** , digite:

```
openfiles /local off
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
