---
title: Catálogo de restauração de WBADMIN
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5876a44b178025baac7ee5901cdc32c1b5d33dad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851707"
---
# <a name="wbadmin-restore-catalog"></a>Catálogo de restauração de WBADMIN



Recupera um catálogo de backup para o computador local de um local de armazenamento que você especificar.

Para recuperar um catálogo de backup com este subcomando, você deve ser um membro do **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um atalho de prompt de comando com privilégios elevados **Prompt de comando**e, em seguida, clique em **executar como administrador**.)

Para obter exemplos de como usar esse subcomando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
wbadmin restore catalog
-backupTarget:{<BackupDestinationVolume> | <NetworkShareHostingBackup>}
[-machine:<BackupMachineName>]
[-quiet]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-backupTarget|Especifica o local do catálogo de backup do sistema, como era o momento depois que o backup foi criado.|
|-machine|Especifica o nome do computador que você deseja recuperar para o catálogo de backup. Use quando backups de vários computadores foram armazenados no mesmo local. Deve ser usado quando **- backupTarget** for especificado.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="remarks"></a>Comentários

Se o local (disco, DVD ou pasta compartilhada remota) onde armazenar seus backups for danificado ou perdido e não pode ser usado para restaurar o catálogo de backup, use **wbadmin Excluir catálogo** para excluir o catálogo corrompido. Nesse caso, você deve criar um novo backup depois que o catálogo de backup é excluído.

## <a name="BKMK_examples"></a>Exemplos

Para restaurar um catálogo de um backup armazenado em disco d, digite:
```
wbadmin restore catalog -backupTarget:d
```
Para restaurar um catálogo de um backup armazenado na pasta compartilhada \\ \\servername\share de server01, digite:
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Restauração WBCatalog](https://technet.microsoft.com/library/jj902437.aspx) cmdlet