---
title: Wbadmin start systemstaterecovery
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 208b1be9-3452-4aba-bb49-46bc587fca96
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4282da2011c39daec0315a7f3836d5517f29debb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440199"
---
# <a name="wbadmin-start-systemstaterecovery"></a>Wbadmin start systemstaterecovery



Executa uma recuperação de estado do sistema para um local e de um backup que você especificar.

> [!NOTE]
> Backup do Windows Server não fazer backup ou recuperação hives do registro de usuário (HKEY_CURRENT_USER) como parte do backup do estado do sistema ou recuperação de estado do sistema.

Para executar uma recuperação de estado do sistema com essa subcomando, você deve ser um membro dos **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um atalho de prompt de comando com privilégios elevados **Prompt de comando**e, em seguida, clique em **executar como administrador**.)

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

Sintaxe do Windows Server 2008:
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-quiet]
```
Sintaxe para o Windows Server 2008 R2 ou posterior:
```
wbadmin start systemstaterecovery
-version:<VersionIdentifier>
-showsummary
[-backupTarget:{<BackupDestinationVolume> | <NetworkSharePath>}]
[-machine:<BackupMachineName>]
[-recoveryTarget:<TargetPathForRecovery>]
[-authsysvol]
[-autoReboot]
[-quiet]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-versão|Especifica o identificador de versão para o backup recuperar no MM/DD/AAAA-formato hh: mm. Se você não souber o identificador de versão, digite **wbadmin obter versões**.|
|-showsummary|Informa o resumo da última recuperação de estado do sistema (após o reinício necessário para concluir a operação). Esse parâmetro não pode ser acompanhado por quaisquer outros parâmetros.|
|-backupTarget|Especifica o local de armazenamento que contém o backup ou backups que você deseja recuperar. Esse parâmetro é útil quando o local de armazenamento é diferente de onde os backups desse computador geralmente são armazenados.|
|-machine|Especifica o nome do computador que você deseja recuperar. Esse parâmetro é útil quando vários computadores tiverem sido copiados para o mesmo local. Deve ser usado quando o **- backupTarget** parâmetro for especificado.|
|-recoveryTarget|Especifica o diretório para restaurar para. Esse parâmetro é útil se o backup é restaurado para um local alternativo.|
|-authsysvol|Se usado, executa uma restauração autoritativa do SYSVOL (o diretório compartilhado do Volume do sistema).|
|-autoReboot|Especifica para reiniciar o sistema no final da operação de recuperação de estado do sistema. Esse parâmetro é válido somente para uma recuperação no local original. Não é recomendável que usar esse parâmetro se você precisar executar etapas após a operação de recuperação.|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="BKMK_examples"></a>Exemplos

- Para executar uma recuperação de estado do sistema do backup de 31/03/2013 às 9 da manhã, digite:  
  ```
  wbadmin start systemstaterecovery -version:03/31/2013-09:00
  ```  
- Para executar uma recuperação de estado do sistema do backup de 30/04/2013 às 9 da manhã que é armazenado no recurso compartilhado \\ \\servername\share para o server01, digite:  
  ```
  wbadmin start systemstaterecovery -version:04/30/2013-09:00 -backupTarget:\\servername\share -machine:server01
  ```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Start-WBSystemStateRecovery](https://technet.microsoft.com/library/jj902449.aspx) cmdlet