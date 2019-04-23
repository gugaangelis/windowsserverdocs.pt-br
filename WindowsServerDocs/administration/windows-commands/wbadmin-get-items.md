---
title: Wbadmin get itens
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27d08ce3-6e06-4260-b264-fc1bde132d09
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eeb7c29ff552f968b4785612f626a86baf154ad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842877"
---
# <a name="wbadmin-get-items"></a>Wbadmin get itens



Lista os itens incluídos em um backup específico.

Para usar este subcomando, você deve ser um membro do **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um atalho de prompt de comando com privilégios elevados **Prompt de comando** e, em seguida, clique em **executar como administrador**.)

Para obter exemplos de como usar esse subcomando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
wbadmin get items
-version:<VersionIdentifier>
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-versão|Especifica a versão do backup em MM/DD/AAAA-formato hh: mm. Se você não souber as informações de versão, digite **wbadmin obter versões**.|
|-backupTarget|Especifica o local de armazenamento que contém os backups para o qual você deseja que os detalhes. Use para listar os backups armazenados nesse local de destino. Locais de destino de backup podem ser uma unidade de disco conectada localmente ou em uma pasta compartilhada remota. Se **wbadmin obter itens**é executado no mesmo computador em que o backup foi criado, esse parâmetro não é necessário. No entanto, este parâmetro é necessário para obter informações sobre um backup criado em outro computador.|
|-machine|Especifica o nome do computador que você deseja que os detalhes de backup para. Útil quando vários computadores tiverem sido copiados para o mesmo local. Deve ser usado quando **- backupTarget** for especificado.|

## <a name="BKMK_examples"></a>Exemplos

Para listar os itens do backup que foi executado em 31 de março de 2013 às 9h00, tipo:
```
wbadmin get items -version:03/31/2013-09:00
```
Para listar itens do backup de server01 que foi executado em 30 de abril de 2013 às 9h00 e armazenado no \\ \\servername\share, tipo:
```
wbadmin get items -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupSet](https://technet.microsoft.com/library/jj902473.aspx) cmdlet