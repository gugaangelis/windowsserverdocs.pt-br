---
title: wbadmin restore catalog
description: Artigo de referência para Wbadmin restore catalog, que recupera um catálogo de backup para o computador local de um local de armazenamento que você especificar.
ms.topic: reference
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c551aa77ff98a66d3faacc3901806be8cc67b8f2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637602"
---
# <a name="wbadmin-restore-catalog"></a>wbadmin restore catalog

Recupera um catálogo de backup do computador local de um local de armazenamento que você especificar.

Para recuperar um catálogo de backup com este subcomando, você deve ser um membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-backupTarget|Especifica o local do catálogo de backup do sistema como ele estava no ponto em que o backup foi criado.|
|-computador|Especifica o nome do computador para o qual você deseja recuperar o catálogo de backup. Use quando os backups de vários computadores tiverem sido armazenados no mesmo local. Deve ser usado quando **-backupTarget** é especificado.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="remarks"></a>Comentários

Se o local (disco, DVD ou pasta compartilhada remota) em que você armazena seus backups estiver danificado ou perdido e não puder ser usado para restaurar o catálogo de backup, use **Wbadmin Delete Catalog** para excluir o catálogo corrompido. Nesse caso, você deve criar um novo backup depois que o catálogo de backup for excluído.

## <a name="examples"></a>Exemplos

Para restaurar um catálogo de um backup armazenado em disco d:, digite:
```
wbadmin restore catalog -backupTarget:d
```
Para restaurar um catálogo de um backup armazenado na pasta compartilhada \\ \\ servername\share de Server01, digite:
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Restore-WBCatalog](/powershell/module/windowserverbackup/?view=winserver2012r2-ps)
