---
title: wbadmin delete systemstatebackup
description: Artigo de referência para o comando Wbadmin Delete systemstatebackup, que exclui os backups de estado do sistema que você especificar.
ms.topic: reference
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 05ce1e2deb8a2466df70b56a43c4532871d491b9
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156074"
---
# <a name="wbadmin-delete-systemstatebackup"></a>wbadmin delete systemstatebackup

Exclui os backups de estado do sistema que você especificar. Se o volume especificado contiver backups diferentes dos backups de estado do sistema do servidor local, esses backups não serão excluídos.

Para excluir um backup de estado do sistema usando esse comando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

> [!NOTE]
> Backup do Windows Server não faz backup ou recupera hives de usuário do registro (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou da recuperação do estado do sistema.

## <a name="syntax"></a>Syntax

```
wbadmin delete systemstatebackup {-keepVersions:<numberofcopies> | -version:<versionidentifier> | -deleteoldest} [-backupTarget:<volumename>] [-machine:<backupmachinename>] [-quiet]
```

> [!IMPORTANT]
> Você deve especificar apenas um destes parâmetros: **-keepVersions**, **-version**ou **-deleteOldest**.

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -keepVersions | Especifica o número de backups de estado do sistema mais recentes a serem mantidos. O valor deve ser um inteiro positivo. O valor do parâmetro **-keepversions: 0** exclui todos os backups de estado do sistema. |
| -version | Especifica o identificador de versão do backup no formato MM/DD/AAAA-HH: MM. Se você não souber o identificador de versão, execute o comando [Wbadmin Get Versions](wbadmin-get-versions.md) .<p>As versões feitas de backups de estado do sistema exclusivamente podem ser excluídas usando esse comando. Execute o comando [Wbadmin Get Items](wbadmin-get-items.md) para exibir o tipo de versão. |
| -deleteOldest | Exclui o backup de estado do sistema mais antigo. |
| -backupTarget | Especifica o local de armazenamento do backup que você deseja excluir. O local de armazenamento para backups de disco pode ser uma letra de unidade, um ponto de montagem ou um caminho de volume baseado em GUID. Esse valor precisa ser especificado apenas para localizar backups que não estão no computador local. As informações sobre backups do computador local estão disponíveis no catálogo de backup no computador local. |
| -computador | Especifica o computador cujo backup de estado do sistema você deseja excluir. Útil quando é feito o backup de vários computadores no mesmo local. Deve ser usado quando o parâmetro **-backupTarget** é especificado. |
| -quiet | Executa o comando sem prompts para o usuário. |

## <a name="examples"></a>Exemplos

Para excluir o backup de estado do sistema criado em 31 de março de 2013 às 10:00, digite:

```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```

Para excluir todos os backups de estado do sistema, exceto os três mais recentes, digite:

```
wbadmin delete systemstatebackup -keepVersions:3
```

Para excluir o backup de estado do sistema mais antigo armazenado no disco f:, digite:

```
wbadmin delete systemstatebackup -backupTarget:f:\ -deleteOldest
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin Get Versions](wbadmin-get-versions.md)

- [comando Wbadmin Get Items](wbadmin-get-items.md)
