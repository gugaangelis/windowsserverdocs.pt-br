---
title: versões do Wbadmin get
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b986acc4-d083-4d32-9434-862314ed5e97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4ebbd0d78de0ffbff1ee8c658d6d9811b87df1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813527"
---
# <a name="wbadmin-get-versions"></a>versões do Wbadmin get



Lista detalhes sobre os backups disponíveis que são armazenados no computador local ou em outro computador. Quando esse subcomando for usado sem parâmetros, ele lista todos os backups do computador local, mesmo se esses backups não estão disponíveis. Os detalhes fornecidos para backup incluem o tempo de backup, o local de armazenamento de backup, o identificador de versão (necessário para o **wbadmin obter itens** subcomando e realizar recuperações) e o tipo de recuperações, você pode executar.

Para obter detalhes sobre os backups disponíveis usando este subcomando, você deve ser um membro do **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido delegado apropriado permissões. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um prompt de comando com privilégios elevados **Prompt de comando** e, em seguida, clique em **executar como administrador**.)

Para obter exemplos de como usar esse subcomando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
wbadmin get versions
[-backupTarget:{<BackupTargetLocation> | <NetworkSharePath>}]
[-machine:BackupMachineName]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-backupTarget|Especifica o local de armazenamento que contém os backups que você deseja que os detalhes para. Use para listar os backups armazenados nesse local de destino. Locais de destino de backup podem ser unidades de disco conectadas localmente, volumes, pastas compartilhadas remotas, mídia removível como unidades de DVD ou outra mídia óptica. Se **wbadmin obter versões** é executado no mesmo computador em que o backup foi criado, esse parâmetro não é necessário. No entanto, este parâmetro é necessário para obter informações sobre um backup criado em outro computador.|
|-machine|Especifica o computador que você deseja para detalhes de backup. Use quando backups de vários computadores são armazenados no mesmo local. Deve ser usado quando **- backupTarget** for especificado.|

## <a name="remarks"></a>Comentários

Para listar itens disponíveis para recuperação de um backup específico, use **wbadmin obter itens**.

## <a name="BKMK_examples"></a>Exemplos

Para ver uma lista de backups disponíveis armazenados no volume h, digite:
```
wbadmin get versions -backupTarget:h:
```
Para ver uma lista de backups disponíveis armazenados na pasta compartilhada remota \\ \\servername\share para o computador server01, digite:
```
wbadmin get versions -backupTarget:\\servername\share -machine:server01
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBBackupTarget](https://technet.microsoft.com/library/jj902447.aspx) cmdlet