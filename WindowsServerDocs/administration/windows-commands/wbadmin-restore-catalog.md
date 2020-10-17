---
title: wbadmin restore catalog
description: Artigo de referência para o comando Wbadmin restore catalog, que recupera um catálogo de backup para o computador local de um local de armazenamento que você especificar.
ms.topic: reference
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 046c4b4162c9512d5893c3c06e83e7260386c990
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92155864"
---
# <a name="wbadmin-restore-catalog"></a>wbadmin restore catalog

Recupera um catálogo de backup do computador local de um local de armazenamento que você especificar.

Para recuperar um catálogo de backup incluído em um backup específico usando esse comando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados, clicando com o botão direito do mouse em **prompt de comando**e selecionando **Executar como administrador**.

> [!NOTE]
> Se o local (disco, DVD ou pasta compartilhada remota) em que você armazena seus backups estiver danificado ou perdido e não puder ser usado para restaurar o catálogo de backup, execute o comando [Wbadmin Delete Catalog](wbadmin-delete-catalog.md) para excluir o catálogo corrompido. Nesse caso, é recomendável criar um novo backup após a exclusão do catálogo de backup.

## <a name="syntax"></a>Sintaxe

```
wbadmin restore catalog -backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>} [-machine:<BackupMachineName>] [-quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| -backupTarget | Especifica o local do catálogo de backup do sistema como ele estava no ponto em que o backup foi criado. |
| -computador | Especifica o nome do computador para o qual você deseja recuperar o catálogo de backup. Use quando os backups de vários computadores tiverem sido armazenados no mesmo local. Deve ser usado quando **-backupTarget** é especificado. |
| -quiet | Executa o comando sem prompts para o usuário. |

## <a name="examples"></a>Exemplos

Para restaurar um catálogo de um backup armazenado em disco D:, digite:

```
wbadmin restore catalog -backupTarget:D
```

Para restaurar um catálogo de um backup armazenado na pasta compartilhada `\\<servername>\<share>` do Server01, digite:

```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Wbadmin](wbadmin.md)

- [comando Wbadmin Delete Catalog](wbadmin-delete-catalog.md)

- [Restore-WBCatalog](/powershell/module/windowserverbackup/Restore-WBCatalog)
