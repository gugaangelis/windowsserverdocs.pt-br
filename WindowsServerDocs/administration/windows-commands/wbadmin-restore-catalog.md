---
title: Wbadmin restore catalog
description: Tópico de referência para Wbadmin restore catalog, que recupera um catálogo de backup para o computador local de um local de armazenamento que você especificar.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce1e24a0-821d-4353-b09d-8f82c5c4ad56
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de9ce6b64f996e50fb85a8c612104bc6851ebdfd
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720137"
---
# <a name="wbadmin-restore-catalog"></a>Wbadmin restore catalog

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
Para restaurar um catálogo de um backup armazenado na pasta \\ \\compartilhada servername\share de Server01, digite:
```
wbadmin restore catalog -backupTarget:\\servername\share -machine:server01
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Restore-WBCatalog](https://technet.microsoft.com/library/jj902437.aspx)