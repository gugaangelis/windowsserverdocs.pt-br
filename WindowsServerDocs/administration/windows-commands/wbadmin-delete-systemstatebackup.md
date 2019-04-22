---
title: Wbadmin delete systemstatebackup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 707d37cb-448d-4542-b6ac-1fc89e749788
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6801ca5985af626ccb7f6170fbcd6f8fc8305ba1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813147"
---
# <a name="wbadmin-delete-systemstatebackup"></a>Wbadmin delete systemstatebackup



Exclui os backups de estado do sistema que você especificar. Se o volume especificado contém backups que não sejam backups de estado do sistema do seu servidor local, esses backups não serão excluídos.

> [!NOTE]
> Backup do Windows Server não fazer backup ou recuperação hives do registro de usuário (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou recuperação de estado do sistema.

Para excluir um backup de estado do sistema com essa subcomando, você deve ser um membro do **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um atalho de prompt de comando com privilégios elevados **Prompt de comando**e, em seguida, clique em **executar como administrador**.)

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
wbadmin delete systemstatebackup
{-keepVersions:<NumberofCopies> | -version:<VersionIdentifier> | -deleteOldest}
[-backupTarget:<VolumeName>]
[-machine:<BackupMachineName>]
[-quiet]
```

> [!IMPORTANT]
> Apenas um desses parâmetros devem ser especificadas: **- keepVersions**, **-versão**, ou **- deleteOldest**.

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-keepVersions|Especifica o número dos backups de estado do sistema mais recentes para manter. O valor deve ser um inteiro positivo. O valor do parâmetro **- keepVersions: 0** exclui todos os backups de estado do sistema.|
|-versão|Especifica o identificador de versão do backup em MM/DD/AAAA-formato hh: mm. Se você não souber o identificador de versão, digite **wbadmin obter versões**.</br>As versões que são exclusivamente os backups de estado do sistema podem ser excluídas usando este comando. Use **wbadmin obter itens** para exibir o tipo de versão.|
|-deleteOldest|Exclui o backup de estado do sistema mais antigo.|
|-backupTarget|Especifica o local de armazenamento para o backup que você deseja excluir. O local de armazenamento para backups de discos pode ser uma letra de unidade, um ponto de montagem ou um caminho de volume baseada em GUID. Esse valor só precisa ser especificado para a localização de backups que não são do computador local. Informações sobre backups do computador local estarão disponíveis no catálogo de backup no computador local.|
|-machine|Especifica o computador cujo backup de estado do sistema que deseja excluir. Útil quando vários computadores foram feitos no mesmo local. Deve ser usado quando o **- backupTarget** parâmetro for especificado.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="BKMK_examples"></a>Exemplos

Para excluir o backup de estado do sistema criado em 31 de março de 2013 às 10H00, digite:
```
wbadmin delete systemstatebackup -version:03/31/2013-10:00
```
Para excluir todos os backups de estado do sistema, exceto os três mais recentes, digite:
```
wbadmin delete systemstatebackup -keepVersions:3
```
Para excluir o backup de estado do sistema mais antigo armazenado no disco f, digite:
```
wbadmin delete systemstatebackup -backupTarget:f -deleteOldest
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)